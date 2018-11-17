---
title: Dados espaciais – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817904"
---
# <a name="spatial-data"></a>Dados espaciais

> [!NOTE]
> Esse recurso é novo no EF Core 2.2.

Dados espaciais representam o local físico e a forma de objetos. Muitos bancos de dados oferecem suporte para esse tipo de dados para que possa ser indexado e consultado junto com outros dados. Cenários comuns incluem consultas para objetos dentro de uma determinada distância de um local, ou selecionando o objeto cuja borda contém um determinado local. O EF Core dá suporte ao mapeamento para tipos de dados espaciais usando o [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) biblioteca espacial.

## <a name="installing"></a>Instalando o

Para usar dados espaciais com o EF Core, você precisa instalar o pacote do NuGet suporte apropriado. Qual pacote você precisa instalar depende do provedor que você está usando.

Provedor do EF Core                        | Pacote do NuGet espacial
--------------------------------------- | ---------------------
Entityframeworkcore | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Entityframeworkcore    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Entityframeworkcore  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Engenharia reversa

O NuGet espacial também pacotes enable [engenharia reversa](../managing-schemas/scaffolding.md) modelos com propriedades espaciais, mas você precisam instalar o pacote ***antes*** executando `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`. Se você não fizer isso, você receberá avisos sobre a localização não mapeamentos de tipo para as colunas e as colunas serão ignoradas.

## <a name="nettopologysuite-nts"></a>NetTopologySuite NTS)

NetTopologySuite é uma biblioteca espacial para .NET. O EF Core permite mapear tipos de no banco de dados para dados espaciais usando tipos ntos de interrupção em seu modelo.

Para habilitar o mapeamento para tipos espaciais via NTS, chame o método de UseNetTopologySuite no construtor de opções de DbContext do provedor. Por exemplo, com o SQL Server você chamaria-lo como este.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Há vários tipos de dados espaciais. Tipo a ser usado depende dos tipos de formas que você deseja permitir. Aqui está a hierarquia de tipos ntos de interrupção que você pode usar para as propriedades em seu modelo. Elas estão localizadas dentro do `NetTopologySuite.Geometries` namespace. As interfaces correspondentes no pacote GeoAPI (`GeoAPI.Geometries` namespace) também pode ser usado.

* geometria
  * Ponto
  * LineString
  * Polígono
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve e CurePolygon não são suportadas pelo ntos de interrupção.

Usar o tipo de geometria base permite que qualquer tipo de forma a ser especificado pela propriedade.

As seguintes classes de entidade podem ser usadas para mapear a tabelas na [banco de dados de exemplo Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Criação de valores

Você pode usar construtores para criar objetos de geometria; No entanto, NTS recomenda usar uma fábrica de geometria em vez disso. Isso permite que você especifique um SRID (o sistema de referência espacial usado pelas coordenadas) padrão e lhe dá controle sobre as coisas mais avançadas, como o modelo de precisão (usado durante os cálculos) e a sequência de coordenadas (determina quais ordinates – dimensões e medidas – estão disponíveis).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 se refere a WGS 84, um padrão usado em GPS e outros sistemas geográficos.

### <a name="longitude-and-latitude"></a>Longitude e Latitude

As coordenadas em NTS estão em termos de valores X e Y. Para representar a longitude e latitude, use X para longitude e Y para latitude. Observe que isso **com versões anteriores** do `latitude, longitude` formato no qual você normalmente verá esses valores.

### <a name="srid-ignored-during-client-operations"></a>SRID ignorado durante operações do cliente

NTS ignora os valores SRID durante as operações. Ele pressupõe que um sistema de coordenadas planar. Isso significa que, se você especificar as coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área será em graus, não os medidores. Para valores mais significativos, você primeiro precisa projetar as coordenadas para outro sistema de coordenadas, uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.

Se uma operação é avaliada de servidor pelo EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.

Aqui está um exemplo de uso ProjNet4GeoAPI para calcular a distância entre duas cidades.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Consultar Dados

No LINQ, os métodos NTS e propriedades disponíveis, como funções de banco de dados serão convertidas em SQL. Por exemplo, os métodos de distância e Contains são convertidos nas consultas a seguir. A tabela no final deste artigo mostra membros que são suportados por vários provedores do EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Se você estiver usando o SQL Server, há algumas coisas adicionais, que você deve estar atento.

### <a name="geography-or-geometry"></a>Geometria ou geografia

Por padrão, as propriedades espaciais são mapeadas para `geography` colunas no SQL Server. Para usar `geometry`, [configurar o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.

### <a name="geography-polygon-rings"></a>Anéis de polígono de Geografia

Ao usar o `geography` tipo de coluna, o SQL Server impõe requisitos adicionais no anel exterior (ou shell) e interior anéis (ou falhas). O anel exterior deve ser orientado no sentido anti-horário e o interior anéis no sentido horário. NTS valida isso antes de enviar valores para o banco de dados.

### <a name="fullglobe"></a>FullGlobe

O SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o `geography` tipo de coluna. Ele também tem uma forma de representar os polígonos com base em todo o mundo completo (sem um anel exterior). Nenhum desses são compatíveis com NTS.

> [!WARNING]
> FullGlobe e polígonos com base nele não são compatíveis com NTS.

## <a name="sqlite"></a>SQLite

Aqui estão algumas informações adicionais para aqueles que usam o SQLite.

### <a name="installing-spatialite"></a>Instalando SpatiaLite

No Windows, a biblioteca nativa mod_spatialite é distribuída como uma dependência de pacote do NuGet. Outras plataformas é necessário instalá-lo separadamente. Isso normalmente é feito usando um Gerenciador de pacotes de software. Por exemplo, você pode usar o APT no Ubuntu e Homebrew no MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Configurando o SRID

No SpatiaLite, colunas precisam especificar um SRID por coluna. O padrão é de SRID `0`. Especifique um SRID diferente usando o método ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimensão

Semelhante a SRID, dimensão de uma coluna (ou ordinates) também são especificados como parte da coluna. Os ordinates padrão são X e Y. Habilite o ordinates adicional (Z e M) usando o método ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operações traduzidas

Esta tabela mostra quais membros NTS são convertidos em SQL por cada provedor do EF Core.

NetTopologySuite | SQL Server (geometria) | SQL Server (geografia) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (geometria, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (geometria, cadeia de caracteres) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Recursos adicionais

* [Dados espaciais no SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Home page do SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Documentação do Npgsql espacial](http://www.npgsql.org/efcore/mapping/nts.html)
* [Documentação de PostGIS](http://postgis.net/documentation/)
