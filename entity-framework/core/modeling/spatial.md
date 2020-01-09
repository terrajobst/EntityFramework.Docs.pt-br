---
title: Dados espaciais-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502234"
---
# <a name="spatial-data"></a><span data-ttu-id="b626c-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="b626c-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="b626c-103">Esse recurso foi adicionado no EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="b626c-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="b626c-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="b626c-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="b626c-105">Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="b626c-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="b626c-106">Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="b626c-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="b626c-107">EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="b626c-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="b626c-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="b626c-108">Installing</span></span>

<span data-ttu-id="b626c-109">Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="b626c-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="b626c-110">O pacote que você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="b626c-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="b626c-111">Provedor do EF Core</span><span class="sxs-lookup"><span data-stu-id="b626c-111">EF Core Provider</span></span>                        | <span data-ttu-id="b626c-112">Pacote NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="b626c-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="b626c-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="b626c-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="b626c-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="b626c-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="b626c-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="b626c-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="b626c-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="b626c-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="b626c-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="b626c-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="b626c-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="b626c-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="b626c-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b626c-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="b626c-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="b626c-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="b626c-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="b626c-121">Reverse engineering</span></span>

<span data-ttu-id="b626c-122">Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa instalar o pacote ***antes*** de executar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="b626c-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="b626c-123">Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="b626c-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="b626c-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="b626c-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="b626c-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="b626c-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="b626c-126">EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="b626c-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="b626c-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="b626c-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="b626c-128">Por exemplo, com SQL Server você o chamaria assim.</span><span class="sxs-lookup"><span data-stu-id="b626c-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="b626c-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="b626c-129">There are several spatial data types.</span></span> <span data-ttu-id="b626c-130">O tipo usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="b626c-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="b626c-131">Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="b626c-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="b626c-132">Eles estão localizados dentro do namespace `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="b626c-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="b626c-133">Geometria</span><span class="sxs-lookup"><span data-stu-id="b626c-133">Geometry</span></span>
  * <span data-ttu-id="b626c-134">Ponto</span><span class="sxs-lookup"><span data-stu-id="b626c-134">Point</span></span>
  * <span data-ttu-id="b626c-135">LineString</span><span class="sxs-lookup"><span data-stu-id="b626c-135">LineString</span></span>
  * <span data-ttu-id="b626c-136">Polígono</span><span class="sxs-lookup"><span data-stu-id="b626c-136">Polygon</span></span>
  * <span data-ttu-id="b626c-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="b626c-137">GeometryCollection</span></span>
    * <span data-ttu-id="b626c-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="b626c-138">MultiPoint</span></span>
    * <span data-ttu-id="b626c-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="b626c-139">MultiLineString</span></span>
    * <span data-ttu-id="b626c-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="b626c-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="b626c-141">Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.</span><span class="sxs-lookup"><span data-stu-id="b626c-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="b626c-142">Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="b626c-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="b626c-143">As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="b626c-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="b626c-144">Criando valores</span><span class="sxs-lookup"><span data-stu-id="b626c-144">Creating values</span></span>

<span data-ttu-id="b626c-145">Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="b626c-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="b626c-146">Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="b626c-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="b626c-147">4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="b626c-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="b626c-148">Longitude e latitude</span><span class="sxs-lookup"><span data-stu-id="b626c-148">Longitude and Latitude</span></span>

<span data-ttu-id="b626c-149">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="b626c-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="b626c-150">Para representar a longitude e a latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="b626c-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="b626c-151">Observe que isso é **retroativa** no formato de `latitude, longitude` no qual você normalmente vê esses valores.</span><span class="sxs-lookup"><span data-stu-id="b626c-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="b626c-152">SRID ignorado durante as operações do cliente</span><span class="sxs-lookup"><span data-stu-id="b626c-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="b626c-153">NTS ignora os valores de SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="b626c-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="b626c-154">Ele assume um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="b626c-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="b626c-155">Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros.</span><span class="sxs-lookup"><span data-stu-id="b626c-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="b626c-156">Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="b626c-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="b626c-157">Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b626c-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="b626c-158">Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="b626c-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="b626c-159">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="b626c-159">Querying Data</span></span>

<span data-ttu-id="b626c-160">No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL.</span><span class="sxs-lookup"><span data-stu-id="b626c-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="b626c-161">Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="b626c-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="b626c-162">A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="b626c-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="b626c-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b626c-163">SQL Server</span></span>

<span data-ttu-id="b626c-164">Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="b626c-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="b626c-165">Geografia ou Geometry</span><span class="sxs-lookup"><span data-stu-id="b626c-165">Geography or geometry</span></span>

<span data-ttu-id="b626c-166">Por padrão, as propriedades espaciais são mapeadas para `geography` colunas em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b626c-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="b626c-167">Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/entity-properties#column-data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="b626c-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="b626c-168">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="b626c-168">Geography polygon rings</span></span>

<span data-ttu-id="b626c-169">Ao usar o tipo de coluna `geography`, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos).</span><span class="sxs-lookup"><span data-stu-id="b626c-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="b626c-170">O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="b626c-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="b626c-171">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b626c-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="b626c-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="b626c-172">FullGlobe</span></span>

<span data-ttu-id="b626c-173">SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o tipo de coluna `geography`.</span><span class="sxs-lookup"><span data-stu-id="b626c-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="b626c-174">Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="b626c-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="b626c-175">Nenhuma delas é suportada pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="b626c-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="b626c-176">Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="b626c-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="b626c-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="b626c-177">SQLite</span></span>

<span data-ttu-id="b626c-178">Aqui estão algumas informações adicionais para as que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="b626c-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="b626c-179">Instalando o SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="b626c-179">Installing SpatiaLite</span></span>

<span data-ttu-id="b626c-180">No Windows, a biblioteca de mod_spatialite nativa é distribuída como uma dependência de pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="b626c-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="b626c-181">Outras plataformas precisam instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="b626c-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="b626c-182">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="b626c-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="b626c-183">Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="b626c-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="b626c-184">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="b626c-184">Configuring SRID</span></span>

<span data-ttu-id="b626c-185">No SpatiaLite, as colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="b626c-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="b626c-186">O SRID padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="b626c-186">The default SRID is `0`.</span></span> <span data-ttu-id="b626c-187">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="b626c-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="b626c-188">Dimensão</span><span class="sxs-lookup"><span data-stu-id="b626c-188">Dimension</span></span>

<span data-ttu-id="b626c-189">Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="b626c-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="b626c-190">As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="b626c-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="b626c-191">Operações convertidas</span><span class="sxs-lookup"><span data-stu-id="b626c-191">Translated Operations</span></span>

<span data-ttu-id="b626c-192">Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="b626c-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="b626c-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="b626c-193">NetTopologySuite</span></span> | <span data-ttu-id="b626c-194">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-194">SQL Server (geometry)</span></span> | <span data-ttu-id="b626c-195">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="b626c-195">SQL Server (geography)</span></span> | <span data-ttu-id="b626c-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="b626c-196">SQLite</span></span> | <span data-ttu-id="b626c-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="b626c-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="b626c-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="b626c-198">Geometry.Area</span></span> | <span data-ttu-id="b626c-199">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-199">✔</span></span> | <span data-ttu-id="b626c-200">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-200">✔</span></span> | <span data-ttu-id="b626c-201">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-201">✔</span></span> | <span data-ttu-id="b626c-202">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-202">✔</span></span>
<span data-ttu-id="b626c-203">Geometry. asbinary ()</span><span class="sxs-lookup"><span data-stu-id="b626c-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="b626c-204">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-204">✔</span></span> | <span data-ttu-id="b626c-205">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-205">✔</span></span> | <span data-ttu-id="b626c-206">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-206">✔</span></span> | <span data-ttu-id="b626c-207">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-207">✔</span></span>
<span data-ttu-id="b626c-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="b626c-208">Geometry.AsText()</span></span> | <span data-ttu-id="b626c-209">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-209">✔</span></span> | <span data-ttu-id="b626c-210">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-210">✔</span></span> | <span data-ttu-id="b626c-211">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-211">✔</span></span> | <span data-ttu-id="b626c-212">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-212">✔</span></span>
<span data-ttu-id="b626c-213">Geometry. limite</span><span class="sxs-lookup"><span data-stu-id="b626c-213">Geometry.Boundary</span></span> | <span data-ttu-id="b626c-214">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-214">✔</span></span> | | <span data-ttu-id="b626c-215">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-215">✔</span></span> | <span data-ttu-id="b626c-216">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-216">✔</span></span>
<span data-ttu-id="b626c-217">Geometry. buffer (duplo)</span><span class="sxs-lookup"><span data-stu-id="b626c-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="b626c-218">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-218">✔</span></span> | <span data-ttu-id="b626c-219">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-219">✔</span></span> | <span data-ttu-id="b626c-220">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-220">✔</span></span> | <span data-ttu-id="b626c-221">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-221">✔</span></span>
<span data-ttu-id="b626c-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="b626c-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="b626c-223">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-223">✔</span></span> | <span data-ttu-id="b626c-224">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-224">✔</span></span>
<span data-ttu-id="b626c-225">Geometry. centróide</span><span class="sxs-lookup"><span data-stu-id="b626c-225">Geometry.Centroid</span></span> | <span data-ttu-id="b626c-226">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-226">✔</span></span> | | <span data-ttu-id="b626c-227">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-227">✔</span></span> | <span data-ttu-id="b626c-228">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-228">✔</span></span>
<span data-ttu-id="b626c-229">Geometry. Contains (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="b626c-230">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-230">✔</span></span> | <span data-ttu-id="b626c-231">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-231">✔</span></span> | <span data-ttu-id="b626c-232">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-232">✔</span></span> | <span data-ttu-id="b626c-233">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-233">✔</span></span>
<span data-ttu-id="b626c-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="b626c-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="b626c-235">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-235">✔</span></span> | <span data-ttu-id="b626c-236">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-236">✔</span></span> | <span data-ttu-id="b626c-237">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-237">✔</span></span> | <span data-ttu-id="b626c-238">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-238">✔</span></span>
<span data-ttu-id="b626c-239">Geometry. CoveredBy (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="b626c-240">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-240">✔</span></span> | <span data-ttu-id="b626c-241">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-241">✔</span></span>
<span data-ttu-id="b626c-242">Geometry. cubras (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="b626c-243">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-243">✔</span></span> | <span data-ttu-id="b626c-244">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-244">✔</span></span>
<span data-ttu-id="b626c-245">Geometry. Crosss (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="b626c-246">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-246">✔</span></span> | | <span data-ttu-id="b626c-247">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-247">✔</span></span> | <span data-ttu-id="b626c-248">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-248">✔</span></span>
<span data-ttu-id="b626c-249">Geometry. Difference (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="b626c-250">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-250">✔</span></span> | <span data-ttu-id="b626c-251">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-251">✔</span></span> | <span data-ttu-id="b626c-252">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-252">✔</span></span> | <span data-ttu-id="b626c-253">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-253">✔</span></span>
<span data-ttu-id="b626c-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="b626c-254">Geometry.Dimension</span></span> | <span data-ttu-id="b626c-255">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-255">✔</span></span> | <span data-ttu-id="b626c-256">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-256">✔</span></span> | <span data-ttu-id="b626c-257">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-257">✔</span></span> | <span data-ttu-id="b626c-258">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-258">✔</span></span>
<span data-ttu-id="b626c-259">Geometry. disjunção (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="b626c-260">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-260">✔</span></span> | <span data-ttu-id="b626c-261">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-261">✔</span></span> | <span data-ttu-id="b626c-262">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-262">✔</span></span> | <span data-ttu-id="b626c-263">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-263">✔</span></span>
<span data-ttu-id="b626c-264">Geometry. distance (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="b626c-265">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-265">✔</span></span> | <span data-ttu-id="b626c-266">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-266">✔</span></span> | <span data-ttu-id="b626c-267">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-267">✔</span></span> | <span data-ttu-id="b626c-268">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-268">✔</span></span>
<span data-ttu-id="b626c-269">Geometry. envelope</span><span class="sxs-lookup"><span data-stu-id="b626c-269">Geometry.Envelope</span></span> | <span data-ttu-id="b626c-270">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-270">✔</span></span> | | <span data-ttu-id="b626c-271">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-271">✔</span></span> | <span data-ttu-id="b626c-272">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-272">✔</span></span>
<span data-ttu-id="b626c-273">Geometry. EqualsExact (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="b626c-274">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-274">✔</span></span>
<span data-ttu-id="b626c-275">Geometry. EqualsTopologically (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="b626c-276">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-276">✔</span></span> | <span data-ttu-id="b626c-277">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-277">✔</span></span> | <span data-ttu-id="b626c-278">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-278">✔</span></span> | <span data-ttu-id="b626c-279">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-279">✔</span></span>
<span data-ttu-id="b626c-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="b626c-280">Geometry.GeometryType</span></span> | <span data-ttu-id="b626c-281">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-281">✔</span></span> | <span data-ttu-id="b626c-282">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-282">✔</span></span> | <span data-ttu-id="b626c-283">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-283">✔</span></span> | <span data-ttu-id="b626c-284">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-284">✔</span></span>
<span data-ttu-id="b626c-285">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="b626c-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="b626c-286">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-286">✔</span></span> | | <span data-ttu-id="b626c-287">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-287">✔</span></span> | <span data-ttu-id="b626c-288">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-288">✔</span></span>
<span data-ttu-id="b626c-289">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="b626c-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="b626c-290">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-290">✔</span></span> | | <span data-ttu-id="b626c-291">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-291">✔</span></span> | <span data-ttu-id="b626c-292">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-292">✔</span></span>
<span data-ttu-id="b626c-293">Geometry. Intersection (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="b626c-294">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-294">✔</span></span> | <span data-ttu-id="b626c-295">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-295">✔</span></span> | <span data-ttu-id="b626c-296">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-296">✔</span></span> | <span data-ttu-id="b626c-297">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-297">✔</span></span>
<span data-ttu-id="b626c-298">Geometry. Intersects (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="b626c-299">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-299">✔</span></span> | <span data-ttu-id="b626c-300">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-300">✔</span></span> | <span data-ttu-id="b626c-301">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-301">✔</span></span> | <span data-ttu-id="b626c-302">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-302">✔</span></span>
<span data-ttu-id="b626c-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="b626c-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="b626c-304">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-304">✔</span></span> | <span data-ttu-id="b626c-305">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-305">✔</span></span> | <span data-ttu-id="b626c-306">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-306">✔</span></span> | <span data-ttu-id="b626c-307">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-307">✔</span></span>
<span data-ttu-id="b626c-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="b626c-308">Geometry.IsSimple</span></span> | <span data-ttu-id="b626c-309">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-309">✔</span></span> | | <span data-ttu-id="b626c-310">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-310">✔</span></span> | <span data-ttu-id="b626c-311">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-311">✔</span></span>
<span data-ttu-id="b626c-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="b626c-312">Geometry.IsValid</span></span> | <span data-ttu-id="b626c-313">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-313">✔</span></span> | <span data-ttu-id="b626c-314">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-314">✔</span></span> | <span data-ttu-id="b626c-315">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-315">✔</span></span> | <span data-ttu-id="b626c-316">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-316">✔</span></span>
<span data-ttu-id="b626c-317">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="b626c-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="b626c-318">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-318">✔</span></span> | | <span data-ttu-id="b626c-319">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-319">✔</span></span> | <span data-ttu-id="b626c-320">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-320">✔</span></span>
<span data-ttu-id="b626c-321">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="b626c-321">Geometry.Length</span></span> | <span data-ttu-id="b626c-322">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-322">✔</span></span> | <span data-ttu-id="b626c-323">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-323">✔</span></span> | <span data-ttu-id="b626c-324">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-324">✔</span></span> | <span data-ttu-id="b626c-325">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-325">✔</span></span>
<span data-ttu-id="b626c-326">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="b626c-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="b626c-327">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-327">✔</span></span> | <span data-ttu-id="b626c-328">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-328">✔</span></span> | <span data-ttu-id="b626c-329">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-329">✔</span></span> | <span data-ttu-id="b626c-330">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-330">✔</span></span>
<span data-ttu-id="b626c-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="b626c-331">Geometry.NumPoints</span></span> | <span data-ttu-id="b626c-332">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-332">✔</span></span> | <span data-ttu-id="b626c-333">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-333">✔</span></span> | <span data-ttu-id="b626c-334">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-334">✔</span></span> | <span data-ttu-id="b626c-335">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-335">✔</span></span>
<span data-ttu-id="b626c-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="b626c-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="b626c-337">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-337">✔</span></span> | <span data-ttu-id="b626c-338">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-338">✔</span></span> | <span data-ttu-id="b626c-339">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-339">✔</span></span> | <span data-ttu-id="b626c-340">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-340">✔</span></span>
<span data-ttu-id="b626c-341">Geometry. oversobrepõetes (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="b626c-342">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-342">✔</span></span> | <span data-ttu-id="b626c-343">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-343">✔</span></span> | <span data-ttu-id="b626c-344">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-344">✔</span></span> | <span data-ttu-id="b626c-345">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-345">✔</span></span>
<span data-ttu-id="b626c-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="b626c-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="b626c-347">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-347">✔</span></span> | | <span data-ttu-id="b626c-348">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-348">✔</span></span> | <span data-ttu-id="b626c-349">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-349">✔</span></span>
<span data-ttu-id="b626c-350">Geometry. relacioná (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="b626c-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="b626c-351">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-351">✔</span></span> | | <span data-ttu-id="b626c-352">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-352">✔</span></span> | <span data-ttu-id="b626c-353">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-353">✔</span></span>
<span data-ttu-id="b626c-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="b626c-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="b626c-355">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-355">✔</span></span> | <span data-ttu-id="b626c-356">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-356">✔</span></span>
<span data-ttu-id="b626c-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="b626c-357">Geometry.SRID</span></span> | <span data-ttu-id="b626c-358">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-358">✔</span></span> | <span data-ttu-id="b626c-359">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-359">✔</span></span> | <span data-ttu-id="b626c-360">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-360">✔</span></span> | <span data-ttu-id="b626c-361">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-361">✔</span></span>
<span data-ttu-id="b626c-362">Geometry. SymmetricDifference (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="b626c-363">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-363">✔</span></span> | <span data-ttu-id="b626c-364">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-364">✔</span></span> | <span data-ttu-id="b626c-365">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-365">✔</span></span> | <span data-ttu-id="b626c-366">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-366">✔</span></span>
<span data-ttu-id="b626c-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="b626c-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="b626c-368">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-368">✔</span></span> | <span data-ttu-id="b626c-369">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-369">✔</span></span> | <span data-ttu-id="b626c-370">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-370">✔</span></span> | <span data-ttu-id="b626c-371">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-371">✔</span></span>
<span data-ttu-id="b626c-372">Geometry. totext ()</span><span class="sxs-lookup"><span data-stu-id="b626c-372">Geometry.ToText()</span></span> | <span data-ttu-id="b626c-373">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-373">✔</span></span> | <span data-ttu-id="b626c-374">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-374">✔</span></span> | <span data-ttu-id="b626c-375">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-375">✔</span></span> | <span data-ttu-id="b626c-376">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-376">✔</span></span>
<span data-ttu-id="b626c-377">Geometry. toques (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="b626c-378">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-378">✔</span></span> | | <span data-ttu-id="b626c-379">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-379">✔</span></span> | <span data-ttu-id="b626c-380">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-380">✔</span></span>
<span data-ttu-id="b626c-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="b626c-381">Geometry.Union()</span></span> | | | <span data-ttu-id="b626c-382">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-382">✔</span></span> | <span data-ttu-id="b626c-383">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-383">✔</span></span>
<span data-ttu-id="b626c-384">Geometry. Union (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="b626c-385">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-385">✔</span></span> | <span data-ttu-id="b626c-386">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-386">✔</span></span> | <span data-ttu-id="b626c-387">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-387">✔</span></span> | <span data-ttu-id="b626c-388">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-388">✔</span></span>
<span data-ttu-id="b626c-389">Geometry. Within (geometry)</span><span class="sxs-lookup"><span data-stu-id="b626c-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="b626c-390">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-390">✔</span></span> | <span data-ttu-id="b626c-391">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-391">✔</span></span> | <span data-ttu-id="b626c-392">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-392">✔</span></span> | <span data-ttu-id="b626c-393">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-393">✔</span></span>
<span data-ttu-id="b626c-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="b626c-394">GeometryCollection.Count</span></span> | <span data-ttu-id="b626c-395">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-395">✔</span></span> | <span data-ttu-id="b626c-396">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-396">✔</span></span> | <span data-ttu-id="b626c-397">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-397">✔</span></span> | <span data-ttu-id="b626c-398">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-398">✔</span></span>
<span data-ttu-id="b626c-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="b626c-399">GeometryCollection[int]</span></span> | <span data-ttu-id="b626c-400">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-400">✔</span></span> | <span data-ttu-id="b626c-401">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-401">✔</span></span> | <span data-ttu-id="b626c-402">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-402">✔</span></span> | <span data-ttu-id="b626c-403">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-403">✔</span></span>
<span data-ttu-id="b626c-404">Linhastring. contagem</span><span class="sxs-lookup"><span data-stu-id="b626c-404">LineString.Count</span></span> | <span data-ttu-id="b626c-405">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-405">✔</span></span> | <span data-ttu-id="b626c-406">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-406">✔</span></span> | <span data-ttu-id="b626c-407">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-407">✔</span></span> | <span data-ttu-id="b626c-408">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-408">✔</span></span>
<span data-ttu-id="b626c-409">LineString. ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="b626c-409">LineString.EndPoint</span></span> | <span data-ttu-id="b626c-410">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-410">✔</span></span> | <span data-ttu-id="b626c-411">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-411">✔</span></span> | <span data-ttu-id="b626c-412">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-412">✔</span></span> | <span data-ttu-id="b626c-413">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-413">✔</span></span>
<span data-ttu-id="b626c-414">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="b626c-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="b626c-415">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-415">✔</span></span> | <span data-ttu-id="b626c-416">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-416">✔</span></span> | <span data-ttu-id="b626c-417">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-417">✔</span></span> | <span data-ttu-id="b626c-418">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-418">✔</span></span>
<span data-ttu-id="b626c-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="b626c-419">LineString.IsClosed</span></span> | <span data-ttu-id="b626c-420">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-420">✔</span></span> | <span data-ttu-id="b626c-421">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-421">✔</span></span> | <span data-ttu-id="b626c-422">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-422">✔</span></span> | <span data-ttu-id="b626c-423">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-423">✔</span></span>
<span data-ttu-id="b626c-424">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="b626c-424">LineString.IsRing</span></span> | <span data-ttu-id="b626c-425">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-425">✔</span></span> | | <span data-ttu-id="b626c-426">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-426">✔</span></span> | <span data-ttu-id="b626c-427">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-427">✔</span></span>
<span data-ttu-id="b626c-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="b626c-428">LineString.StartPoint</span></span> | <span data-ttu-id="b626c-429">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-429">✔</span></span> | <span data-ttu-id="b626c-430">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-430">✔</span></span> | <span data-ttu-id="b626c-431">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-431">✔</span></span> | <span data-ttu-id="b626c-432">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-432">✔</span></span>
<span data-ttu-id="b626c-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="b626c-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="b626c-434">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-434">✔</span></span> | <span data-ttu-id="b626c-435">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-435">✔</span></span> | <span data-ttu-id="b626c-436">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-436">✔</span></span> | <span data-ttu-id="b626c-437">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-437">✔</span></span>
<span data-ttu-id="b626c-438">Ponto. M</span><span class="sxs-lookup"><span data-stu-id="b626c-438">Point.M</span></span> | <span data-ttu-id="b626c-439">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-439">✔</span></span> | <span data-ttu-id="b626c-440">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-440">✔</span></span> | <span data-ttu-id="b626c-441">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-441">✔</span></span> | <span data-ttu-id="b626c-442">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-442">✔</span></span>
<span data-ttu-id="b626c-443">Ponto. X</span><span class="sxs-lookup"><span data-stu-id="b626c-443">Point.X</span></span> | <span data-ttu-id="b626c-444">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-444">✔</span></span> | <span data-ttu-id="b626c-445">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-445">✔</span></span> | <span data-ttu-id="b626c-446">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-446">✔</span></span> | <span data-ttu-id="b626c-447">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-447">✔</span></span>
<span data-ttu-id="b626c-448">Ponto. Y</span><span class="sxs-lookup"><span data-stu-id="b626c-448">Point.Y</span></span> | <span data-ttu-id="b626c-449">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-449">✔</span></span> | <span data-ttu-id="b626c-450">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-450">✔</span></span> | <span data-ttu-id="b626c-451">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-451">✔</span></span> | <span data-ttu-id="b626c-452">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-452">✔</span></span>
<span data-ttu-id="b626c-453">Ponto. Z</span><span class="sxs-lookup"><span data-stu-id="b626c-453">Point.Z</span></span> | <span data-ttu-id="b626c-454">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-454">✔</span></span> | <span data-ttu-id="b626c-455">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-455">✔</span></span> | <span data-ttu-id="b626c-456">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-456">✔</span></span> | <span data-ttu-id="b626c-457">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-457">✔</span></span>
<span data-ttu-id="b626c-458">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="b626c-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="b626c-459">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-459">✔</span></span> | <span data-ttu-id="b626c-460">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-460">✔</span></span> | <span data-ttu-id="b626c-461">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-461">✔</span></span> | <span data-ttu-id="b626c-462">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-462">✔</span></span>
<span data-ttu-id="b626c-463">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="b626c-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="b626c-464">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-464">✔</span></span> | <span data-ttu-id="b626c-465">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-465">✔</span></span> | <span data-ttu-id="b626c-466">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-466">✔</span></span> | <span data-ttu-id="b626c-467">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-467">✔</span></span>
<span data-ttu-id="b626c-468">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="b626c-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="b626c-469">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-469">✔</span></span> | <span data-ttu-id="b626c-470">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-470">✔</span></span> | <span data-ttu-id="b626c-471">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-471">✔</span></span> | <span data-ttu-id="b626c-472">✔</span><span class="sxs-lookup"><span data-stu-id="b626c-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b626c-473">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b626c-473">Additional resources</span></span>

* [<span data-ttu-id="b626c-474">Dados espaciais em SQL Server</span><span class="sxs-lookup"><span data-stu-id="b626c-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="b626c-475">Home Page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="b626c-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="b626c-476">Documentação espacial do Npgsql</span><span class="sxs-lookup"><span data-stu-id="b626c-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="b626c-477">Documentação do PostGIS</span><span class="sxs-lookup"><span data-stu-id="b626c-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
