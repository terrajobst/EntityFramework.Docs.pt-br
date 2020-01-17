---
title: Dados espaciais-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124425"
---
# <a name="spatial-data"></a><span data-ttu-id="1a405-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="1a405-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="1a405-103">Esse recurso foi adicionado no EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="1a405-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="1a405-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="1a405-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="1a405-105">Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="1a405-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="1a405-106">Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="1a405-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="1a405-107">EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="1a405-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="1a405-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="1a405-108">Installing</span></span>

<span data-ttu-id="1a405-109">Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="1a405-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="1a405-110">O pacote que você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="1a405-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="1a405-111">Provedor do EF Core</span><span class="sxs-lookup"><span data-stu-id="1a405-111">EF Core Provider</span></span>                        | <span data-ttu-id="1a405-112">Pacote NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="1a405-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="1a405-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="1a405-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="1a405-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1a405-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="1a405-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="1a405-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="1a405-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1a405-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="1a405-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="1a405-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="1a405-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1a405-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="1a405-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1a405-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="1a405-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1a405-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="1a405-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="1a405-121">Reverse engineering</span></span>

<span data-ttu-id="1a405-122">Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa instalar o pacote ***antes*** de executar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="1a405-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="1a405-123">Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="1a405-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="1a405-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="1a405-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="1a405-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="1a405-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="1a405-126">EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="1a405-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="1a405-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="1a405-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="1a405-128">Por exemplo, com SQL Server você o chamaria assim.</span><span class="sxs-lookup"><span data-stu-id="1a405-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="1a405-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="1a405-129">There are several spatial data types.</span></span> <span data-ttu-id="1a405-130">O tipo usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="1a405-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="1a405-131">Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="1a405-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="1a405-132">Eles estão localizados dentro do namespace `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="1a405-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="1a405-133">Geometria</span><span class="sxs-lookup"><span data-stu-id="1a405-133">Geometry</span></span>
  * <span data-ttu-id="1a405-134">Ponto</span><span class="sxs-lookup"><span data-stu-id="1a405-134">Point</span></span>
  * <span data-ttu-id="1a405-135">LineString</span><span class="sxs-lookup"><span data-stu-id="1a405-135">LineString</span></span>
  * <span data-ttu-id="1a405-136">Polígono</span><span class="sxs-lookup"><span data-stu-id="1a405-136">Polygon</span></span>
  * <span data-ttu-id="1a405-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="1a405-137">GeometryCollection</span></span>
    * <span data-ttu-id="1a405-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="1a405-138">MultiPoint</span></span>
    * <span data-ttu-id="1a405-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="1a405-139">MultiLineString</span></span>
    * <span data-ttu-id="1a405-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="1a405-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="1a405-141">Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.</span><span class="sxs-lookup"><span data-stu-id="1a405-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="1a405-142">Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="1a405-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="1a405-143">As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="1a405-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="1a405-144">Criando valores</span><span class="sxs-lookup"><span data-stu-id="1a405-144">Creating values</span></span>

<span data-ttu-id="1a405-145">Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="1a405-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="1a405-146">Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="1a405-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="1a405-147">4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="1a405-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="1a405-148">Longitude e latitude</span><span class="sxs-lookup"><span data-stu-id="1a405-148">Longitude and Latitude</span></span>

<span data-ttu-id="1a405-149">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="1a405-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="1a405-150">Para representar a longitude e a latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="1a405-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="1a405-151">Observe que isso é **retroativa** no formato de `latitude, longitude` no qual você normalmente vê esses valores.</span><span class="sxs-lookup"><span data-stu-id="1a405-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="1a405-152">SRID ignorado durante as operações do cliente</span><span class="sxs-lookup"><span data-stu-id="1a405-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="1a405-153">NTS ignora os valores de SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="1a405-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="1a405-154">Ele assume um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="1a405-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="1a405-155">Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros.</span><span class="sxs-lookup"><span data-stu-id="1a405-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="1a405-156">Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="1a405-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="1a405-157">Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1a405-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="1a405-158">Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="1a405-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="1a405-159">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="1a405-159">Querying Data</span></span>

<span data-ttu-id="1a405-160">No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL.</span><span class="sxs-lookup"><span data-stu-id="1a405-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="1a405-161">Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="1a405-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="1a405-162">A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a405-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="1a405-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1a405-163">SQL Server</span></span>

<span data-ttu-id="1a405-164">Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="1a405-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="1a405-165">Geografia ou Geometry</span><span class="sxs-lookup"><span data-stu-id="1a405-165">Geography or geometry</span></span>

<span data-ttu-id="1a405-166">Por padrão, as propriedades espaciais são mapeadas para `geography` colunas em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1a405-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="1a405-167">Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/entity-properties#column-data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="1a405-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="1a405-168">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="1a405-168">Geography polygon rings</span></span>

<span data-ttu-id="1a405-169">Ao usar o tipo de coluna `geography`, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos).</span><span class="sxs-lookup"><span data-stu-id="1a405-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="1a405-170">O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="1a405-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="1a405-171">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1a405-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="1a405-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="1a405-172">FullGlobe</span></span>

<span data-ttu-id="1a405-173">SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o tipo de coluna `geography`.</span><span class="sxs-lookup"><span data-stu-id="1a405-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="1a405-174">Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="1a405-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="1a405-175">Nenhuma delas é suportada pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="1a405-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="1a405-176">Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="1a405-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="1a405-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="1a405-177">SQLite</span></span>

<span data-ttu-id="1a405-178">Aqui estão algumas informações adicionais para as que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="1a405-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="1a405-179">Instalando o SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="1a405-179">Installing SpatiaLite</span></span>

<span data-ttu-id="1a405-180">No Windows, a biblioteca de mod_spatialite nativa é distribuída como uma dependência de pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="1a405-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="1a405-181">Outras plataformas precisam instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="1a405-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="1a405-182">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="1a405-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="1a405-183">Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="1a405-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="1a405-184">Infelizmente, as versões mais recentes do PROJ (uma dependência de SpatiaLite) são incompatíveis com o [pacote SQLitePCLRaw](/dotnet/standard/data/sqlite/custom-versions#bundles)padrão do EF.</span><span class="sxs-lookup"><span data-stu-id="1a405-184">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="1a405-185">Você pode contornar isso criando um [provedor de SQLitePCLRaw](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) personalizado que usa a biblioteca SQLite do sistema, ou pode instalar uma compilação personalizada do suporte à desabilitação do spatialite.</span><span class="sxs-lookup"><span data-stu-id="1a405-185">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a><span data-ttu-id="1a405-186">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="1a405-186">Configuring SRID</span></span>

<span data-ttu-id="1a405-187">No SpatiaLite, as colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="1a405-187">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="1a405-188">O SRID padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="1a405-188">The default SRID is `0`.</span></span> <span data-ttu-id="1a405-189">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="1a405-189">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="1a405-190">Dimensão</span><span class="sxs-lookup"><span data-stu-id="1a405-190">Dimension</span></span>

<span data-ttu-id="1a405-191">Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="1a405-191">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="1a405-192">As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="1a405-192">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="1a405-193">Operações convertidas</span><span class="sxs-lookup"><span data-stu-id="1a405-193">Translated Operations</span></span>

<span data-ttu-id="1a405-194">Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1a405-194">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="1a405-195">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1a405-195">NetTopologySuite</span></span> | <span data-ttu-id="1a405-196">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-196">SQL Server (geometry)</span></span> | <span data-ttu-id="1a405-197">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="1a405-197">SQL Server (geography)</span></span> | <span data-ttu-id="1a405-198">SQLite</span><span class="sxs-lookup"><span data-stu-id="1a405-198">SQLite</span></span> | <span data-ttu-id="1a405-199">Npgsql</span><span class="sxs-lookup"><span data-stu-id="1a405-199">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="1a405-200">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="1a405-200">Geometry.Area</span></span> | <span data-ttu-id="1a405-201">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-201">✔</span></span> | <span data-ttu-id="1a405-202">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-202">✔</span></span> | <span data-ttu-id="1a405-203">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-203">✔</span></span> | <span data-ttu-id="1a405-204">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-204">✔</span></span>
<span data-ttu-id="1a405-205">Geometry. asbinary ()</span><span class="sxs-lookup"><span data-stu-id="1a405-205">Geometry.AsBinary()</span></span> | <span data-ttu-id="1a405-206">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-206">✔</span></span> | <span data-ttu-id="1a405-207">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-207">✔</span></span> | <span data-ttu-id="1a405-208">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-208">✔</span></span> | <span data-ttu-id="1a405-209">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-209">✔</span></span>
<span data-ttu-id="1a405-210">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="1a405-210">Geometry.AsText()</span></span> | <span data-ttu-id="1a405-211">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-211">✔</span></span> | <span data-ttu-id="1a405-212">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-212">✔</span></span> | <span data-ttu-id="1a405-213">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-213">✔</span></span> | <span data-ttu-id="1a405-214">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-214">✔</span></span>
<span data-ttu-id="1a405-215">Geometry. limite</span><span class="sxs-lookup"><span data-stu-id="1a405-215">Geometry.Boundary</span></span> | <span data-ttu-id="1a405-216">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-216">✔</span></span> | | <span data-ttu-id="1a405-217">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-217">✔</span></span> | <span data-ttu-id="1a405-218">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-218">✔</span></span>
<span data-ttu-id="1a405-219">Geometry. buffer (duplo)</span><span class="sxs-lookup"><span data-stu-id="1a405-219">Geometry.Buffer(double)</span></span> | <span data-ttu-id="1a405-220">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-220">✔</span></span> | <span data-ttu-id="1a405-221">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-221">✔</span></span> | <span data-ttu-id="1a405-222">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-222">✔</span></span> | <span data-ttu-id="1a405-223">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-223">✔</span></span>
<span data-ttu-id="1a405-224">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="1a405-224">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="1a405-225">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-225">✔</span></span> | <span data-ttu-id="1a405-226">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-226">✔</span></span>
<span data-ttu-id="1a405-227">Geometry. centróide</span><span class="sxs-lookup"><span data-stu-id="1a405-227">Geometry.Centroid</span></span> | <span data-ttu-id="1a405-228">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-228">✔</span></span> | | <span data-ttu-id="1a405-229">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-229">✔</span></span> | <span data-ttu-id="1a405-230">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-230">✔</span></span>
<span data-ttu-id="1a405-231">Geometry. Contains (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-231">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="1a405-232">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-232">✔</span></span> | <span data-ttu-id="1a405-233">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-233">✔</span></span> | <span data-ttu-id="1a405-234">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-234">✔</span></span> | <span data-ttu-id="1a405-235">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-235">✔</span></span>
<span data-ttu-id="1a405-236">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="1a405-236">Geometry.ConvexHull()</span></span> | <span data-ttu-id="1a405-237">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-237">✔</span></span> | <span data-ttu-id="1a405-238">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-238">✔</span></span> | <span data-ttu-id="1a405-239">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-239">✔</span></span> | <span data-ttu-id="1a405-240">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-240">✔</span></span>
<span data-ttu-id="1a405-241">Geometry. CoveredBy (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-241">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="1a405-242">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-242">✔</span></span> | <span data-ttu-id="1a405-243">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-243">✔</span></span>
<span data-ttu-id="1a405-244">Geometry. cubras (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-244">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="1a405-245">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-245">✔</span></span> | <span data-ttu-id="1a405-246">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-246">✔</span></span>
<span data-ttu-id="1a405-247">Geometry. Crosss (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-247">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="1a405-248">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-248">✔</span></span> | | <span data-ttu-id="1a405-249">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-249">✔</span></span> | <span data-ttu-id="1a405-250">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-250">✔</span></span>
<span data-ttu-id="1a405-251">Geometry. Difference (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-251">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="1a405-252">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-252">✔</span></span> | <span data-ttu-id="1a405-253">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-253">✔</span></span> | <span data-ttu-id="1a405-254">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-254">✔</span></span> | <span data-ttu-id="1a405-255">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-255">✔</span></span>
<span data-ttu-id="1a405-256">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="1a405-256">Geometry.Dimension</span></span> | <span data-ttu-id="1a405-257">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-257">✔</span></span> | <span data-ttu-id="1a405-258">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-258">✔</span></span> | <span data-ttu-id="1a405-259">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-259">✔</span></span> | <span data-ttu-id="1a405-260">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-260">✔</span></span>
<span data-ttu-id="1a405-261">Geometry. disjunção (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-261">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="1a405-262">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-262">✔</span></span> | <span data-ttu-id="1a405-263">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-263">✔</span></span> | <span data-ttu-id="1a405-264">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-264">✔</span></span> | <span data-ttu-id="1a405-265">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-265">✔</span></span>
<span data-ttu-id="1a405-266">Geometry. distance (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-266">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="1a405-267">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-267">✔</span></span> | <span data-ttu-id="1a405-268">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-268">✔</span></span> | <span data-ttu-id="1a405-269">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-269">✔</span></span> | <span data-ttu-id="1a405-270">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-270">✔</span></span>
<span data-ttu-id="1a405-271">Geometry. envelope</span><span class="sxs-lookup"><span data-stu-id="1a405-271">Geometry.Envelope</span></span> | <span data-ttu-id="1a405-272">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-272">✔</span></span> | | <span data-ttu-id="1a405-273">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-273">✔</span></span> | <span data-ttu-id="1a405-274">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-274">✔</span></span>
<span data-ttu-id="1a405-275">Geometry. EqualsExact (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-275">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="1a405-276">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-276">✔</span></span>
<span data-ttu-id="1a405-277">Geometry. EqualsTopologically (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-277">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="1a405-278">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-278">✔</span></span> | <span data-ttu-id="1a405-279">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-279">✔</span></span> | <span data-ttu-id="1a405-280">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-280">✔</span></span> | <span data-ttu-id="1a405-281">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-281">✔</span></span>
<span data-ttu-id="1a405-282">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="1a405-282">Geometry.GeometryType</span></span> | <span data-ttu-id="1a405-283">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-283">✔</span></span> | <span data-ttu-id="1a405-284">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-284">✔</span></span> | <span data-ttu-id="1a405-285">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-285">✔</span></span> | <span data-ttu-id="1a405-286">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-286">✔</span></span>
<span data-ttu-id="1a405-287">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="1a405-287">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="1a405-288">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-288">✔</span></span> | | <span data-ttu-id="1a405-289">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-289">✔</span></span> | <span data-ttu-id="1a405-290">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-290">✔</span></span>
<span data-ttu-id="1a405-291">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="1a405-291">Geometry.InteriorPoint</span></span> | <span data-ttu-id="1a405-292">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-292">✔</span></span> | | <span data-ttu-id="1a405-293">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-293">✔</span></span> | <span data-ttu-id="1a405-294">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-294">✔</span></span>
<span data-ttu-id="1a405-295">Geometry. Intersection (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-295">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="1a405-296">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-296">✔</span></span> | <span data-ttu-id="1a405-297">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-297">✔</span></span> | <span data-ttu-id="1a405-298">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-298">✔</span></span> | <span data-ttu-id="1a405-299">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-299">✔</span></span>
<span data-ttu-id="1a405-300">Geometry. Intersects (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-300">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="1a405-301">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-301">✔</span></span> | <span data-ttu-id="1a405-302">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-302">✔</span></span> | <span data-ttu-id="1a405-303">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-303">✔</span></span> | <span data-ttu-id="1a405-304">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-304">✔</span></span>
<span data-ttu-id="1a405-305">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="1a405-305">Geometry.IsEmpty</span></span> | <span data-ttu-id="1a405-306">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-306">✔</span></span> | <span data-ttu-id="1a405-307">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-307">✔</span></span> | <span data-ttu-id="1a405-308">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-308">✔</span></span> | <span data-ttu-id="1a405-309">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-309">✔</span></span>
<span data-ttu-id="1a405-310">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="1a405-310">Geometry.IsSimple</span></span> | <span data-ttu-id="1a405-311">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-311">✔</span></span> | | <span data-ttu-id="1a405-312">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-312">✔</span></span> | <span data-ttu-id="1a405-313">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-313">✔</span></span>
<span data-ttu-id="1a405-314">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="1a405-314">Geometry.IsValid</span></span> | <span data-ttu-id="1a405-315">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-315">✔</span></span> | <span data-ttu-id="1a405-316">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-316">✔</span></span> | <span data-ttu-id="1a405-317">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-317">✔</span></span> | <span data-ttu-id="1a405-318">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-318">✔</span></span>
<span data-ttu-id="1a405-319">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="1a405-319">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="1a405-320">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-320">✔</span></span> | | <span data-ttu-id="1a405-321">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-321">✔</span></span> | <span data-ttu-id="1a405-322">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-322">✔</span></span>
<span data-ttu-id="1a405-323">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="1a405-323">Geometry.Length</span></span> | <span data-ttu-id="1a405-324">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-324">✔</span></span> | <span data-ttu-id="1a405-325">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-325">✔</span></span> | <span data-ttu-id="1a405-326">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-326">✔</span></span> | <span data-ttu-id="1a405-327">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-327">✔</span></span>
<span data-ttu-id="1a405-328">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="1a405-328">Geometry.NumGeometries</span></span> | <span data-ttu-id="1a405-329">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-329">✔</span></span> | <span data-ttu-id="1a405-330">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-330">✔</span></span> | <span data-ttu-id="1a405-331">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-331">✔</span></span> | <span data-ttu-id="1a405-332">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-332">✔</span></span>
<span data-ttu-id="1a405-333">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="1a405-333">Geometry.NumPoints</span></span> | <span data-ttu-id="1a405-334">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-334">✔</span></span> | <span data-ttu-id="1a405-335">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-335">✔</span></span> | <span data-ttu-id="1a405-336">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-336">✔</span></span> | <span data-ttu-id="1a405-337">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-337">✔</span></span>
<span data-ttu-id="1a405-338">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="1a405-338">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="1a405-339">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-339">✔</span></span> | <span data-ttu-id="1a405-340">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-340">✔</span></span> | <span data-ttu-id="1a405-341">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-341">✔</span></span> | <span data-ttu-id="1a405-342">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-342">✔</span></span>
<span data-ttu-id="1a405-343">Geometry. oversobrepõetes (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-343">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="1a405-344">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-344">✔</span></span> | <span data-ttu-id="1a405-345">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-345">✔</span></span> | <span data-ttu-id="1a405-346">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-346">✔</span></span> | <span data-ttu-id="1a405-347">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-347">✔</span></span>
<span data-ttu-id="1a405-348">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="1a405-348">Geometry.PointOnSurface</span></span> | <span data-ttu-id="1a405-349">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-349">✔</span></span> | | <span data-ttu-id="1a405-350">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-350">✔</span></span> | <span data-ttu-id="1a405-351">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-351">✔</span></span>
<span data-ttu-id="1a405-352">Geometry. relacioná (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="1a405-352">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="1a405-353">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-353">✔</span></span> | | <span data-ttu-id="1a405-354">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-354">✔</span></span> | <span data-ttu-id="1a405-355">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-355">✔</span></span>
<span data-ttu-id="1a405-356">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="1a405-356">Geometry.Reverse()</span></span> | | | <span data-ttu-id="1a405-357">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-357">✔</span></span> | <span data-ttu-id="1a405-358">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-358">✔</span></span>
<span data-ttu-id="1a405-359">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="1a405-359">Geometry.SRID</span></span> | <span data-ttu-id="1a405-360">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-360">✔</span></span> | <span data-ttu-id="1a405-361">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-361">✔</span></span> | <span data-ttu-id="1a405-362">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-362">✔</span></span> | <span data-ttu-id="1a405-363">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-363">✔</span></span>
<span data-ttu-id="1a405-364">Geometry. SymmetricDifference (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-364">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="1a405-365">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-365">✔</span></span> | <span data-ttu-id="1a405-366">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-366">✔</span></span> | <span data-ttu-id="1a405-367">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-367">✔</span></span> | <span data-ttu-id="1a405-368">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-368">✔</span></span>
<span data-ttu-id="1a405-369">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="1a405-369">Geometry.ToBinary()</span></span> | <span data-ttu-id="1a405-370">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-370">✔</span></span> | <span data-ttu-id="1a405-371">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-371">✔</span></span> | <span data-ttu-id="1a405-372">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-372">✔</span></span> | <span data-ttu-id="1a405-373">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-373">✔</span></span>
<span data-ttu-id="1a405-374">Geometry. totext ()</span><span class="sxs-lookup"><span data-stu-id="1a405-374">Geometry.ToText()</span></span> | <span data-ttu-id="1a405-375">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-375">✔</span></span> | <span data-ttu-id="1a405-376">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-376">✔</span></span> | <span data-ttu-id="1a405-377">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-377">✔</span></span> | <span data-ttu-id="1a405-378">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-378">✔</span></span>
<span data-ttu-id="1a405-379">Geometry. toques (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-379">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="1a405-380">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-380">✔</span></span> | | <span data-ttu-id="1a405-381">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-381">✔</span></span> | <span data-ttu-id="1a405-382">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-382">✔</span></span>
<span data-ttu-id="1a405-383">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="1a405-383">Geometry.Union()</span></span> | | | <span data-ttu-id="1a405-384">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-384">✔</span></span> | <span data-ttu-id="1a405-385">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-385">✔</span></span>
<span data-ttu-id="1a405-386">Geometry. Union (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-386">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="1a405-387">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-387">✔</span></span> | <span data-ttu-id="1a405-388">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-388">✔</span></span> | <span data-ttu-id="1a405-389">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-389">✔</span></span> | <span data-ttu-id="1a405-390">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-390">✔</span></span>
<span data-ttu-id="1a405-391">Geometry. Within (geometry)</span><span class="sxs-lookup"><span data-stu-id="1a405-391">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="1a405-392">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-392">✔</span></span> | <span data-ttu-id="1a405-393">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-393">✔</span></span> | <span data-ttu-id="1a405-394">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-394">✔</span></span> | <span data-ttu-id="1a405-395">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-395">✔</span></span>
<span data-ttu-id="1a405-396">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="1a405-396">GeometryCollection.Count</span></span> | <span data-ttu-id="1a405-397">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-397">✔</span></span> | <span data-ttu-id="1a405-398">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-398">✔</span></span> | <span data-ttu-id="1a405-399">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-399">✔</span></span> | <span data-ttu-id="1a405-400">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-400">✔</span></span>
<span data-ttu-id="1a405-401">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="1a405-401">GeometryCollection[int]</span></span> | <span data-ttu-id="1a405-402">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-402">✔</span></span> | <span data-ttu-id="1a405-403">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-403">✔</span></span> | <span data-ttu-id="1a405-404">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-404">✔</span></span> | <span data-ttu-id="1a405-405">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-405">✔</span></span>
<span data-ttu-id="1a405-406">Linhastring. contagem</span><span class="sxs-lookup"><span data-stu-id="1a405-406">LineString.Count</span></span> | <span data-ttu-id="1a405-407">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-407">✔</span></span> | <span data-ttu-id="1a405-408">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-408">✔</span></span> | <span data-ttu-id="1a405-409">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-409">✔</span></span> | <span data-ttu-id="1a405-410">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-410">✔</span></span>
<span data-ttu-id="1a405-411">LineString. ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="1a405-411">LineString.EndPoint</span></span> | <span data-ttu-id="1a405-412">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-412">✔</span></span> | <span data-ttu-id="1a405-413">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-413">✔</span></span> | <span data-ttu-id="1a405-414">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-414">✔</span></span> | <span data-ttu-id="1a405-415">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-415">✔</span></span>
<span data-ttu-id="1a405-416">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="1a405-416">LineString.GetPointN(int)</span></span> | <span data-ttu-id="1a405-417">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-417">✔</span></span> | <span data-ttu-id="1a405-418">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-418">✔</span></span> | <span data-ttu-id="1a405-419">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-419">✔</span></span> | <span data-ttu-id="1a405-420">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-420">✔</span></span>
<span data-ttu-id="1a405-421">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="1a405-421">LineString.IsClosed</span></span> | <span data-ttu-id="1a405-422">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-422">✔</span></span> | <span data-ttu-id="1a405-423">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-423">✔</span></span> | <span data-ttu-id="1a405-424">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-424">✔</span></span> | <span data-ttu-id="1a405-425">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-425">✔</span></span>
<span data-ttu-id="1a405-426">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="1a405-426">LineString.IsRing</span></span> | <span data-ttu-id="1a405-427">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-427">✔</span></span> | | <span data-ttu-id="1a405-428">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-428">✔</span></span> | <span data-ttu-id="1a405-429">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-429">✔</span></span>
<span data-ttu-id="1a405-430">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="1a405-430">LineString.StartPoint</span></span> | <span data-ttu-id="1a405-431">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-431">✔</span></span> | <span data-ttu-id="1a405-432">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-432">✔</span></span> | <span data-ttu-id="1a405-433">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-433">✔</span></span> | <span data-ttu-id="1a405-434">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-434">✔</span></span>
<span data-ttu-id="1a405-435">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="1a405-435">MultiLineString.IsClosed</span></span> | <span data-ttu-id="1a405-436">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-436">✔</span></span> | <span data-ttu-id="1a405-437">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-437">✔</span></span> | <span data-ttu-id="1a405-438">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-438">✔</span></span> | <span data-ttu-id="1a405-439">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-439">✔</span></span>
<span data-ttu-id="1a405-440">Ponto. M</span><span class="sxs-lookup"><span data-stu-id="1a405-440">Point.M</span></span> | <span data-ttu-id="1a405-441">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-441">✔</span></span> | <span data-ttu-id="1a405-442">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-442">✔</span></span> | <span data-ttu-id="1a405-443">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-443">✔</span></span> | <span data-ttu-id="1a405-444">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-444">✔</span></span>
<span data-ttu-id="1a405-445">Ponto. X</span><span class="sxs-lookup"><span data-stu-id="1a405-445">Point.X</span></span> | <span data-ttu-id="1a405-446">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-446">✔</span></span> | <span data-ttu-id="1a405-447">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-447">✔</span></span> | <span data-ttu-id="1a405-448">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-448">✔</span></span> | <span data-ttu-id="1a405-449">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-449">✔</span></span>
<span data-ttu-id="1a405-450">Ponto. Y</span><span class="sxs-lookup"><span data-stu-id="1a405-450">Point.Y</span></span> | <span data-ttu-id="1a405-451">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-451">✔</span></span> | <span data-ttu-id="1a405-452">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-452">✔</span></span> | <span data-ttu-id="1a405-453">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-453">✔</span></span> | <span data-ttu-id="1a405-454">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-454">✔</span></span>
<span data-ttu-id="1a405-455">Ponto. Z</span><span class="sxs-lookup"><span data-stu-id="1a405-455">Point.Z</span></span> | <span data-ttu-id="1a405-456">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-456">✔</span></span> | <span data-ttu-id="1a405-457">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-457">✔</span></span> | <span data-ttu-id="1a405-458">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-458">✔</span></span> | <span data-ttu-id="1a405-459">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-459">✔</span></span>
<span data-ttu-id="1a405-460">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="1a405-460">Polygon.ExteriorRing</span></span> | <span data-ttu-id="1a405-461">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-461">✔</span></span> | <span data-ttu-id="1a405-462">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-462">✔</span></span> | <span data-ttu-id="1a405-463">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-463">✔</span></span> | <span data-ttu-id="1a405-464">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-464">✔</span></span>
<span data-ttu-id="1a405-465">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="1a405-465">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="1a405-466">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-466">✔</span></span> | <span data-ttu-id="1a405-467">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-467">✔</span></span> | <span data-ttu-id="1a405-468">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-468">✔</span></span> | <span data-ttu-id="1a405-469">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-469">✔</span></span>
<span data-ttu-id="1a405-470">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="1a405-470">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="1a405-471">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-471">✔</span></span> | <span data-ttu-id="1a405-472">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-472">✔</span></span> | <span data-ttu-id="1a405-473">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-473">✔</span></span> | <span data-ttu-id="1a405-474">✔</span><span class="sxs-lookup"><span data-stu-id="1a405-474">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a405-475">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1a405-475">Additional resources</span></span>

* [<span data-ttu-id="1a405-476">Dados espaciais em SQL Server</span><span class="sxs-lookup"><span data-stu-id="1a405-476">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="1a405-477">Home Page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="1a405-477">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="1a405-478">Documentação espacial do Npgsql</span><span class="sxs-lookup"><span data-stu-id="1a405-478">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="1a405-479">Documentação do PostGIS</span><span class="sxs-lookup"><span data-stu-id="1a405-479">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
