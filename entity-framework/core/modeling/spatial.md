---
title: Dados espaciais – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688779"
---
# <a name="spatial-data"></a><span data-ttu-id="89829-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="89829-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="89829-103">Esse recurso é novo no EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="89829-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="89829-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="89829-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="89829-105">Muitos bancos de dados oferecem suporte para esse tipo de dados para que possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="89829-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="89829-106">Cenários comuns incluem consultas para objetos dentro de uma determinada distância de um local, ou selecionando o objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="89829-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="89829-107">O EF Core dá suporte ao mapeamento para tipos de dados espaciais usando o [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) biblioteca espacial.</span><span class="sxs-lookup"><span data-stu-id="89829-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="89829-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="89829-108">Installing</span></span>

<span data-ttu-id="89829-109">Para usar dados espaciais com o EF Core, você precisa instalar o pacote do NuGet suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="89829-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="89829-110">Qual pacote você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="89829-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="89829-111">Provedor do EF Core</span><span class="sxs-lookup"><span data-stu-id="89829-111">EF Core Provider</span></span>                        | <span data-ttu-id="89829-112">Pacote do NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="89829-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="89829-113">Entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="89829-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="89829-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="89829-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="89829-115">Entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="89829-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="89829-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="89829-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="89829-117">Entityframeworkcore</span><span class="sxs-lookup"><span data-stu-id="89829-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="89829-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="89829-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="89829-119">Npgsql</span><span class="sxs-lookup"><span data-stu-id="89829-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="89829-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="89829-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="89829-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="89829-121">Reverse engineering</span></span>

<span data-ttu-id="89829-122">O NuGet espacial também pacotes enable [engenharia reversa](../managing-schemas/scaffolding.md) modelos com propriedades espaciais, mas você precisam instalar o pacote ***antes*** executando `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="89829-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="89829-123">Se você não fizer isso, você receberá avisos sobre a localização não mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="89829-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="89829-124">NetTopologySuite NTS)</span><span class="sxs-lookup"><span data-stu-id="89829-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="89829-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="89829-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="89829-126">O EF Core permite mapear tipos de no banco de dados para dados espaciais usando tipos ntos de interrupção em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="89829-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="89829-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método de UseNetTopologySuite no construtor de opções de DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="89829-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="89829-128">Por exemplo, com o SQL Server você chamaria-lo como este.</span><span class="sxs-lookup"><span data-stu-id="89829-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="89829-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="89829-129">There are several spatial data types.</span></span> <span data-ttu-id="89829-130">Tipo a ser usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="89829-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="89829-131">Aqui está a hierarquia de tipos ntos de interrupção que você pode usar para as propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="89829-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="89829-132">Elas estão localizadas dentro do `NetTopologySuite.Geometries` namespace.</span><span class="sxs-lookup"><span data-stu-id="89829-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="89829-133">As interfaces correspondentes no pacote GeoAPI (`GeoAPI.Geometries` namespace) também pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="89829-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="89829-134">geometria</span><span class="sxs-lookup"><span data-stu-id="89829-134">Geometry</span></span>
  * <span data-ttu-id="89829-135">Ponto</span><span class="sxs-lookup"><span data-stu-id="89829-135">Point</span></span>
  * <span data-ttu-id="89829-136">LineString</span><span class="sxs-lookup"><span data-stu-id="89829-136">LineString</span></span>
  * <span data-ttu-id="89829-137">Polígono</span><span class="sxs-lookup"><span data-stu-id="89829-137">Polygon</span></span>
  * <span data-ttu-id="89829-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="89829-138">GeometryCollection</span></span>
    * <span data-ttu-id="89829-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="89829-139">MultiPoint</span></span>
    * <span data-ttu-id="89829-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="89829-140">MultiLineString</span></span>
    * <span data-ttu-id="89829-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="89829-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="89829-142">CircularString, CompoundCurve e CurePolygon não são suportadas pelo ntos de interrupção.</span><span class="sxs-lookup"><span data-stu-id="89829-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="89829-143">Usar o tipo de geometria base permite que qualquer tipo de forma a ser especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="89829-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="89829-144">As seguintes classes de entidade podem ser usadas para mapear a tabelas na [banco de dados de exemplo Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="89829-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="89829-145">Criação de valores</span><span class="sxs-lookup"><span data-stu-id="89829-145">Creating values</span></span>

<span data-ttu-id="89829-146">Você pode usar construtores para criar objetos de geometria; No entanto, NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="89829-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="89829-147">Isso permite que você especifique um SRID (o sistema de referência espacial usado pelas coordenadas) padrão e lhe dá controle sobre as coisas mais avançadas, como o modelo de precisão (usado durante os cálculos) e a sequência de coordenadas (determina quais ordinates – dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="89829-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="89829-148">4326 se refere a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="89829-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="89829-149">Longitude e Latitude</span><span class="sxs-lookup"><span data-stu-id="89829-149">Longitude and Latitude</span></span>

<span data-ttu-id="89829-150">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="89829-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="89829-151">Para representar a longitude e latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="89829-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="89829-152">Observe que isso **com versões anteriores** do `latitude, longitude` formato no qual você normalmente verá esses valores.</span><span class="sxs-lookup"><span data-stu-id="89829-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="89829-153">SRID ignorado durante operações do cliente</span><span class="sxs-lookup"><span data-stu-id="89829-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="89829-154">NTS ignora os valores SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="89829-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="89829-155">Ele pressupõe que um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="89829-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="89829-156">Isso significa que, se você especificar as coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área será em graus, não os medidores.</span><span class="sxs-lookup"><span data-stu-id="89829-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="89829-157">Para valores mais significativos, você primeiro precisa projetar as coordenadas para outro sistema de coordenadas, uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="89829-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="89829-158">Se uma operação é avaliada de servidor pelo EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="89829-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="89829-159">Aqui está um exemplo de uso ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="89829-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="89829-160">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="89829-160">Querying Data</span></span>

<span data-ttu-id="89829-161">No LINQ, os métodos NTS e propriedades disponíveis, como funções de banco de dados serão convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="89829-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="89829-162">Por exemplo, os métodos de distância e Contains são convertidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="89829-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="89829-163">A tabela no final deste artigo mostra membros que são suportados por vários provedores do EF Core.</span><span class="sxs-lookup"><span data-stu-id="89829-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="89829-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="89829-164">SQL Server</span></span>

<span data-ttu-id="89829-165">Se você estiver usando o SQL Server, há algumas coisas adicionais, que você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="89829-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="89829-166">Geometria ou geografia</span><span class="sxs-lookup"><span data-stu-id="89829-166">Geography or geometry</span></span>

<span data-ttu-id="89829-167">Por padrão, as propriedades espaciais são mapeadas para `geography` colunas no SQL Server.</span><span class="sxs-lookup"><span data-stu-id="89829-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="89829-168">Para usar `geometry`, [configurar o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="89829-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="89829-169">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="89829-169">Geography polygon rings</span></span>

<span data-ttu-id="89829-170">Ao usar o `geography` tipo de coluna, o SQL Server impõe requisitos adicionais no anel exterior (ou shell) e interior anéis (ou falhas).</span><span class="sxs-lookup"><span data-stu-id="89829-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="89829-171">O anel exterior deve ser orientado no sentido anti-horário e o interior anéis no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="89829-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="89829-172">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="89829-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="89829-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="89829-173">FullGlobe</span></span>

<span data-ttu-id="89829-174">O SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o `geography` tipo de coluna.</span><span class="sxs-lookup"><span data-stu-id="89829-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="89829-175">Ele também tem uma forma de representar os polígonos com base em todo o mundo completo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="89829-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="89829-176">Nenhum desses são compatíveis com NTS.</span><span class="sxs-lookup"><span data-stu-id="89829-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="89829-177">FullGlobe e polígonos com base nele não são compatíveis com NTS.</span><span class="sxs-lookup"><span data-stu-id="89829-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="89829-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="89829-178">SQLite</span></span>

<span data-ttu-id="89829-179">Aqui estão algumas informações adicionais para aqueles que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="89829-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="89829-180">Instalando SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="89829-180">Installing SpatiaLite</span></span>

<span data-ttu-id="89829-181">No Windows, a biblioteca nativa mod_spatialite é distribuída como uma dependência de pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="89829-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="89829-182">Outras plataformas é necessário instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="89829-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="89829-183">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="89829-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="89829-184">Por exemplo, você pode usar o APT no Ubuntu e Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="89829-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="89829-185">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="89829-185">Configuring SRID</span></span>

<span data-ttu-id="89829-186">No SpatiaLite, colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="89829-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="89829-187">O padrão é de SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="89829-187">The default SRID is `0`.</span></span> <span data-ttu-id="89829-188">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="89829-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="89829-189">Dimensão</span><span class="sxs-lookup"><span data-stu-id="89829-189">Dimension</span></span>

<span data-ttu-id="89829-190">Semelhante a SRID, dimensão de uma coluna (ou ordinates) também são especificados como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="89829-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="89829-191">Os ordinates padrão são X e Y. Habilite o ordinates adicional (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="89829-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="89829-192">Operações traduzidas</span><span class="sxs-lookup"><span data-stu-id="89829-192">Translated Operations</span></span>

<span data-ttu-id="89829-193">Esta tabela mostra quais membros NTS são convertidos em SQL por cada provedor do EF Core.</span><span class="sxs-lookup"><span data-stu-id="89829-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="89829-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="89829-194">NetTopologySuite</span></span> | <span data-ttu-id="89829-195">SQL Server (geometria)</span><span class="sxs-lookup"><span data-stu-id="89829-195">SQL Server (geometry)</span></span> | <span data-ttu-id="89829-196">SQL Server (geografia)</span><span class="sxs-lookup"><span data-stu-id="89829-196">SQL Server (geography)</span></span> | <span data-ttu-id="89829-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="89829-197">SQLite</span></span> | <span data-ttu-id="89829-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="89829-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="89829-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="89829-199">Geometry.Area</span></span> | <span data-ttu-id="89829-200">✔</span><span class="sxs-lookup"><span data-stu-id="89829-200">✔</span></span> | <span data-ttu-id="89829-201">✔</span><span class="sxs-lookup"><span data-stu-id="89829-201">✔</span></span> | <span data-ttu-id="89829-202">✔</span><span class="sxs-lookup"><span data-stu-id="89829-202">✔</span></span> | <span data-ttu-id="89829-203">✔</span><span class="sxs-lookup"><span data-stu-id="89829-203">✔</span></span>
<span data-ttu-id="89829-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="89829-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="89829-205">✔</span><span class="sxs-lookup"><span data-stu-id="89829-205">✔</span></span> | <span data-ttu-id="89829-206">✔</span><span class="sxs-lookup"><span data-stu-id="89829-206">✔</span></span> | <span data-ttu-id="89829-207">✔</span><span class="sxs-lookup"><span data-stu-id="89829-207">✔</span></span> | <span data-ttu-id="89829-208">✔</span><span class="sxs-lookup"><span data-stu-id="89829-208">✔</span></span>
<span data-ttu-id="89829-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="89829-209">Geometry.AsText()</span></span> | <span data-ttu-id="89829-210">✔</span><span class="sxs-lookup"><span data-stu-id="89829-210">✔</span></span> | <span data-ttu-id="89829-211">✔</span><span class="sxs-lookup"><span data-stu-id="89829-211">✔</span></span> | <span data-ttu-id="89829-212">✔</span><span class="sxs-lookup"><span data-stu-id="89829-212">✔</span></span> | <span data-ttu-id="89829-213">✔</span><span class="sxs-lookup"><span data-stu-id="89829-213">✔</span></span>
<span data-ttu-id="89829-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="89829-214">Geometry.Boundary</span></span> | <span data-ttu-id="89829-215">✔</span><span class="sxs-lookup"><span data-stu-id="89829-215">✔</span></span> | | <span data-ttu-id="89829-216">✔</span><span class="sxs-lookup"><span data-stu-id="89829-216">✔</span></span> | <span data-ttu-id="89829-217">✔</span><span class="sxs-lookup"><span data-stu-id="89829-217">✔</span></span>
<span data-ttu-id="89829-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="89829-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="89829-219">✔</span><span class="sxs-lookup"><span data-stu-id="89829-219">✔</span></span> | <span data-ttu-id="89829-220">✔</span><span class="sxs-lookup"><span data-stu-id="89829-220">✔</span></span> | <span data-ttu-id="89829-221">✔</span><span class="sxs-lookup"><span data-stu-id="89829-221">✔</span></span> | <span data-ttu-id="89829-222">✔</span><span class="sxs-lookup"><span data-stu-id="89829-222">✔</span></span>
<span data-ttu-id="89829-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="89829-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="89829-224">✔</span><span class="sxs-lookup"><span data-stu-id="89829-224">✔</span></span>
<span data-ttu-id="89829-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="89829-225">Geometry.Centroid</span></span> | <span data-ttu-id="89829-226">✔</span><span class="sxs-lookup"><span data-stu-id="89829-226">✔</span></span> | | <span data-ttu-id="89829-227">✔</span><span class="sxs-lookup"><span data-stu-id="89829-227">✔</span></span> | <span data-ttu-id="89829-228">✔</span><span class="sxs-lookup"><span data-stu-id="89829-228">✔</span></span>
<span data-ttu-id="89829-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="89829-230">✔</span><span class="sxs-lookup"><span data-stu-id="89829-230">✔</span></span> | <span data-ttu-id="89829-231">✔</span><span class="sxs-lookup"><span data-stu-id="89829-231">✔</span></span> | <span data-ttu-id="89829-232">✔</span><span class="sxs-lookup"><span data-stu-id="89829-232">✔</span></span> | <span data-ttu-id="89829-233">✔</span><span class="sxs-lookup"><span data-stu-id="89829-233">✔</span></span>
<span data-ttu-id="89829-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="89829-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="89829-235">✔</span><span class="sxs-lookup"><span data-stu-id="89829-235">✔</span></span> | <span data-ttu-id="89829-236">✔</span><span class="sxs-lookup"><span data-stu-id="89829-236">✔</span></span> | <span data-ttu-id="89829-237">✔</span><span class="sxs-lookup"><span data-stu-id="89829-237">✔</span></span> | <span data-ttu-id="89829-238">✔</span><span class="sxs-lookup"><span data-stu-id="89829-238">✔</span></span>
<span data-ttu-id="89829-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="89829-240">✔</span><span class="sxs-lookup"><span data-stu-id="89829-240">✔</span></span> | <span data-ttu-id="89829-241">✔</span><span class="sxs-lookup"><span data-stu-id="89829-241">✔</span></span>
<span data-ttu-id="89829-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="89829-243">✔</span><span class="sxs-lookup"><span data-stu-id="89829-243">✔</span></span> | <span data-ttu-id="89829-244">✔</span><span class="sxs-lookup"><span data-stu-id="89829-244">✔</span></span>
<span data-ttu-id="89829-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="89829-246">✔</span><span class="sxs-lookup"><span data-stu-id="89829-246">✔</span></span> | | <span data-ttu-id="89829-247">✔</span><span class="sxs-lookup"><span data-stu-id="89829-247">✔</span></span> | <span data-ttu-id="89829-248">✔</span><span class="sxs-lookup"><span data-stu-id="89829-248">✔</span></span>
<span data-ttu-id="89829-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="89829-250">✔</span><span class="sxs-lookup"><span data-stu-id="89829-250">✔</span></span> | <span data-ttu-id="89829-251">✔</span><span class="sxs-lookup"><span data-stu-id="89829-251">✔</span></span> | <span data-ttu-id="89829-252">✔</span><span class="sxs-lookup"><span data-stu-id="89829-252">✔</span></span> | <span data-ttu-id="89829-253">✔</span><span class="sxs-lookup"><span data-stu-id="89829-253">✔</span></span>
<span data-ttu-id="89829-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="89829-254">Geometry.Dimension</span></span> | <span data-ttu-id="89829-255">✔</span><span class="sxs-lookup"><span data-stu-id="89829-255">✔</span></span> | <span data-ttu-id="89829-256">✔</span><span class="sxs-lookup"><span data-stu-id="89829-256">✔</span></span> | <span data-ttu-id="89829-257">✔</span><span class="sxs-lookup"><span data-stu-id="89829-257">✔</span></span> | <span data-ttu-id="89829-258">✔</span><span class="sxs-lookup"><span data-stu-id="89829-258">✔</span></span>
<span data-ttu-id="89829-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="89829-260">✔</span><span class="sxs-lookup"><span data-stu-id="89829-260">✔</span></span> | <span data-ttu-id="89829-261">✔</span><span class="sxs-lookup"><span data-stu-id="89829-261">✔</span></span> | <span data-ttu-id="89829-262">✔</span><span class="sxs-lookup"><span data-stu-id="89829-262">✔</span></span> | <span data-ttu-id="89829-263">✔</span><span class="sxs-lookup"><span data-stu-id="89829-263">✔</span></span>
<span data-ttu-id="89829-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="89829-265">✔</span><span class="sxs-lookup"><span data-stu-id="89829-265">✔</span></span> | <span data-ttu-id="89829-266">✔</span><span class="sxs-lookup"><span data-stu-id="89829-266">✔</span></span> | <span data-ttu-id="89829-267">✔</span><span class="sxs-lookup"><span data-stu-id="89829-267">✔</span></span> | <span data-ttu-id="89829-268">✔</span><span class="sxs-lookup"><span data-stu-id="89829-268">✔</span></span>
<span data-ttu-id="89829-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="89829-269">Geometry.Envelope</span></span> | <span data-ttu-id="89829-270">✔</span><span class="sxs-lookup"><span data-stu-id="89829-270">✔</span></span> | | <span data-ttu-id="89829-271">✔</span><span class="sxs-lookup"><span data-stu-id="89829-271">✔</span></span> | <span data-ttu-id="89829-272">✔</span><span class="sxs-lookup"><span data-stu-id="89829-272">✔</span></span>
<span data-ttu-id="89829-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="89829-274">✔</span><span class="sxs-lookup"><span data-stu-id="89829-274">✔</span></span>
<span data-ttu-id="89829-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="89829-276">✔</span><span class="sxs-lookup"><span data-stu-id="89829-276">✔</span></span> | <span data-ttu-id="89829-277">✔</span><span class="sxs-lookup"><span data-stu-id="89829-277">✔</span></span> | <span data-ttu-id="89829-278">✔</span><span class="sxs-lookup"><span data-stu-id="89829-278">✔</span></span> | <span data-ttu-id="89829-279">✔</span><span class="sxs-lookup"><span data-stu-id="89829-279">✔</span></span>
<span data-ttu-id="89829-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="89829-280">Geometry.GeometryType</span></span> | <span data-ttu-id="89829-281">✔</span><span class="sxs-lookup"><span data-stu-id="89829-281">✔</span></span> | <span data-ttu-id="89829-282">✔</span><span class="sxs-lookup"><span data-stu-id="89829-282">✔</span></span> | <span data-ttu-id="89829-283">✔</span><span class="sxs-lookup"><span data-stu-id="89829-283">✔</span></span> | <span data-ttu-id="89829-284">✔</span><span class="sxs-lookup"><span data-stu-id="89829-284">✔</span></span>
<span data-ttu-id="89829-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="89829-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="89829-286">✔</span><span class="sxs-lookup"><span data-stu-id="89829-286">✔</span></span> | | <span data-ttu-id="89829-287">✔</span><span class="sxs-lookup"><span data-stu-id="89829-287">✔</span></span> | <span data-ttu-id="89829-288">✔</span><span class="sxs-lookup"><span data-stu-id="89829-288">✔</span></span>
<span data-ttu-id="89829-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="89829-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="89829-290">✔</span><span class="sxs-lookup"><span data-stu-id="89829-290">✔</span></span> | | <span data-ttu-id="89829-291">✔</span><span class="sxs-lookup"><span data-stu-id="89829-291">✔</span></span>
<span data-ttu-id="89829-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="89829-293">✔</span><span class="sxs-lookup"><span data-stu-id="89829-293">✔</span></span> | <span data-ttu-id="89829-294">✔</span><span class="sxs-lookup"><span data-stu-id="89829-294">✔</span></span> | <span data-ttu-id="89829-295">✔</span><span class="sxs-lookup"><span data-stu-id="89829-295">✔</span></span> | <span data-ttu-id="89829-296">✔</span><span class="sxs-lookup"><span data-stu-id="89829-296">✔</span></span>
<span data-ttu-id="89829-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="89829-298">✔</span><span class="sxs-lookup"><span data-stu-id="89829-298">✔</span></span> | <span data-ttu-id="89829-299">✔</span><span class="sxs-lookup"><span data-stu-id="89829-299">✔</span></span> | <span data-ttu-id="89829-300">✔</span><span class="sxs-lookup"><span data-stu-id="89829-300">✔</span></span> | <span data-ttu-id="89829-301">✔</span><span class="sxs-lookup"><span data-stu-id="89829-301">✔</span></span>
<span data-ttu-id="89829-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="89829-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="89829-303">✔</span><span class="sxs-lookup"><span data-stu-id="89829-303">✔</span></span> | <span data-ttu-id="89829-304">✔</span><span class="sxs-lookup"><span data-stu-id="89829-304">✔</span></span> | <span data-ttu-id="89829-305">✔</span><span class="sxs-lookup"><span data-stu-id="89829-305">✔</span></span> | <span data-ttu-id="89829-306">✔</span><span class="sxs-lookup"><span data-stu-id="89829-306">✔</span></span>
<span data-ttu-id="89829-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="89829-307">Geometry.IsSimple</span></span> | <span data-ttu-id="89829-308">✔</span><span class="sxs-lookup"><span data-stu-id="89829-308">✔</span></span> | | <span data-ttu-id="89829-309">✔</span><span class="sxs-lookup"><span data-stu-id="89829-309">✔</span></span> | <span data-ttu-id="89829-310">✔</span><span class="sxs-lookup"><span data-stu-id="89829-310">✔</span></span>
<span data-ttu-id="89829-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="89829-311">Geometry.IsValid</span></span> | <span data-ttu-id="89829-312">✔</span><span class="sxs-lookup"><span data-stu-id="89829-312">✔</span></span> | <span data-ttu-id="89829-313">✔</span><span class="sxs-lookup"><span data-stu-id="89829-313">✔</span></span> | <span data-ttu-id="89829-314">✔</span><span class="sxs-lookup"><span data-stu-id="89829-314">✔</span></span> | <span data-ttu-id="89829-315">✔</span><span class="sxs-lookup"><span data-stu-id="89829-315">✔</span></span>
<span data-ttu-id="89829-316">Geometry.IsWithinDistance (geometria, double)</span><span class="sxs-lookup"><span data-stu-id="89829-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="89829-317">✔</span><span class="sxs-lookup"><span data-stu-id="89829-317">✔</span></span> | | <span data-ttu-id="89829-318">✔</span><span class="sxs-lookup"><span data-stu-id="89829-318">✔</span></span>
<span data-ttu-id="89829-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="89829-319">Geometry.Length</span></span> | <span data-ttu-id="89829-320">✔</span><span class="sxs-lookup"><span data-stu-id="89829-320">✔</span></span> | <span data-ttu-id="89829-321">✔</span><span class="sxs-lookup"><span data-stu-id="89829-321">✔</span></span> | <span data-ttu-id="89829-322">✔</span><span class="sxs-lookup"><span data-stu-id="89829-322">✔</span></span> | <span data-ttu-id="89829-323">✔</span><span class="sxs-lookup"><span data-stu-id="89829-323">✔</span></span>
<span data-ttu-id="89829-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="89829-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="89829-325">✔</span><span class="sxs-lookup"><span data-stu-id="89829-325">✔</span></span> | <span data-ttu-id="89829-326">✔</span><span class="sxs-lookup"><span data-stu-id="89829-326">✔</span></span> | <span data-ttu-id="89829-327">✔</span><span class="sxs-lookup"><span data-stu-id="89829-327">✔</span></span> | <span data-ttu-id="89829-328">✔</span><span class="sxs-lookup"><span data-stu-id="89829-328">✔</span></span>
<span data-ttu-id="89829-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="89829-329">Geometry.NumPoints</span></span> | <span data-ttu-id="89829-330">✔</span><span class="sxs-lookup"><span data-stu-id="89829-330">✔</span></span> | <span data-ttu-id="89829-331">✔</span><span class="sxs-lookup"><span data-stu-id="89829-331">✔</span></span> | <span data-ttu-id="89829-332">✔</span><span class="sxs-lookup"><span data-stu-id="89829-332">✔</span></span> | <span data-ttu-id="89829-333">✔</span><span class="sxs-lookup"><span data-stu-id="89829-333">✔</span></span>
<span data-ttu-id="89829-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="89829-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="89829-335">✔</span><span class="sxs-lookup"><span data-stu-id="89829-335">✔</span></span> | <span data-ttu-id="89829-336">✔</span><span class="sxs-lookup"><span data-stu-id="89829-336">✔</span></span> | <span data-ttu-id="89829-337">✔</span><span class="sxs-lookup"><span data-stu-id="89829-337">✔</span></span>
<span data-ttu-id="89829-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="89829-339">✔</span><span class="sxs-lookup"><span data-stu-id="89829-339">✔</span></span> | <span data-ttu-id="89829-340">✔</span><span class="sxs-lookup"><span data-stu-id="89829-340">✔</span></span> | <span data-ttu-id="89829-341">✔</span><span class="sxs-lookup"><span data-stu-id="89829-341">✔</span></span> | <span data-ttu-id="89829-342">✔</span><span class="sxs-lookup"><span data-stu-id="89829-342">✔</span></span>
<span data-ttu-id="89829-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="89829-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="89829-344">✔</span><span class="sxs-lookup"><span data-stu-id="89829-344">✔</span></span> | | <span data-ttu-id="89829-345">✔</span><span class="sxs-lookup"><span data-stu-id="89829-345">✔</span></span> | <span data-ttu-id="89829-346">✔</span><span class="sxs-lookup"><span data-stu-id="89829-346">✔</span></span>
<span data-ttu-id="89829-347">Geometry.Relate (geometria, cadeia de caracteres)</span><span class="sxs-lookup"><span data-stu-id="89829-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="89829-348">✔</span><span class="sxs-lookup"><span data-stu-id="89829-348">✔</span></span> | | <span data-ttu-id="89829-349">✔</span><span class="sxs-lookup"><span data-stu-id="89829-349">✔</span></span> | <span data-ttu-id="89829-350">✔</span><span class="sxs-lookup"><span data-stu-id="89829-350">✔</span></span>
<span data-ttu-id="89829-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="89829-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="89829-352">✔</span><span class="sxs-lookup"><span data-stu-id="89829-352">✔</span></span> | <span data-ttu-id="89829-353">✔</span><span class="sxs-lookup"><span data-stu-id="89829-353">✔</span></span>
<span data-ttu-id="89829-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="89829-354">Geometry.SRID</span></span> | <span data-ttu-id="89829-355">✔</span><span class="sxs-lookup"><span data-stu-id="89829-355">✔</span></span> | <span data-ttu-id="89829-356">✔</span><span class="sxs-lookup"><span data-stu-id="89829-356">✔</span></span> | <span data-ttu-id="89829-357">✔</span><span class="sxs-lookup"><span data-stu-id="89829-357">✔</span></span> | <span data-ttu-id="89829-358">✔</span><span class="sxs-lookup"><span data-stu-id="89829-358">✔</span></span>
<span data-ttu-id="89829-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="89829-360">✔</span><span class="sxs-lookup"><span data-stu-id="89829-360">✔</span></span> | <span data-ttu-id="89829-361">✔</span><span class="sxs-lookup"><span data-stu-id="89829-361">✔</span></span> | <span data-ttu-id="89829-362">✔</span><span class="sxs-lookup"><span data-stu-id="89829-362">✔</span></span> | <span data-ttu-id="89829-363">✔</span><span class="sxs-lookup"><span data-stu-id="89829-363">✔</span></span>
<span data-ttu-id="89829-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="89829-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="89829-365">✔</span><span class="sxs-lookup"><span data-stu-id="89829-365">✔</span></span> | <span data-ttu-id="89829-366">✔</span><span class="sxs-lookup"><span data-stu-id="89829-366">✔</span></span> | <span data-ttu-id="89829-367">✔</span><span class="sxs-lookup"><span data-stu-id="89829-367">✔</span></span> | <span data-ttu-id="89829-368">✔</span><span class="sxs-lookup"><span data-stu-id="89829-368">✔</span></span>
<span data-ttu-id="89829-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="89829-369">Geometry.ToText()</span></span> | <span data-ttu-id="89829-370">✔</span><span class="sxs-lookup"><span data-stu-id="89829-370">✔</span></span> | <span data-ttu-id="89829-371">✔</span><span class="sxs-lookup"><span data-stu-id="89829-371">✔</span></span> | <span data-ttu-id="89829-372">✔</span><span class="sxs-lookup"><span data-stu-id="89829-372">✔</span></span> | <span data-ttu-id="89829-373">✔</span><span class="sxs-lookup"><span data-stu-id="89829-373">✔</span></span>
<span data-ttu-id="89829-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="89829-375">✔</span><span class="sxs-lookup"><span data-stu-id="89829-375">✔</span></span> | | <span data-ttu-id="89829-376">✔</span><span class="sxs-lookup"><span data-stu-id="89829-376">✔</span></span> | <span data-ttu-id="89829-377">✔</span><span class="sxs-lookup"><span data-stu-id="89829-377">✔</span></span>
<span data-ttu-id="89829-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="89829-378">Geometry.Union()</span></span> | | | <span data-ttu-id="89829-379">✔</span><span class="sxs-lookup"><span data-stu-id="89829-379">✔</span></span>
<span data-ttu-id="89829-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="89829-381">✔</span><span class="sxs-lookup"><span data-stu-id="89829-381">✔</span></span> | <span data-ttu-id="89829-382">✔</span><span class="sxs-lookup"><span data-stu-id="89829-382">✔</span></span> | <span data-ttu-id="89829-383">✔</span><span class="sxs-lookup"><span data-stu-id="89829-383">✔</span></span> | <span data-ttu-id="89829-384">✔</span><span class="sxs-lookup"><span data-stu-id="89829-384">✔</span></span>
<span data-ttu-id="89829-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="89829-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="89829-386">✔</span><span class="sxs-lookup"><span data-stu-id="89829-386">✔</span></span> | <span data-ttu-id="89829-387">✔</span><span class="sxs-lookup"><span data-stu-id="89829-387">✔</span></span> | <span data-ttu-id="89829-388">✔</span><span class="sxs-lookup"><span data-stu-id="89829-388">✔</span></span> | <span data-ttu-id="89829-389">✔</span><span class="sxs-lookup"><span data-stu-id="89829-389">✔</span></span>
<span data-ttu-id="89829-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="89829-390">GeometryCollection.Count</span></span> | <span data-ttu-id="89829-391">✔</span><span class="sxs-lookup"><span data-stu-id="89829-391">✔</span></span> | <span data-ttu-id="89829-392">✔</span><span class="sxs-lookup"><span data-stu-id="89829-392">✔</span></span> | <span data-ttu-id="89829-393">✔</span><span class="sxs-lookup"><span data-stu-id="89829-393">✔</span></span> | <span data-ttu-id="89829-394">✔</span><span class="sxs-lookup"><span data-stu-id="89829-394">✔</span></span>
<span data-ttu-id="89829-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="89829-395">GeometryCollection[int]</span></span> | <span data-ttu-id="89829-396">✔</span><span class="sxs-lookup"><span data-stu-id="89829-396">✔</span></span> | <span data-ttu-id="89829-397">✔</span><span class="sxs-lookup"><span data-stu-id="89829-397">✔</span></span> | <span data-ttu-id="89829-398">✔</span><span class="sxs-lookup"><span data-stu-id="89829-398">✔</span></span> | <span data-ttu-id="89829-399">✔</span><span class="sxs-lookup"><span data-stu-id="89829-399">✔</span></span>
<span data-ttu-id="89829-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="89829-400">LineString.Count</span></span> | <span data-ttu-id="89829-401">✔</span><span class="sxs-lookup"><span data-stu-id="89829-401">✔</span></span> | <span data-ttu-id="89829-402">✔</span><span class="sxs-lookup"><span data-stu-id="89829-402">✔</span></span> | <span data-ttu-id="89829-403">✔</span><span class="sxs-lookup"><span data-stu-id="89829-403">✔</span></span> | <span data-ttu-id="89829-404">✔</span><span class="sxs-lookup"><span data-stu-id="89829-404">✔</span></span>
<span data-ttu-id="89829-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="89829-405">LineString.EndPoint</span></span> | <span data-ttu-id="89829-406">✔</span><span class="sxs-lookup"><span data-stu-id="89829-406">✔</span></span> | <span data-ttu-id="89829-407">✔</span><span class="sxs-lookup"><span data-stu-id="89829-407">✔</span></span> | <span data-ttu-id="89829-408">✔</span><span class="sxs-lookup"><span data-stu-id="89829-408">✔</span></span> | <span data-ttu-id="89829-409">✔</span><span class="sxs-lookup"><span data-stu-id="89829-409">✔</span></span>
<span data-ttu-id="89829-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="89829-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="89829-411">✔</span><span class="sxs-lookup"><span data-stu-id="89829-411">✔</span></span> | <span data-ttu-id="89829-412">✔</span><span class="sxs-lookup"><span data-stu-id="89829-412">✔</span></span> | <span data-ttu-id="89829-413">✔</span><span class="sxs-lookup"><span data-stu-id="89829-413">✔</span></span> | <span data-ttu-id="89829-414">✔</span><span class="sxs-lookup"><span data-stu-id="89829-414">✔</span></span>
<span data-ttu-id="89829-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="89829-415">LineString.IsClosed</span></span> | <span data-ttu-id="89829-416">✔</span><span class="sxs-lookup"><span data-stu-id="89829-416">✔</span></span> | <span data-ttu-id="89829-417">✔</span><span class="sxs-lookup"><span data-stu-id="89829-417">✔</span></span> | <span data-ttu-id="89829-418">✔</span><span class="sxs-lookup"><span data-stu-id="89829-418">✔</span></span> | <span data-ttu-id="89829-419">✔</span><span class="sxs-lookup"><span data-stu-id="89829-419">✔</span></span>
<span data-ttu-id="89829-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="89829-420">LineString.IsRing</span></span> | <span data-ttu-id="89829-421">✔</span><span class="sxs-lookup"><span data-stu-id="89829-421">✔</span></span> | | <span data-ttu-id="89829-422">✔</span><span class="sxs-lookup"><span data-stu-id="89829-422">✔</span></span> | <span data-ttu-id="89829-423">✔</span><span class="sxs-lookup"><span data-stu-id="89829-423">✔</span></span>
<span data-ttu-id="89829-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="89829-424">LineString.StartPoint</span></span> | <span data-ttu-id="89829-425">✔</span><span class="sxs-lookup"><span data-stu-id="89829-425">✔</span></span> | <span data-ttu-id="89829-426">✔</span><span class="sxs-lookup"><span data-stu-id="89829-426">✔</span></span> | <span data-ttu-id="89829-427">✔</span><span class="sxs-lookup"><span data-stu-id="89829-427">✔</span></span> | <span data-ttu-id="89829-428">✔</span><span class="sxs-lookup"><span data-stu-id="89829-428">✔</span></span>
<span data-ttu-id="89829-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="89829-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="89829-430">✔</span><span class="sxs-lookup"><span data-stu-id="89829-430">✔</span></span> | <span data-ttu-id="89829-431">✔</span><span class="sxs-lookup"><span data-stu-id="89829-431">✔</span></span> | <span data-ttu-id="89829-432">✔</span><span class="sxs-lookup"><span data-stu-id="89829-432">✔</span></span> | <span data-ttu-id="89829-433">✔</span><span class="sxs-lookup"><span data-stu-id="89829-433">✔</span></span>
<span data-ttu-id="89829-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="89829-434">Point.M</span></span> | <span data-ttu-id="89829-435">✔</span><span class="sxs-lookup"><span data-stu-id="89829-435">✔</span></span> | <span data-ttu-id="89829-436">✔</span><span class="sxs-lookup"><span data-stu-id="89829-436">✔</span></span> | <span data-ttu-id="89829-437">✔</span><span class="sxs-lookup"><span data-stu-id="89829-437">✔</span></span> | <span data-ttu-id="89829-438">✔</span><span class="sxs-lookup"><span data-stu-id="89829-438">✔</span></span>
<span data-ttu-id="89829-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="89829-439">Point.X</span></span> | <span data-ttu-id="89829-440">✔</span><span class="sxs-lookup"><span data-stu-id="89829-440">✔</span></span> | <span data-ttu-id="89829-441">✔</span><span class="sxs-lookup"><span data-stu-id="89829-441">✔</span></span> | <span data-ttu-id="89829-442">✔</span><span class="sxs-lookup"><span data-stu-id="89829-442">✔</span></span> | <span data-ttu-id="89829-443">✔</span><span class="sxs-lookup"><span data-stu-id="89829-443">✔</span></span>
<span data-ttu-id="89829-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="89829-444">Point.Y</span></span> | <span data-ttu-id="89829-445">✔</span><span class="sxs-lookup"><span data-stu-id="89829-445">✔</span></span> | <span data-ttu-id="89829-446">✔</span><span class="sxs-lookup"><span data-stu-id="89829-446">✔</span></span> | <span data-ttu-id="89829-447">✔</span><span class="sxs-lookup"><span data-stu-id="89829-447">✔</span></span> | <span data-ttu-id="89829-448">✔</span><span class="sxs-lookup"><span data-stu-id="89829-448">✔</span></span>
<span data-ttu-id="89829-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="89829-449">Point.Z</span></span> | <span data-ttu-id="89829-450">✔</span><span class="sxs-lookup"><span data-stu-id="89829-450">✔</span></span> | <span data-ttu-id="89829-451">✔</span><span class="sxs-lookup"><span data-stu-id="89829-451">✔</span></span> | <span data-ttu-id="89829-452">✔</span><span class="sxs-lookup"><span data-stu-id="89829-452">✔</span></span> | <span data-ttu-id="89829-453">✔</span><span class="sxs-lookup"><span data-stu-id="89829-453">✔</span></span>
<span data-ttu-id="89829-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="89829-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="89829-455">✔</span><span class="sxs-lookup"><span data-stu-id="89829-455">✔</span></span> | <span data-ttu-id="89829-456">✔</span><span class="sxs-lookup"><span data-stu-id="89829-456">✔</span></span> | <span data-ttu-id="89829-457">✔</span><span class="sxs-lookup"><span data-stu-id="89829-457">✔</span></span> | <span data-ttu-id="89829-458">✔</span><span class="sxs-lookup"><span data-stu-id="89829-458">✔</span></span>
<span data-ttu-id="89829-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="89829-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="89829-460">✔</span><span class="sxs-lookup"><span data-stu-id="89829-460">✔</span></span> | <span data-ttu-id="89829-461">✔</span><span class="sxs-lookup"><span data-stu-id="89829-461">✔</span></span> | <span data-ttu-id="89829-462">✔</span><span class="sxs-lookup"><span data-stu-id="89829-462">✔</span></span> | <span data-ttu-id="89829-463">✔</span><span class="sxs-lookup"><span data-stu-id="89829-463">✔</span></span>
<span data-ttu-id="89829-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="89829-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="89829-465">✔</span><span class="sxs-lookup"><span data-stu-id="89829-465">✔</span></span> | <span data-ttu-id="89829-466">✔</span><span class="sxs-lookup"><span data-stu-id="89829-466">✔</span></span> | <span data-ttu-id="89829-467">✔</span><span class="sxs-lookup"><span data-stu-id="89829-467">✔</span></span> | <span data-ttu-id="89829-468">✔</span><span class="sxs-lookup"><span data-stu-id="89829-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89829-469">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="89829-469">Additional resources</span></span>

* [<span data-ttu-id="89829-470">Dados espaciais no SQL Server</span><span class="sxs-lookup"><span data-stu-id="89829-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="89829-471">Home page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="89829-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="89829-472">Documentação de PostGIS</span><span class="sxs-lookup"><span data-stu-id="89829-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
