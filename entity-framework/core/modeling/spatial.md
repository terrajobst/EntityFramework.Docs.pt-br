---
title: Dados espaciais-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 026df735473e31f1c1463c1fbc6f46c4fd6dfd4f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921730"
---
# <a name="spatial-data"></a><span data-ttu-id="e7c97-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="e7c97-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="e7c97-103">Esse recurso foi adicionado no EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="e7c97-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="e7c97-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="e7c97-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="e7c97-105">Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="e7c97-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="e7c97-106">Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="e7c97-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="e7c97-107">EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="e7c97-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="e7c97-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="e7c97-108">Installing</span></span>

<span data-ttu-id="e7c97-109">Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="e7c97-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="e7c97-110">O pacote que você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="e7c97-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="e7c97-111">Provedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="e7c97-111">EF Core Provider</span></span>                        | <span data-ttu-id="e7c97-112">Pacote NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="e7c97-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="e7c97-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e7c97-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="e7c97-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e7c97-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="e7c97-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="e7c97-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="e7c97-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e7c97-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="e7c97-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="e7c97-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="e7c97-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e7c97-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="e7c97-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e7c97-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="e7c97-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e7c97-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="e7c97-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="e7c97-121">Reverse engineering</span></span>

<span data-ttu-id="e7c97-122">Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa `Scaffold-DbContext` instalar `dotnet ef dbcontext scaffold`o pacote ***antes*** de executar ou.</span><span class="sxs-lookup"><span data-stu-id="e7c97-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="e7c97-123">Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="e7c97-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="e7c97-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="e7c97-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="e7c97-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="e7c97-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="e7c97-126">EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e7c97-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="e7c97-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="e7c97-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="e7c97-128">Por exemplo, com SQL Server você o chamaria assim.</span><span class="sxs-lookup"><span data-stu-id="e7c97-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="e7c97-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="e7c97-129">There are several spatial data types.</span></span> <span data-ttu-id="e7c97-130">O tipo usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="e7c97-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="e7c97-131">Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e7c97-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="e7c97-132">Eles estão localizados no `NetTopologySuite.Geometries` namespace.</span><span class="sxs-lookup"><span data-stu-id="e7c97-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="e7c97-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="e7c97-133">Geometry</span></span>
  * <span data-ttu-id="e7c97-134">Ponto</span><span class="sxs-lookup"><span data-stu-id="e7c97-134">Point</span></span>
  * <span data-ttu-id="e7c97-135">LineString</span><span class="sxs-lookup"><span data-stu-id="e7c97-135">LineString</span></span>
  * <span data-ttu-id="e7c97-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="e7c97-136">Polygon</span></span>
  * <span data-ttu-id="e7c97-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="e7c97-137">GeometryCollection</span></span>
    * <span data-ttu-id="e7c97-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="e7c97-138">MultiPoint</span></span>
    * <span data-ttu-id="e7c97-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="e7c97-139">MultiLineString</span></span>
    * <span data-ttu-id="e7c97-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="e7c97-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="e7c97-141">Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.</span><span class="sxs-lookup"><span data-stu-id="e7c97-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="e7c97-142">Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="e7c97-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="e7c97-143">As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="e7c97-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="e7c97-144">Criando valores</span><span class="sxs-lookup"><span data-stu-id="e7c97-144">Creating values</span></span>

<span data-ttu-id="e7c97-145">Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="e7c97-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="e7c97-146">Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="e7c97-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="e7c97-147">4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="e7c97-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="e7c97-148">Longitude e latitude</span><span class="sxs-lookup"><span data-stu-id="e7c97-148">Longitude and Latitude</span></span>

<span data-ttu-id="e7c97-149">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="e7c97-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="e7c97-150">Para representar a longitude e a latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="e7c97-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="e7c97-151">Observe que isso é para **trás** do `latitude, longitude` formato em que você normalmente vê esses valores.</span><span class="sxs-lookup"><span data-stu-id="e7c97-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="e7c97-152">SRID ignorado durante as operações do cliente</span><span class="sxs-lookup"><span data-stu-id="e7c97-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="e7c97-153">NTS ignora os valores de SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="e7c97-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="e7c97-154">Ele assume um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="e7c97-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="e7c97-155">Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros.</span><span class="sxs-lookup"><span data-stu-id="e7c97-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="e7c97-156">Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="e7c97-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="e7c97-157">Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7c97-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="e7c97-158">Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="e7c97-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="e7c97-159">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="e7c97-159">Querying Data</span></span>

<span data-ttu-id="e7c97-160">No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL.</span><span class="sxs-lookup"><span data-stu-id="e7c97-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="e7c97-161">Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7c97-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="e7c97-162">A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7c97-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="e7c97-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e7c97-163">SQL Server</span></span>

<span data-ttu-id="e7c97-164">Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="e7c97-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="e7c97-165">Geografia ou Geometry</span><span class="sxs-lookup"><span data-stu-id="e7c97-165">Geography or geometry</span></span>

<span data-ttu-id="e7c97-166">Por padrão, as propriedades espaciais são `geography` mapeadas para colunas em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e7c97-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="e7c97-167">Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e7c97-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="e7c97-168">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="e7c97-168">Geography polygon rings</span></span>

<span data-ttu-id="e7c97-169">Ao usar o `geography` tipo de coluna, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos).</span><span class="sxs-lookup"><span data-stu-id="e7c97-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="e7c97-170">O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="e7c97-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="e7c97-171">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e7c97-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="e7c97-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="e7c97-172">FullGlobe</span></span>

<span data-ttu-id="e7c97-173">SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o `geography` tipo de coluna.</span><span class="sxs-lookup"><span data-stu-id="e7c97-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="e7c97-174">Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="e7c97-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="e7c97-175">Nenhuma delas é suportada pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="e7c97-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="e7c97-176">Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="e7c97-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="e7c97-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="e7c97-177">SQLite</span></span>

<span data-ttu-id="e7c97-178">Aqui estão algumas informações adicionais para as que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="e7c97-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="e7c97-179">Instalando o SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e7c97-179">Installing SpatiaLite</span></span>

<span data-ttu-id="e7c97-180">No Windows, a biblioteca mod_spatialite nativa é distribuída como uma dependência de pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7c97-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="e7c97-181">Outras plataformas precisam instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="e7c97-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="e7c97-182">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="e7c97-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="e7c97-183">Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="e7c97-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="e7c97-184">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="e7c97-184">Configuring SRID</span></span>

<span data-ttu-id="e7c97-185">No SpatiaLite, as colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="e7c97-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="e7c97-186">O SRID padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="e7c97-186">The default SRID is `0`.</span></span> <span data-ttu-id="e7c97-187">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="e7c97-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="e7c97-188">Dimensão</span><span class="sxs-lookup"><span data-stu-id="e7c97-188">Dimension</span></span>

<span data-ttu-id="e7c97-189">Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="e7c97-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="e7c97-190">As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="e7c97-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="e7c97-191">Operações convertidas</span><span class="sxs-lookup"><span data-stu-id="e7c97-191">Translated Operations</span></span>

<span data-ttu-id="e7c97-192">Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7c97-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="e7c97-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e7c97-193">NetTopologySuite</span></span> | <span data-ttu-id="e7c97-194">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-194">SQL Server (geometry)</span></span> | <span data-ttu-id="e7c97-195">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="e7c97-195">SQL Server (geography)</span></span> | <span data-ttu-id="e7c97-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="e7c97-196">SQLite</span></span> | <span data-ttu-id="e7c97-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="e7c97-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="e7c97-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="e7c97-198">Geometry.Area</span></span> | <span data-ttu-id="e7c97-199">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-199">✔</span></span> | <span data-ttu-id="e7c97-200">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-200">✔</span></span> | <span data-ttu-id="e7c97-201">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-201">✔</span></span> | <span data-ttu-id="e7c97-202">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-202">✔</span></span>
<span data-ttu-id="e7c97-203">Geometry. asbinary ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="e7c97-204">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-204">✔</span></span> | <span data-ttu-id="e7c97-205">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-205">✔</span></span> | <span data-ttu-id="e7c97-206">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-206">✔</span></span> | <span data-ttu-id="e7c97-207">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-207">✔</span></span>
<span data-ttu-id="e7c97-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-208">Geometry.AsText()</span></span> | <span data-ttu-id="e7c97-209">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-209">✔</span></span> | <span data-ttu-id="e7c97-210">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-210">✔</span></span> | <span data-ttu-id="e7c97-211">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-211">✔</span></span> | <span data-ttu-id="e7c97-212">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-212">✔</span></span>
<span data-ttu-id="e7c97-213">Geometry. limite</span><span class="sxs-lookup"><span data-stu-id="e7c97-213">Geometry.Boundary</span></span> | <span data-ttu-id="e7c97-214">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-214">✔</span></span> | | <span data-ttu-id="e7c97-215">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-215">✔</span></span> | <span data-ttu-id="e7c97-216">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-216">✔</span></span>
<span data-ttu-id="e7c97-217">Geometry. buffer (duplo)</span><span class="sxs-lookup"><span data-stu-id="e7c97-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="e7c97-218">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-218">✔</span></span> | <span data-ttu-id="e7c97-219">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-219">✔</span></span> | <span data-ttu-id="e7c97-220">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-220">✔</span></span> | <span data-ttu-id="e7c97-221">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-221">✔</span></span>
<span data-ttu-id="e7c97-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="e7c97-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="e7c97-223">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-223">✔</span></span>
<span data-ttu-id="e7c97-224">Geometry. centróide</span><span class="sxs-lookup"><span data-stu-id="e7c97-224">Geometry.Centroid</span></span> | <span data-ttu-id="e7c97-225">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-225">✔</span></span> | | <span data-ttu-id="e7c97-226">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-226">✔</span></span> | <span data-ttu-id="e7c97-227">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-227">✔</span></span>
<span data-ttu-id="e7c97-228">Geometry. Contains (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="e7c97-229">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-229">✔</span></span> | <span data-ttu-id="e7c97-230">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-230">✔</span></span> | <span data-ttu-id="e7c97-231">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-231">✔</span></span> | <span data-ttu-id="e7c97-232">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-232">✔</span></span>
<span data-ttu-id="e7c97-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="e7c97-234">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-234">✔</span></span> | <span data-ttu-id="e7c97-235">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-235">✔</span></span> | <span data-ttu-id="e7c97-236">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-236">✔</span></span> | <span data-ttu-id="e7c97-237">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-237">✔</span></span>
<span data-ttu-id="e7c97-238">Geometry. CoveredBy (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="e7c97-239">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-239">✔</span></span> | <span data-ttu-id="e7c97-240">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-240">✔</span></span>
<span data-ttu-id="e7c97-241">Geometry. cubras (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="e7c97-242">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-242">✔</span></span> | <span data-ttu-id="e7c97-243">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-243">✔</span></span>
<span data-ttu-id="e7c97-244">Geometry. Crosss (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="e7c97-245">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-245">✔</span></span> | | <span data-ttu-id="e7c97-246">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-246">✔</span></span> | <span data-ttu-id="e7c97-247">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-247">✔</span></span>
<span data-ttu-id="e7c97-248">Geometry. Difference (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="e7c97-249">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-249">✔</span></span> | <span data-ttu-id="e7c97-250">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-250">✔</span></span> | <span data-ttu-id="e7c97-251">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-251">✔</span></span> | <span data-ttu-id="e7c97-252">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-252">✔</span></span>
<span data-ttu-id="e7c97-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="e7c97-253">Geometry.Dimension</span></span> | <span data-ttu-id="e7c97-254">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-254">✔</span></span> | <span data-ttu-id="e7c97-255">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-255">✔</span></span> | <span data-ttu-id="e7c97-256">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-256">✔</span></span> | <span data-ttu-id="e7c97-257">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-257">✔</span></span>
<span data-ttu-id="e7c97-258">Geometry. disjunção (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="e7c97-259">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-259">✔</span></span> | <span data-ttu-id="e7c97-260">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-260">✔</span></span> | <span data-ttu-id="e7c97-261">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-261">✔</span></span> | <span data-ttu-id="e7c97-262">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-262">✔</span></span>
<span data-ttu-id="e7c97-263">Geometry. distance (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="e7c97-264">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-264">✔</span></span> | <span data-ttu-id="e7c97-265">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-265">✔</span></span> | <span data-ttu-id="e7c97-266">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-266">✔</span></span> | <span data-ttu-id="e7c97-267">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-267">✔</span></span>
<span data-ttu-id="e7c97-268">Geometry. envelope</span><span class="sxs-lookup"><span data-stu-id="e7c97-268">Geometry.Envelope</span></span> | <span data-ttu-id="e7c97-269">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-269">✔</span></span> | | <span data-ttu-id="e7c97-270">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-270">✔</span></span> | <span data-ttu-id="e7c97-271">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-271">✔</span></span>
<span data-ttu-id="e7c97-272">Geometry. EqualsExact (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="e7c97-273">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-273">✔</span></span>
<span data-ttu-id="e7c97-274">Geometry. EqualsTopologically (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="e7c97-275">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-275">✔</span></span> | <span data-ttu-id="e7c97-276">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-276">✔</span></span> | <span data-ttu-id="e7c97-277">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-277">✔</span></span> | <span data-ttu-id="e7c97-278">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-278">✔</span></span>
<span data-ttu-id="e7c97-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="e7c97-279">Geometry.GeometryType</span></span> | <span data-ttu-id="e7c97-280">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-280">✔</span></span> | <span data-ttu-id="e7c97-281">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-281">✔</span></span> | <span data-ttu-id="e7c97-282">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-282">✔</span></span> | <span data-ttu-id="e7c97-283">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-283">✔</span></span>
<span data-ttu-id="e7c97-284">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="e7c97-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="e7c97-285">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-285">✔</span></span> | | <span data-ttu-id="e7c97-286">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-286">✔</span></span> | <span data-ttu-id="e7c97-287">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-287">✔</span></span>
<span data-ttu-id="e7c97-288">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="e7c97-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="e7c97-289">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-289">✔</span></span> | | <span data-ttu-id="e7c97-290">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-290">✔</span></span>
<span data-ttu-id="e7c97-291">Geometry. Intersection (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="e7c97-292">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-292">✔</span></span> | <span data-ttu-id="e7c97-293">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-293">✔</span></span> | <span data-ttu-id="e7c97-294">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-294">✔</span></span> | <span data-ttu-id="e7c97-295">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-295">✔</span></span>
<span data-ttu-id="e7c97-296">Geometry. Intersects (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="e7c97-297">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-297">✔</span></span> | <span data-ttu-id="e7c97-298">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-298">✔</span></span> | <span data-ttu-id="e7c97-299">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-299">✔</span></span> | <span data-ttu-id="e7c97-300">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-300">✔</span></span>
<span data-ttu-id="e7c97-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="e7c97-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="e7c97-302">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-302">✔</span></span> | <span data-ttu-id="e7c97-303">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-303">✔</span></span> | <span data-ttu-id="e7c97-304">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-304">✔</span></span> | <span data-ttu-id="e7c97-305">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-305">✔</span></span>
<span data-ttu-id="e7c97-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="e7c97-306">Geometry.IsSimple</span></span> | <span data-ttu-id="e7c97-307">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-307">✔</span></span> | | <span data-ttu-id="e7c97-308">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-308">✔</span></span> | <span data-ttu-id="e7c97-309">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-309">✔</span></span>
<span data-ttu-id="e7c97-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="e7c97-310">Geometry.IsValid</span></span> | <span data-ttu-id="e7c97-311">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-311">✔</span></span> | <span data-ttu-id="e7c97-312">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-312">✔</span></span> | <span data-ttu-id="e7c97-313">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-313">✔</span></span> | <span data-ttu-id="e7c97-314">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-314">✔</span></span>
<span data-ttu-id="e7c97-315">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="e7c97-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="e7c97-316">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-316">✔</span></span> | | <span data-ttu-id="e7c97-317">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-317">✔</span></span>
<span data-ttu-id="e7c97-318">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="e7c97-318">Geometry.Length</span></span> | <span data-ttu-id="e7c97-319">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-319">✔</span></span> | <span data-ttu-id="e7c97-320">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-320">✔</span></span> | <span data-ttu-id="e7c97-321">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-321">✔</span></span> | <span data-ttu-id="e7c97-322">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-322">✔</span></span>
<span data-ttu-id="e7c97-323">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="e7c97-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="e7c97-324">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-324">✔</span></span> | <span data-ttu-id="e7c97-325">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-325">✔</span></span> | <span data-ttu-id="e7c97-326">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-326">✔</span></span> | <span data-ttu-id="e7c97-327">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-327">✔</span></span>
<span data-ttu-id="e7c97-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="e7c97-328">Geometry.NumPoints</span></span> | <span data-ttu-id="e7c97-329">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-329">✔</span></span> | <span data-ttu-id="e7c97-330">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-330">✔</span></span> | <span data-ttu-id="e7c97-331">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-331">✔</span></span> | <span data-ttu-id="e7c97-332">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-332">✔</span></span>
<span data-ttu-id="e7c97-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="e7c97-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="e7c97-334">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-334">✔</span></span> | <span data-ttu-id="e7c97-335">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-335">✔</span></span> | <span data-ttu-id="e7c97-336">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-336">✔</span></span>
<span data-ttu-id="e7c97-337">Geometry. oversobrepõetes (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="e7c97-338">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-338">✔</span></span> | <span data-ttu-id="e7c97-339">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-339">✔</span></span> | <span data-ttu-id="e7c97-340">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-340">✔</span></span> | <span data-ttu-id="e7c97-341">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-341">✔</span></span>
<span data-ttu-id="e7c97-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="e7c97-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="e7c97-343">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-343">✔</span></span> | | <span data-ttu-id="e7c97-344">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-344">✔</span></span> | <span data-ttu-id="e7c97-345">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-345">✔</span></span>
<span data-ttu-id="e7c97-346">Geometry. relacioná (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="e7c97-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="e7c97-347">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-347">✔</span></span> | | <span data-ttu-id="e7c97-348">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-348">✔</span></span> | <span data-ttu-id="e7c97-349">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-349">✔</span></span>
<span data-ttu-id="e7c97-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="e7c97-351">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-351">✔</span></span> | <span data-ttu-id="e7c97-352">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-352">✔</span></span>
<span data-ttu-id="e7c97-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="e7c97-353">Geometry.SRID</span></span> | <span data-ttu-id="e7c97-354">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-354">✔</span></span> | <span data-ttu-id="e7c97-355">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-355">✔</span></span> | <span data-ttu-id="e7c97-356">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-356">✔</span></span> | <span data-ttu-id="e7c97-357">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-357">✔</span></span>
<span data-ttu-id="e7c97-358">Geometry. SymmetricDifference (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="e7c97-359">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-359">✔</span></span> | <span data-ttu-id="e7c97-360">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-360">✔</span></span> | <span data-ttu-id="e7c97-361">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-361">✔</span></span> | <span data-ttu-id="e7c97-362">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-362">✔</span></span>
<span data-ttu-id="e7c97-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="e7c97-364">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-364">✔</span></span> | <span data-ttu-id="e7c97-365">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-365">✔</span></span> | <span data-ttu-id="e7c97-366">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-366">✔</span></span> | <span data-ttu-id="e7c97-367">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-367">✔</span></span>
<span data-ttu-id="e7c97-368">Geometry. totext ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-368">Geometry.ToText()</span></span> | <span data-ttu-id="e7c97-369">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-369">✔</span></span> | <span data-ttu-id="e7c97-370">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-370">✔</span></span> | <span data-ttu-id="e7c97-371">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-371">✔</span></span> | <span data-ttu-id="e7c97-372">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-372">✔</span></span>
<span data-ttu-id="e7c97-373">Geometry. toques (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="e7c97-374">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-374">✔</span></span> | | <span data-ttu-id="e7c97-375">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-375">✔</span></span> | <span data-ttu-id="e7c97-376">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-376">✔</span></span>
<span data-ttu-id="e7c97-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="e7c97-377">Geometry.Union()</span></span> | | | <span data-ttu-id="e7c97-378">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-378">✔</span></span>
<span data-ttu-id="e7c97-379">Geometry. Union (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="e7c97-380">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-380">✔</span></span> | <span data-ttu-id="e7c97-381">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-381">✔</span></span> | <span data-ttu-id="e7c97-382">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-382">✔</span></span> | <span data-ttu-id="e7c97-383">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-383">✔</span></span>
<span data-ttu-id="e7c97-384">Geometry. Within (geometry)</span><span class="sxs-lookup"><span data-stu-id="e7c97-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="e7c97-385">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-385">✔</span></span> | <span data-ttu-id="e7c97-386">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-386">✔</span></span> | <span data-ttu-id="e7c97-387">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-387">✔</span></span> | <span data-ttu-id="e7c97-388">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-388">✔</span></span>
<span data-ttu-id="e7c97-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="e7c97-389">GeometryCollection.Count</span></span> | <span data-ttu-id="e7c97-390">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-390">✔</span></span> | <span data-ttu-id="e7c97-391">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-391">✔</span></span> | <span data-ttu-id="e7c97-392">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-392">✔</span></span> | <span data-ttu-id="e7c97-393">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-393">✔</span></span>
<span data-ttu-id="e7c97-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="e7c97-394">GeometryCollection[int]</span></span> | <span data-ttu-id="e7c97-395">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-395">✔</span></span> | <span data-ttu-id="e7c97-396">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-396">✔</span></span> | <span data-ttu-id="e7c97-397">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-397">✔</span></span> | <span data-ttu-id="e7c97-398">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-398">✔</span></span>
<span data-ttu-id="e7c97-399">Linhastring. contagem</span><span class="sxs-lookup"><span data-stu-id="e7c97-399">LineString.Count</span></span> | <span data-ttu-id="e7c97-400">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-400">✔</span></span> | <span data-ttu-id="e7c97-401">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-401">✔</span></span> | <span data-ttu-id="e7c97-402">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-402">✔</span></span> | <span data-ttu-id="e7c97-403">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-403">✔</span></span>
<span data-ttu-id="e7c97-404">LineString. ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="e7c97-404">LineString.EndPoint</span></span> | <span data-ttu-id="e7c97-405">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-405">✔</span></span> | <span data-ttu-id="e7c97-406">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-406">✔</span></span> | <span data-ttu-id="e7c97-407">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-407">✔</span></span> | <span data-ttu-id="e7c97-408">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-408">✔</span></span>
<span data-ttu-id="e7c97-409">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="e7c97-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="e7c97-410">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-410">✔</span></span> | <span data-ttu-id="e7c97-411">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-411">✔</span></span> | <span data-ttu-id="e7c97-412">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-412">✔</span></span> | <span data-ttu-id="e7c97-413">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-413">✔</span></span>
<span data-ttu-id="e7c97-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="e7c97-414">LineString.IsClosed</span></span> | <span data-ttu-id="e7c97-415">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-415">✔</span></span> | <span data-ttu-id="e7c97-416">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-416">✔</span></span> | <span data-ttu-id="e7c97-417">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-417">✔</span></span> | <span data-ttu-id="e7c97-418">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-418">✔</span></span>
<span data-ttu-id="e7c97-419">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="e7c97-419">LineString.IsRing</span></span> | <span data-ttu-id="e7c97-420">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-420">✔</span></span> | | <span data-ttu-id="e7c97-421">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-421">✔</span></span> | <span data-ttu-id="e7c97-422">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-422">✔</span></span>
<span data-ttu-id="e7c97-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="e7c97-423">LineString.StartPoint</span></span> | <span data-ttu-id="e7c97-424">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-424">✔</span></span> | <span data-ttu-id="e7c97-425">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-425">✔</span></span> | <span data-ttu-id="e7c97-426">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-426">✔</span></span> | <span data-ttu-id="e7c97-427">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-427">✔</span></span>
<span data-ttu-id="e7c97-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="e7c97-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="e7c97-429">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-429">✔</span></span> | <span data-ttu-id="e7c97-430">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-430">✔</span></span> | <span data-ttu-id="e7c97-431">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-431">✔</span></span> | <span data-ttu-id="e7c97-432">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-432">✔</span></span>
<span data-ttu-id="e7c97-433">Ponto. M</span><span class="sxs-lookup"><span data-stu-id="e7c97-433">Point.M</span></span> | <span data-ttu-id="e7c97-434">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-434">✔</span></span> | <span data-ttu-id="e7c97-435">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-435">✔</span></span> | <span data-ttu-id="e7c97-436">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-436">✔</span></span> | <span data-ttu-id="e7c97-437">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-437">✔</span></span>
<span data-ttu-id="e7c97-438">Ponto. X</span><span class="sxs-lookup"><span data-stu-id="e7c97-438">Point.X</span></span> | <span data-ttu-id="e7c97-439">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-439">✔</span></span> | <span data-ttu-id="e7c97-440">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-440">✔</span></span> | <span data-ttu-id="e7c97-441">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-441">✔</span></span> | <span data-ttu-id="e7c97-442">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-442">✔</span></span>
<span data-ttu-id="e7c97-443">Ponto. Y</span><span class="sxs-lookup"><span data-stu-id="e7c97-443">Point.Y</span></span> | <span data-ttu-id="e7c97-444">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-444">✔</span></span> | <span data-ttu-id="e7c97-445">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-445">✔</span></span> | <span data-ttu-id="e7c97-446">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-446">✔</span></span> | <span data-ttu-id="e7c97-447">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-447">✔</span></span>
<span data-ttu-id="e7c97-448">Ponto. Z</span><span class="sxs-lookup"><span data-stu-id="e7c97-448">Point.Z</span></span> | <span data-ttu-id="e7c97-449">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-449">✔</span></span> | <span data-ttu-id="e7c97-450">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-450">✔</span></span> | <span data-ttu-id="e7c97-451">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-451">✔</span></span> | <span data-ttu-id="e7c97-452">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-452">✔</span></span>
<span data-ttu-id="e7c97-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="e7c97-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="e7c97-454">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-454">✔</span></span> | <span data-ttu-id="e7c97-455">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-455">✔</span></span> | <span data-ttu-id="e7c97-456">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-456">✔</span></span> | <span data-ttu-id="e7c97-457">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-457">✔</span></span>
<span data-ttu-id="e7c97-458">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="e7c97-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="e7c97-459">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-459">✔</span></span> | <span data-ttu-id="e7c97-460">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-460">✔</span></span> | <span data-ttu-id="e7c97-461">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-461">✔</span></span> | <span data-ttu-id="e7c97-462">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-462">✔</span></span>
<span data-ttu-id="e7c97-463">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="e7c97-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="e7c97-464">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-464">✔</span></span> | <span data-ttu-id="e7c97-465">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-465">✔</span></span> | <span data-ttu-id="e7c97-466">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-466">✔</span></span> | <span data-ttu-id="e7c97-467">✔</span><span class="sxs-lookup"><span data-stu-id="e7c97-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7c97-468">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e7c97-468">Additional resources</span></span>

* [<span data-ttu-id="e7c97-469">Dados espaciais em SQL Server</span><span class="sxs-lookup"><span data-stu-id="e7c97-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="e7c97-470">Home Page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e7c97-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="e7c97-471">Documentação espacial do Npgsql</span><span class="sxs-lookup"><span data-stu-id="e7c97-471">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="e7c97-472">Documentação do PostGIS</span><span class="sxs-lookup"><span data-stu-id="e7c97-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
