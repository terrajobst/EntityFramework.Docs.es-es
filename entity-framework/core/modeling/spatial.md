---
title: 'Datos espaciales: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502245"
---
# <a name="spatial-data"></a><span data-ttu-id="07e5a-102">Datos espaciales</span><span class="sxs-lookup"><span data-stu-id="07e5a-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="07e5a-103">Esta característica se agregó en EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="07e5a-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="07e5a-104">Los datos espaciales representan la ubicación física y la forma de los objetos.</span><span class="sxs-lookup"><span data-stu-id="07e5a-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="07e5a-105">Muchas bases de datos proporcionan compatibilidad con este tipo de datos, por lo que se puede indizar y consultar junto con otros datos.</span><span class="sxs-lookup"><span data-stu-id="07e5a-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="07e5a-106">Entre los escenarios comunes se incluyen las consultas de objetos dentro de una distancia determinada desde una ubicación o la selección del objeto cuyo borde contiene una ubicación determinada.</span><span class="sxs-lookup"><span data-stu-id="07e5a-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="07e5a-107">EF Core admite la asignación a tipos de datos espaciales mediante la biblioteca espacial [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="07e5a-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="07e5a-108">Instalar</span><span class="sxs-lookup"><span data-stu-id="07e5a-108">Installing</span></span>

<span data-ttu-id="07e5a-109">Para usar los datos espaciales con EF Core, debe instalar el paquete NuGet de soporte adecuado.</span><span class="sxs-lookup"><span data-stu-id="07e5a-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="07e5a-110">El paquete que necesita instalar depende del proveedor que esté usando.</span><span class="sxs-lookup"><span data-stu-id="07e5a-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="07e5a-111">Proveedor de EF Core</span><span class="sxs-lookup"><span data-stu-id="07e5a-111">EF Core Provider</span></span>                        | <span data-ttu-id="07e5a-112">Paquete de NuGet espacial</span><span class="sxs-lookup"><span data-stu-id="07e5a-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="07e5a-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="07e5a-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="07e5a-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07e5a-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="07e5a-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="07e5a-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="07e5a-116">Microsoft. EntityFrameworkCore. SQLite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07e5a-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="07e5a-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="07e5a-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="07e5a-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07e5a-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="07e5a-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="07e5a-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="07e5a-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07e5a-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="07e5a-121">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="07e5a-121">Reverse engineering</span></span>

<span data-ttu-id="07e5a-122">Los paquetes de NuGet espaciales también habilitan los modelos de [ingeniería inversa](../managing-schemas/scaffolding.md) con propiedades espaciales, pero debe instalar el paquete ***antes*** de ejecutar `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="07e5a-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="07e5a-123">Si no lo hace, recibirá advertencias sobre cómo no encontrar las asignaciones de tipos para las columnas y se omitirán las columnas.</span><span class="sxs-lookup"><span data-stu-id="07e5a-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="07e5a-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="07e5a-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="07e5a-125">NetTopologySuite es una biblioteca espacial para .NET.</span><span class="sxs-lookup"><span data-stu-id="07e5a-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="07e5a-126">EF Core permite la asignación a tipos de datos espaciales en la base de datos mediante el uso de tipos NTS en el modelo.</span><span class="sxs-lookup"><span data-stu-id="07e5a-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="07e5a-127">Para habilitar la asignación a tipos espaciales a través de NTS, llame al método UseNetTopologySuite en el generador de opciones DbContext del proveedor.</span><span class="sxs-lookup"><span data-stu-id="07e5a-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="07e5a-128">Por ejemplo, con SQL Server le llamaría como esto.</span><span class="sxs-lookup"><span data-stu-id="07e5a-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="07e5a-129">Hay varios tipos de datos espaciales.</span><span class="sxs-lookup"><span data-stu-id="07e5a-129">There are several spatial data types.</span></span> <span data-ttu-id="07e5a-130">El tipo que use dependerá de los tipos de formas que desee permitir.</span><span class="sxs-lookup"><span data-stu-id="07e5a-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="07e5a-131">Esta es la jerarquía de tipos NTS que puede usar para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="07e5a-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="07e5a-132">Están ubicados en el espacio de nombres `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="07e5a-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="07e5a-133">Geometría</span><span class="sxs-lookup"><span data-stu-id="07e5a-133">Geometry</span></span>
  * <span data-ttu-id="07e5a-134">Punto</span><span class="sxs-lookup"><span data-stu-id="07e5a-134">Point</span></span>
  * <span data-ttu-id="07e5a-135">LineString</span><span class="sxs-lookup"><span data-stu-id="07e5a-135">LineString</span></span>
  * <span data-ttu-id="07e5a-136">Polígono</span><span class="sxs-lookup"><span data-stu-id="07e5a-136">Polygon</span></span>
  * <span data-ttu-id="07e5a-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="07e5a-137">GeometryCollection</span></span>
    * <span data-ttu-id="07e5a-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="07e5a-138">MultiPoint</span></span>
    * <span data-ttu-id="07e5a-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="07e5a-139">MultiLineString</span></span>
    * <span data-ttu-id="07e5a-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="07e5a-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="07e5a-141">La CircularString, CompoundCurve y CurePolygon no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="07e5a-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="07e5a-142">El uso del tipo de geometría base permite que la propiedad especifique cualquier tipo de forma.</span><span class="sxs-lookup"><span data-stu-id="07e5a-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="07e5a-143">Las siguientes clases de entidad se pueden usar para asignar tablas en la [base de datos de ejemplo Wide World Importers](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="07e5a-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="07e5a-144">Crear valores</span><span class="sxs-lookup"><span data-stu-id="07e5a-144">Creating values</span></span>

<span data-ttu-id="07e5a-145">Puede usar constructores para crear objetos Geometry; sin embargo, NTS recomienda el uso de un generador de geometría en su lugar.</span><span class="sxs-lookup"><span data-stu-id="07e5a-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="07e5a-146">Esto le permite especificar un valor predeterminado de SRID (el sistema de referencia espacial que usan las coordenadas) y le proporciona el control sobre aspectos más avanzados, como el modelo de precisión (usado durante los cálculos) y la secuencia de coordenadas (determina las coordenadas: dimensiones). y las medidas--están disponibles).</span><span class="sxs-lookup"><span data-stu-id="07e5a-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="07e5a-147">4326 hace referencia a WGS 84, un estándar que se usa en GPS y en otros sistemas geográficos.</span><span class="sxs-lookup"><span data-stu-id="07e5a-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="07e5a-148">Longitud y latitud</span><span class="sxs-lookup"><span data-stu-id="07e5a-148">Longitude and Latitude</span></span>

<span data-ttu-id="07e5a-149">Las coordenadas en NTS están en términos de valores X e y.</span><span class="sxs-lookup"><span data-stu-id="07e5a-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="07e5a-150">Para representar la longitud y la latitud, use X para longitud e y para latitud.</span><span class="sxs-lookup"><span data-stu-id="07e5a-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="07e5a-151">Tenga en cuenta que esto es **hacia atrás** desde el formato de `latitude, longitude` en el que normalmente se ven estos valores.</span><span class="sxs-lookup"><span data-stu-id="07e5a-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="07e5a-152">SRID omitido durante las operaciones de cliente</span><span class="sxs-lookup"><span data-stu-id="07e5a-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="07e5a-153">NTS omite los valores de SRID durante las operaciones.</span><span class="sxs-lookup"><span data-stu-id="07e5a-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="07e5a-154">Supone un sistema de coordenadas plano.</span><span class="sxs-lookup"><span data-stu-id="07e5a-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="07e5a-155">Esto significa que si especifica coordenadas en términos de longitud y latitud, algunos valores evaluados por el cliente como la distancia, la longitud y el área estarán en grados, no en metros.</span><span class="sxs-lookup"><span data-stu-id="07e5a-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="07e5a-156">Para valores más significativos, primero debe proyectar las coordenadas en otro sistema de coordenadas mediante una biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular estos valores.</span><span class="sxs-lookup"><span data-stu-id="07e5a-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="07e5a-157">Si una operación es evaluada por el servidor mediante EF Core a través de SQL, la unidad del resultado se determinará por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="07e5a-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="07e5a-158">Este es un ejemplo del uso de ProjNet4GeoAPI para calcular la distancia entre dos ciudades.</span><span class="sxs-lookup"><span data-stu-id="07e5a-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="07e5a-159">Consulta de datos</span><span class="sxs-lookup"><span data-stu-id="07e5a-159">Querying Data</span></span>

<span data-ttu-id="07e5a-160">En LINQ, los métodos y las propiedades NTS disponibles como funciones de base de datos se traducirán a SQL.</span><span class="sxs-lookup"><span data-stu-id="07e5a-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="07e5a-161">Por ejemplo, los métodos Distance y Contains se traducen en las siguientes consultas.</span><span class="sxs-lookup"><span data-stu-id="07e5a-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="07e5a-162">En la tabla al final de este artículo se muestran los miembros que son compatibles con varios proveedores de EF Core.</span><span class="sxs-lookup"><span data-stu-id="07e5a-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="07e5a-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="07e5a-163">SQL Server</span></span>

<span data-ttu-id="07e5a-164">Si usa SQL Server, hay algunos aspectos adicionales que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="07e5a-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="07e5a-165">Geografía o geometría</span><span class="sxs-lookup"><span data-stu-id="07e5a-165">Geography or geometry</span></span>

<span data-ttu-id="07e5a-166">De forma predeterminada, las propiedades espaciales se asignan a `geography` columnas en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="07e5a-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="07e5a-167">Para usar `geometry`, [Configure el tipo de columna](xref:core/modeling/entity-properties#column-data-types) en el modelo.</span><span class="sxs-lookup"><span data-stu-id="07e5a-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="07e5a-168">Anillos de polígono de geografía</span><span class="sxs-lookup"><span data-stu-id="07e5a-168">Geography polygon rings</span></span>

<span data-ttu-id="07e5a-169">Cuando se usa el tipo de columna `geography`, SQL Server impone requisitos adicionales en el anillo exterior (o shell) y los anillos interiores (o agujeros).</span><span class="sxs-lookup"><span data-stu-id="07e5a-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="07e5a-170">El anillo exterior debe estar orientado en sentido contrario a las agujas del reloj y los anillos interiores hacia la derecha.</span><span class="sxs-lookup"><span data-stu-id="07e5a-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="07e5a-171">NTS valida esto antes de enviar los valores a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="07e5a-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="07e5a-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="07e5a-172">FullGlobe</span></span>

<span data-ttu-id="07e5a-173">SQL Server tiene un tipo de geometría no estándar para representar todo el globo terráqueo cuando se usa el tipo de columna `geography`.</span><span class="sxs-lookup"><span data-stu-id="07e5a-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="07e5a-174">También tiene una forma de representar polígonos basados en el globo completo (sin un anillo exterior).</span><span class="sxs-lookup"><span data-stu-id="07e5a-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="07e5a-175">Ninguno de ellos es compatible con NTS.</span><span class="sxs-lookup"><span data-stu-id="07e5a-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="07e5a-176">Los FullGlobe y los polígonos basados en ellos no son compatibles con NTS.</span><span class="sxs-lookup"><span data-stu-id="07e5a-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="07e5a-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="07e5a-177">SQLite</span></span>

<span data-ttu-id="07e5a-178">A continuación se muestra información adicional para los usuarios que usan SQLite.</span><span class="sxs-lookup"><span data-stu-id="07e5a-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="07e5a-179">Instalación de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="07e5a-179">Installing SpatiaLite</span></span>

<span data-ttu-id="07e5a-180">En Windows, la biblioteca de mod_spatialite nativa se distribuye como una dependencia del paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="07e5a-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="07e5a-181">Otras plataformas deben instalarse por separado.</span><span class="sxs-lookup"><span data-stu-id="07e5a-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="07e5a-182">Esto se suele hacer mediante un administrador de paquetes de software.</span><span class="sxs-lookup"><span data-stu-id="07e5a-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="07e5a-183">Por ejemplo, puede usar APT en Ubuntu y homebrew en MacOS.</span><span class="sxs-lookup"><span data-stu-id="07e5a-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="07e5a-184">Configuración de SRID</span><span class="sxs-lookup"><span data-stu-id="07e5a-184">Configuring SRID</span></span>

<span data-ttu-id="07e5a-185">En SpatiaLite, las columnas deben especificar un SRID por columna.</span><span class="sxs-lookup"><span data-stu-id="07e5a-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="07e5a-186">El valor predeterminado de SRID es `0`.</span><span class="sxs-lookup"><span data-stu-id="07e5a-186">The default SRID is `0`.</span></span> <span data-ttu-id="07e5a-187">Especifique otro SRID con el método ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="07e5a-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="07e5a-188">Dimensión</span><span class="sxs-lookup"><span data-stu-id="07e5a-188">Dimension</span></span>

<span data-ttu-id="07e5a-189">De forma similar a SRID, la dimensión de una columna (o las ordenadas) también se especifica como parte de la columna.</span><span class="sxs-lookup"><span data-stu-id="07e5a-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="07e5a-190">Las ordenadas predeterminadas son X e y. Habilite las ordenadas adicionales (Z y M) mediante el método ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="07e5a-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="07e5a-191">Operaciones traducidas</span><span class="sxs-lookup"><span data-stu-id="07e5a-191">Translated Operations</span></span>

<span data-ttu-id="07e5a-192">En esta tabla se muestran los miembros de NTS que cada proveedor de EF Core traduce en SQL.</span><span class="sxs-lookup"><span data-stu-id="07e5a-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="07e5a-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="07e5a-193">NetTopologySuite</span></span> | <span data-ttu-id="07e5a-194">SQL Server (geometría)</span><span class="sxs-lookup"><span data-stu-id="07e5a-194">SQL Server (geometry)</span></span> | <span data-ttu-id="07e5a-195">SQL Server (Geografía)</span><span class="sxs-lookup"><span data-stu-id="07e5a-195">SQL Server (geography)</span></span> | <span data-ttu-id="07e5a-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="07e5a-196">SQLite</span></span> | <span data-ttu-id="07e5a-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="07e5a-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="07e5a-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="07e5a-198">Geometry.Area</span></span> | <span data-ttu-id="07e5a-199">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-199">✔</span></span> | <span data-ttu-id="07e5a-200">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-200">✔</span></span> | <span data-ttu-id="07e5a-201">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-201">✔</span></span> | <span data-ttu-id="07e5a-202">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-202">✔</span></span>
<span data-ttu-id="07e5a-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="07e5a-204">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-204">✔</span></span> | <span data-ttu-id="07e5a-205">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-205">✔</span></span> | <span data-ttu-id="07e5a-206">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-206">✔</span></span> | <span data-ttu-id="07e5a-207">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-207">✔</span></span>
<span data-ttu-id="07e5a-208">Geometry. astext ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-208">Geometry.AsText()</span></span> | <span data-ttu-id="07e5a-209">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-209">✔</span></span> | <span data-ttu-id="07e5a-210">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-210">✔</span></span> | <span data-ttu-id="07e5a-211">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-211">✔</span></span> | <span data-ttu-id="07e5a-212">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-212">✔</span></span>
<span data-ttu-id="07e5a-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="07e5a-213">Geometry.Boundary</span></span> | <span data-ttu-id="07e5a-214">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-214">✔</span></span> | | <span data-ttu-id="07e5a-215">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-215">✔</span></span> | <span data-ttu-id="07e5a-216">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-216">✔</span></span>
<span data-ttu-id="07e5a-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="07e5a-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="07e5a-218">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-218">✔</span></span> | <span data-ttu-id="07e5a-219">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-219">✔</span></span> | <span data-ttu-id="07e5a-220">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-220">✔</span></span> | <span data-ttu-id="07e5a-221">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-221">✔</span></span>
<span data-ttu-id="07e5a-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="07e5a-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="07e5a-223">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-223">✔</span></span> | <span data-ttu-id="07e5a-224">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-224">✔</span></span>
<span data-ttu-id="07e5a-225">Geometry. centroide</span><span class="sxs-lookup"><span data-stu-id="07e5a-225">Geometry.Centroid</span></span> | <span data-ttu-id="07e5a-226">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-226">✔</span></span> | | <span data-ttu-id="07e5a-227">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-227">✔</span></span> | <span data-ttu-id="07e5a-228">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-228">✔</span></span>
<span data-ttu-id="07e5a-229">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="07e5a-230">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-230">✔</span></span> | <span data-ttu-id="07e5a-231">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-231">✔</span></span> | <span data-ttu-id="07e5a-232">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-232">✔</span></span> | <span data-ttu-id="07e5a-233">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-233">✔</span></span>
<span data-ttu-id="07e5a-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="07e5a-235">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-235">✔</span></span> | <span data-ttu-id="07e5a-236">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-236">✔</span></span> | <span data-ttu-id="07e5a-237">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-237">✔</span></span> | <span data-ttu-id="07e5a-238">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-238">✔</span></span>
<span data-ttu-id="07e5a-239">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="07e5a-240">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-240">✔</span></span> | <span data-ttu-id="07e5a-241">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-241">✔</span></span>
<span data-ttu-id="07e5a-242">Geometry. cubiertas (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="07e5a-243">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-243">✔</span></span> | <span data-ttu-id="07e5a-244">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-244">✔</span></span>
<span data-ttu-id="07e5a-245">Geometry. Crosses (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="07e5a-246">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-246">✔</span></span> | | <span data-ttu-id="07e5a-247">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-247">✔</span></span> | <span data-ttu-id="07e5a-248">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-248">✔</span></span>
<span data-ttu-id="07e5a-249">Geometry. Difference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="07e5a-250">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-250">✔</span></span> | <span data-ttu-id="07e5a-251">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-251">✔</span></span> | <span data-ttu-id="07e5a-252">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-252">✔</span></span> | <span data-ttu-id="07e5a-253">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-253">✔</span></span>
<span data-ttu-id="07e5a-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="07e5a-254">Geometry.Dimension</span></span> | <span data-ttu-id="07e5a-255">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-255">✔</span></span> | <span data-ttu-id="07e5a-256">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-256">✔</span></span> | <span data-ttu-id="07e5a-257">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-257">✔</span></span> | <span data-ttu-id="07e5a-258">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-258">✔</span></span>
<span data-ttu-id="07e5a-259">Geometry. disunion (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="07e5a-260">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-260">✔</span></span> | <span data-ttu-id="07e5a-261">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-261">✔</span></span> | <span data-ttu-id="07e5a-262">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-262">✔</span></span> | <span data-ttu-id="07e5a-263">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-263">✔</span></span>
<span data-ttu-id="07e5a-264">Geometry. Distance (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="07e5a-265">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-265">✔</span></span> | <span data-ttu-id="07e5a-266">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-266">✔</span></span> | <span data-ttu-id="07e5a-267">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-267">✔</span></span> | <span data-ttu-id="07e5a-268">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-268">✔</span></span>
<span data-ttu-id="07e5a-269">Geometría. sobre</span><span class="sxs-lookup"><span data-stu-id="07e5a-269">Geometry.Envelope</span></span> | <span data-ttu-id="07e5a-270">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-270">✔</span></span> | | <span data-ttu-id="07e5a-271">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-271">✔</span></span> | <span data-ttu-id="07e5a-272">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-272">✔</span></span>
<span data-ttu-id="07e5a-273">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="07e5a-274">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-274">✔</span></span>
<span data-ttu-id="07e5a-275">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="07e5a-276">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-276">✔</span></span> | <span data-ttu-id="07e5a-277">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-277">✔</span></span> | <span data-ttu-id="07e5a-278">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-278">✔</span></span> | <span data-ttu-id="07e5a-279">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-279">✔</span></span>
<span data-ttu-id="07e5a-280">Geometry. GeometryType</span><span class="sxs-lookup"><span data-stu-id="07e5a-280">Geometry.GeometryType</span></span> | <span data-ttu-id="07e5a-281">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-281">✔</span></span> | <span data-ttu-id="07e5a-282">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-282">✔</span></span> | <span data-ttu-id="07e5a-283">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-283">✔</span></span> | <span data-ttu-id="07e5a-284">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-284">✔</span></span>
<span data-ttu-id="07e5a-285">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="07e5a-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="07e5a-286">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-286">✔</span></span> | | <span data-ttu-id="07e5a-287">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-287">✔</span></span> | <span data-ttu-id="07e5a-288">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-288">✔</span></span>
<span data-ttu-id="07e5a-289">Geometry. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="07e5a-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="07e5a-290">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-290">✔</span></span> | | <span data-ttu-id="07e5a-291">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-291">✔</span></span> | <span data-ttu-id="07e5a-292">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-292">✔</span></span>
<span data-ttu-id="07e5a-293">Geometry. Intersection (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="07e5a-294">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-294">✔</span></span> | <span data-ttu-id="07e5a-295">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-295">✔</span></span> | <span data-ttu-id="07e5a-296">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-296">✔</span></span> | <span data-ttu-id="07e5a-297">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-297">✔</span></span>
<span data-ttu-id="07e5a-298">Geometry. Intersects (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="07e5a-299">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-299">✔</span></span> | <span data-ttu-id="07e5a-300">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-300">✔</span></span> | <span data-ttu-id="07e5a-301">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-301">✔</span></span> | <span data-ttu-id="07e5a-302">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-302">✔</span></span>
<span data-ttu-id="07e5a-303">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="07e5a-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="07e5a-304">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-304">✔</span></span> | <span data-ttu-id="07e5a-305">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-305">✔</span></span> | <span data-ttu-id="07e5a-306">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-306">✔</span></span> | <span data-ttu-id="07e5a-307">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-307">✔</span></span>
<span data-ttu-id="07e5a-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="07e5a-308">Geometry.IsSimple</span></span> | <span data-ttu-id="07e5a-309">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-309">✔</span></span> | | <span data-ttu-id="07e5a-310">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-310">✔</span></span> | <span data-ttu-id="07e5a-311">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-311">✔</span></span>
<span data-ttu-id="07e5a-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="07e5a-312">Geometry.IsValid</span></span> | <span data-ttu-id="07e5a-313">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-313">✔</span></span> | <span data-ttu-id="07e5a-314">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-314">✔</span></span> | <span data-ttu-id="07e5a-315">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-315">✔</span></span> | <span data-ttu-id="07e5a-316">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-316">✔</span></span>
<span data-ttu-id="07e5a-317">Geometry. IsWithinDistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="07e5a-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="07e5a-318">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-318">✔</span></span> | | <span data-ttu-id="07e5a-319">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-319">✔</span></span> | <span data-ttu-id="07e5a-320">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-320">✔</span></span>
<span data-ttu-id="07e5a-321">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="07e5a-321">Geometry.Length</span></span> | <span data-ttu-id="07e5a-322">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-322">✔</span></span> | <span data-ttu-id="07e5a-323">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-323">✔</span></span> | <span data-ttu-id="07e5a-324">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-324">✔</span></span> | <span data-ttu-id="07e5a-325">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-325">✔</span></span>
<span data-ttu-id="07e5a-326">Geometry. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="07e5a-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="07e5a-327">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-327">✔</span></span> | <span data-ttu-id="07e5a-328">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-328">✔</span></span> | <span data-ttu-id="07e5a-329">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-329">✔</span></span> | <span data-ttu-id="07e5a-330">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-330">✔</span></span>
<span data-ttu-id="07e5a-331">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="07e5a-331">Geometry.NumPoints</span></span> | <span data-ttu-id="07e5a-332">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-332">✔</span></span> | <span data-ttu-id="07e5a-333">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-333">✔</span></span> | <span data-ttu-id="07e5a-334">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-334">✔</span></span> | <span data-ttu-id="07e5a-335">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-335">✔</span></span>
<span data-ttu-id="07e5a-336">Geometry. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="07e5a-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="07e5a-337">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-337">✔</span></span> | <span data-ttu-id="07e5a-338">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-338">✔</span></span> | <span data-ttu-id="07e5a-339">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-339">✔</span></span> | <span data-ttu-id="07e5a-340">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-340">✔</span></span>
<span data-ttu-id="07e5a-341">Geometry. superpone (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="07e5a-342">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-342">✔</span></span> | <span data-ttu-id="07e5a-343">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-343">✔</span></span> | <span data-ttu-id="07e5a-344">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-344">✔</span></span> | <span data-ttu-id="07e5a-345">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-345">✔</span></span>
<span data-ttu-id="07e5a-346">Geometry. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="07e5a-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="07e5a-347">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-347">✔</span></span> | | <span data-ttu-id="07e5a-348">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-348">✔</span></span> | <span data-ttu-id="07e5a-349">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-349">✔</span></span>
<span data-ttu-id="07e5a-350">Geometry. Relate (Geometry, String)</span><span class="sxs-lookup"><span data-stu-id="07e5a-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="07e5a-351">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-351">✔</span></span> | | <span data-ttu-id="07e5a-352">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-352">✔</span></span> | <span data-ttu-id="07e5a-353">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-353">✔</span></span>
<span data-ttu-id="07e5a-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="07e5a-355">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-355">✔</span></span> | <span data-ttu-id="07e5a-356">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-356">✔</span></span>
<span data-ttu-id="07e5a-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="07e5a-357">Geometry.SRID</span></span> | <span data-ttu-id="07e5a-358">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-358">✔</span></span> | <span data-ttu-id="07e5a-359">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-359">✔</span></span> | <span data-ttu-id="07e5a-360">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-360">✔</span></span> | <span data-ttu-id="07e5a-361">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-361">✔</span></span>
<span data-ttu-id="07e5a-362">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="07e5a-363">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-363">✔</span></span> | <span data-ttu-id="07e5a-364">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-364">✔</span></span> | <span data-ttu-id="07e5a-365">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-365">✔</span></span> | <span data-ttu-id="07e5a-366">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-366">✔</span></span>
<span data-ttu-id="07e5a-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="07e5a-368">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-368">✔</span></span> | <span data-ttu-id="07e5a-369">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-369">✔</span></span> | <span data-ttu-id="07e5a-370">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-370">✔</span></span> | <span data-ttu-id="07e5a-371">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-371">✔</span></span>
<span data-ttu-id="07e5a-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-372">Geometry.ToText()</span></span> | <span data-ttu-id="07e5a-373">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-373">✔</span></span> | <span data-ttu-id="07e5a-374">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-374">✔</span></span> | <span data-ttu-id="07e5a-375">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-375">✔</span></span> | <span data-ttu-id="07e5a-376">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-376">✔</span></span>
<span data-ttu-id="07e5a-377">Geometry. toques (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="07e5a-378">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-378">✔</span></span> | | <span data-ttu-id="07e5a-379">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-379">✔</span></span> | <span data-ttu-id="07e5a-380">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-380">✔</span></span>
<span data-ttu-id="07e5a-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="07e5a-381">Geometry.Union()</span></span> | | | <span data-ttu-id="07e5a-382">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-382">✔</span></span> | <span data-ttu-id="07e5a-383">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-383">✔</span></span>
<span data-ttu-id="07e5a-384">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="07e5a-385">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-385">✔</span></span> | <span data-ttu-id="07e5a-386">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-386">✔</span></span> | <span data-ttu-id="07e5a-387">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-387">✔</span></span> | <span data-ttu-id="07e5a-388">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-388">✔</span></span>
<span data-ttu-id="07e5a-389">Geometry. Within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="07e5a-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="07e5a-390">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-390">✔</span></span> | <span data-ttu-id="07e5a-391">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-391">✔</span></span> | <span data-ttu-id="07e5a-392">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-392">✔</span></span> | <span data-ttu-id="07e5a-393">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-393">✔</span></span>
<span data-ttu-id="07e5a-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="07e5a-394">GeometryCollection.Count</span></span> | <span data-ttu-id="07e5a-395">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-395">✔</span></span> | <span data-ttu-id="07e5a-396">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-396">✔</span></span> | <span data-ttu-id="07e5a-397">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-397">✔</span></span> | <span data-ttu-id="07e5a-398">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-398">✔</span></span>
<span data-ttu-id="07e5a-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="07e5a-399">GeometryCollection[int]</span></span> | <span data-ttu-id="07e5a-400">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-400">✔</span></span> | <span data-ttu-id="07e5a-401">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-401">✔</span></span> | <span data-ttu-id="07e5a-402">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-402">✔</span></span> | <span data-ttu-id="07e5a-403">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-403">✔</span></span>
<span data-ttu-id="07e5a-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="07e5a-404">LineString.Count</span></span> | <span data-ttu-id="07e5a-405">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-405">✔</span></span> | <span data-ttu-id="07e5a-406">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-406">✔</span></span> | <span data-ttu-id="07e5a-407">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-407">✔</span></span> | <span data-ttu-id="07e5a-408">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-408">✔</span></span>
<span data-ttu-id="07e5a-409">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="07e5a-409">LineString.EndPoint</span></span> | <span data-ttu-id="07e5a-410">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-410">✔</span></span> | <span data-ttu-id="07e5a-411">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-411">✔</span></span> | <span data-ttu-id="07e5a-412">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-412">✔</span></span> | <span data-ttu-id="07e5a-413">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-413">✔</span></span>
<span data-ttu-id="07e5a-414">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="07e5a-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="07e5a-415">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-415">✔</span></span> | <span data-ttu-id="07e5a-416">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-416">✔</span></span> | <span data-ttu-id="07e5a-417">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-417">✔</span></span> | <span data-ttu-id="07e5a-418">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-418">✔</span></span>
<span data-ttu-id="07e5a-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="07e5a-419">LineString.IsClosed</span></span> | <span data-ttu-id="07e5a-420">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-420">✔</span></span> | <span data-ttu-id="07e5a-421">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-421">✔</span></span> | <span data-ttu-id="07e5a-422">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-422">✔</span></span> | <span data-ttu-id="07e5a-423">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-423">✔</span></span>
<span data-ttu-id="07e5a-424">LineString. IsRing</span><span class="sxs-lookup"><span data-stu-id="07e5a-424">LineString.IsRing</span></span> | <span data-ttu-id="07e5a-425">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-425">✔</span></span> | | <span data-ttu-id="07e5a-426">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-426">✔</span></span> | <span data-ttu-id="07e5a-427">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-427">✔</span></span>
<span data-ttu-id="07e5a-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="07e5a-428">LineString.StartPoint</span></span> | <span data-ttu-id="07e5a-429">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-429">✔</span></span> | <span data-ttu-id="07e5a-430">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-430">✔</span></span> | <span data-ttu-id="07e5a-431">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-431">✔</span></span> | <span data-ttu-id="07e5a-432">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-432">✔</span></span>
<span data-ttu-id="07e5a-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="07e5a-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="07e5a-434">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-434">✔</span></span> | <span data-ttu-id="07e5a-435">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-435">✔</span></span> | <span data-ttu-id="07e5a-436">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-436">✔</span></span> | <span data-ttu-id="07e5a-437">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-437">✔</span></span>
<span data-ttu-id="07e5a-438">Punto. M</span><span class="sxs-lookup"><span data-stu-id="07e5a-438">Point.M</span></span> | <span data-ttu-id="07e5a-439">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-439">✔</span></span> | <span data-ttu-id="07e5a-440">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-440">✔</span></span> | <span data-ttu-id="07e5a-441">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-441">✔</span></span> | <span data-ttu-id="07e5a-442">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-442">✔</span></span>
<span data-ttu-id="07e5a-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="07e5a-443">Point.X</span></span> | <span data-ttu-id="07e5a-444">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-444">✔</span></span> | <span data-ttu-id="07e5a-445">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-445">✔</span></span> | <span data-ttu-id="07e5a-446">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-446">✔</span></span> | <span data-ttu-id="07e5a-447">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-447">✔</span></span>
<span data-ttu-id="07e5a-448">Punto. Y</span><span class="sxs-lookup"><span data-stu-id="07e5a-448">Point.Y</span></span> | <span data-ttu-id="07e5a-449">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-449">✔</span></span> | <span data-ttu-id="07e5a-450">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-450">✔</span></span> | <span data-ttu-id="07e5a-451">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-451">✔</span></span> | <span data-ttu-id="07e5a-452">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-452">✔</span></span>
<span data-ttu-id="07e5a-453">Punto. Z</span><span class="sxs-lookup"><span data-stu-id="07e5a-453">Point.Z</span></span> | <span data-ttu-id="07e5a-454">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-454">✔</span></span> | <span data-ttu-id="07e5a-455">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-455">✔</span></span> | <span data-ttu-id="07e5a-456">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-456">✔</span></span> | <span data-ttu-id="07e5a-457">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-457">✔</span></span>
<span data-ttu-id="07e5a-458">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="07e5a-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="07e5a-459">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-459">✔</span></span> | <span data-ttu-id="07e5a-460">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-460">✔</span></span> | <span data-ttu-id="07e5a-461">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-461">✔</span></span> | <span data-ttu-id="07e5a-462">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-462">✔</span></span>
<span data-ttu-id="07e5a-463">Polygon. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="07e5a-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="07e5a-464">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-464">✔</span></span> | <span data-ttu-id="07e5a-465">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-465">✔</span></span> | <span data-ttu-id="07e5a-466">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-466">✔</span></span> | <span data-ttu-id="07e5a-467">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-467">✔</span></span>
<span data-ttu-id="07e5a-468">Polygon. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="07e5a-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="07e5a-469">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-469">✔</span></span> | <span data-ttu-id="07e5a-470">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-470">✔</span></span> | <span data-ttu-id="07e5a-471">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-471">✔</span></span> | <span data-ttu-id="07e5a-472">✔</span><span class="sxs-lookup"><span data-stu-id="07e5a-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="07e5a-473">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="07e5a-473">Additional resources</span></span>

* [<span data-ttu-id="07e5a-474">Datos espaciales en SQL Server</span><span class="sxs-lookup"><span data-stu-id="07e5a-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="07e5a-475">Página principal de SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="07e5a-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="07e5a-476">Documentación espacial Npgsql</span><span class="sxs-lookup"><span data-stu-id="07e5a-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="07e5a-477">Documentación de PostGIS</span><span class="sxs-lookup"><span data-stu-id="07e5a-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
