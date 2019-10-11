---
title: 'Datos espaciales: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cced53edadb890e4e86753ec2628218ffc4d1d5b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181390"
---
# <a name="spatial-data"></a><span data-ttu-id="e99f5-102">Datos espaciales</span><span class="sxs-lookup"><span data-stu-id="e99f5-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="e99f5-103">Esta característica se agregó en EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="e99f5-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="e99f5-104">Los datos espaciales representan la ubicación física y la forma de los objetos.</span><span class="sxs-lookup"><span data-stu-id="e99f5-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="e99f5-105">Muchas bases de datos proporcionan compatibilidad con este tipo de datos, por lo que se puede indizar y consultar junto con otros datos.</span><span class="sxs-lookup"><span data-stu-id="e99f5-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="e99f5-106">Entre los escenarios comunes se incluyen las consultas de objetos dentro de una distancia determinada desde una ubicación o la selección del objeto cuyo borde contiene una ubicación determinada.</span><span class="sxs-lookup"><span data-stu-id="e99f5-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="e99f5-107">EF Core admite la asignación a tipos de datos espaciales mediante la biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="e99f5-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="e99f5-108">Instalación</span><span class="sxs-lookup"><span data-stu-id="e99f5-108">Installing</span></span>

<span data-ttu-id="e99f5-109">Para usar los datos espaciales con EF Core, debe instalar el paquete NuGet de soporte adecuado.</span><span class="sxs-lookup"><span data-stu-id="e99f5-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="e99f5-110">El paquete que necesita instalar depende del proveedor que esté usando.</span><span class="sxs-lookup"><span data-stu-id="e99f5-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="e99f5-111">Proveedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="e99f5-111">EF Core Provider</span></span>                        | <span data-ttu-id="e99f5-112">Paquete de NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="e99f5-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="e99f5-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e99f5-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="e99f5-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e99f5-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="e99f5-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="e99f5-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="e99f5-116">Microsoft. EntityFrameworkCore. SQLite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e99f5-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="e99f5-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="e99f5-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="e99f5-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e99f5-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="e99f5-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e99f5-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="e99f5-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e99f5-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="e99f5-121">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="e99f5-121">Reverse engineering</span></span>

<span data-ttu-id="e99f5-122">Los paquetes de NuGet espaciales también habilitan los modelos de [ingeniería inversa](../managing-schemas/scaffolding.md) con propiedades espaciales, pero debe instalar el paquete ***antes*** de ejecutar `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="e99f5-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="e99f5-123">Si no lo hace, recibirá advertencias sobre cómo no encontrar las asignaciones de tipos para las columnas y se omitirán las columnas.</span><span class="sxs-lookup"><span data-stu-id="e99f5-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="e99f5-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="e99f5-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="e99f5-125">NetTopologySuite es una biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="e99f5-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="e99f5-126">EF Core permite la asignación a tipos de datos espaciales en la base de datos mediante el uso de tipos NTS en el modelo.</span><span class="sxs-lookup"><span data-stu-id="e99f5-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="e99f5-127">Para habilitar la asignación a tipos espaciales a través de NTS, llame al método UseNetTopologySuite en el generador de opciones DbContext del proveedor.</span><span class="sxs-lookup"><span data-stu-id="e99f5-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="e99f5-128">Por ejemplo, con SQL Server le llamaría como esto.</span><span class="sxs-lookup"><span data-stu-id="e99f5-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="e99f5-129">Hay varios tipos de datos espaciales.</span><span class="sxs-lookup"><span data-stu-id="e99f5-129">There are several spatial data types.</span></span> <span data-ttu-id="e99f5-130">El tipo que use dependerá de los tipos de formas que desee permitir.</span><span class="sxs-lookup"><span data-stu-id="e99f5-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="e99f5-131">Esta es la jerarquía de tipos NTS que puede usar para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="e99f5-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="e99f5-132">Están ubicados en el espacio de nombres `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="e99f5-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="e99f5-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="e99f5-133">Geometry</span></span>
  * <span data-ttu-id="e99f5-134">Punto</span><span class="sxs-lookup"><span data-stu-id="e99f5-134">Point</span></span>
  * <span data-ttu-id="e99f5-135">LineString</span><span class="sxs-lookup"><span data-stu-id="e99f5-135">LineString</span></span>
  * <span data-ttu-id="e99f5-136">Polígono</span><span class="sxs-lookup"><span data-stu-id="e99f5-136">Polygon</span></span>
  * <span data-ttu-id="e99f5-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="e99f5-137">GeometryCollection</span></span>
    * <span data-ttu-id="e99f5-138">Point</span><span class="sxs-lookup"><span data-stu-id="e99f5-138">MultiPoint</span></span>
    * <span data-ttu-id="e99f5-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="e99f5-139">MultiLineString</span></span>
    * <span data-ttu-id="e99f5-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="e99f5-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="e99f5-141">La CircularString, CompoundCurve y CurePolygon no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="e99f5-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="e99f5-142">El uso del tipo de geometría base permite que la propiedad especifique cualquier tipo de forma.</span><span class="sxs-lookup"><span data-stu-id="e99f5-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="e99f5-143">Las siguientes clases de entidad se pueden usar para asignar tablas en la [base de datos de ejemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="e99f5-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="e99f5-144">Crear valores</span><span class="sxs-lookup"><span data-stu-id="e99f5-144">Creating values</span></span>

<span data-ttu-id="e99f5-145">Puede usar constructores para crear objetos Geometry; sin embargo, NTS recomienda el uso de un generador de geometría en su lugar.</span><span class="sxs-lookup"><span data-stu-id="e99f5-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="e99f5-146">Esto le permite especificar un valor predeterminado de SRID (el sistema de referencia espacial que usan las coordenadas) y le proporciona el control sobre aspectos más avanzados, como el modelo de precisión (usado durante los cálculos) y la secuencia de coordenadas (determina las coordenadas: dimensiones). y las medidas--están disponibles).</span><span class="sxs-lookup"><span data-stu-id="e99f5-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="e99f5-147">4326 hace referencia a WGS 84, un estándar que se usa en GPS y en otros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="e99f5-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="e99f5-148">Longitud y latitud</span><span class="sxs-lookup"><span data-stu-id="e99f5-148">Longitude and Latitude</span></span>

<span data-ttu-id="e99f5-149">Las coordenadas en NTS están en términos de valores X e y.</span><span class="sxs-lookup"><span data-stu-id="e99f5-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="e99f5-150">Para representar la longitud y la latitud, use X para longitud e y para latitud.</span><span class="sxs-lookup"><span data-stu-id="e99f5-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="e99f5-151">Tenga en cuenta que esto es **hacia atrás** desde el formato `latitude, longitude` en el que normalmente se ven estos valores.</span><span class="sxs-lookup"><span data-stu-id="e99f5-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="e99f5-152">SRID omitido durante las operaciones de cliente</span><span class="sxs-lookup"><span data-stu-id="e99f5-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="e99f5-153">NTS omite los valores de SRID durante las operaciones.</span><span class="sxs-lookup"><span data-stu-id="e99f5-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="e99f5-154">Supone un sistema de coordenadas plano.</span><span class="sxs-lookup"><span data-stu-id="e99f5-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="e99f5-155">Esto significa que si especifica coordenadas en términos de longitud y latitud, algunos valores evaluados por el cliente como la distancia, la longitud y el área estarán en grados, no en metros.</span><span class="sxs-lookup"><span data-stu-id="e99f5-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="e99f5-156">Para valores más significativos, primero debe proyectar las coordenadas en otro sistema de coordenadas mediante una biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular estos valores.</span><span class="sxs-lookup"><span data-stu-id="e99f5-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="e99f5-157">Si una operación es evaluada por el servidor mediante EF Core a través de SQL, la unidad del resultado se determinará por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e99f5-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="e99f5-158">Este es un ejemplo del uso de ProjNet4GeoAPI para calcular la distancia entre dos ciudades.</span><span class="sxs-lookup"><span data-stu-id="e99f5-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="e99f5-159">Consulta de datos</span><span class="sxs-lookup"><span data-stu-id="e99f5-159">Querying Data</span></span>

<span data-ttu-id="e99f5-160">En LINQ, los métodos y las propiedades NTS disponibles como funciones de base de datos se traducirán a SQL.</span><span class="sxs-lookup"><span data-stu-id="e99f5-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="e99f5-161">Por ejemplo, los métodos Distance y Contains se traducen en las siguientes consultas.</span><span class="sxs-lookup"><span data-stu-id="e99f5-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="e99f5-162">En la tabla al final de este artículo se muestran los miembros que son compatibles con varios proveedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="e99f5-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="e99f5-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e99f5-163">SQL Server</span></span>

<span data-ttu-id="e99f5-164">Si usa SQL Server, hay algunos aspectos adicionales que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="e99f5-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="e99f5-165">Geografía o geometría</span><span class="sxs-lookup"><span data-stu-id="e99f5-165">Geography or geometry</span></span>

<span data-ttu-id="e99f5-166">De forma predeterminada, las propiedades espaciales se asignan a las columnas `geography` en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e99f5-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="e99f5-167">Para usar `geometry`, [Configure el tipo de columna](xref:core/modeling/relational/data-types) en el modelo.</span><span class="sxs-lookup"><span data-stu-id="e99f5-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="e99f5-168">Anillos de polígono de geografía</span><span class="sxs-lookup"><span data-stu-id="e99f5-168">Geography polygon rings</span></span>

<span data-ttu-id="e99f5-169">Cuando se usa el tipo de columna `geography`, SQL Server impone requisitos adicionales en el anillo exterior (o shell) y los anillos interiores (o agujeros).</span><span class="sxs-lookup"><span data-stu-id="e99f5-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="e99f5-170">El anillo exterior debe estar orientado en sentido contrario a las agujas del reloj y los anillos interiores hacia la derecha.</span><span class="sxs-lookup"><span data-stu-id="e99f5-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="e99f5-171">NTS valida esto antes de enviar los valores a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e99f5-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="e99f5-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="e99f5-172">FullGlobe</span></span>

<span data-ttu-id="e99f5-173">SQL Server tiene un tipo de geometría no estándar para representar todo el globo terráqueo al usar el tipo de columna `geography`.</span><span class="sxs-lookup"><span data-stu-id="e99f5-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="e99f5-174">También tiene una forma de representar polígonos basados en el globo completo (sin un anillo exterior).</span><span class="sxs-lookup"><span data-stu-id="e99f5-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="e99f5-175">Ninguno de ellos es compatible con NTS.</span><span class="sxs-lookup"><span data-stu-id="e99f5-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="e99f5-176">Los FullGlobe y los polígonos basados en ellos no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="e99f5-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="e99f5-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="e99f5-177">SQLite</span></span>

<span data-ttu-id="e99f5-178">A continuación se muestra información adicional para los usuarios que usan SQLite.</span><span class="sxs-lookup"><span data-stu-id="e99f5-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="e99f5-179">Instalación de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e99f5-179">Installing SpatiaLite</span></span>

<span data-ttu-id="e99f5-180">En Windows, la biblioteca nativa de mod_spatialite se distribuye como una dependencia del paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="e99f5-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="e99f5-181">Otras plataformas deben instalarse por separado.</span><span class="sxs-lookup"><span data-stu-id="e99f5-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="e99f5-182">Esto se suele hacer mediante un administrador de paquetes de software.</span><span class="sxs-lookup"><span data-stu-id="e99f5-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="e99f5-183">Por ejemplo, puede usar APT en Ubuntu y homebrew en MacOS.</span><span class="sxs-lookup"><span data-stu-id="e99f5-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="e99f5-184">Configuración de SRID</span><span class="sxs-lookup"><span data-stu-id="e99f5-184">Configuring SRID</span></span>

<span data-ttu-id="e99f5-185">En SpatiaLite, las columnas deben especificar un SRID por columna.</span><span class="sxs-lookup"><span data-stu-id="e99f5-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="e99f5-186">El valor predeterminado de SRID es `0`.</span><span class="sxs-lookup"><span data-stu-id="e99f5-186">The default SRID is `0`.</span></span> <span data-ttu-id="e99f5-187">Especifique otro SRID con el método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="e99f5-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="e99f5-188">Dimensión</span><span class="sxs-lookup"><span data-stu-id="e99f5-188">Dimension</span></span>

<span data-ttu-id="e99f5-189">De forma similar a SRID, la dimensión de una columna (o las ordenadas) también se especifica como parte de la columna.</span><span class="sxs-lookup"><span data-stu-id="e99f5-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="e99f5-190">Las ordenadas predeterminadas son X e y. Habilite las ordenadas adicionales (Z y M) mediante el método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="e99f5-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="e99f5-191">Operaciones traducidas</span><span class="sxs-lookup"><span data-stu-id="e99f5-191">Translated Operations</span></span>

<span data-ttu-id="e99f5-192">En esta tabla se muestran los miembros de NTS que cada proveedor de EF Core traduce en SQL.</span><span class="sxs-lookup"><span data-stu-id="e99f5-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="e99f5-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e99f5-193">NetTopologySuite</span></span> | <span data-ttu-id="e99f5-194">SQL Server (geometría)</span><span class="sxs-lookup"><span data-stu-id="e99f5-194">SQL Server (geometry)</span></span> | <span data-ttu-id="e99f5-195">SQL Server (Geografía)</span><span class="sxs-lookup"><span data-stu-id="e99f5-195">SQL Server (geography)</span></span> | <span data-ttu-id="e99f5-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="e99f5-196">SQLite</span></span> | <span data-ttu-id="e99f5-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="e99f5-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="e99f5-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="e99f5-198">Geometry.Area</span></span> | <span data-ttu-id="e99f5-199">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-199">✔</span></span> | <span data-ttu-id="e99f5-200">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-200">✔</span></span> | <span data-ttu-id="e99f5-201">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-201">✔</span></span> | <span data-ttu-id="e99f5-202">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-202">✔</span></span>
<span data-ttu-id="e99f5-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="e99f5-204">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-204">✔</span></span> | <span data-ttu-id="e99f5-205">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-205">✔</span></span> | <span data-ttu-id="e99f5-206">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-206">✔</span></span> | <span data-ttu-id="e99f5-207">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-207">✔</span></span>
<span data-ttu-id="e99f5-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-208">Geometry.AsText()</span></span> | <span data-ttu-id="e99f5-209">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-209">✔</span></span> | <span data-ttu-id="e99f5-210">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-210">✔</span></span> | <span data-ttu-id="e99f5-211">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-211">✔</span></span> | <span data-ttu-id="e99f5-212">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-212">✔</span></span>
<span data-ttu-id="e99f5-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="e99f5-213">Geometry.Boundary</span></span> | <span data-ttu-id="e99f5-214">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-214">✔</span></span> | | <span data-ttu-id="e99f5-215">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-215">✔</span></span> | <span data-ttu-id="e99f5-216">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-216">✔</span></span>
<span data-ttu-id="e99f5-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="e99f5-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="e99f5-218">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-218">✔</span></span> | <span data-ttu-id="e99f5-219">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-219">✔</span></span> | <span data-ttu-id="e99f5-220">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-220">✔</span></span> | <span data-ttu-id="e99f5-221">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-221">✔</span></span>
<span data-ttu-id="e99f5-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="e99f5-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="e99f5-223">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-223">✔</span></span>
<span data-ttu-id="e99f5-224">Geometry. centroide</span><span class="sxs-lookup"><span data-stu-id="e99f5-224">Geometry.Centroid</span></span> | <span data-ttu-id="e99f5-225">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-225">✔</span></span> | | <span data-ttu-id="e99f5-226">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-226">✔</span></span> | <span data-ttu-id="e99f5-227">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-227">✔</span></span>
<span data-ttu-id="e99f5-228">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="e99f5-229">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-229">✔</span></span> | <span data-ttu-id="e99f5-230">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-230">✔</span></span> | <span data-ttu-id="e99f5-231">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-231">✔</span></span> | <span data-ttu-id="e99f5-232">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-232">✔</span></span>
<span data-ttu-id="e99f5-233">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="e99f5-234">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-234">✔</span></span> | <span data-ttu-id="e99f5-235">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-235">✔</span></span> | <span data-ttu-id="e99f5-236">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-236">✔</span></span> | <span data-ttu-id="e99f5-237">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-237">✔</span></span>
<span data-ttu-id="e99f5-238">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="e99f5-239">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-239">✔</span></span> | <span data-ttu-id="e99f5-240">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-240">✔</span></span>
<span data-ttu-id="e99f5-241">Geometry. cubiertas (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="e99f5-242">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-242">✔</span></span> | <span data-ttu-id="e99f5-243">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-243">✔</span></span>
<span data-ttu-id="e99f5-244">Geometry. Crosses (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="e99f5-245">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-245">✔</span></span> | | <span data-ttu-id="e99f5-246">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-246">✔</span></span> | <span data-ttu-id="e99f5-247">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-247">✔</span></span>
<span data-ttu-id="e99f5-248">Geometry. Difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="e99f5-249">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-249">✔</span></span> | <span data-ttu-id="e99f5-250">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-250">✔</span></span> | <span data-ttu-id="e99f5-251">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-251">✔</span></span> | <span data-ttu-id="e99f5-252">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-252">✔</span></span>
<span data-ttu-id="e99f5-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="e99f5-253">Geometry.Dimension</span></span> | <span data-ttu-id="e99f5-254">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-254">✔</span></span> | <span data-ttu-id="e99f5-255">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-255">✔</span></span> | <span data-ttu-id="e99f5-256">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-256">✔</span></span> | <span data-ttu-id="e99f5-257">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-257">✔</span></span>
<span data-ttu-id="e99f5-258">Geometry. disunion (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="e99f5-259">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-259">✔</span></span> | <span data-ttu-id="e99f5-260">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-260">✔</span></span> | <span data-ttu-id="e99f5-261">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-261">✔</span></span> | <span data-ttu-id="e99f5-262">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-262">✔</span></span>
<span data-ttu-id="e99f5-263">Geometry. Distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="e99f5-264">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-264">✔</span></span> | <span data-ttu-id="e99f5-265">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-265">✔</span></span> | <span data-ttu-id="e99f5-266">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-266">✔</span></span> | <span data-ttu-id="e99f5-267">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-267">✔</span></span>
<span data-ttu-id="e99f5-268">Geometría. sobre</span><span class="sxs-lookup"><span data-stu-id="e99f5-268">Geometry.Envelope</span></span> | <span data-ttu-id="e99f5-269">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-269">✔</span></span> | | <span data-ttu-id="e99f5-270">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-270">✔</span></span> | <span data-ttu-id="e99f5-271">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-271">✔</span></span>
<span data-ttu-id="e99f5-272">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="e99f5-273">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-273">✔</span></span>
<span data-ttu-id="e99f5-274">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="e99f5-275">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-275">✔</span></span> | <span data-ttu-id="e99f5-276">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-276">✔</span></span> | <span data-ttu-id="e99f5-277">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-277">✔</span></span> | <span data-ttu-id="e99f5-278">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-278">✔</span></span>
<span data-ttu-id="e99f5-279">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="e99f5-279">Geometry.GeometryType</span></span> | <span data-ttu-id="e99f5-280">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-280">✔</span></span> | <span data-ttu-id="e99f5-281">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-281">✔</span></span> | <span data-ttu-id="e99f5-282">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-282">✔</span></span> | <span data-ttu-id="e99f5-283">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-283">✔</span></span>
<span data-ttu-id="e99f5-284">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="e99f5-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="e99f5-285">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-285">✔</span></span> | | <span data-ttu-id="e99f5-286">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-286">✔</span></span> | <span data-ttu-id="e99f5-287">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-287">✔</span></span>
<span data-ttu-id="e99f5-288">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="e99f5-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="e99f5-289">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-289">✔</span></span> | | <span data-ttu-id="e99f5-290">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-290">✔</span></span>
<span data-ttu-id="e99f5-291">Geometry. Intersection (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="e99f5-292">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-292">✔</span></span> | <span data-ttu-id="e99f5-293">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-293">✔</span></span> | <span data-ttu-id="e99f5-294">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-294">✔</span></span> | <span data-ttu-id="e99f5-295">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-295">✔</span></span>
<span data-ttu-id="e99f5-296">Geometry. Intersects (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="e99f5-297">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-297">✔</span></span> | <span data-ttu-id="e99f5-298">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-298">✔</span></span> | <span data-ttu-id="e99f5-299">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-299">✔</span></span> | <span data-ttu-id="e99f5-300">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-300">✔</span></span>
<span data-ttu-id="e99f5-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="e99f5-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="e99f5-302">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-302">✔</span></span> | <span data-ttu-id="e99f5-303">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-303">✔</span></span> | <span data-ttu-id="e99f5-304">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-304">✔</span></span> | <span data-ttu-id="e99f5-305">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-305">✔</span></span>
<span data-ttu-id="e99f5-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="e99f5-306">Geometry.IsSimple</span></span> | <span data-ttu-id="e99f5-307">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-307">✔</span></span> | | <span data-ttu-id="e99f5-308">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-308">✔</span></span> | <span data-ttu-id="e99f5-309">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-309">✔</span></span>
<span data-ttu-id="e99f5-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="e99f5-310">Geometry.IsValid</span></span> | <span data-ttu-id="e99f5-311">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-311">✔</span></span> | <span data-ttu-id="e99f5-312">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-312">✔</span></span> | <span data-ttu-id="e99f5-313">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-313">✔</span></span> | <span data-ttu-id="e99f5-314">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-314">✔</span></span>
<span data-ttu-id="e99f5-315">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="e99f5-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="e99f5-316">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-316">✔</span></span> | | <span data-ttu-id="e99f5-317">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-317">✔</span></span>
<span data-ttu-id="e99f5-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="e99f5-318">Geometry.Length</span></span> | <span data-ttu-id="e99f5-319">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-319">✔</span></span> | <span data-ttu-id="e99f5-320">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-320">✔</span></span> | <span data-ttu-id="e99f5-321">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-321">✔</span></span> | <span data-ttu-id="e99f5-322">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-322">✔</span></span>
<span data-ttu-id="e99f5-323">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="e99f5-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="e99f5-324">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-324">✔</span></span> | <span data-ttu-id="e99f5-325">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-325">✔</span></span> | <span data-ttu-id="e99f5-326">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-326">✔</span></span> | <span data-ttu-id="e99f5-327">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-327">✔</span></span>
<span data-ttu-id="e99f5-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="e99f5-328">Geometry.NumPoints</span></span> | <span data-ttu-id="e99f5-329">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-329">✔</span></span> | <span data-ttu-id="e99f5-330">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-330">✔</span></span> | <span data-ttu-id="e99f5-331">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-331">✔</span></span> | <span data-ttu-id="e99f5-332">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-332">✔</span></span>
<span data-ttu-id="e99f5-333">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="e99f5-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="e99f5-334">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-334">✔</span></span> | <span data-ttu-id="e99f5-335">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-335">✔</span></span> | <span data-ttu-id="e99f5-336">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-336">✔</span></span>
<span data-ttu-id="e99f5-337">Geometry. superpone (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="e99f5-338">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-338">✔</span></span> | <span data-ttu-id="e99f5-339">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-339">✔</span></span> | <span data-ttu-id="e99f5-340">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-340">✔</span></span> | <span data-ttu-id="e99f5-341">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-341">✔</span></span>
<span data-ttu-id="e99f5-342">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="e99f5-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="e99f5-343">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-343">✔</span></span> | | <span data-ttu-id="e99f5-344">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-344">✔</span></span> | <span data-ttu-id="e99f5-345">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-345">✔</span></span>
<span data-ttu-id="e99f5-346">Geometry. Relate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="e99f5-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="e99f5-347">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-347">✔</span></span> | | <span data-ttu-id="e99f5-348">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-348">✔</span></span> | <span data-ttu-id="e99f5-349">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-349">✔</span></span>
<span data-ttu-id="e99f5-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="e99f5-351">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-351">✔</span></span> | <span data-ttu-id="e99f5-352">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-352">✔</span></span>
<span data-ttu-id="e99f5-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="e99f5-353">Geometry.SRID</span></span> | <span data-ttu-id="e99f5-354">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-354">✔</span></span> | <span data-ttu-id="e99f5-355">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-355">✔</span></span> | <span data-ttu-id="e99f5-356">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-356">✔</span></span> | <span data-ttu-id="e99f5-357">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-357">✔</span></span>
<span data-ttu-id="e99f5-358">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="e99f5-359">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-359">✔</span></span> | <span data-ttu-id="e99f5-360">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-360">✔</span></span> | <span data-ttu-id="e99f5-361">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-361">✔</span></span> | <span data-ttu-id="e99f5-362">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-362">✔</span></span>
<span data-ttu-id="e99f5-363">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="e99f5-364">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-364">✔</span></span> | <span data-ttu-id="e99f5-365">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-365">✔</span></span> | <span data-ttu-id="e99f5-366">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-366">✔</span></span> | <span data-ttu-id="e99f5-367">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-367">✔</span></span>
<span data-ttu-id="e99f5-368">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-368">Geometry.ToText()</span></span> | <span data-ttu-id="e99f5-369">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-369">✔</span></span> | <span data-ttu-id="e99f5-370">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-370">✔</span></span> | <span data-ttu-id="e99f5-371">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-371">✔</span></span> | <span data-ttu-id="e99f5-372">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-372">✔</span></span>
<span data-ttu-id="e99f5-373">Geometry. toques (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="e99f5-374">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-374">✔</span></span> | | <span data-ttu-id="e99f5-375">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-375">✔</span></span> | <span data-ttu-id="e99f5-376">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-376">✔</span></span>
<span data-ttu-id="e99f5-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="e99f5-377">Geometry.Union()</span></span> | | | <span data-ttu-id="e99f5-378">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-378">✔</span></span>
<span data-ttu-id="e99f5-379">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="e99f5-380">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-380">✔</span></span> | <span data-ttu-id="e99f5-381">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-381">✔</span></span> | <span data-ttu-id="e99f5-382">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-382">✔</span></span> | <span data-ttu-id="e99f5-383">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-383">✔</span></span>
<span data-ttu-id="e99f5-384">Geometry. Within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="e99f5-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="e99f5-385">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-385">✔</span></span> | <span data-ttu-id="e99f5-386">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-386">✔</span></span> | <span data-ttu-id="e99f5-387">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-387">✔</span></span> | <span data-ttu-id="e99f5-388">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-388">✔</span></span>
<span data-ttu-id="e99f5-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="e99f5-389">GeometryCollection.Count</span></span> | <span data-ttu-id="e99f5-390">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-390">✔</span></span> | <span data-ttu-id="e99f5-391">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-391">✔</span></span> | <span data-ttu-id="e99f5-392">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-392">✔</span></span> | <span data-ttu-id="e99f5-393">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-393">✔</span></span>
<span data-ttu-id="e99f5-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="e99f5-394">GeometryCollection[int]</span></span> | <span data-ttu-id="e99f5-395">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-395">✔</span></span> | <span data-ttu-id="e99f5-396">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-396">✔</span></span> | <span data-ttu-id="e99f5-397">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-397">✔</span></span> | <span data-ttu-id="e99f5-398">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-398">✔</span></span>
<span data-ttu-id="e99f5-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="e99f5-399">LineString.Count</span></span> | <span data-ttu-id="e99f5-400">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-400">✔</span></span> | <span data-ttu-id="e99f5-401">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-401">✔</span></span> | <span data-ttu-id="e99f5-402">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-402">✔</span></span> | <span data-ttu-id="e99f5-403">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-403">✔</span></span>
<span data-ttu-id="e99f5-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="e99f5-404">LineString.EndPoint</span></span> | <span data-ttu-id="e99f5-405">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-405">✔</span></span> | <span data-ttu-id="e99f5-406">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-406">✔</span></span> | <span data-ttu-id="e99f5-407">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-407">✔</span></span> | <span data-ttu-id="e99f5-408">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-408">✔</span></span>
<span data-ttu-id="e99f5-409">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="e99f5-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="e99f5-410">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-410">✔</span></span> | <span data-ttu-id="e99f5-411">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-411">✔</span></span> | <span data-ttu-id="e99f5-412">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-412">✔</span></span> | <span data-ttu-id="e99f5-413">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-413">✔</span></span>
<span data-ttu-id="e99f5-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="e99f5-414">LineString.IsClosed</span></span> | <span data-ttu-id="e99f5-415">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-415">✔</span></span> | <span data-ttu-id="e99f5-416">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-416">✔</span></span> | <span data-ttu-id="e99f5-417">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-417">✔</span></span> | <span data-ttu-id="e99f5-418">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-418">✔</span></span>
<span data-ttu-id="e99f5-419">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="e99f5-419">LineString.IsRing</span></span> | <span data-ttu-id="e99f5-420">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-420">✔</span></span> | | <span data-ttu-id="e99f5-421">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-421">✔</span></span> | <span data-ttu-id="e99f5-422">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-422">✔</span></span>
<span data-ttu-id="e99f5-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="e99f5-423">LineString.StartPoint</span></span> | <span data-ttu-id="e99f5-424">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-424">✔</span></span> | <span data-ttu-id="e99f5-425">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-425">✔</span></span> | <span data-ttu-id="e99f5-426">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-426">✔</span></span> | <span data-ttu-id="e99f5-427">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-427">✔</span></span>
<span data-ttu-id="e99f5-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="e99f5-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="e99f5-429">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-429">✔</span></span> | <span data-ttu-id="e99f5-430">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-430">✔</span></span> | <span data-ttu-id="e99f5-431">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-431">✔</span></span> | <span data-ttu-id="e99f5-432">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-432">✔</span></span>
<span data-ttu-id="e99f5-433">Punto. M</span><span class="sxs-lookup"><span data-stu-id="e99f5-433">Point.M</span></span> | <span data-ttu-id="e99f5-434">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-434">✔</span></span> | <span data-ttu-id="e99f5-435">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-435">✔</span></span> | <span data-ttu-id="e99f5-436">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-436">✔</span></span> | <span data-ttu-id="e99f5-437">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-437">✔</span></span>
<span data-ttu-id="e99f5-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="e99f5-438">Point.X</span></span> | <span data-ttu-id="e99f5-439">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-439">✔</span></span> | <span data-ttu-id="e99f5-440">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-440">✔</span></span> | <span data-ttu-id="e99f5-441">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-441">✔</span></span> | <span data-ttu-id="e99f5-442">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-442">✔</span></span>
<span data-ttu-id="e99f5-443">Punto. Y</span><span class="sxs-lookup"><span data-stu-id="e99f5-443">Point.Y</span></span> | <span data-ttu-id="e99f5-444">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-444">✔</span></span> | <span data-ttu-id="e99f5-445">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-445">✔</span></span> | <span data-ttu-id="e99f5-446">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-446">✔</span></span> | <span data-ttu-id="e99f5-447">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-447">✔</span></span>
<span data-ttu-id="e99f5-448">Punto. Z</span><span class="sxs-lookup"><span data-stu-id="e99f5-448">Point.Z</span></span> | <span data-ttu-id="e99f5-449">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-449">✔</span></span> | <span data-ttu-id="e99f5-450">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-450">✔</span></span> | <span data-ttu-id="e99f5-451">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-451">✔</span></span> | <span data-ttu-id="e99f5-452">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-452">✔</span></span>
<span data-ttu-id="e99f5-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="e99f5-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="e99f5-454">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-454">✔</span></span> | <span data-ttu-id="e99f5-455">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-455">✔</span></span> | <span data-ttu-id="e99f5-456">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-456">✔</span></span> | <span data-ttu-id="e99f5-457">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-457">✔</span></span>
<span data-ttu-id="e99f5-458">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="e99f5-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="e99f5-459">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-459">✔</span></span> | <span data-ttu-id="e99f5-460">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-460">✔</span></span> | <span data-ttu-id="e99f5-461">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-461">✔</span></span> | <span data-ttu-id="e99f5-462">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-462">✔</span></span>
<span data-ttu-id="e99f5-463">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="e99f5-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="e99f5-464">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-464">✔</span></span> | <span data-ttu-id="e99f5-465">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-465">✔</span></span> | <span data-ttu-id="e99f5-466">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-466">✔</span></span> | <span data-ttu-id="e99f5-467">✔</span><span class="sxs-lookup"><span data-stu-id="e99f5-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e99f5-468">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e99f5-468">Additional resources</span></span>

* [<span data-ttu-id="e99f5-469">Datos espaciales en SQL Server</span><span class="sxs-lookup"><span data-stu-id="e99f5-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="e99f5-470">Página principal de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e99f5-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="e99f5-471">Documentación espacial Npgsql</span><span class="sxs-lookup"><span data-stu-id="e99f5-471">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="e99f5-472">Documentación de PostGIS</span><span class="sxs-lookup"><span data-stu-id="e99f5-472">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
