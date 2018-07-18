---
title: Tipos complejos - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
caps.latest.revision: 3
ms.openlocfilehash: 6d0744921085afdafa35d137d276c54738cdd465
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122279"
---
# <a name="complex-types---ef-designer"></a>Tipos complejos - EF Designer
En este tema se muestra cómo asignar tipos complejos con Entity Framework Designer (Diseñador de EF) y cómo consultar entidades que contienen propiedades de tipo complejo.

La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Cuando se compila el modelo conceptual, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores. Puede omitir estas advertencias porque después de elegir generar la base de datos del modelo, los errores desaparecerán.

## <a name="what-is-a-complex-type"></a>¿Qué es un tipo complejo

Los tipos complejos son propiedades no escalares de tipos de entidad que permiten organizar las propiedades escalares dentro de las entidades. Al igual que las entidades, los tipos complejos están compuestos de propiedades escalares u otras propiedades de tipo complejo.

Cuando se trabaja con objetos que representan tipos complejos, tenga en cuenta lo siguiente:

-   Tipos complejos no tienen claves y, por tanto, no pueden existir de forma independiente. Los tipos complejos solo pueden existir como propiedades de tipos de entidad u otros tipos complejos.
-   Tipos complejos no pueden participar en las asociaciones y no pueden contener las propiedades de navegación.
-   Propiedades de tipo complejo no pueden ser **null**. Un ** InvalidOperationException ** se produce cuando **DbContext.SaveChanges** se denomina y se encuentra un objeto complejo null. Las propiedades escalares de objetos complejos pueden ser **null**.
-   Los tipos complejos no pueden heredar de otros tipos complejos.
-   Debe definir el tipo complejo como un **clase**. 
-   EF detecta los cambios en los miembros en un objeto de tipo complejo cuando **DbContext.DetectChanges** se llama. Entity Framework llama **DetectChanges** automáticamente cuando se llama a los siguientes miembros: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refactorizar propiedades de la entidad en el nuevo tipo complejo

Si ya tiene una entidad en el modelo conceptual que se desea refactorizar algunas de las propiedades en una propiedad de tipo complejo.

En la superficie del diseñador, seleccione uno o más propiedades (excepto las propiedades de navegación) de una entidad, a continuación, haga clic en y seleccione **refactorizar -&gt; mover al nuevo tipo complejo**.

![Refactorizar](~/ef6/media/refactor.png)

Se agrega un nuevo tipo complejo con las propiedades seleccionadas a la **Explorador de modelos**. Se asigna un nombre predeterminado al tipo complejo.

Una propiedad compleja del tipo que se acaba de crear reemplaza las propiedades seleccionadas. Se conservan todas las asignaciones de propiedades.

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Crear un nuevo tipo complejo

También puede crear un nuevo tipo complejo que no contiene las propiedades de una entidad existente.

Haga clic en el **tipos complejos** carpeta en el Explorador de modelos, apunte a **tipo complejo de AddNew...** . Como alternativa, puede seleccionar la **tipos complejos** carpeta y presione la **insertar** en el teclado.

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

Un nuevo tipo complejo se agrega a la carpeta con un nombre predeterminado. Ahora puede agregar propiedades al tipo.

## <a name="add-properties-to-a-complex-type"></a>Agregar propiedades a un tipo complejo

Las propiedades de un tipo complejo pueden ser tipos escalares o los tipos complejos existentes. Sin embargo, las propiedades de tipo complejo no pueden tener las referencias circulares. Por ejemplo, un tipo complejo **OnsiteCourseDetails** no puede tener una propiedad de tipo complejo **OnsiteCourseDetails**.

Puede agregar una propiedad a un tipo complejo de cualquiera de las formas enumeradas a continuación.

-   Haga clic en un tipo complejo en el Explorador de modelos, seleccione **agregar**, a continuación, elija **propiedad escalar** o **propiedad compleja**, a continuación, seleccione el tipo de propiedad deseado. Como alternativa, puede seleccionar un tipo complejo y, a continuación, presione el **insertar** en el teclado.  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    Se agrega una nueva propiedad al tipo complejo con un nombre predeterminado.

- OR:

-   Haga clic en una propiedad de entidad en el **EF Designer** superficie y seleccione **copia**, a continuación, haga clic en el tipo complejo en el **Explorador de modelos** y seleccione  **Pegar**.

## <a name="rename-a-complex-type"></a>Cambiar el nombre de un tipo complejo

Al cambiar el nombre de un tipo complejo, todas las referencias al tipo se actualizan en el proyecto.

-   Lentamente, haga doble clic en un tipo complejo en el **Explorador de modelos**.
    El nombre se seleccionará y se abrirá en modo de edición.

- OR:

-   Haga clic en un tipo complejo en el **Explorador de modelos** y seleccione **cambiar el nombre**.

- OR:

-   Seleccione un tipo complejo en el Explorador de modelos y presione la tecla F2.

- OR:

-   Haga clic en un tipo complejo en el **Explorador de modelos** y seleccione **propiedades**. Edite el nombre de la **propiedades** ventana.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Agregar un tipo complejo existente a una entidad y sus propiedades se asignan a columnas de tabla

1.  Haga clic en una entidad, seleccione **Agregar nuevo**y seleccione **propiedad compleja**.
    Se agrega a la entidad una propiedad de tipo complejo con un nombre predeterminado. Se asigna un tipo predeterminado (que se elige entre los tipos complejos existentes) a la propiedad.
2.  Asigne el tipo deseado a la propiedad en el **propiedades** ventana.
    Después de agregar una propiedad de tipo complejo a una entidad, debe asignar sus propiedades a las columnas de una tabla.
3.  Haga clic en un tipo de entidad en la superficie de diseño o en el **Explorador de modelos** y seleccione **asignaciones de tabla**.
    Las asignaciones de tabla se muestran en el **detalles de Mapping** ventana.
4.  Expanda el **se asigna a &lt;nombre de la tabla&gt;**  nodo.
    Un **asignaciones de columnas** aparece del nodo.
5.  Expanda el **asignaciones de columnas** nodo.
    Aparece una lista de todas las columnas de la tabla. Las propiedades predeterminadas (si existe) para que se asignan las columnas se muestran bajo el **valor/propiedad** encabezado.
6.  Seleccione la columna que desea asignar y, a continuación, haga clic en el correspondiente **valor/propiedad** campo.
    Se muestra una lista desplegable de todas las propiedades escalares.
7.  Seleccione la propiedad apropiada.

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  Repita los pasos 6 y 7 para cada columna de tabla.

>[!NOTE]
> Para eliminar una asignación de columna, seleccione la columna que desea asignar y, a continuación, haga clic en el **valor/propiedad** campo. A continuación, seleccione **eliminar** en la lista desplegable.

## <a name="map-a-function-import-to-a-complex-type"></a>Asignar una importación de función a un tipo complejo

Las importaciones de función se basan en procedimientos almacenados. Para asignar una importación de función a un tipo complejo, las columnas devueltas por el procedimiento almacenado correspondiente deben coincidir con el número de propiedades del tipo complejo y deben tener tipos de almacenamiento compatibles con los tipos de propiedad.

-   Haga doble clic en una función importada que desea asignar a un tipo complejo.

    ![FunctionImports](~/ef6/media/functionimports.png)

-   Rellene los valores para la nueva importación de función de la siguiente forma:
    -   Especifique el procedimiento almacenado para el que está creando una importación de función en el **nombre de procedimiento almacenado** campo. El campo es una lista desplegable que muestra todos los procedimientos almacenados del modelo de almacenamiento.
    -   Especifique el nombre de la importación de función en el **nombre de Function Import** campo.
    -   Seleccione **complejos** como el valor devuelto escriba y, a continuación, especifique el tipo devuelto complejo específico eligiendo el tipo adecuado de la lista desplegable.

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   Haga clic en **Aceptar**.
    Se crea la entrada de importación de función en el modelo conceptual.

### <a name="customize-column-mapping-for-function-import"></a>Personalizar para la importación de función de la asignación de columnas

-   Haga clic en la importación de función en el Explorador de modelos y seleccione **asignación de importación de función**.
    El **detalles de Mapping** ventana aparece y muestra la asignación predeterminada para la importación de función. Las flechas indican las asignaciones entre los valores de columna y de propiedad. De forma predeterminada, se presupone que los nombres de columna son los mismos que los nombres de propiedad del tipo complejo. Los nombres de columna predeterminados aparecen en texto gris.
-   Si es necesario, cambie los nombres de columna para que coincidan con los nombres de columna devueltos por el procedimiento almacenado que corresponden a la importación de función.

## <a name="delete-a-complex-type"></a>Eliminar un tipo complejo

Al eliminar un tipo complejo, se elimina el tipo del modelo conceptual, así como las asignaciones para todas las instancias del tipo. Sin embargo, no se actualizan las referencias al tipo. Por ejemplo, si una entidad tiene una propiedad de tipo complejo de tipo ComplexType1 y ComplexType1 se elimina en el **Explorador de modelos**, no se actualiza la propiedad de entidad correspondiente. El modelo no se validará porque contiene una entidad que hace referencia a un tipo complejo eliminado. Puede actualizar o eliminar referencias a los tipos complejos eliminados utilizando Entity Designer.

-   Haga clic en un tipo complejo en el Explorador de modelos y seleccione **eliminar**.

- OR:

-   Seleccione un tipo complejo en el Explorador de modelos y presione la tecla Suprimir del teclado.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Consulta de entidades que contiene las propiedades de tipo complejo

El código siguiente muestra cómo ejecutar una consulta que devuelve una colección de entidad en objetos de tipo que contienen una propiedad de tipo complejo.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
