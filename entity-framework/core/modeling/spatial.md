---
title: 'Datos espaciales: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 026df735473e31f1c1463c1fbc6f46c4fd6dfd4f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921732"
---
# <a name="spatial-data"></a><span data-ttu-id="d3684-102">Datos espaciales</span><span class="sxs-lookup"><span data-stu-id="d3684-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="d3684-103">Esta característica se agregó en EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="d3684-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="d3684-104">Los datos espaciales representan la ubicación física y la forma de los objetos.</span><span class="sxs-lookup"><span data-stu-id="d3684-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="d3684-105">Muchas bases de datos proporcionan compatibilidad con este tipo de datos, por lo que se puede indizar y consultar junto con otros datos.</span><span class="sxs-lookup"><span data-stu-id="d3684-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="d3684-106">Entre los escenarios comunes se incluyen las consultas de objetos dentro de una distancia determinada desde una ubicación o la selección del objeto cuyo borde contiene una ubicación determinada.</span><span class="sxs-lookup"><span data-stu-id="d3684-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="d3684-107">EF Core admite la asignación a tipos de datos espaciales mediante la biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="d3684-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="d3684-108">Instalación</span><span class="sxs-lookup"><span data-stu-id="d3684-108">Installing</span></span>

<span data-ttu-id="d3684-109">Para usar los datos espaciales con EF Core, debe instalar el paquete NuGet de soporte adecuado.</span><span class="sxs-lookup"><span data-stu-id="d3684-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="d3684-110">El paquete que necesita instalar depende del proveedor que esté usando.</span><span class="sxs-lookup"><span data-stu-id="d3684-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="d3684-111">Proveedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="d3684-111">EF Core Provider</span></span>                        | <span data-ttu-id="d3684-112">Paquete de NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="d3684-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="d3684-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d3684-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="d3684-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d3684-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="d3684-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="d3684-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="d3684-116">Microsoft. EntityFrameworkCore. SQLite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d3684-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="d3684-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="d3684-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="d3684-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d3684-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="d3684-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d3684-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="d3684-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d3684-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="d3684-121">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="d3684-121">Reverse engineering</span></span>

<span data-ttu-id="d3684-122">Los paquetes de NuGet espaciales también habilitan los modelos de [ingeniería inversa](../managing-schemas/scaffolding.md) con propiedades espaciales, pero debe instalar `Scaffold-DbContext` el `dotnet ef dbcontext scaffold`paquete ***antes*** de ejecutar o.</span><span class="sxs-lookup"><span data-stu-id="d3684-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="d3684-123">Si no lo hace, recibirá advertencias sobre cómo no encontrar las asignaciones de tipos para las columnas y se omitirán las columnas.</span><span class="sxs-lookup"><span data-stu-id="d3684-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="d3684-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="d3684-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="d3684-125">NetTopologySuite es una biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="d3684-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="d3684-126">EF Core permite la asignación a tipos de datos espaciales en la base de datos mediante el uso de tipos NTS en el modelo.</span><span class="sxs-lookup"><span data-stu-id="d3684-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="d3684-127">Para habilitar la asignación a tipos espaciales a través de NTS, llame al método UseNetTopologySuite en el generador de opciones DbContext del proveedor.</span><span class="sxs-lookup"><span data-stu-id="d3684-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="d3684-128">Por ejemplo, con SQL Server le llamaría como esto.</span><span class="sxs-lookup"><span data-stu-id="d3684-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="d3684-129">Hay varios tipos de datos espaciales.</span><span class="sxs-lookup"><span data-stu-id="d3684-129">There are several spatial data types.</span></span> <span data-ttu-id="d3684-130">El tipo que use dependerá de los tipos de formas que desee permitir.</span><span class="sxs-lookup"><span data-stu-id="d3684-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="d3684-131">Esta es la jerarquía de tipos NTS que puede usar para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="d3684-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="d3684-132">Están ubicados en el `NetTopologySuite.Geometries` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="d3684-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="d3684-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="d3684-133">Geometry</span></span>
  * <span data-ttu-id="d3684-134">Punto</span><span class="sxs-lookup"><span data-stu-id="d3684-134">Point</span></span>
  * <span data-ttu-id="d3684-135">LineString</span><span class="sxs-lookup"><span data-stu-id="d3684-135">LineString</span></span>
  * <span data-ttu-id="d3684-136">Polígono</span><span class="sxs-lookup"><span data-stu-id="d3684-136">Polygon</span></span>
  * <span data-ttu-id="d3684-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="d3684-137">GeometryCollection</span></span>
    * <span data-ttu-id="d3684-138">Point</span><span class="sxs-lookup"><span data-stu-id="d3684-138">MultiPoint</span></span>
    * <span data-ttu-id="d3684-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="d3684-139">MultiLineString</span></span>
    * <span data-ttu-id="d3684-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="d3684-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="d3684-141">La CircularString, CompoundCurve y CurePolygon no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="d3684-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="d3684-142">El uso del tipo de geometría base permite que la propiedad especifique cualquier tipo de forma.</span><span class="sxs-lookup"><span data-stu-id="d3684-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="d3684-143">Las siguientes clases de entidad se pueden usar para asignar tablas en la [base de datos de ejemplo Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="d3684-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="d3684-144">Crear valores</span><span class="sxs-lookup"><span data-stu-id="d3684-144">Creating values</span></span>

<span data-ttu-id="d3684-145">Puede usar constructores para crear objetos Geometry; sin embargo, NTS recomienda el uso de un generador de geometría en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d3684-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="d3684-146">Esto le permite especificar un valor predeterminado de SRID (el sistema de referencia espacial que usan las coordenadas) y le proporciona el control sobre aspectos más avanzados, como el modelo de precisión (usado durante los cálculos) y la secuencia de coordenadas (determina las coordenadas: dimensiones). y las medidas--están disponibles).</span><span class="sxs-lookup"><span data-stu-id="d3684-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="d3684-147">4326 hace referencia a WGS 84, un estándar que se usa en GPS y en otros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="d3684-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="d3684-148">Longitud y latitud</span><span class="sxs-lookup"><span data-stu-id="d3684-148">Longitude and Latitude</span></span>

<span data-ttu-id="d3684-149">Las coordenadas en NTS están en términos de valores X e y.</span><span class="sxs-lookup"><span data-stu-id="d3684-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="d3684-150">Para representar la longitud y la latitud, use X para longitud e y para latitud.</span><span class="sxs-lookup"><span data-stu-id="d3684-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="d3684-151">Tenga en cuenta que esto es **hacia atrás** desde el formato en el `latitude, longitude` que normalmente se ven estos valores.</span><span class="sxs-lookup"><span data-stu-id="d3684-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="d3684-152">SRID omitido durante las operaciones de cliente</span><span class="sxs-lookup"><span data-stu-id="d3684-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="d3684-153">NTS omite los valores de SRID durante las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d3684-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="d3684-154">Supone un sistema de coordenadas plano.</span><span class="sxs-lookup"><span data-stu-id="d3684-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="d3684-155">Esto significa que si especifica coordenadas en términos de longitud y latitud, algunos valores evaluados por el cliente como la distancia, la longitud y el área estarán en grados, no en metros.</span><span class="sxs-lookup"><span data-stu-id="d3684-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="d3684-156">Para valores más significativos, primero debe proyectar las coordenadas en otro sistema de coordenadas mediante una biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular estos valores.</span><span class="sxs-lookup"><span data-stu-id="d3684-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="d3684-157">Si una operación es evaluada por el servidor mediante EF Core a través de SQL, la unidad del resultado se determinará por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3684-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="d3684-158">Este es un ejemplo del uso de ProjNet4GeoAPI para calcular la distancia entre dos ciudades.</span><span class="sxs-lookup"><span data-stu-id="d3684-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="d3684-159">Consulta de datos</span><span class="sxs-lookup"><span data-stu-id="d3684-159">Querying Data</span></span>

<span data-ttu-id="d3684-160">En LINQ, los métodos y las propiedades NTS disponibles como funciones de base de datos se traducirán a SQL.</span><span class="sxs-lookup"><span data-stu-id="d3684-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="d3684-161">Por ejemplo, los métodos Distance y Contains se traducen en las siguientes consultas.</span><span class="sxs-lookup"><span data-stu-id="d3684-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="d3684-162">En la tabla al final de este artículo se muestran los miembros que son compatibles con varios proveedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="d3684-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="d3684-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d3684-163">SQL Server</span></span>

<span data-ttu-id="d3684-164">Si usa SQL Server, hay algunos aspectos adicionales que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="d3684-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="d3684-165">Geografía o geometría</span><span class="sxs-lookup"><span data-stu-id="d3684-165">Geography or geometry</span></span>

<span data-ttu-id="d3684-166">De forma predeterminada, las propiedades espaciales `geography` se asignan a las columnas de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d3684-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="d3684-167">Para usar `geometry`, [Configure el tipo de columna](xref:core/modeling/relational/data-types) en el modelo.</span><span class="sxs-lookup"><span data-stu-id="d3684-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="d3684-168">Anillos de polígono de geografía</span><span class="sxs-lookup"><span data-stu-id="d3684-168">Geography polygon rings</span></span>

<span data-ttu-id="d3684-169">Al usar el `geography` tipo de columna, SQL Server impone requisitos adicionales en el anillo exterior (o shell) y los anillos interiores (o agujeros).</span><span class="sxs-lookup"><span data-stu-id="d3684-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="d3684-170">El anillo exterior debe estar orientado en sentido contrario a las agujas del reloj y los anillos interiores hacia la derecha.</span><span class="sxs-lookup"><span data-stu-id="d3684-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="d3684-171">NTS valida esto antes de enviar los valores a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3684-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="d3684-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="d3684-172">FullGlobe</span></span>

<span data-ttu-id="d3684-173">SQL Server tiene un tipo de geometría no estándar para representar todo el globo terráqueo cuando se `geography` usa el tipo de columna.</span><span class="sxs-lookup"><span data-stu-id="d3684-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="d3684-174">También tiene una forma de representar polígonos basados en el globo completo (sin un anillo exterior).</span><span class="sxs-lookup"><span data-stu-id="d3684-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="d3684-175">Ninguno de ellos es compatible con NTS.</span><span class="sxs-lookup"><span data-stu-id="d3684-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="d3684-176">Los FullGlobe y los polígonos basados en ellos no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="d3684-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="d3684-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="d3684-177">SQLite</span></span>

<span data-ttu-id="d3684-178">A continuación se muestra información adicional para los usuarios que usan SQLite.</span><span class="sxs-lookup"><span data-stu-id="d3684-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="d3684-179">Instalación de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="d3684-179">Installing SpatiaLite</span></span>

<span data-ttu-id="d3684-180">En Windows, la biblioteca nativa de mod_spatialite se distribuye como una dependencia del paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3684-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="d3684-181">Otras plataformas deben instalarse por separado.</span><span class="sxs-lookup"><span data-stu-id="d3684-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="d3684-182">Esto se suele hacer mediante un administrador de paquetes de software.</span><span class="sxs-lookup"><span data-stu-id="d3684-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="d3684-183">Por ejemplo, puede usar APT en Ubuntu y homebrew en MacOS.</span><span class="sxs-lookup"><span data-stu-id="d3684-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="d3684-184">Configuración de SRID</span><span class="sxs-lookup"><span data-stu-id="d3684-184">Configuring SRID</span></span>

<span data-ttu-id="d3684-185">En SpatiaLite, las columnas deben especificar un SRID por columna.</span><span class="sxs-lookup"><span data-stu-id="d3684-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="d3684-186">El valor predeterminado de `0`SRID es.</span><span class="sxs-lookup"><span data-stu-id="d3684-186">The default SRID is `0`.</span></span> <span data-ttu-id="d3684-187">Especifique otro SRID con el método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="d3684-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="d3684-188">Dimensión</span><span class="sxs-lookup"><span data-stu-id="d3684-188">Dimension</span></span>

<span data-ttu-id="d3684-189">De forma similar a SRID, la dimensión de una columna (o las ordenadas) también se especifica como parte de la columna.</span><span class="sxs-lookup"><span data-stu-id="d3684-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="d3684-190">Las ordenadas predeterminadas son X e y. Habilite las ordenadas adicionales (Z y M) mediante el método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="d3684-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="d3684-191">Operaciones traducidas</span><span class="sxs-lookup"><span data-stu-id="d3684-191">Translated Operations</span></span>

<span data-ttu-id="d3684-192">En esta tabla se muestran los miembros de NTS que cada proveedor de EF Core traduce en SQL.</span><span class="sxs-lookup"><span data-stu-id="d3684-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="d3684-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="d3684-193">NetTopologySuite</span></span> | <span data-ttu-id="d3684-194">SQL Server (geometría)</span><span class="sxs-lookup"><span data-stu-id="d3684-194">SQL Server (geometry)</span></span> | <span data-ttu-id="d3684-195">SQL Server (Geografía)</span><span class="sxs-lookup"><span data-stu-id="d3684-195">SQL Server (geography)</span></span> | <span data-ttu-id="d3684-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="d3684-196">SQLite</span></span> | <span data-ttu-id="d3684-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="d3684-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="d3684-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="d3684-198">Geometry.Area</span></span> | <span data-ttu-id="d3684-199">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-199">✔</span></span> | <span data-ttu-id="d3684-200">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-200">✔</span></span> | <span data-ttu-id="d3684-201">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-201">✔</span></span> | <span data-ttu-id="d3684-202">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-202">✔</span></span>
<span data-ttu-id="d3684-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="d3684-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="d3684-204">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-204">✔</span></span> | <span data-ttu-id="d3684-205">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-205">✔</span></span> | <span data-ttu-id="d3684-206">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-206">✔</span></span> | <span data-ttu-id="d3684-207">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-207">✔</span></span>
<span data-ttu-id="d3684-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="d3684-208">Geometry.AsText()</span></span> | <span data-ttu-id="d3684-209">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-209">✔</span></span> | <span data-ttu-id="d3684-210">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-210">✔</span></span> | <span data-ttu-id="d3684-211">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-211">✔</span></span> | <span data-ttu-id="d3684-212">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-212">✔</span></span>
<span data-ttu-id="d3684-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="d3684-213">Geometry.Boundary</span></span> | <span data-ttu-id="d3684-214">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-214">✔</span></span> | | <span data-ttu-id="d3684-215">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-215">✔</span></span> | <span data-ttu-id="d3684-216">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-216">✔</span></span>
<span data-ttu-id="d3684-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="d3684-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="d3684-218">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-218">✔</span></span> | <span data-ttu-id="d3684-219">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-219">✔</span></span> | <span data-ttu-id="d3684-220">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-220">✔</span></span> | <span data-ttu-id="d3684-221">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-221">✔</span></span>
<span data-ttu-id="d3684-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="d3684-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="d3684-223">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-223">✔</span></span>
<span data-ttu-id="d3684-224">Geometry. centroide</span><span class="sxs-lookup"><span data-stu-id="d3684-224">Geometry.Centroid</span></span> | <span data-ttu-id="d3684-225">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-225">✔</span></span> | | <span data-ttu-id="d3684-226">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-226">✔</span></span> | <span data-ttu-id="d3684-227">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-227">✔</span></span>
<span data-ttu-id="d3684-228">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="d3684-229">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-229">✔</span></span> | <span data-ttu-id="d3684-230">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-230">✔</span></span> | <span data-ttu-id="d3684-231">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-231">✔</span></span> | <span data-ttu-id="d3684-232">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-232">✔</span></span>
<span data-ttu-id="d3684-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="d3684-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="d3684-234">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-234">✔</span></span> | <span data-ttu-id="d3684-235">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-235">✔</span></span> | <span data-ttu-id="d3684-236">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-236">✔</span></span> | <span data-ttu-id="d3684-237">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-237">✔</span></span>
<span data-ttu-id="d3684-238">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="d3684-239">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-239">✔</span></span> | <span data-ttu-id="d3684-240">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-240">✔</span></span>
<span data-ttu-id="d3684-241">Geometry. cubiertas (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="d3684-242">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-242">✔</span></span> | <span data-ttu-id="d3684-243">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-243">✔</span></span>
<span data-ttu-id="d3684-244">Geometry. Crosses (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="d3684-245">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-245">✔</span></span> | | <span data-ttu-id="d3684-246">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-246">✔</span></span> | <span data-ttu-id="d3684-247">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-247">✔</span></span>
<span data-ttu-id="d3684-248">Geometry. Difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="d3684-249">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-249">✔</span></span> | <span data-ttu-id="d3684-250">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-250">✔</span></span> | <span data-ttu-id="d3684-251">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-251">✔</span></span> | <span data-ttu-id="d3684-252">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-252">✔</span></span>
<span data-ttu-id="d3684-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="d3684-253">Geometry.Dimension</span></span> | <span data-ttu-id="d3684-254">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-254">✔</span></span> | <span data-ttu-id="d3684-255">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-255">✔</span></span> | <span data-ttu-id="d3684-256">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-256">✔</span></span> | <span data-ttu-id="d3684-257">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-257">✔</span></span>
<span data-ttu-id="d3684-258">Geometry. disunion (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="d3684-259">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-259">✔</span></span> | <span data-ttu-id="d3684-260">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-260">✔</span></span> | <span data-ttu-id="d3684-261">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-261">✔</span></span> | <span data-ttu-id="d3684-262">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-262">✔</span></span>
<span data-ttu-id="d3684-263">Geometry. Distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="d3684-264">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-264">✔</span></span> | <span data-ttu-id="d3684-265">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-265">✔</span></span> | <span data-ttu-id="d3684-266">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-266">✔</span></span> | <span data-ttu-id="d3684-267">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-267">✔</span></span>
<span data-ttu-id="d3684-268">Geometría. sobre</span><span class="sxs-lookup"><span data-stu-id="d3684-268">Geometry.Envelope</span></span> | <span data-ttu-id="d3684-269">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-269">✔</span></span> | | <span data-ttu-id="d3684-270">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-270">✔</span></span> | <span data-ttu-id="d3684-271">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-271">✔</span></span>
<span data-ttu-id="d3684-272">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="d3684-273">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-273">✔</span></span>
<span data-ttu-id="d3684-274">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="d3684-275">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-275">✔</span></span> | <span data-ttu-id="d3684-276">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-276">✔</span></span> | <span data-ttu-id="d3684-277">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-277">✔</span></span> | <span data-ttu-id="d3684-278">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-278">✔</span></span>
<span data-ttu-id="d3684-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="d3684-279">Geometry.GeometryType</span></span> | <span data-ttu-id="d3684-280">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-280">✔</span></span> | <span data-ttu-id="d3684-281">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-281">✔</span></span> | <span data-ttu-id="d3684-282">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-282">✔</span></span> | <span data-ttu-id="d3684-283">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-283">✔</span></span>
<span data-ttu-id="d3684-284">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="d3684-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="d3684-285">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-285">✔</span></span> | | <span data-ttu-id="d3684-286">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-286">✔</span></span> | <span data-ttu-id="d3684-287">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-287">✔</span></span>
<span data-ttu-id="d3684-288">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="d3684-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="d3684-289">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-289">✔</span></span> | | <span data-ttu-id="d3684-290">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-290">✔</span></span>
<span data-ttu-id="d3684-291">Geometry. Intersection (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="d3684-292">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-292">✔</span></span> | <span data-ttu-id="d3684-293">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-293">✔</span></span> | <span data-ttu-id="d3684-294">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-294">✔</span></span> | <span data-ttu-id="d3684-295">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-295">✔</span></span>
<span data-ttu-id="d3684-296">Geometry. Intersects (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="d3684-297">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-297">✔</span></span> | <span data-ttu-id="d3684-298">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-298">✔</span></span> | <span data-ttu-id="d3684-299">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-299">✔</span></span> | <span data-ttu-id="d3684-300">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-300">✔</span></span>
<span data-ttu-id="d3684-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="d3684-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="d3684-302">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-302">✔</span></span> | <span data-ttu-id="d3684-303">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-303">✔</span></span> | <span data-ttu-id="d3684-304">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-304">✔</span></span> | <span data-ttu-id="d3684-305">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-305">✔</span></span>
<span data-ttu-id="d3684-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="d3684-306">Geometry.IsSimple</span></span> | <span data-ttu-id="d3684-307">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-307">✔</span></span> | | <span data-ttu-id="d3684-308">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-308">✔</span></span> | <span data-ttu-id="d3684-309">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-309">✔</span></span>
<span data-ttu-id="d3684-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="d3684-310">Geometry.IsValid</span></span> | <span data-ttu-id="d3684-311">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-311">✔</span></span> | <span data-ttu-id="d3684-312">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-312">✔</span></span> | <span data-ttu-id="d3684-313">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-313">✔</span></span> | <span data-ttu-id="d3684-314">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-314">✔</span></span>
<span data-ttu-id="d3684-315">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="d3684-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="d3684-316">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-316">✔</span></span> | | <span data-ttu-id="d3684-317">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-317">✔</span></span>
<span data-ttu-id="d3684-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="d3684-318">Geometry.Length</span></span> | <span data-ttu-id="d3684-319">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-319">✔</span></span> | <span data-ttu-id="d3684-320">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-320">✔</span></span> | <span data-ttu-id="d3684-321">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-321">✔</span></span> | <span data-ttu-id="d3684-322">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-322">✔</span></span>
<span data-ttu-id="d3684-323">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="d3684-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="d3684-324">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-324">✔</span></span> | <span data-ttu-id="d3684-325">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-325">✔</span></span> | <span data-ttu-id="d3684-326">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-326">✔</span></span> | <span data-ttu-id="d3684-327">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-327">✔</span></span>
<span data-ttu-id="d3684-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="d3684-328">Geometry.NumPoints</span></span> | <span data-ttu-id="d3684-329">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-329">✔</span></span> | <span data-ttu-id="d3684-330">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-330">✔</span></span> | <span data-ttu-id="d3684-331">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-331">✔</span></span> | <span data-ttu-id="d3684-332">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-332">✔</span></span>
<span data-ttu-id="d3684-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="d3684-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="d3684-334">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-334">✔</span></span> | <span data-ttu-id="d3684-335">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-335">✔</span></span> | <span data-ttu-id="d3684-336">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-336">✔</span></span>
<span data-ttu-id="d3684-337">Geometry. superpone (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="d3684-338">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-338">✔</span></span> | <span data-ttu-id="d3684-339">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-339">✔</span></span> | <span data-ttu-id="d3684-340">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-340">✔</span></span> | <span data-ttu-id="d3684-341">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-341">✔</span></span>
<span data-ttu-id="d3684-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="d3684-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="d3684-343">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-343">✔</span></span> | | <span data-ttu-id="d3684-344">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-344">✔</span></span> | <span data-ttu-id="d3684-345">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-345">✔</span></span>
<span data-ttu-id="d3684-346">Geometry. Relate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="d3684-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="d3684-347">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-347">✔</span></span> | | <span data-ttu-id="d3684-348">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-348">✔</span></span> | <span data-ttu-id="d3684-349">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-349">✔</span></span>
<span data-ttu-id="d3684-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="d3684-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="d3684-351">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-351">✔</span></span> | <span data-ttu-id="d3684-352">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-352">✔</span></span>
<span data-ttu-id="d3684-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="d3684-353">Geometry.SRID</span></span> | <span data-ttu-id="d3684-354">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-354">✔</span></span> | <span data-ttu-id="d3684-355">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-355">✔</span></span> | <span data-ttu-id="d3684-356">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-356">✔</span></span> | <span data-ttu-id="d3684-357">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-357">✔</span></span>
<span data-ttu-id="d3684-358">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="d3684-359">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-359">✔</span></span> | <span data-ttu-id="d3684-360">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-360">✔</span></span> | <span data-ttu-id="d3684-361">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-361">✔</span></span> | <span data-ttu-id="d3684-362">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-362">✔</span></span>
<span data-ttu-id="d3684-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="d3684-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="d3684-364">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-364">✔</span></span> | <span data-ttu-id="d3684-365">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-365">✔</span></span> | <span data-ttu-id="d3684-366">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-366">✔</span></span> | <span data-ttu-id="d3684-367">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-367">✔</span></span>
<span data-ttu-id="d3684-368">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="d3684-368">Geometry.ToText()</span></span> | <span data-ttu-id="d3684-369">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-369">✔</span></span> | <span data-ttu-id="d3684-370">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-370">✔</span></span> | <span data-ttu-id="d3684-371">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-371">✔</span></span> | <span data-ttu-id="d3684-372">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-372">✔</span></span>
<span data-ttu-id="d3684-373">Geometry. toques (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="d3684-374">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-374">✔</span></span> | | <span data-ttu-id="d3684-375">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-375">✔</span></span> | <span data-ttu-id="d3684-376">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-376">✔</span></span>
<span data-ttu-id="d3684-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="d3684-377">Geometry.Union()</span></span> | | | <span data-ttu-id="d3684-378">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-378">✔</span></span>
<span data-ttu-id="d3684-379">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="d3684-380">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-380">✔</span></span> | <span data-ttu-id="d3684-381">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-381">✔</span></span> | <span data-ttu-id="d3684-382">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-382">✔</span></span> | <span data-ttu-id="d3684-383">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-383">✔</span></span>
<span data-ttu-id="d3684-384">Geometry. Within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="d3684-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="d3684-385">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-385">✔</span></span> | <span data-ttu-id="d3684-386">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-386">✔</span></span> | <span data-ttu-id="d3684-387">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-387">✔</span></span> | <span data-ttu-id="d3684-388">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-388">✔</span></span>
<span data-ttu-id="d3684-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="d3684-389">GeometryCollection.Count</span></span> | <span data-ttu-id="d3684-390">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-390">✔</span></span> | <span data-ttu-id="d3684-391">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-391">✔</span></span> | <span data-ttu-id="d3684-392">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-392">✔</span></span> | <span data-ttu-id="d3684-393">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-393">✔</span></span>
<span data-ttu-id="d3684-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="d3684-394">GeometryCollection[int]</span></span> | <span data-ttu-id="d3684-395">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-395">✔</span></span> | <span data-ttu-id="d3684-396">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-396">✔</span></span> | <span data-ttu-id="d3684-397">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-397">✔</span></span> | <span data-ttu-id="d3684-398">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-398">✔</span></span>
<span data-ttu-id="d3684-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="d3684-399">LineString.Count</span></span> | <span data-ttu-id="d3684-400">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-400">✔</span></span> | <span data-ttu-id="d3684-401">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-401">✔</span></span> | <span data-ttu-id="d3684-402">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-402">✔</span></span> | <span data-ttu-id="d3684-403">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-403">✔</span></span>
<span data-ttu-id="d3684-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="d3684-404">LineString.EndPoint</span></span> | <span data-ttu-id="d3684-405">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-405">✔</span></span> | <span data-ttu-id="d3684-406">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-406">✔</span></span> | <span data-ttu-id="d3684-407">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-407">✔</span></span> | <span data-ttu-id="d3684-408">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-408">✔</span></span>
<span data-ttu-id="d3684-409">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="d3684-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="d3684-410">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-410">✔</span></span> | <span data-ttu-id="d3684-411">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-411">✔</span></span> | <span data-ttu-id="d3684-412">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-412">✔</span></span> | <span data-ttu-id="d3684-413">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-413">✔</span></span>
<span data-ttu-id="d3684-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="d3684-414">LineString.IsClosed</span></span> | <span data-ttu-id="d3684-415">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-415">✔</span></span> | <span data-ttu-id="d3684-416">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-416">✔</span></span> | <span data-ttu-id="d3684-417">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-417">✔</span></span> | <span data-ttu-id="d3684-418">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-418">✔</span></span>
<span data-ttu-id="d3684-419">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="d3684-419">LineString.IsRing</span></span> | <span data-ttu-id="d3684-420">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-420">✔</span></span> | | <span data-ttu-id="d3684-421">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-421">✔</span></span> | <span data-ttu-id="d3684-422">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-422">✔</span></span>
<span data-ttu-id="d3684-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="d3684-423">LineString.StartPoint</span></span> | <span data-ttu-id="d3684-424">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-424">✔</span></span> | <span data-ttu-id="d3684-425">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-425">✔</span></span> | <span data-ttu-id="d3684-426">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-426">✔</span></span> | <span data-ttu-id="d3684-427">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-427">✔</span></span>
<span data-ttu-id="d3684-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="d3684-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="d3684-429">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-429">✔</span></span> | <span data-ttu-id="d3684-430">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-430">✔</span></span> | <span data-ttu-id="d3684-431">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-431">✔</span></span> | <span data-ttu-id="d3684-432">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-432">✔</span></span>
<span data-ttu-id="d3684-433">Punto. M</span><span class="sxs-lookup"><span data-stu-id="d3684-433">Point.M</span></span> | <span data-ttu-id="d3684-434">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-434">✔</span></span> | <span data-ttu-id="d3684-435">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-435">✔</span></span> | <span data-ttu-id="d3684-436">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-436">✔</span></span> | <span data-ttu-id="d3684-437">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-437">✔</span></span>
<span data-ttu-id="d3684-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="d3684-438">Point.X</span></span> | <span data-ttu-id="d3684-439">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-439">✔</span></span> | <span data-ttu-id="d3684-440">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-440">✔</span></span> | <span data-ttu-id="d3684-441">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-441">✔</span></span> | <span data-ttu-id="d3684-442">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-442">✔</span></span>
<span data-ttu-id="d3684-443">Punto. Y</span><span class="sxs-lookup"><span data-stu-id="d3684-443">Point.Y</span></span> | <span data-ttu-id="d3684-444">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-444">✔</span></span> | <span data-ttu-id="d3684-445">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-445">✔</span></span> | <span data-ttu-id="d3684-446">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-446">✔</span></span> | <span data-ttu-id="d3684-447">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-447">✔</span></span>
<span data-ttu-id="d3684-448">Punto. Z</span><span class="sxs-lookup"><span data-stu-id="d3684-448">Point.Z</span></span> | <span data-ttu-id="d3684-449">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-449">✔</span></span> | <span data-ttu-id="d3684-450">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-450">✔</span></span> | <span data-ttu-id="d3684-451">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-451">✔</span></span> | <span data-ttu-id="d3684-452">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-452">✔</span></span>
<span data-ttu-id="d3684-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="d3684-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="d3684-454">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-454">✔</span></span> | <span data-ttu-id="d3684-455">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-455">✔</span></span> | <span data-ttu-id="d3684-456">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-456">✔</span></span> | <span data-ttu-id="d3684-457">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-457">✔</span></span>
<span data-ttu-id="d3684-458">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="d3684-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="d3684-459">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-459">✔</span></span> | <span data-ttu-id="d3684-460">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-460">✔</span></span> | <span data-ttu-id="d3684-461">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-461">✔</span></span> | <span data-ttu-id="d3684-462">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-462">✔</span></span>
<span data-ttu-id="d3684-463">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="d3684-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="d3684-464">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-464">✔</span></span> | <span data-ttu-id="d3684-465">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-465">✔</span></span> | <span data-ttu-id="d3684-466">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-466">✔</span></span> | <span data-ttu-id="d3684-467">✔</span><span class="sxs-lookup"><span data-stu-id="d3684-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3684-468">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d3684-468">Additional resources</span></span>

* [<span data-ttu-id="d3684-469">Datos espaciales en SQL Server</span><span class="sxs-lookup"><span data-stu-id="d3684-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="d3684-470">Página principal de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="d3684-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="d3684-471">Documentación espacial Npgsql</span><span class="sxs-lookup"><span data-stu-id="d3684-471">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="d3684-472">Documentación de PostGIS</span><span class="sxs-lookup"><span data-stu-id="d3684-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
