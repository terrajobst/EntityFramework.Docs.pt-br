---
title: Dados espaciais-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cced53edadb890e4e86753ec2628218ffc4d1d5b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181388"
---
# <a name="spatial-data"></a>Dados espaciais

> [!NOTE]
> Esse recurso foi adicionado no EF Core 2,2.

Dados espaciais representam o local físico e a forma de objetos. Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados. Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local. EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .

## <a name="installing"></a>Instalando o

Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado. O pacote que você precisa instalar depende do provedor que você está usando.

Provedor de EF Core                        | Pacote NuGet espacial
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Engenharia reversa

Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa instalar o pacote ***antes*** de executar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`. Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite é uma biblioteca espacial para .NET. EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.

Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor. Por exemplo, com SQL Server você o chamaria assim.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Há vários tipos de dados espaciais. O tipo usado depende dos tipos de formas que você deseja permitir. Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo. Eles estão localizados no namespace `NetTopologySuite.Geometries`.

* Geometry
  * Ponto
  * LineString
  * Polygon
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.

Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.

As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>Criando valores

Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso. Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.

### <a name="longitude-and-latitude"></a>Longitude e latitude

As coordenadas em NTS estão em termos de valores X e Y. Para representar a longitude e a latitude, use X para longitude e Y para latitude. Observe que isso é **retroativa** no formato `latitude, longitude` no qual você normalmente vê esses valores.

### <a name="srid-ignored-during-client-operations"></a>SRID ignorado durante as operações do cliente

NTS ignora os valores de SRID durante as operações. Ele assume um sistema de coordenadas planar. Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros. Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.

Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.

Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Consultar Dados

No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL. Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir. A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.

### <a name="geography-or-geometry"></a>Geografia ou Geometry

Por padrão, as propriedades espaciais são mapeadas para colunas `geography` em SQL Server. Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.

### <a name="geography-polygon-rings"></a>Anéis de polígono de Geografia

Ao usar o tipo de coluna `geography`, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos). O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário. NTS valida isso antes de enviar valores para o banco de dados.

### <a name="fullglobe"></a>FullGlobe

SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o tipo de coluna `geography`. Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior). Nenhuma delas é suportada pelo NTS.

> [!WARNING]
> Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.

## <a name="sqlite"></a>SQLite

Aqui estão algumas informações adicionais para as que usam o SQLite.

### <a name="installing-spatialite"></a>Instalando o SpatiaLite

No Windows, a biblioteca mod_spatialite nativa é distribuída como uma dependência de pacote NuGet. Outras plataformas precisam instalá-lo separadamente. Isso normalmente é feito usando um Gerenciador de pacotes de software. Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Configurando o SRID

No SpatiaLite, as colunas precisam especificar um SRID por coluna. O SRID padrão é `0`. Especifique um SRID diferente usando o método ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimensão

Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna. As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operações convertidas

Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.

NetTopologySuite | SQL Server (geometry) | SQL Server (Geografia) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry. Area | ✔ | ✔ | ✔ | ✔
Geometry. asbinary () | ✔ | ✔ | ✔ | ✔
Geometry. astext () | ✔ | ✔ | ✔ | ✔
Geometry. limite | ✔ | | ✔ | ✔
Geometry. buffer (duplo) | ✔ | ✔ | ✔ | ✔
Geometry. buffer (Double, int) | | | ✔
Geometry. centróide | ✔ | | ✔ | ✔
Geometry. Contains (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. ConvexHull () | ✔ | ✔ | ✔ | ✔
Geometry. CoveredBy (geometry) | | | ✔ | ✔
Geometry. cubras (geometry) | | | ✔ | ✔
Geometry. Crosss (geometry) | ✔ | | ✔ | ✔
Geometry. Difference (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. Dimension | ✔ | ✔ | ✔ | ✔
Geometry. disjunção (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. distance (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. envelope | ✔ | | ✔ | ✔
Geometry. EqualsExact (geometry) | | | | ✔
Geometry. EqualsTopologically (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. GeometryType | ✔ | ✔ | ✔ | ✔
Geometry. getgeometryn (int) | ✔ | | ✔ | ✔
Geometry. InteriorPoint | ✔ | | ✔
Geometry. Intersection (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. Intersects (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry. IsSimple | ✔ | | ✔ | ✔
Geometry. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. IsWithinDistance (Geometry, Double) | ✔ | | ✔
Geometry. Length | ✔ | ✔ | ✔ | ✔
Geometry. NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry. NumPoints | ✔ | ✔ | ✔ | ✔
Geometry. OgcGeometryType | ✔ | ✔ | ✔
Geometry. oversobrepõetes (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. PointOnSurface | ✔ | | ✔ | ✔
Geometry. relacioná (Geometry, String) | ✔ | | ✔ | ✔
Geometry. Reverse () | | | ✔ | ✔
Geometry. SRID | ✔ | ✔ | ✔ | ✔
Geometry. SymmetricDifference (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. ToBinary () | ✔ | ✔ | ✔ | ✔
Geometry. totext () | ✔ | ✔ | ✔ | ✔
Geometry. toques (geometry) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔
Geometry. Union (geometry) | ✔ | ✔ | ✔ | ✔
Geometry. Within (geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection. Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
Linhastring. contagem | ✔ | ✔ | ✔ | ✔
LineString. ponto de extremidade | ✔ | ✔ | ✔ | ✔
LineString. getpointn (int) | ✔ | ✔ | ✔ | ✔
LineString. IsClosed | ✔ | ✔ | ✔ | ✔
LineString. IsRing | ✔ | | ✔ | ✔
LineString. StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString. IsClosed | ✔ | ✔ | ✔ | ✔
Ponto. M | ✔ | ✔ | ✔ | ✔
Ponto. X | ✔ | ✔ | ✔ | ✔
Ponto. Y | ✔ | ✔ | ✔ | ✔
Ponto. Z | ✔ | ✔ | ✔ | ✔
Polygon. ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon. GetInteriorRingN (int) | ✔ | ✔ | ✔ | ✔
Polygon. NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Recursos adicionais

* [Dados espaciais em SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Home Page do SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Documentação espacial do Npgsql](https://www.npgsql.org/efcore/mapping/nts.html)
* [Documentação do PostGIS](https://postgis.net/documentation/)
