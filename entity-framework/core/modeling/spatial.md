---
title: Datos espaciales - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688795"
---
# <a name="spatial-data"></a>Datos espaciales

> [!NOTE]
> Esta característica es nueva en EF Core 2.2.

Los datos espaciales representan la ubicación física y la forma de objetos. Muchas bases de datos proporcionan compatibilidad para este tipo de datos, por lo que se pueden indexar y consultar junto con otros datos. Escenarios habituales incluyen consultar los objetos dentro de una distancia especificada desde una ubicación, o seleccione el objeto cuyo borde contiene una ubicación determinada. EF Core admite la asignación a tipos de datos espaciales mediante la [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) biblioteca espacial.

## <a name="installing"></a>Instalación

Para usar datos espaciales con EF Core, deberá instalar el paquete NuGet auxiliar adecuado. Qué paquetes necesita instalar depende del proveedor que está utilizando.

Proveedor de EF Core                        | Paquete de NuGet espacial
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Ingeniería inversa

Paquetes de NuGet espacial también habilitar [ingeniería inversa](../managing-schemas/scaffolding.md) modelos con propiedades espaciales, pero se deben instalar el paquete ***antes*** ejecutando `Scaffold-DbContext` o `dotnet ef dbcontext scaffold`. Si no lo hace, recibirá advertencias acerca de cómo no buscar las asignaciones de tipos para las columnas y las columnas se omitirán.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite es una biblioteca espacial para. NET. EF Core permite la asignación de datos espaciales tipos en la base de datos mediante el uso de tipos de interrupción en el modelo.

Para habilitar la asignación de tipos espaciales a través de la interrupción, llame al método UseNetTopologySuite en el generador de opciones de DbContext del proveedor. Por ejemplo, con SQL Server podría llamarlo similar al siguiente.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Hay varios tipos de datos espaciales. Qué tipo se utiliza depende de los tipos de formas que desee permitir. Aquí es la jerarquía de tipos de interrupción que puede usar para las propiedades del modelo. Que se encuentren dentro de la `NetTopologySuite.Geometries` espacio de nombres. Las interfaces correspondientes en el paquete GeoAPI (`GeoAPI.Geometries` espacio de nombres) también se puede usar.

* geometría
  * Punto
  * LineString
  * Polígono
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve y CurePolygon no son compatibles con interrupción.

El tipo de geometría base permite que cualquier tipo de forma que se especifique la propiedad.

Las clases de entidad siguiente podrían usarse para asignar a tablas en el [base de datos de ejemplo de Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).

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

### <a name="creating-values"></a>Creación de valores

Puede usar constructores para crear objetos de geometría; Sin embargo, NTS recomienda usar una fábrica de geometría en su lugar. Esto le permite especificar un valor predeterminado SRID (el sistema de referencia espacial utilizado por las coordenadas) y le ofrece control sobre las cosas más avanzadas, como el modelo de precisión (que se usa durante los cálculos) y la secuencia de coordenadas (determina qué coordenadas--dimensiones y las medidas, están disponibles).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 hace referencia a WGS 84, un estándar que se usa en GPS y otros sistemas geográficas.

### <a name="longitude-and-latitude"></a>Longitud y latitud

Coordenadas de NTS están en términos de valores X e Y. Para representar la longitud y latitud, utilice X para latitud e Y para latitude. Tenga en cuenta que esto es **hacia atrás** desde el `latitude, longitude` formato en el que podría ver estos valores.

### <a name="srid-ignored-during-client-operations"></a>SRID omite durante las operaciones de cliente

NTS omite los valores SRID durante las operaciones. Se supone que un sistema de coordenadas plano. Esto significa que si especifica las coordenadas en términos de longitud y latitud, algunos valores evaluado como cliente, como distancia, longitud y área estarán en grados, no los medidores. Para los valores más significativos, primero necesita proyectar las coordenadas a otro sistema de coordenadas mediante una biblioteca como [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) antes de calcular estos valores.

Si hay una operación evaluado como EF Core a través de SQL server, la unidad del resultado se determinará la base de datos.

Este es un ejemplo del uso de ProjNet4GeoAPI para calcular la distancia entre dos ciudades.

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

## <a name="querying-data"></a>Consulta de datos

En LINQ, los métodos de interrupción y las propiedades disponibles como funciones de base de datos se traducirá a SQL. Por ejemplo, los métodos de distancia y Contains se traducen en las consultas siguientes. La tabla al final de este artículo muestran los miembros que son compatibles con varios proveedores de EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Si usa SQL Server, hay algunos aspectos adicionales que debe tener en cuenta.

### <a name="geography-or-geometry"></a>Geografía o geometría

De forma predeterminada, las propiedades espaciales se asignan a `geography` columnas en SQL Server. Para usar `geometry`, [configurar el tipo de columna](xref:core/modeling/relational/data-types) en el modelo.

### <a name="geography-polygon-rings"></a>Anillos de polígono de geografía

Cuando se usa el `geography` tipo de columna, SQL Server impone requisitos adicionales en el anillo exterior (o el shell) e interiores anillos (ni marcadores). El anillo exterior debe ser orientado a la izquierda y el interior a la derecha los anillos. NTS Esto valida antes de enviar valores a la base de datos.

### <a name="fullglobe"></a>FullGlobe

SQL Server tiene un tipo de geometría no estándar para representar el globo terráqueo cuando se usa el `geography` tipo de columna. También tiene una forma de representar los polígonos basándose en el globo terráqueo (sin un anillo exterior). Ninguna de ellas son compatibles con interrupción.

> [!WARNING]
> NTS no admiten FullGlobe y polígonos basados en él.

## <a name="sqlite"></a>SQLite

Esta es información adicional para los usuarios de SQLite.

### <a name="installing-spatialite"></a>Instalar SpatiaLite

En Windows, la biblioteca nativa mod_spatialite se distribuye como una dependencia del paquete NuGet. Otras plataformas necesitan instalarlo por separado. Esto se suele realizar mediante un administrador de paquetes de software. Por ejemplo, puede usar APT en Ubuntu y Homebrew en MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Configurar SRID

En SpatiaLite, las columnas deben especificar un SRID por columna. El valor predeterminado SRID es `0`. Especifique un SRID diferentes mediante el método ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimensión

Al igual que los SRID, dimensión de una columna (o coordenadas) también se especifica como parte de la columna. Las coordenadas de forma predeterminada son X y Y. Habilitar adicionales ordenadas (Z y M) mediante el método ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Operaciones traducidas

Esta tabla muestra qué miembros NTS se traducen en SQL por cada proveedor de EF Core.

NetTopologySuite | SQL Server (geometry) | SQL Server (geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, cadena) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Recursos adicionales

* [Datos espaciales de SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Página principal de SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Documentación de PostGIS](http://postgis.net/documentation/)
