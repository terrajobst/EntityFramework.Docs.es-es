---
title: Espacial - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995217"
---
# <a name="spatial---code-first"></a>Espacial - Code First
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

El tutorial de vídeo y paso a paso muestra cómo asignar tipos espaciales con Entity Framework Code First. También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.

En este tutorial utilizará Code First para crear una nueva base de datos, pero también puede usar [Code First para una base de datos](~/ef6/modeling/code-first/workflows/existing-database.md).

Compatibilidad con tipos espaciales se introdujo en Entity Framework 5. Tenga en cuenta que para utilizar las nuevas características, como el tipo espacial, enumeraciones y funciones con valores de tabla, debe tener como destino .NET Framework 4.5. Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.

Para utilizar tipos de datos espaciales también debe usar un proveedor de Entity Framework que tiene compatibilidad espacial. Consulte [compatibilidad del proveedor con tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md) para obtener más información.

Hay dos tipos principales de los datos espaciales: geography y geometry. El tipo de datos geography almacena datos elípticos (por ejemplo, coordenadas GPS latitud y longitud). El tipo de datos geometry representa el sistema de coordenadas euclidiano (plano).

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo muestra cómo asignar tipos espaciales con Entity Framework Code First. También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.

**Presentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Requisitos previos

Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.

## <a name="set-up-the-project"></a>Configurar el proyecto

1.  Abra Visual Studio 2012
2.  En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**
3.  En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla
4.  Escriba **SpatialCodeFirst** como el nombre del proyecto y haga clic en **Aceptar**

## <a name="define-a-new-model-using-code-first"></a>Definir un modelo nuevo mediante Code First

Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio). El código siguiente define la clase de la universidad.

La Universidad tiene la propiedad de ubicación del tipo de DbGeography. Para usar el tipo de DbGeography, debe agregar una referencia al ensamblado System.Data.Entity y también agregar la instrucción using de System.Data.Spatial.

Abra el archivo Program.cs y pegue lo siguiente mediante instrucciones en la parte superior del archivo:

``` csharp
using System.Data.Spatial;
```

En el archivo Program.cs, agregue la siguiente definición de clase de la universidad.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Definir la clase DbContext tipo derivado

Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades. La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.

Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.

Los tipos de DbContext y DbSet se definen en el ensamblado de Entity Framework. Se agregará una referencia a este archivo DLL mediante el paquete EntityFramework NuGet.

1.  En el Explorador de soluciones, haga doble clic en el nombre del proyecto.
2.  Seleccione **administrar paquetes NuGet...**
3.  En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete.
4.  Haga clic en **instalar**

Tenga en cuenta que el ensamblado de Entity Framework, además de una referencia al ensamblado System.ComponentModel.DataAnnotations también se agrega.

En la parte superior del archivo Program.cs, agregue la siguiente instrucción using:

``` csharp
using System.Data.Entity;
```

En el archivo Program.cs, agregue la definición del contexto. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Conservar y recuperar datos

Abra el archivo Program.cs, donde se define el método Main. Agregue el código siguiente en la función Main.

El código agrega dos nuevos objetos de la Universidad del contexto. Propiedades espaciales se inicializan mediante el método DbGeography.FromText. Punto de geografía que se representan como WellKnownText se pasa al método. El código, a continuación, guarda los datos. A continuación, la consulta LINQ que devuelve un objeto de la Universidad donde su ubicación es más cercana a la ubicación especificada, se genera y ejecuta.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compile y ejecute la aplicación. El programa produce el siguiente resultado:

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Vista de la base de datos generado

Al ejecutar la aplicación por primera vez, Entity Framework crea una base de datos. Dado que tenemos instalado Visual Studio 2012, se creará la base de datos en la instancia de LocalDB. De forma predeterminada, Entity Framework los nombres de la base de datos después del nombre completo del contexto derivado (en este ejemplo es **SpatialCodeFirst.UniversityContext**). Las siguientes veces que se usará la base de datos existente.  

Tenga en cuenta que si realiza cualquier cambio en el modelo una vez creada la base de datos, debe usar migraciones de Code First para actualizar el esquema de base de datos. Consulte [Code First para una base de datos](~/ef6/modeling/code-first/workflows/new-database.md) para obtener un ejemplo del uso de las migraciones.

Para ver la base de datos y los datos, realice lo siguiente:

1.  En el menú principal de Visual Studio 2012, seleccione **vista**  - &gt; **Explorador de objetos de SQL Server**.
2.  Si LocalDB no está en la lista de servidores, haga clic en el botón secundario del mouse en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse a la la instancia de LocalDB
3.  Expanda el nodo de LocalDB
4.  Expandir el **bases de datos** carpeta para ver la nueva base de datos y busque el **universidades** tabla
5.  Para ver los datos, haga doble clic en la tabla y seleccione **ver datos**

## <a name="summary"></a>Resumen

En este tutorial analizamos cómo usar tipos espaciales con Entity Framework Code First. 
