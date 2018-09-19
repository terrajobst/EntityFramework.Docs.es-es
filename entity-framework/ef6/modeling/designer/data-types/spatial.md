---
title: 'Espacial EF6 - EF Designer:'
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 67cc6c007a4477b650e7c4875de8fac36a9ba2be
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283763"
---
# <a name="spatial---ef-designer"></a>Espacial - EF Designer
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

El tutorial de vídeo y paso a paso muestra cómo asignar tipos espaciales con Entity Framework Designer. También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.

En este tutorial usará Model First para crear una nueva base de datos, pero también se puede usar el Diseñador de EF con el [Database First](~/ef6/modeling/designer/workflows/database-first.md) flujo de trabajo para asignar a una base de datos existente.

Compatibilidad con tipos espaciales se introdujo en Entity Framework 5. Tenga en cuenta que para utilizar las nuevas características, como el tipo espacial, enumeraciones y funciones con valores de tabla, debe tener como destino .NET Framework 4.5. Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.

Para utilizar tipos de datos espaciales también debe usar un proveedor de Entity Framework que tiene compatibilidad espacial. Consulte [compatibilidad del proveedor con tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md) para obtener más información.

Hay dos tipos principales de los datos espaciales: geography y geometry. El tipo de datos geography almacena datos elípticos (por ejemplo, coordenadas GPS latitud y longitud). El tipo de datos geometry representa el sistema de coordenadas euclidiano (plano).

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo muestra cómo asignar tipos espaciales con Entity Framework Designer. También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.

**Presentado por**: Julia Kornich

**Vídeo**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Requisitos previos

Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.

## <a name="set-up-the-project"></a>Configurar el proyecto

1.  Abra Visual Studio 2012
2.  En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**
3.  En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla
4.  Escriba **SpatialEFDesigner** como el nombre del proyecto y haga clic en **Aceptar**

## <a name="create-a-new-model-using-the-ef-designer"></a>Crear un nuevo modelo con EF Designer

1.  Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**
2.  Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel de plantillas
3.  Escriba **UniversityModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**
4.  En la página del Asistente para Entity Data Model, seleccione **modelo vacío** en el cuadro de diálogo Elegir contenido del modelo
5.  Haga clic en **finalizar**

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.

El asistente realiza las siguientes acciones:

-   Genera el archivo EnumTestModel.edmx que define el modelo conceptual, el modelo de almacenamiento y la asignación entre ellos. Establece la propiedad Metadata Artifact Processing del archivo .edmx para insertar en el ensamblado de salida para que los archivos de metadatos generados se incrustan en el ensamblado.
-   Agrega una referencia a los ensamblados siguientes: Entity Framework, System.ComponentModel.DataAnnotations y System.Data.Entity.
-   Crea los archivos UniversityModel.tt y UniversityModel.Context.tt y los agrega en el archivo .edmx. Estos archivos de plantilla T4 generarán el código que define el tipo de DbContext derivado y tipos POCO que se asignan a las entidades en el modelo edmx

## <a name="add-a-new-entity-type"></a>Agregar un nuevo tipo de entidad

1.  Haga clic en un área vacía de la superficie de diseño, seleccione **Add -&gt; entidad**, aparece el cuadro de diálogo New Entity
2.  Especificar **Universidad** para el tipo de nombre y especificar **ID** para el nombre de propiedad de clave, deje el tipo como **Int32**
3.  Haga clic en **Aceptar**.
4.  Haga clic en la entidad y seleccione **Add New -&gt; propiedad escalar**
5.  Cambiar el nombre de la nueva propiedad a **nombre**
6.  Agregar otra propiedad escalar y cambie su nombre a **ubicación** abrir la ventana Propiedades y cambie el tipo de la nueva propiedad a **Geography**
7.  Guardar el modelo y compile el proyecto
    > [!NOTE]
    > Cuando se compila, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores. Puede omitir estas advertencias porque una vez que se elija Generar la base de datos del modelo, los errores desaparecerán.

## <a name="generate-database-from-model"></a>Generar la base de datos de modelo

Ahora podemos generar una base de datos que se basa en el modelo.

1.  Haga clic en un espacio vacío en el Entity Designer superficie y seleccione **generar base de datos de modelo**
2.  Haga clic en mostrará la elija el cuadro de diálogo de conexión de datos del Asistente para generar base de datos es el **nueva conexión** botón especifique **(localdb)\\mssqllocaldb** para el nombre del servidor y  **Universidad** para la base de datos y haga clic en **Aceptar**
3.  Se abrirá, haga clic en un cuadro de diálogo que le pregunta si desea crear una nueva base de datos **Sí**.
4.  Haga clic en **siguiente** y Asistente para crear base de datos genera el lenguaje de definición de datos (DDL) para crear una base de datos de la DDL generada se muestra en el resumen y una nota de cuadro de diálogo Configuración, el DDL no contiene una definición para un tabla que se asigna al tipo de enumeración
5.  Haga clic en **finalizar** no ejecuta el script DDL al hacer clic en Finalizar.
6.  El Asistente para crear base de datos hace lo siguiente: abre el **UniversityModel.edmx.sql** en T-SQL, Editor genera las secciones de esquema y asignación de almacén de EDMX archivo agrega información de la cadena de conexión al archivo App.config
7.  Haga clic en el botón secundario del mouse en el Editor de Transact-SQL y seleccione **Execute** aparece el diálogo Conectar a servidor, escriba la información de conexión del paso 2 y haga clic en **Connect**
8.  Para ver el esquema generado, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**

## <a name="persist-and-retrieve-data"></a>Conservar y recuperar datos

Abra el archivo Program.cs, donde se define el método Main. Agregue el código siguiente en la función Main.

El código agrega dos nuevos objetos de la Universidad del contexto. Propiedades espaciales se inicializan mediante el método DbGeography.FromText. Punto de geografía que se representan como WellKnownText se pasa al método. El código, a continuación, guarda los datos. A continuación, la consulta LINQ que devuelve un objeto de la Universidad donde su ubicación es más cercana a la ubicación especificada, se genera y ejecuta.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

Para ver los datos en la base de datos, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**. A continuación, haga clic en el botón secundario del mouse en la tabla y seleccione **ver datos**.

## <a name="summary"></a>Resumen

En este tutorial explicamos cómo asignar tipos espaciales mediante el Diseñador de Entity Framework y cómo usar tipos espaciales en el código. 
