---
title: Dados espaciais-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 335d4f3a601624f7c994b7dcacefe4ef6798beb3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655608"
---
# <a name="spatial-data"></a><span data-ttu-id="c00d8-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="c00d8-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="c00d8-103">Esse recurso foi adicionado no EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="c00d8-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="c00d8-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="c00d8-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="c00d8-105">Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="c00d8-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="c00d8-106">Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="c00d8-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="c00d8-107">EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="c00d8-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="c00d8-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="c00d8-108">Installing</span></span>

<span data-ttu-id="c00d8-109">Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="c00d8-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="c00d8-110">O pacote que você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="c00d8-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="c00d8-111">Provedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="c00d8-111">EF Core Provider</span></span>                        | <span data-ttu-id="c00d8-112">Pacote NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="c00d8-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="c00d8-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="c00d8-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="c00d8-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c00d8-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="c00d8-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="c00d8-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="c00d8-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c00d8-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="c00d8-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="c00d8-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="c00d8-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c00d8-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="c00d8-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c00d8-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="c00d8-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c00d8-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="c00d8-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="c00d8-121">Reverse engineering</span></span>

<span data-ttu-id="c00d8-122">Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa instalar o pacote ***antes*** de executar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="c00d8-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="c00d8-123">Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="c00d8-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="c00d8-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="c00d8-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="c00d8-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="c00d8-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="c00d8-126">EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="c00d8-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="c00d8-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="c00d8-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="c00d8-128">Por exemplo, com SQL Server você o chamaria assim.</span><span class="sxs-lookup"><span data-stu-id="c00d8-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="c00d8-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="c00d8-129">There are several spatial data types.</span></span> <span data-ttu-id="c00d8-130">O tipo usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="c00d8-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="c00d8-131">Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="c00d8-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="c00d8-132">Eles estão localizados dentro do namespace `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="c00d8-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="c00d8-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="c00d8-133">Geometry</span></span>
  * <span data-ttu-id="c00d8-134">Ponto</span><span class="sxs-lookup"><span data-stu-id="c00d8-134">Point</span></span>
  * <span data-ttu-id="c00d8-135">LineString</span><span class="sxs-lookup"><span data-stu-id="c00d8-135">LineString</span></span>
  * <span data-ttu-id="c00d8-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="c00d8-136">Polygon</span></span>
  * <span data-ttu-id="c00d8-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="c00d8-137">GeometryCollection</span></span>
    * <span data-ttu-id="c00d8-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="c00d8-138">MultiPoint</span></span>
    * <span data-ttu-id="c00d8-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="c00d8-139">MultiLineString</span></span>
    * <span data-ttu-id="c00d8-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="c00d8-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="c00d8-141">Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.</span><span class="sxs-lookup"><span data-stu-id="c00d8-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="c00d8-142">Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="c00d8-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="c00d8-143">As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="c00d8-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="c00d8-144">Criando valores</span><span class="sxs-lookup"><span data-stu-id="c00d8-144">Creating values</span></span>

<span data-ttu-id="c00d8-145">Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="c00d8-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="c00d8-146">Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="c00d8-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="c00d8-147">4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="c00d8-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="c00d8-148">Longitude e latitude</span><span class="sxs-lookup"><span data-stu-id="c00d8-148">Longitude and Latitude</span></span>

<span data-ttu-id="c00d8-149">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="c00d8-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="c00d8-150">Para representar a longitude e a latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="c00d8-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="c00d8-151">Observe que isso é **retroativa** no formato de `latitude, longitude` no qual você normalmente vê esses valores.</span><span class="sxs-lookup"><span data-stu-id="c00d8-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="c00d8-152">SRID ignorado durante as operações do cliente</span><span class="sxs-lookup"><span data-stu-id="c00d8-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="c00d8-153">NTS ignora os valores de SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="c00d8-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="c00d8-154">Ele assume um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="c00d8-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="c00d8-155">Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros.</span><span class="sxs-lookup"><span data-stu-id="c00d8-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="c00d8-156">Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="c00d8-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="c00d8-157">Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c00d8-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="c00d8-158">Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="c00d8-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="c00d8-159">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="c00d8-159">Querying Data</span></span>

<span data-ttu-id="c00d8-160">No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL.</span><span class="sxs-lookup"><span data-stu-id="c00d8-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="c00d8-161">Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="c00d8-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="c00d8-162">A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c00d8-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="c00d8-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c00d8-163">SQL Server</span></span>

<span data-ttu-id="c00d8-164">Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="c00d8-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="c00d8-165">Geografia ou Geometry</span><span class="sxs-lookup"><span data-stu-id="c00d8-165">Geography or geometry</span></span>

<span data-ttu-id="c00d8-166">Por padrão, as propriedades espaciais são mapeadas para `geography` colunas em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c00d8-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="c00d8-167">Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="c00d8-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="c00d8-168">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="c00d8-168">Geography polygon rings</span></span>

<span data-ttu-id="c00d8-169">Ao usar o tipo de coluna `geography`, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos).</span><span class="sxs-lookup"><span data-stu-id="c00d8-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="c00d8-170">O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="c00d8-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="c00d8-171">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c00d8-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="c00d8-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="c00d8-172">FullGlobe</span></span>

<span data-ttu-id="c00d8-173">SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o tipo de coluna `geography`.</span><span class="sxs-lookup"><span data-stu-id="c00d8-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="c00d8-174">Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="c00d8-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="c00d8-175">Nenhuma delas é suportada pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="c00d8-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="c00d8-176">Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="c00d8-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="c00d8-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="c00d8-177">SQLite</span></span>

<span data-ttu-id="c00d8-178">Aqui estão algumas informações adicionais para as que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="c00d8-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="c00d8-179">Instalando o SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="c00d8-179">Installing SpatiaLite</span></span>

<span data-ttu-id="c00d8-180">No Windows, a biblioteca de mod_spatialite nativa é distribuída como uma dependência de pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="c00d8-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="c00d8-181">Outras plataformas precisam instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="c00d8-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="c00d8-182">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="c00d8-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="c00d8-183">Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="c00d8-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="c00d8-184">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="c00d8-184">Configuring SRID</span></span>

<span data-ttu-id="c00d8-185">No SpatiaLite, as colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="c00d8-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="c00d8-186">O SRID padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="c00d8-186">The default SRID is `0`.</span></span> <span data-ttu-id="c00d8-187">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="c00d8-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="c00d8-188">Dimensão</span><span class="sxs-lookup"><span data-stu-id="c00d8-188">Dimension</span></span>

<span data-ttu-id="c00d8-189">Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="c00d8-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="c00d8-190">As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="c00d8-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="c00d8-191">Operações convertidas</span><span class="sxs-lookup"><span data-stu-id="c00d8-191">Translated Operations</span></span>

<span data-ttu-id="c00d8-192">Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c00d8-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="c00d8-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c00d8-193">NetTopologySuite</span></span> | <span data-ttu-id="c00d8-194">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-194">SQL Server (geometry)</span></span> | <span data-ttu-id="c00d8-195">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="c00d8-195">SQL Server (geography)</span></span> | <span data-ttu-id="c00d8-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="c00d8-196">SQLite</span></span> | <span data-ttu-id="c00d8-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="c00d8-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="c00d8-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="c00d8-198">Geometry.Area</span></span> | <span data-ttu-id="c00d8-199">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-199">✔</span></span> | <span data-ttu-id="c00d8-200">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-200">✔</span></span> | <span data-ttu-id="c00d8-201">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-201">✔</span></span> | <span data-ttu-id="c00d8-202">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-202">✔</span></span>
<span data-ttu-id="c00d8-203">Geometry. asbinary ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="c00d8-204">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-204">✔</span></span> | <span data-ttu-id="c00d8-205">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-205">✔</span></span> | <span data-ttu-id="c00d8-206">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-206">✔</span></span> | <span data-ttu-id="c00d8-207">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-207">✔</span></span>
<span data-ttu-id="c00d8-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-208">Geometry.AsText()</span></span> | <span data-ttu-id="c00d8-209">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-209">✔</span></span> | <span data-ttu-id="c00d8-210">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-210">✔</span></span> | <span data-ttu-id="c00d8-211">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-211">✔</span></span> | <span data-ttu-id="c00d8-212">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-212">✔</span></span>
<span data-ttu-id="c00d8-213">Geometry. limite</span><span class="sxs-lookup"><span data-stu-id="c00d8-213">Geometry.Boundary</span></span> | <span data-ttu-id="c00d8-214">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-214">✔</span></span> | | <span data-ttu-id="c00d8-215">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-215">✔</span></span> | <span data-ttu-id="c00d8-216">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-216">✔</span></span>
<span data-ttu-id="c00d8-217">Geometry. buffer (duplo)</span><span class="sxs-lookup"><span data-stu-id="c00d8-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="c00d8-218">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-218">✔</span></span> | <span data-ttu-id="c00d8-219">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-219">✔</span></span> | <span data-ttu-id="c00d8-220">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-220">✔</span></span> | <span data-ttu-id="c00d8-221">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-221">✔</span></span>
<span data-ttu-id="c00d8-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="c00d8-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="c00d8-223">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-223">✔</span></span> | <span data-ttu-id="c00d8-224">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-224">✔</span></span>
<span data-ttu-id="c00d8-225">Geometry. centróide</span><span class="sxs-lookup"><span data-stu-id="c00d8-225">Geometry.Centroid</span></span> | <span data-ttu-id="c00d8-226">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-226">✔</span></span> | | <span data-ttu-id="c00d8-227">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-227">✔</span></span> | <span data-ttu-id="c00d8-228">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-228">✔</span></span>
<span data-ttu-id="c00d8-229">Geometry. Contains (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="c00d8-230">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-230">✔</span></span> | <span data-ttu-id="c00d8-231">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-231">✔</span></span> | <span data-ttu-id="c00d8-232">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-232">✔</span></span> | <span data-ttu-id="c00d8-233">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-233">✔</span></span>
<span data-ttu-id="c00d8-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="c00d8-235">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-235">✔</span></span> | <span data-ttu-id="c00d8-236">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-236">✔</span></span> | <span data-ttu-id="c00d8-237">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-237">✔</span></span> | <span data-ttu-id="c00d8-238">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-238">✔</span></span>
<span data-ttu-id="c00d8-239">Geometry. CoveredBy (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="c00d8-240">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-240">✔</span></span> | <span data-ttu-id="c00d8-241">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-241">✔</span></span>
<span data-ttu-id="c00d8-242">Geometry. cubras (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="c00d8-243">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-243">✔</span></span> | <span data-ttu-id="c00d8-244">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-244">✔</span></span>
<span data-ttu-id="c00d8-245">Geometry. Crosss (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="c00d8-246">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-246">✔</span></span> | | <span data-ttu-id="c00d8-247">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-247">✔</span></span> | <span data-ttu-id="c00d8-248">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-248">✔</span></span>
<span data-ttu-id="c00d8-249">Geometry. Difference (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="c00d8-250">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-250">✔</span></span> | <span data-ttu-id="c00d8-251">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-251">✔</span></span> | <span data-ttu-id="c00d8-252">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-252">✔</span></span> | <span data-ttu-id="c00d8-253">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-253">✔</span></span>
<span data-ttu-id="c00d8-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="c00d8-254">Geometry.Dimension</span></span> | <span data-ttu-id="c00d8-255">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-255">✔</span></span> | <span data-ttu-id="c00d8-256">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-256">✔</span></span> | <span data-ttu-id="c00d8-257">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-257">✔</span></span> | <span data-ttu-id="c00d8-258">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-258">✔</span></span>
<span data-ttu-id="c00d8-259">Geometry. disjunção (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="c00d8-260">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-260">✔</span></span> | <span data-ttu-id="c00d8-261">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-261">✔</span></span> | <span data-ttu-id="c00d8-262">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-262">✔</span></span> | <span data-ttu-id="c00d8-263">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-263">✔</span></span>
<span data-ttu-id="c00d8-264">Geometry. distance (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="c00d8-265">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-265">✔</span></span> | <span data-ttu-id="c00d8-266">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-266">✔</span></span> | <span data-ttu-id="c00d8-267">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-267">✔</span></span> | <span data-ttu-id="c00d8-268">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-268">✔</span></span>
<span data-ttu-id="c00d8-269">Geometry. envelope</span><span class="sxs-lookup"><span data-stu-id="c00d8-269">Geometry.Envelope</span></span> | <span data-ttu-id="c00d8-270">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-270">✔</span></span> | | <span data-ttu-id="c00d8-271">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-271">✔</span></span> | <span data-ttu-id="c00d8-272">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-272">✔</span></span>
<span data-ttu-id="c00d8-273">Geometry. EqualsExact (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="c00d8-274">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-274">✔</span></span>
<span data-ttu-id="c00d8-275">Geometry. EqualsTopologically (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="c00d8-276">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-276">✔</span></span> | <span data-ttu-id="c00d8-277">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-277">✔</span></span> | <span data-ttu-id="c00d8-278">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-278">✔</span></span> | <span data-ttu-id="c00d8-279">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-279">✔</span></span>
<span data-ttu-id="c00d8-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="c00d8-280">Geometry.GeometryType</span></span> | <span data-ttu-id="c00d8-281">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-281">✔</span></span> | <span data-ttu-id="c00d8-282">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-282">✔</span></span> | <span data-ttu-id="c00d8-283">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-283">✔</span></span> | <span data-ttu-id="c00d8-284">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-284">✔</span></span>
<span data-ttu-id="c00d8-285">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="c00d8-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="c00d8-286">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-286">✔</span></span> | | <span data-ttu-id="c00d8-287">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-287">✔</span></span> | <span data-ttu-id="c00d8-288">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-288">✔</span></span>
<span data-ttu-id="c00d8-289">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="c00d8-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="c00d8-290">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-290">✔</span></span> | | <span data-ttu-id="c00d8-291">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-291">✔</span></span> | <span data-ttu-id="c00d8-292">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-292">✔</span></span>
<span data-ttu-id="c00d8-293">Geometry. Intersection (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="c00d8-294">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-294">✔</span></span> | <span data-ttu-id="c00d8-295">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-295">✔</span></span> | <span data-ttu-id="c00d8-296">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-296">✔</span></span> | <span data-ttu-id="c00d8-297">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-297">✔</span></span>
<span data-ttu-id="c00d8-298">Geometry. Intersects (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="c00d8-299">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-299">✔</span></span> | <span data-ttu-id="c00d8-300">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-300">✔</span></span> | <span data-ttu-id="c00d8-301">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-301">✔</span></span> | <span data-ttu-id="c00d8-302">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-302">✔</span></span>
<span data-ttu-id="c00d8-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="c00d8-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="c00d8-304">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-304">✔</span></span> | <span data-ttu-id="c00d8-305">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-305">✔</span></span> | <span data-ttu-id="c00d8-306">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-306">✔</span></span> | <span data-ttu-id="c00d8-307">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-307">✔</span></span>
<span data-ttu-id="c00d8-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="c00d8-308">Geometry.IsSimple</span></span> | <span data-ttu-id="c00d8-309">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-309">✔</span></span> | | <span data-ttu-id="c00d8-310">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-310">✔</span></span> | <span data-ttu-id="c00d8-311">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-311">✔</span></span>
<span data-ttu-id="c00d8-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="c00d8-312">Geometry.IsValid</span></span> | <span data-ttu-id="c00d8-313">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-313">✔</span></span> | <span data-ttu-id="c00d8-314">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-314">✔</span></span> | <span data-ttu-id="c00d8-315">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-315">✔</span></span> | <span data-ttu-id="c00d8-316">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-316">✔</span></span>
<span data-ttu-id="c00d8-317">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="c00d8-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="c00d8-318">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-318">✔</span></span> | | <span data-ttu-id="c00d8-319">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-319">✔</span></span> | <span data-ttu-id="c00d8-320">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-320">✔</span></span>
<span data-ttu-id="c00d8-321">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="c00d8-321">Geometry.Length</span></span> | <span data-ttu-id="c00d8-322">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-322">✔</span></span> | <span data-ttu-id="c00d8-323">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-323">✔</span></span> | <span data-ttu-id="c00d8-324">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-324">✔</span></span> | <span data-ttu-id="c00d8-325">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-325">✔</span></span>
<span data-ttu-id="c00d8-326">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="c00d8-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="c00d8-327">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-327">✔</span></span> | <span data-ttu-id="c00d8-328">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-328">✔</span></span> | <span data-ttu-id="c00d8-329">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-329">✔</span></span> | <span data-ttu-id="c00d8-330">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-330">✔</span></span>
<span data-ttu-id="c00d8-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="c00d8-331">Geometry.NumPoints</span></span> | <span data-ttu-id="c00d8-332">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-332">✔</span></span> | <span data-ttu-id="c00d8-333">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-333">✔</span></span> | <span data-ttu-id="c00d8-334">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-334">✔</span></span> | <span data-ttu-id="c00d8-335">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-335">✔</span></span>
<span data-ttu-id="c00d8-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="c00d8-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="c00d8-337">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-337">✔</span></span> | <span data-ttu-id="c00d8-338">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-338">✔</span></span> | <span data-ttu-id="c00d8-339">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-339">✔</span></span> | <span data-ttu-id="c00d8-340">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-340">✔</span></span>
<span data-ttu-id="c00d8-341">Geometry. oversobrepõetes (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="c00d8-342">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-342">✔</span></span> | <span data-ttu-id="c00d8-343">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-343">✔</span></span> | <span data-ttu-id="c00d8-344">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-344">✔</span></span> | <span data-ttu-id="c00d8-345">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-345">✔</span></span>
<span data-ttu-id="c00d8-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="c00d8-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="c00d8-347">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-347">✔</span></span> | | <span data-ttu-id="c00d8-348">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-348">✔</span></span> | <span data-ttu-id="c00d8-349">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-349">✔</span></span>
<span data-ttu-id="c00d8-350">Geometry. relacioná (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="c00d8-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="c00d8-351">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-351">✔</span></span> | | <span data-ttu-id="c00d8-352">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-352">✔</span></span> | <span data-ttu-id="c00d8-353">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-353">✔</span></span>
<span data-ttu-id="c00d8-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="c00d8-355">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-355">✔</span></span> | <span data-ttu-id="c00d8-356">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-356">✔</span></span>
<span data-ttu-id="c00d8-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="c00d8-357">Geometry.SRID</span></span> | <span data-ttu-id="c00d8-358">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-358">✔</span></span> | <span data-ttu-id="c00d8-359">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-359">✔</span></span> | <span data-ttu-id="c00d8-360">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-360">✔</span></span> | <span data-ttu-id="c00d8-361">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-361">✔</span></span>
<span data-ttu-id="c00d8-362">Geometry. SymmetricDifference (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="c00d8-363">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-363">✔</span></span> | <span data-ttu-id="c00d8-364">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-364">✔</span></span> | <span data-ttu-id="c00d8-365">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-365">✔</span></span> | <span data-ttu-id="c00d8-366">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-366">✔</span></span>
<span data-ttu-id="c00d8-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="c00d8-368">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-368">✔</span></span> | <span data-ttu-id="c00d8-369">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-369">✔</span></span> | <span data-ttu-id="c00d8-370">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-370">✔</span></span> | <span data-ttu-id="c00d8-371">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-371">✔</span></span>
<span data-ttu-id="c00d8-372">Geometry. totext ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-372">Geometry.ToText()</span></span> | <span data-ttu-id="c00d8-373">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-373">✔</span></span> | <span data-ttu-id="c00d8-374">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-374">✔</span></span> | <span data-ttu-id="c00d8-375">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-375">✔</span></span> | <span data-ttu-id="c00d8-376">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-376">✔</span></span>
<span data-ttu-id="c00d8-377">Geometry. toques (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="c00d8-378">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-378">✔</span></span> | | <span data-ttu-id="c00d8-379">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-379">✔</span></span> | <span data-ttu-id="c00d8-380">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-380">✔</span></span>
<span data-ttu-id="c00d8-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="c00d8-381">Geometry.Union()</span></span> | | | <span data-ttu-id="c00d8-382">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-382">✔</span></span> | <span data-ttu-id="c00d8-383">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-383">✔</span></span>
<span data-ttu-id="c00d8-384">Geometry. Union (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="c00d8-385">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-385">✔</span></span> | <span data-ttu-id="c00d8-386">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-386">✔</span></span> | <span data-ttu-id="c00d8-387">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-387">✔</span></span> | <span data-ttu-id="c00d8-388">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-388">✔</span></span>
<span data-ttu-id="c00d8-389">Geometry. Within (geometry)</span><span class="sxs-lookup"><span data-stu-id="c00d8-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="c00d8-390">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-390">✔</span></span> | <span data-ttu-id="c00d8-391">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-391">✔</span></span> | <span data-ttu-id="c00d8-392">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-392">✔</span></span> | <span data-ttu-id="c00d8-393">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-393">✔</span></span>
<span data-ttu-id="c00d8-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="c00d8-394">GeometryCollection.Count</span></span> | <span data-ttu-id="c00d8-395">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-395">✔</span></span> | <span data-ttu-id="c00d8-396">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-396">✔</span></span> | <span data-ttu-id="c00d8-397">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-397">✔</span></span> | <span data-ttu-id="c00d8-398">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-398">✔</span></span>
<span data-ttu-id="c00d8-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="c00d8-399">GeometryCollection[int]</span></span> | <span data-ttu-id="c00d8-400">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-400">✔</span></span> | <span data-ttu-id="c00d8-401">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-401">✔</span></span> | <span data-ttu-id="c00d8-402">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-402">✔</span></span> | <span data-ttu-id="c00d8-403">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-403">✔</span></span>
<span data-ttu-id="c00d8-404">Linhastring. contagem</span><span class="sxs-lookup"><span data-stu-id="c00d8-404">LineString.Count</span></span> | <span data-ttu-id="c00d8-405">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-405">✔</span></span> | <span data-ttu-id="c00d8-406">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-406">✔</span></span> | <span data-ttu-id="c00d8-407">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-407">✔</span></span> | <span data-ttu-id="c00d8-408">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-408">✔</span></span>
<span data-ttu-id="c00d8-409">LineString. ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="c00d8-409">LineString.EndPoint</span></span> | <span data-ttu-id="c00d8-410">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-410">✔</span></span> | <span data-ttu-id="c00d8-411">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-411">✔</span></span> | <span data-ttu-id="c00d8-412">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-412">✔</span></span> | <span data-ttu-id="c00d8-413">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-413">✔</span></span>
<span data-ttu-id="c00d8-414">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="c00d8-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="c00d8-415">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-415">✔</span></span> | <span data-ttu-id="c00d8-416">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-416">✔</span></span> | <span data-ttu-id="c00d8-417">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-417">✔</span></span> | <span data-ttu-id="c00d8-418">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-418">✔</span></span>
<span data-ttu-id="c00d8-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="c00d8-419">LineString.IsClosed</span></span> | <span data-ttu-id="c00d8-420">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-420">✔</span></span> | <span data-ttu-id="c00d8-421">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-421">✔</span></span> | <span data-ttu-id="c00d8-422">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-422">✔</span></span> | <span data-ttu-id="c00d8-423">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-423">✔</span></span>
<span data-ttu-id="c00d8-424">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="c00d8-424">LineString.IsRing</span></span> | <span data-ttu-id="c00d8-425">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-425">✔</span></span> | | <span data-ttu-id="c00d8-426">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-426">✔</span></span> | <span data-ttu-id="c00d8-427">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-427">✔</span></span>
<span data-ttu-id="c00d8-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="c00d8-428">LineString.StartPoint</span></span> | <span data-ttu-id="c00d8-429">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-429">✔</span></span> | <span data-ttu-id="c00d8-430">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-430">✔</span></span> | <span data-ttu-id="c00d8-431">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-431">✔</span></span> | <span data-ttu-id="c00d8-432">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-432">✔</span></span>
<span data-ttu-id="c00d8-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="c00d8-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="c00d8-434">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-434">✔</span></span> | <span data-ttu-id="c00d8-435">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-435">✔</span></span> | <span data-ttu-id="c00d8-436">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-436">✔</span></span> | <span data-ttu-id="c00d8-437">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-437">✔</span></span>
<span data-ttu-id="c00d8-438">Ponto. M</span><span class="sxs-lookup"><span data-stu-id="c00d8-438">Point.M</span></span> | <span data-ttu-id="c00d8-439">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-439">✔</span></span> | <span data-ttu-id="c00d8-440">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-440">✔</span></span> | <span data-ttu-id="c00d8-441">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-441">✔</span></span> | <span data-ttu-id="c00d8-442">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-442">✔</span></span>
<span data-ttu-id="c00d8-443">Ponto. X</span><span class="sxs-lookup"><span data-stu-id="c00d8-443">Point.X</span></span> | <span data-ttu-id="c00d8-444">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-444">✔</span></span> | <span data-ttu-id="c00d8-445">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-445">✔</span></span> | <span data-ttu-id="c00d8-446">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-446">✔</span></span> | <span data-ttu-id="c00d8-447">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-447">✔</span></span>
<span data-ttu-id="c00d8-448">Ponto. Y</span><span class="sxs-lookup"><span data-stu-id="c00d8-448">Point.Y</span></span> | <span data-ttu-id="c00d8-449">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-449">✔</span></span> | <span data-ttu-id="c00d8-450">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-450">✔</span></span> | <span data-ttu-id="c00d8-451">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-451">✔</span></span> | <span data-ttu-id="c00d8-452">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-452">✔</span></span>
<span data-ttu-id="c00d8-453">Ponto. Z</span><span class="sxs-lookup"><span data-stu-id="c00d8-453">Point.Z</span></span> | <span data-ttu-id="c00d8-454">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-454">✔</span></span> | <span data-ttu-id="c00d8-455">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-455">✔</span></span> | <span data-ttu-id="c00d8-456">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-456">✔</span></span> | <span data-ttu-id="c00d8-457">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-457">✔</span></span>
<span data-ttu-id="c00d8-458">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="c00d8-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="c00d8-459">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-459">✔</span></span> | <span data-ttu-id="c00d8-460">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-460">✔</span></span> | <span data-ttu-id="c00d8-461">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-461">✔</span></span> | <span data-ttu-id="c00d8-462">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-462">✔</span></span>
<span data-ttu-id="c00d8-463">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="c00d8-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="c00d8-464">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-464">✔</span></span> | <span data-ttu-id="c00d8-465">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-465">✔</span></span> | <span data-ttu-id="c00d8-466">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-466">✔</span></span> | <span data-ttu-id="c00d8-467">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-467">✔</span></span>
<span data-ttu-id="c00d8-468">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="c00d8-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="c00d8-469">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-469">✔</span></span> | <span data-ttu-id="c00d8-470">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-470">✔</span></span> | <span data-ttu-id="c00d8-471">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-471">✔</span></span> | <span data-ttu-id="c00d8-472">✔</span><span class="sxs-lookup"><span data-stu-id="c00d8-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c00d8-473">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c00d8-473">Additional resources</span></span>

* [<span data-ttu-id="c00d8-474">Dados espaciais em SQL Server</span><span class="sxs-lookup"><span data-stu-id="c00d8-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="c00d8-475">Home Page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="c00d8-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="c00d8-476">Documentação espacial do Npgsql</span><span class="sxs-lookup"><span data-stu-id="c00d8-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="c00d8-477">Documentação do PostGIS</span><span class="sxs-lookup"><span data-stu-id="c00d8-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
