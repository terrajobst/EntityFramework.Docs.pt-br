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
# <a name="spatial-data"></a><span data-ttu-id="58594-102">Dados espaciais</span><span class="sxs-lookup"><span data-stu-id="58594-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="58594-103">Esse recurso foi adicionado no EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="58594-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="58594-104">Dados espaciais representam o local físico e a forma de objetos.</span><span class="sxs-lookup"><span data-stu-id="58594-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="58594-105">Muitos bancos de dados fornecem suporte para esse tipo de dado para que ele possa ser indexado e consultado junto com outros dados.</span><span class="sxs-lookup"><span data-stu-id="58594-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="58594-106">Os cenários comuns incluem a consulta de objetos dentro de uma determinada distância de um local ou a seleção do objeto cuja borda contém um determinado local.</span><span class="sxs-lookup"><span data-stu-id="58594-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="58594-107">EF Core dá suporte ao mapeamento para tipos de dados espaciais usando a biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="58594-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="58594-108">Instalando o</span><span class="sxs-lookup"><span data-stu-id="58594-108">Installing</span></span>

<span data-ttu-id="58594-109">Para usar dados espaciais com EF Core, você precisa instalar o pacote NuGet de suporte apropriado.</span><span class="sxs-lookup"><span data-stu-id="58594-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="58594-110">O pacote que você precisa instalar depende do provedor que você está usando.</span><span class="sxs-lookup"><span data-stu-id="58594-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="58594-111">Provedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="58594-111">EF Core Provider</span></span>                        | <span data-ttu-id="58594-112">Pacote NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="58594-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="58594-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="58594-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="58594-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="58594-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="58594-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="58594-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="58594-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="58594-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="58594-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="58594-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="58594-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="58594-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="58594-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="58594-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="58594-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="58594-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="58594-121">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="58594-121">Reverse engineering</span></span>

<span data-ttu-id="58594-122">Os pacotes de NuGet espaciais também habilitam modelos de [engenharia reversa](../managing-schemas/scaffolding.md) com propriedades espaciais, mas você precisa instalar o pacote ***antes*** de executar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="58594-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="58594-123">Caso contrário, você receberá avisos sobre não encontrar mapeamentos de tipo para as colunas e as colunas serão ignoradas.</span><span class="sxs-lookup"><span data-stu-id="58594-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="58594-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="58594-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="58594-125">NetTopologySuite é uma biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="58594-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="58594-126">EF Core habilita o mapeamento para tipos de dados espaciais no banco de dado usando tipos NTS em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="58594-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="58594-127">Para habilitar o mapeamento para tipos espaciais via NTS, chame o método UseNetTopologySuite no construtor de opções DbContext do provedor.</span><span class="sxs-lookup"><span data-stu-id="58594-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="58594-128">Por exemplo, com SQL Server você o chamaria assim.</span><span class="sxs-lookup"><span data-stu-id="58594-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="58594-129">Há vários tipos de dados espaciais.</span><span class="sxs-lookup"><span data-stu-id="58594-129">There are several spatial data types.</span></span> <span data-ttu-id="58594-130">O tipo usado depende dos tipos de formas que você deseja permitir.</span><span class="sxs-lookup"><span data-stu-id="58594-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="58594-131">Aqui está a hierarquia de tipos NTS que você pode usar para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="58594-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="58594-132">Eles estão localizados no namespace `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="58594-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="58594-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="58594-133">Geometry</span></span>
  * <span data-ttu-id="58594-134">Ponto</span><span class="sxs-lookup"><span data-stu-id="58594-134">Point</span></span>
  * <span data-ttu-id="58594-135">LineString</span><span class="sxs-lookup"><span data-stu-id="58594-135">LineString</span></span>
  * <span data-ttu-id="58594-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="58594-136">Polygon</span></span>
  * <span data-ttu-id="58594-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="58594-137">GeometryCollection</span></span>
    * <span data-ttu-id="58594-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="58594-138">MultiPoint</span></span>
    * <span data-ttu-id="58594-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="58594-139">MultiLineString</span></span>
    * <span data-ttu-id="58594-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="58594-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="58594-141">Circularstring, CompoundCurve e CurePolygon não têm suporte do NTS.</span><span class="sxs-lookup"><span data-stu-id="58594-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="58594-142">Usar o tipo base Geometry permite que qualquer tipo de forma seja especificado pela propriedade.</span><span class="sxs-lookup"><span data-stu-id="58594-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="58594-143">As classes de entidade a seguir podem ser usadas para mapear para tabelas no [banco de dados de exemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="58594-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="58594-144">Criando valores</span><span class="sxs-lookup"><span data-stu-id="58594-144">Creating values</span></span>

<span data-ttu-id="58594-145">Você pode usar construtores para criar objetos Geometry; no entanto, o NTS recomenda usar uma fábrica de geometria em vez disso.</span><span class="sxs-lookup"><span data-stu-id="58594-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="58594-146">Isso permite que você especifique um SRID padrão (o sistema de referência espacial usado pelas coordenadas) e oferece controle sobre itens mais avançados, como o modelo de precisão (usado durante cálculos) e a sequência de coordenadas (determina quais ordenações--dimensões e medidas – estão disponíveis).</span><span class="sxs-lookup"><span data-stu-id="58594-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="58594-147">4326 refere-se a WGS 84, um padrão usado em GPS e outros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="58594-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="58594-148">Longitude e latitude</span><span class="sxs-lookup"><span data-stu-id="58594-148">Longitude and Latitude</span></span>

<span data-ttu-id="58594-149">As coordenadas em NTS estão em termos de valores X e Y.</span><span class="sxs-lookup"><span data-stu-id="58594-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="58594-150">Para representar a longitude e a latitude, use X para longitude e Y para latitude.</span><span class="sxs-lookup"><span data-stu-id="58594-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="58594-151">Observe que isso é **retroativa** no formato `latitude, longitude` no qual você normalmente vê esses valores.</span><span class="sxs-lookup"><span data-stu-id="58594-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="58594-152">SRID ignorado durante as operações do cliente</span><span class="sxs-lookup"><span data-stu-id="58594-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="58594-153">NTS ignora os valores de SRID durante as operações.</span><span class="sxs-lookup"><span data-stu-id="58594-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="58594-154">Ele assume um sistema de coordenadas planar.</span><span class="sxs-lookup"><span data-stu-id="58594-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="58594-155">Isso significa que, se você especificar coordenadas em termos de longitude e latitude, alguns valores avaliados pelo cliente, como distância, comprimento e área, serão em graus, não em metros.</span><span class="sxs-lookup"><span data-stu-id="58594-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="58594-156">Para obter valores mais significativos, primeiro você precisa projetar as coordenadas para outro sistema de coordenadas usando uma biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular esses valores.</span><span class="sxs-lookup"><span data-stu-id="58594-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="58594-157">Se uma operação for avaliada pelo servidor por EF Core por meio do SQL, a unidade do resultado será determinada pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58594-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="58594-158">Aqui está um exemplo de como usar ProjNet4GeoAPI para calcular a distância entre duas cidades.</span><span class="sxs-lookup"><span data-stu-id="58594-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="58594-159">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="58594-159">Querying Data</span></span>

<span data-ttu-id="58594-160">No LINQ, os métodos e as propriedades NTS disponíveis como funções de banco de dados serão convertidos em SQL.</span><span class="sxs-lookup"><span data-stu-id="58594-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="58594-161">Por exemplo, os métodos Distance e Contains são traduzidos nas consultas a seguir.</span><span class="sxs-lookup"><span data-stu-id="58594-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="58594-162">A tabela no final deste artigo mostra quais membros têm suporte em vários provedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="58594-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="58594-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="58594-163">SQL Server</span></span>

<span data-ttu-id="58594-164">Se você estiver usando SQL Server, haverá algumas coisas adicionais das quais você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="58594-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="58594-165">Geografia ou Geometry</span><span class="sxs-lookup"><span data-stu-id="58594-165">Geography or geometry</span></span>

<span data-ttu-id="58594-166">Por padrão, as propriedades espaciais são mapeadas para colunas `geography` em SQL Server.</span><span class="sxs-lookup"><span data-stu-id="58594-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="58594-167">Para usar `geometry`, [Configure o tipo de coluna](xref:core/modeling/relational/data-types) em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="58594-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="58594-168">Anéis de polígono de Geografia</span><span class="sxs-lookup"><span data-stu-id="58594-168">Geography polygon rings</span></span>

<span data-ttu-id="58594-169">Ao usar o tipo de coluna `geography`, SQL Server impõe requisitos adicionais no anel exterior (ou no Shell) e nos anéis interiores (ou buracos).</span><span class="sxs-lookup"><span data-stu-id="58594-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="58594-170">O anel exterior deve ser orientado no sentido anti-horário e nos anéis interiores no sentido horário.</span><span class="sxs-lookup"><span data-stu-id="58594-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="58594-171">NTS valida isso antes de enviar valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58594-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="58594-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="58594-172">FullGlobe</span></span>

<span data-ttu-id="58594-173">SQL Server tem um tipo de geometria não padrão para representar o globo inteiro ao usar o tipo de coluna `geography`.</span><span class="sxs-lookup"><span data-stu-id="58594-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="58594-174">Ele também tem uma maneira de representar polígonos com base em todo o globo (sem um anel exterior).</span><span class="sxs-lookup"><span data-stu-id="58594-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="58594-175">Nenhuma delas é suportada pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="58594-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="58594-176">Os FullGlobe e polígonos baseados nele não são suportados pelo NTS.</span><span class="sxs-lookup"><span data-stu-id="58594-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="58594-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="58594-177">SQLite</span></span>

<span data-ttu-id="58594-178">Aqui estão algumas informações adicionais para as que usam o SQLite.</span><span class="sxs-lookup"><span data-stu-id="58594-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="58594-179">Instalando o SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="58594-179">Installing SpatiaLite</span></span>

<span data-ttu-id="58594-180">No Windows, a biblioteca mod_spatialite nativa é distribuída como uma dependência de pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="58594-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="58594-181">Outras plataformas precisam instalá-lo separadamente.</span><span class="sxs-lookup"><span data-stu-id="58594-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="58594-182">Isso normalmente é feito usando um Gerenciador de pacotes de software.</span><span class="sxs-lookup"><span data-stu-id="58594-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="58594-183">Por exemplo, você pode usar a APT no Ubuntu e no Homebrew no MacOS.</span><span class="sxs-lookup"><span data-stu-id="58594-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="58594-184">Configurando o SRID</span><span class="sxs-lookup"><span data-stu-id="58594-184">Configuring SRID</span></span>

<span data-ttu-id="58594-185">No SpatiaLite, as colunas precisam especificar um SRID por coluna.</span><span class="sxs-lookup"><span data-stu-id="58594-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="58594-186">O SRID padrão é `0`.</span><span class="sxs-lookup"><span data-stu-id="58594-186">The default SRID is `0`.</span></span> <span data-ttu-id="58594-187">Especifique um SRID diferente usando o método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="58594-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="58594-188">Dimensão</span><span class="sxs-lookup"><span data-stu-id="58594-188">Dimension</span></span>

<span data-ttu-id="58594-189">Semelhante ao SRID, a dimensão (ou as ordenada) de uma coluna também é especificada como parte da coluna.</span><span class="sxs-lookup"><span data-stu-id="58594-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="58594-190">As ordenadas padrão são X e Y. Habilite mais ordenada (Z e M) usando o método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="58594-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="58594-191">Operações convertidas</span><span class="sxs-lookup"><span data-stu-id="58594-191">Translated Operations</span></span>

<span data-ttu-id="58594-192">Esta tabela mostra quais membros de NTS são convertidos em SQL por cada provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="58594-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="58594-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="58594-193">NetTopologySuite</span></span> | <span data-ttu-id="58594-194">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-194">SQL Server (geometry)</span></span> | <span data-ttu-id="58594-195">SQL Server (Geografia)</span><span class="sxs-lookup"><span data-stu-id="58594-195">SQL Server (geography)</span></span> | <span data-ttu-id="58594-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="58594-196">SQLite</span></span> | <span data-ttu-id="58594-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="58594-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="58594-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="58594-198">Geometry.Area</span></span> | <span data-ttu-id="58594-199">✔</span><span class="sxs-lookup"><span data-stu-id="58594-199">✔</span></span> | <span data-ttu-id="58594-200">✔</span><span class="sxs-lookup"><span data-stu-id="58594-200">✔</span></span> | <span data-ttu-id="58594-201">✔</span><span class="sxs-lookup"><span data-stu-id="58594-201">✔</span></span> | <span data-ttu-id="58594-202">✔</span><span class="sxs-lookup"><span data-stu-id="58594-202">✔</span></span>
<span data-ttu-id="58594-203">Geometry. asbinary ()</span><span class="sxs-lookup"><span data-stu-id="58594-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="58594-204">✔</span><span class="sxs-lookup"><span data-stu-id="58594-204">✔</span></span> | <span data-ttu-id="58594-205">✔</span><span class="sxs-lookup"><span data-stu-id="58594-205">✔</span></span> | <span data-ttu-id="58594-206">✔</span><span class="sxs-lookup"><span data-stu-id="58594-206">✔</span></span> | <span data-ttu-id="58594-207">✔</span><span class="sxs-lookup"><span data-stu-id="58594-207">✔</span></span>
<span data-ttu-id="58594-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="58594-208">Geometry.AsText()</span></span> | <span data-ttu-id="58594-209">✔</span><span class="sxs-lookup"><span data-stu-id="58594-209">✔</span></span> | <span data-ttu-id="58594-210">✔</span><span class="sxs-lookup"><span data-stu-id="58594-210">✔</span></span> | <span data-ttu-id="58594-211">✔</span><span class="sxs-lookup"><span data-stu-id="58594-211">✔</span></span> | <span data-ttu-id="58594-212">✔</span><span class="sxs-lookup"><span data-stu-id="58594-212">✔</span></span>
<span data-ttu-id="58594-213">Geometry. limite</span><span class="sxs-lookup"><span data-stu-id="58594-213">Geometry.Boundary</span></span> | <span data-ttu-id="58594-214">✔</span><span class="sxs-lookup"><span data-stu-id="58594-214">✔</span></span> | | <span data-ttu-id="58594-215">✔</span><span class="sxs-lookup"><span data-stu-id="58594-215">✔</span></span> | <span data-ttu-id="58594-216">✔</span><span class="sxs-lookup"><span data-stu-id="58594-216">✔</span></span>
<span data-ttu-id="58594-217">Geometry. buffer (duplo)</span><span class="sxs-lookup"><span data-stu-id="58594-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="58594-218">✔</span><span class="sxs-lookup"><span data-stu-id="58594-218">✔</span></span> | <span data-ttu-id="58594-219">✔</span><span class="sxs-lookup"><span data-stu-id="58594-219">✔</span></span> | <span data-ttu-id="58594-220">✔</span><span class="sxs-lookup"><span data-stu-id="58594-220">✔</span></span> | <span data-ttu-id="58594-221">✔</span><span class="sxs-lookup"><span data-stu-id="58594-221">✔</span></span>
<span data-ttu-id="58594-222">Geometry. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="58594-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="58594-223">✔</span><span class="sxs-lookup"><span data-stu-id="58594-223">✔</span></span>
<span data-ttu-id="58594-224">Geometry. centróide</span><span class="sxs-lookup"><span data-stu-id="58594-224">Geometry.Centroid</span></span> | <span data-ttu-id="58594-225">✔</span><span class="sxs-lookup"><span data-stu-id="58594-225">✔</span></span> | | <span data-ttu-id="58594-226">✔</span><span class="sxs-lookup"><span data-stu-id="58594-226">✔</span></span> | <span data-ttu-id="58594-227">✔</span><span class="sxs-lookup"><span data-stu-id="58594-227">✔</span></span>
<span data-ttu-id="58594-228">Geometry. Contains (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="58594-229">✔</span><span class="sxs-lookup"><span data-stu-id="58594-229">✔</span></span> | <span data-ttu-id="58594-230">✔</span><span class="sxs-lookup"><span data-stu-id="58594-230">✔</span></span> | <span data-ttu-id="58594-231">✔</span><span class="sxs-lookup"><span data-stu-id="58594-231">✔</span></span> | <span data-ttu-id="58594-232">✔</span><span class="sxs-lookup"><span data-stu-id="58594-232">✔</span></span>
<span data-ttu-id="58594-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="58594-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="58594-234">✔</span><span class="sxs-lookup"><span data-stu-id="58594-234">✔</span></span> | <span data-ttu-id="58594-235">✔</span><span class="sxs-lookup"><span data-stu-id="58594-235">✔</span></span> | <span data-ttu-id="58594-236">✔</span><span class="sxs-lookup"><span data-stu-id="58594-236">✔</span></span> | <span data-ttu-id="58594-237">✔</span><span class="sxs-lookup"><span data-stu-id="58594-237">✔</span></span>
<span data-ttu-id="58594-238">Geometry. CoveredBy (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="58594-239">✔</span><span class="sxs-lookup"><span data-stu-id="58594-239">✔</span></span> | <span data-ttu-id="58594-240">✔</span><span class="sxs-lookup"><span data-stu-id="58594-240">✔</span></span>
<span data-ttu-id="58594-241">Geometry. cubras (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="58594-242">✔</span><span class="sxs-lookup"><span data-stu-id="58594-242">✔</span></span> | <span data-ttu-id="58594-243">✔</span><span class="sxs-lookup"><span data-stu-id="58594-243">✔</span></span>
<span data-ttu-id="58594-244">Geometry. Crosss (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="58594-245">✔</span><span class="sxs-lookup"><span data-stu-id="58594-245">✔</span></span> | | <span data-ttu-id="58594-246">✔</span><span class="sxs-lookup"><span data-stu-id="58594-246">✔</span></span> | <span data-ttu-id="58594-247">✔</span><span class="sxs-lookup"><span data-stu-id="58594-247">✔</span></span>
<span data-ttu-id="58594-248">Geometry. Difference (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="58594-249">✔</span><span class="sxs-lookup"><span data-stu-id="58594-249">✔</span></span> | <span data-ttu-id="58594-250">✔</span><span class="sxs-lookup"><span data-stu-id="58594-250">✔</span></span> | <span data-ttu-id="58594-251">✔</span><span class="sxs-lookup"><span data-stu-id="58594-251">✔</span></span> | <span data-ttu-id="58594-252">✔</span><span class="sxs-lookup"><span data-stu-id="58594-252">✔</span></span>
<span data-ttu-id="58594-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="58594-253">Geometry.Dimension</span></span> | <span data-ttu-id="58594-254">✔</span><span class="sxs-lookup"><span data-stu-id="58594-254">✔</span></span> | <span data-ttu-id="58594-255">✔</span><span class="sxs-lookup"><span data-stu-id="58594-255">✔</span></span> | <span data-ttu-id="58594-256">✔</span><span class="sxs-lookup"><span data-stu-id="58594-256">✔</span></span> | <span data-ttu-id="58594-257">✔</span><span class="sxs-lookup"><span data-stu-id="58594-257">✔</span></span>
<span data-ttu-id="58594-258">Geometry. disjunção (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="58594-259">✔</span><span class="sxs-lookup"><span data-stu-id="58594-259">✔</span></span> | <span data-ttu-id="58594-260">✔</span><span class="sxs-lookup"><span data-stu-id="58594-260">✔</span></span> | <span data-ttu-id="58594-261">✔</span><span class="sxs-lookup"><span data-stu-id="58594-261">✔</span></span> | <span data-ttu-id="58594-262">✔</span><span class="sxs-lookup"><span data-stu-id="58594-262">✔</span></span>
<span data-ttu-id="58594-263">Geometry. distance (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="58594-264">✔</span><span class="sxs-lookup"><span data-stu-id="58594-264">✔</span></span> | <span data-ttu-id="58594-265">✔</span><span class="sxs-lookup"><span data-stu-id="58594-265">✔</span></span> | <span data-ttu-id="58594-266">✔</span><span class="sxs-lookup"><span data-stu-id="58594-266">✔</span></span> | <span data-ttu-id="58594-267">✔</span><span class="sxs-lookup"><span data-stu-id="58594-267">✔</span></span>
<span data-ttu-id="58594-268">Geometry. envelope</span><span class="sxs-lookup"><span data-stu-id="58594-268">Geometry.Envelope</span></span> | <span data-ttu-id="58594-269">✔</span><span class="sxs-lookup"><span data-stu-id="58594-269">✔</span></span> | | <span data-ttu-id="58594-270">✔</span><span class="sxs-lookup"><span data-stu-id="58594-270">✔</span></span> | <span data-ttu-id="58594-271">✔</span><span class="sxs-lookup"><span data-stu-id="58594-271">✔</span></span>
<span data-ttu-id="58594-272">Geometry. EqualsExact (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="58594-273">✔</span><span class="sxs-lookup"><span data-stu-id="58594-273">✔</span></span>
<span data-ttu-id="58594-274">Geometry. EqualsTopologically (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="58594-275">✔</span><span class="sxs-lookup"><span data-stu-id="58594-275">✔</span></span> | <span data-ttu-id="58594-276">✔</span><span class="sxs-lookup"><span data-stu-id="58594-276">✔</span></span> | <span data-ttu-id="58594-277">✔</span><span class="sxs-lookup"><span data-stu-id="58594-277">✔</span></span> | <span data-ttu-id="58594-278">✔</span><span class="sxs-lookup"><span data-stu-id="58594-278">✔</span></span>
<span data-ttu-id="58594-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="58594-279">Geometry.GeometryType</span></span> | <span data-ttu-id="58594-280">✔</span><span class="sxs-lookup"><span data-stu-id="58594-280">✔</span></span> | <span data-ttu-id="58594-281">✔</span><span class="sxs-lookup"><span data-stu-id="58594-281">✔</span></span> | <span data-ttu-id="58594-282">✔</span><span class="sxs-lookup"><span data-stu-id="58594-282">✔</span></span> | <span data-ttu-id="58594-283">✔</span><span class="sxs-lookup"><span data-stu-id="58594-283">✔</span></span>
<span data-ttu-id="58594-284">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="58594-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="58594-285">✔</span><span class="sxs-lookup"><span data-stu-id="58594-285">✔</span></span> | | <span data-ttu-id="58594-286">✔</span><span class="sxs-lookup"><span data-stu-id="58594-286">✔</span></span> | <span data-ttu-id="58594-287">✔</span><span class="sxs-lookup"><span data-stu-id="58594-287">✔</span></span>
<span data-ttu-id="58594-288">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="58594-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="58594-289">✔</span><span class="sxs-lookup"><span data-stu-id="58594-289">✔</span></span> | | <span data-ttu-id="58594-290">✔</span><span class="sxs-lookup"><span data-stu-id="58594-290">✔</span></span>
<span data-ttu-id="58594-291">Geometry. Intersection (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="58594-292">✔</span><span class="sxs-lookup"><span data-stu-id="58594-292">✔</span></span> | <span data-ttu-id="58594-293">✔</span><span class="sxs-lookup"><span data-stu-id="58594-293">✔</span></span> | <span data-ttu-id="58594-294">✔</span><span class="sxs-lookup"><span data-stu-id="58594-294">✔</span></span> | <span data-ttu-id="58594-295">✔</span><span class="sxs-lookup"><span data-stu-id="58594-295">✔</span></span>
<span data-ttu-id="58594-296">Geometry. Intersects (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="58594-297">✔</span><span class="sxs-lookup"><span data-stu-id="58594-297">✔</span></span> | <span data-ttu-id="58594-298">✔</span><span class="sxs-lookup"><span data-stu-id="58594-298">✔</span></span> | <span data-ttu-id="58594-299">✔</span><span class="sxs-lookup"><span data-stu-id="58594-299">✔</span></span> | <span data-ttu-id="58594-300">✔</span><span class="sxs-lookup"><span data-stu-id="58594-300">✔</span></span>
<span data-ttu-id="58594-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="58594-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="58594-302">✔</span><span class="sxs-lookup"><span data-stu-id="58594-302">✔</span></span> | <span data-ttu-id="58594-303">✔</span><span class="sxs-lookup"><span data-stu-id="58594-303">✔</span></span> | <span data-ttu-id="58594-304">✔</span><span class="sxs-lookup"><span data-stu-id="58594-304">✔</span></span> | <span data-ttu-id="58594-305">✔</span><span class="sxs-lookup"><span data-stu-id="58594-305">✔</span></span>
<span data-ttu-id="58594-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="58594-306">Geometry.IsSimple</span></span> | <span data-ttu-id="58594-307">✔</span><span class="sxs-lookup"><span data-stu-id="58594-307">✔</span></span> | | <span data-ttu-id="58594-308">✔</span><span class="sxs-lookup"><span data-stu-id="58594-308">✔</span></span> | <span data-ttu-id="58594-309">✔</span><span class="sxs-lookup"><span data-stu-id="58594-309">✔</span></span>
<span data-ttu-id="58594-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="58594-310">Geometry.IsValid</span></span> | <span data-ttu-id="58594-311">✔</span><span class="sxs-lookup"><span data-stu-id="58594-311">✔</span></span> | <span data-ttu-id="58594-312">✔</span><span class="sxs-lookup"><span data-stu-id="58594-312">✔</span></span> | <span data-ttu-id="58594-313">✔</span><span class="sxs-lookup"><span data-stu-id="58594-313">✔</span></span> | <span data-ttu-id="58594-314">✔</span><span class="sxs-lookup"><span data-stu-id="58594-314">✔</span></span>
<span data-ttu-id="58594-315">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="58594-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="58594-316">✔</span><span class="sxs-lookup"><span data-stu-id="58594-316">✔</span></span> | | <span data-ttu-id="58594-317">✔</span><span class="sxs-lookup"><span data-stu-id="58594-317">✔</span></span>
<span data-ttu-id="58594-318">Geometry. Length</span><span class="sxs-lookup"><span data-stu-id="58594-318">Geometry.Length</span></span> | <span data-ttu-id="58594-319">✔</span><span class="sxs-lookup"><span data-stu-id="58594-319">✔</span></span> | <span data-ttu-id="58594-320">✔</span><span class="sxs-lookup"><span data-stu-id="58594-320">✔</span></span> | <span data-ttu-id="58594-321">✔</span><span class="sxs-lookup"><span data-stu-id="58594-321">✔</span></span> | <span data-ttu-id="58594-322">✔</span><span class="sxs-lookup"><span data-stu-id="58594-322">✔</span></span>
<span data-ttu-id="58594-323">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="58594-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="58594-324">✔</span><span class="sxs-lookup"><span data-stu-id="58594-324">✔</span></span> | <span data-ttu-id="58594-325">✔</span><span class="sxs-lookup"><span data-stu-id="58594-325">✔</span></span> | <span data-ttu-id="58594-326">✔</span><span class="sxs-lookup"><span data-stu-id="58594-326">✔</span></span> | <span data-ttu-id="58594-327">✔</span><span class="sxs-lookup"><span data-stu-id="58594-327">✔</span></span>
<span data-ttu-id="58594-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="58594-328">Geometry.NumPoints</span></span> | <span data-ttu-id="58594-329">✔</span><span class="sxs-lookup"><span data-stu-id="58594-329">✔</span></span> | <span data-ttu-id="58594-330">✔</span><span class="sxs-lookup"><span data-stu-id="58594-330">✔</span></span> | <span data-ttu-id="58594-331">✔</span><span class="sxs-lookup"><span data-stu-id="58594-331">✔</span></span> | <span data-ttu-id="58594-332">✔</span><span class="sxs-lookup"><span data-stu-id="58594-332">✔</span></span>
<span data-ttu-id="58594-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="58594-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="58594-334">✔</span><span class="sxs-lookup"><span data-stu-id="58594-334">✔</span></span> | <span data-ttu-id="58594-335">✔</span><span class="sxs-lookup"><span data-stu-id="58594-335">✔</span></span> | <span data-ttu-id="58594-336">✔</span><span class="sxs-lookup"><span data-stu-id="58594-336">✔</span></span>
<span data-ttu-id="58594-337">Geometry. oversobrepõetes (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="58594-338">✔</span><span class="sxs-lookup"><span data-stu-id="58594-338">✔</span></span> | <span data-ttu-id="58594-339">✔</span><span class="sxs-lookup"><span data-stu-id="58594-339">✔</span></span> | <span data-ttu-id="58594-340">✔</span><span class="sxs-lookup"><span data-stu-id="58594-340">✔</span></span> | <span data-ttu-id="58594-341">✔</span><span class="sxs-lookup"><span data-stu-id="58594-341">✔</span></span>
<span data-ttu-id="58594-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="58594-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="58594-343">✔</span><span class="sxs-lookup"><span data-stu-id="58594-343">✔</span></span> | | <span data-ttu-id="58594-344">✔</span><span class="sxs-lookup"><span data-stu-id="58594-344">✔</span></span> | <span data-ttu-id="58594-345">✔</span><span class="sxs-lookup"><span data-stu-id="58594-345">✔</span></span>
<span data-ttu-id="58594-346">Geometry. relacioná (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="58594-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="58594-347">✔</span><span class="sxs-lookup"><span data-stu-id="58594-347">✔</span></span> | | <span data-ttu-id="58594-348">✔</span><span class="sxs-lookup"><span data-stu-id="58594-348">✔</span></span> | <span data-ttu-id="58594-349">✔</span><span class="sxs-lookup"><span data-stu-id="58594-349">✔</span></span>
<span data-ttu-id="58594-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="58594-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="58594-351">✔</span><span class="sxs-lookup"><span data-stu-id="58594-351">✔</span></span> | <span data-ttu-id="58594-352">✔</span><span class="sxs-lookup"><span data-stu-id="58594-352">✔</span></span>
<span data-ttu-id="58594-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="58594-353">Geometry.SRID</span></span> | <span data-ttu-id="58594-354">✔</span><span class="sxs-lookup"><span data-stu-id="58594-354">✔</span></span> | <span data-ttu-id="58594-355">✔</span><span class="sxs-lookup"><span data-stu-id="58594-355">✔</span></span> | <span data-ttu-id="58594-356">✔</span><span class="sxs-lookup"><span data-stu-id="58594-356">✔</span></span> | <span data-ttu-id="58594-357">✔</span><span class="sxs-lookup"><span data-stu-id="58594-357">✔</span></span>
<span data-ttu-id="58594-358">Geometry. SymmetricDifference (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="58594-359">✔</span><span class="sxs-lookup"><span data-stu-id="58594-359">✔</span></span> | <span data-ttu-id="58594-360">✔</span><span class="sxs-lookup"><span data-stu-id="58594-360">✔</span></span> | <span data-ttu-id="58594-361">✔</span><span class="sxs-lookup"><span data-stu-id="58594-361">✔</span></span> | <span data-ttu-id="58594-362">✔</span><span class="sxs-lookup"><span data-stu-id="58594-362">✔</span></span>
<span data-ttu-id="58594-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="58594-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="58594-364">✔</span><span class="sxs-lookup"><span data-stu-id="58594-364">✔</span></span> | <span data-ttu-id="58594-365">✔</span><span class="sxs-lookup"><span data-stu-id="58594-365">✔</span></span> | <span data-ttu-id="58594-366">✔</span><span class="sxs-lookup"><span data-stu-id="58594-366">✔</span></span> | <span data-ttu-id="58594-367">✔</span><span class="sxs-lookup"><span data-stu-id="58594-367">✔</span></span>
<span data-ttu-id="58594-368">Geometry. totext ()</span><span class="sxs-lookup"><span data-stu-id="58594-368">Geometry.ToText()</span></span> | <span data-ttu-id="58594-369">✔</span><span class="sxs-lookup"><span data-stu-id="58594-369">✔</span></span> | <span data-ttu-id="58594-370">✔</span><span class="sxs-lookup"><span data-stu-id="58594-370">✔</span></span> | <span data-ttu-id="58594-371">✔</span><span class="sxs-lookup"><span data-stu-id="58594-371">✔</span></span> | <span data-ttu-id="58594-372">✔</span><span class="sxs-lookup"><span data-stu-id="58594-372">✔</span></span>
<span data-ttu-id="58594-373">Geometry. toques (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="58594-374">✔</span><span class="sxs-lookup"><span data-stu-id="58594-374">✔</span></span> | | <span data-ttu-id="58594-375">✔</span><span class="sxs-lookup"><span data-stu-id="58594-375">✔</span></span> | <span data-ttu-id="58594-376">✔</span><span class="sxs-lookup"><span data-stu-id="58594-376">✔</span></span>
<span data-ttu-id="58594-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="58594-377">Geometry.Union()</span></span> | | | <span data-ttu-id="58594-378">✔</span><span class="sxs-lookup"><span data-stu-id="58594-378">✔</span></span>
<span data-ttu-id="58594-379">Geometry. Union (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="58594-380">✔</span><span class="sxs-lookup"><span data-stu-id="58594-380">✔</span></span> | <span data-ttu-id="58594-381">✔</span><span class="sxs-lookup"><span data-stu-id="58594-381">✔</span></span> | <span data-ttu-id="58594-382">✔</span><span class="sxs-lookup"><span data-stu-id="58594-382">✔</span></span> | <span data-ttu-id="58594-383">✔</span><span class="sxs-lookup"><span data-stu-id="58594-383">✔</span></span>
<span data-ttu-id="58594-384">Geometry. Within (geometry)</span><span class="sxs-lookup"><span data-stu-id="58594-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="58594-385">✔</span><span class="sxs-lookup"><span data-stu-id="58594-385">✔</span></span> | <span data-ttu-id="58594-386">✔</span><span class="sxs-lookup"><span data-stu-id="58594-386">✔</span></span> | <span data-ttu-id="58594-387">✔</span><span class="sxs-lookup"><span data-stu-id="58594-387">✔</span></span> | <span data-ttu-id="58594-388">✔</span><span class="sxs-lookup"><span data-stu-id="58594-388">✔</span></span>
<span data-ttu-id="58594-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="58594-389">GeometryCollection.Count</span></span> | <span data-ttu-id="58594-390">✔</span><span class="sxs-lookup"><span data-stu-id="58594-390">✔</span></span> | <span data-ttu-id="58594-391">✔</span><span class="sxs-lookup"><span data-stu-id="58594-391">✔</span></span> | <span data-ttu-id="58594-392">✔</span><span class="sxs-lookup"><span data-stu-id="58594-392">✔</span></span> | <span data-ttu-id="58594-393">✔</span><span class="sxs-lookup"><span data-stu-id="58594-393">✔</span></span>
<span data-ttu-id="58594-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="58594-394">GeometryCollection[int]</span></span> | <span data-ttu-id="58594-395">✔</span><span class="sxs-lookup"><span data-stu-id="58594-395">✔</span></span> | <span data-ttu-id="58594-396">✔</span><span class="sxs-lookup"><span data-stu-id="58594-396">✔</span></span> | <span data-ttu-id="58594-397">✔</span><span class="sxs-lookup"><span data-stu-id="58594-397">✔</span></span> | <span data-ttu-id="58594-398">✔</span><span class="sxs-lookup"><span data-stu-id="58594-398">✔</span></span>
<span data-ttu-id="58594-399">Linhastring. contagem</span><span class="sxs-lookup"><span data-stu-id="58594-399">LineString.Count</span></span> | <span data-ttu-id="58594-400">✔</span><span class="sxs-lookup"><span data-stu-id="58594-400">✔</span></span> | <span data-ttu-id="58594-401">✔</span><span class="sxs-lookup"><span data-stu-id="58594-401">✔</span></span> | <span data-ttu-id="58594-402">✔</span><span class="sxs-lookup"><span data-stu-id="58594-402">✔</span></span> | <span data-ttu-id="58594-403">✔</span><span class="sxs-lookup"><span data-stu-id="58594-403">✔</span></span>
<span data-ttu-id="58594-404">LineString. ponto de extremidade</span><span class="sxs-lookup"><span data-stu-id="58594-404">LineString.EndPoint</span></span> | <span data-ttu-id="58594-405">✔</span><span class="sxs-lookup"><span data-stu-id="58594-405">✔</span></span> | <span data-ttu-id="58594-406">✔</span><span class="sxs-lookup"><span data-stu-id="58594-406">✔</span></span> | <span data-ttu-id="58594-407">✔</span><span class="sxs-lookup"><span data-stu-id="58594-407">✔</span></span> | <span data-ttu-id="58594-408">✔</span><span class="sxs-lookup"><span data-stu-id="58594-408">✔</span></span>
<span data-ttu-id="58594-409">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="58594-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="58594-410">✔</span><span class="sxs-lookup"><span data-stu-id="58594-410">✔</span></span> | <span data-ttu-id="58594-411">✔</span><span class="sxs-lookup"><span data-stu-id="58594-411">✔</span></span> | <span data-ttu-id="58594-412">✔</span><span class="sxs-lookup"><span data-stu-id="58594-412">✔</span></span> | <span data-ttu-id="58594-413">✔</span><span class="sxs-lookup"><span data-stu-id="58594-413">✔</span></span>
<span data-ttu-id="58594-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="58594-414">LineString.IsClosed</span></span> | <span data-ttu-id="58594-415">✔</span><span class="sxs-lookup"><span data-stu-id="58594-415">✔</span></span> | <span data-ttu-id="58594-416">✔</span><span class="sxs-lookup"><span data-stu-id="58594-416">✔</span></span> | <span data-ttu-id="58594-417">✔</span><span class="sxs-lookup"><span data-stu-id="58594-417">✔</span></span> | <span data-ttu-id="58594-418">✔</span><span class="sxs-lookup"><span data-stu-id="58594-418">✔</span></span>
<span data-ttu-id="58594-419">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="58594-419">LineString.IsRing</span></span> | <span data-ttu-id="58594-420">✔</span><span class="sxs-lookup"><span data-stu-id="58594-420">✔</span></span> | | <span data-ttu-id="58594-421">✔</span><span class="sxs-lookup"><span data-stu-id="58594-421">✔</span></span> | <span data-ttu-id="58594-422">✔</span><span class="sxs-lookup"><span data-stu-id="58594-422">✔</span></span>
<span data-ttu-id="58594-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="58594-423">LineString.StartPoint</span></span> | <span data-ttu-id="58594-424">✔</span><span class="sxs-lookup"><span data-stu-id="58594-424">✔</span></span> | <span data-ttu-id="58594-425">✔</span><span class="sxs-lookup"><span data-stu-id="58594-425">✔</span></span> | <span data-ttu-id="58594-426">✔</span><span class="sxs-lookup"><span data-stu-id="58594-426">✔</span></span> | <span data-ttu-id="58594-427">✔</span><span class="sxs-lookup"><span data-stu-id="58594-427">✔</span></span>
<span data-ttu-id="58594-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="58594-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="58594-429">✔</span><span class="sxs-lookup"><span data-stu-id="58594-429">✔</span></span> | <span data-ttu-id="58594-430">✔</span><span class="sxs-lookup"><span data-stu-id="58594-430">✔</span></span> | <span data-ttu-id="58594-431">✔</span><span class="sxs-lookup"><span data-stu-id="58594-431">✔</span></span> | <span data-ttu-id="58594-432">✔</span><span class="sxs-lookup"><span data-stu-id="58594-432">✔</span></span>
<span data-ttu-id="58594-433">Ponto. M</span><span class="sxs-lookup"><span data-stu-id="58594-433">Point.M</span></span> | <span data-ttu-id="58594-434">✔</span><span class="sxs-lookup"><span data-stu-id="58594-434">✔</span></span> | <span data-ttu-id="58594-435">✔</span><span class="sxs-lookup"><span data-stu-id="58594-435">✔</span></span> | <span data-ttu-id="58594-436">✔</span><span class="sxs-lookup"><span data-stu-id="58594-436">✔</span></span> | <span data-ttu-id="58594-437">✔</span><span class="sxs-lookup"><span data-stu-id="58594-437">✔</span></span>
<span data-ttu-id="58594-438">Ponto. X</span><span class="sxs-lookup"><span data-stu-id="58594-438">Point.X</span></span> | <span data-ttu-id="58594-439">✔</span><span class="sxs-lookup"><span data-stu-id="58594-439">✔</span></span> | <span data-ttu-id="58594-440">✔</span><span class="sxs-lookup"><span data-stu-id="58594-440">✔</span></span> | <span data-ttu-id="58594-441">✔</span><span class="sxs-lookup"><span data-stu-id="58594-441">✔</span></span> | <span data-ttu-id="58594-442">✔</span><span class="sxs-lookup"><span data-stu-id="58594-442">✔</span></span>
<span data-ttu-id="58594-443">Ponto. Y</span><span class="sxs-lookup"><span data-stu-id="58594-443">Point.Y</span></span> | <span data-ttu-id="58594-444">✔</span><span class="sxs-lookup"><span data-stu-id="58594-444">✔</span></span> | <span data-ttu-id="58594-445">✔</span><span class="sxs-lookup"><span data-stu-id="58594-445">✔</span></span> | <span data-ttu-id="58594-446">✔</span><span class="sxs-lookup"><span data-stu-id="58594-446">✔</span></span> | <span data-ttu-id="58594-447">✔</span><span class="sxs-lookup"><span data-stu-id="58594-447">✔</span></span>
<span data-ttu-id="58594-448">Ponto. Z</span><span class="sxs-lookup"><span data-stu-id="58594-448">Point.Z</span></span> | <span data-ttu-id="58594-449">✔</span><span class="sxs-lookup"><span data-stu-id="58594-449">✔</span></span> | <span data-ttu-id="58594-450">✔</span><span class="sxs-lookup"><span data-stu-id="58594-450">✔</span></span> | <span data-ttu-id="58594-451">✔</span><span class="sxs-lookup"><span data-stu-id="58594-451">✔</span></span> | <span data-ttu-id="58594-452">✔</span><span class="sxs-lookup"><span data-stu-id="58594-452">✔</span></span>
<span data-ttu-id="58594-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="58594-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="58594-454">✔</span><span class="sxs-lookup"><span data-stu-id="58594-454">✔</span></span> | <span data-ttu-id="58594-455">✔</span><span class="sxs-lookup"><span data-stu-id="58594-455">✔</span></span> | <span data-ttu-id="58594-456">✔</span><span class="sxs-lookup"><span data-stu-id="58594-456">✔</span></span> | <span data-ttu-id="58594-457">✔</span><span class="sxs-lookup"><span data-stu-id="58594-457">✔</span></span>
<span data-ttu-id="58594-458">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="58594-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="58594-459">✔</span><span class="sxs-lookup"><span data-stu-id="58594-459">✔</span></span> | <span data-ttu-id="58594-460">✔</span><span class="sxs-lookup"><span data-stu-id="58594-460">✔</span></span> | <span data-ttu-id="58594-461">✔</span><span class="sxs-lookup"><span data-stu-id="58594-461">✔</span></span> | <span data-ttu-id="58594-462">✔</span><span class="sxs-lookup"><span data-stu-id="58594-462">✔</span></span>
<span data-ttu-id="58594-463">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="58594-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="58594-464">✔</span><span class="sxs-lookup"><span data-stu-id="58594-464">✔</span></span> | <span data-ttu-id="58594-465">✔</span><span class="sxs-lookup"><span data-stu-id="58594-465">✔</span></span> | <span data-ttu-id="58594-466">✔</span><span class="sxs-lookup"><span data-stu-id="58594-466">✔</span></span> | <span data-ttu-id="58594-467">✔</span><span class="sxs-lookup"><span data-stu-id="58594-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58594-468">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="58594-468">Additional resources</span></span>

* [<span data-ttu-id="58594-469">Dados espaciais em SQL Server</span><span class="sxs-lookup"><span data-stu-id="58594-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="58594-470">Home Page do SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="58594-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="58594-471">Documentação espacial do Npgsql</span><span class="sxs-lookup"><span data-stu-id="58594-471">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="58594-472">Documentação do PostGIS</span><span class="sxs-lookup"><span data-stu-id="58594-472">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
