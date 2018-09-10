---
title: 'Diseñador CUD procedimientos almacenados: EF6'
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 36c9b97b77fec30136cba1d850a0259c689e69ae
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250925"
---
# <a name="designer-cud-stored-procedures"></a>Diseñador CUD procedimientos almacenados
En este tutorial paso a paso muestran cómo asignar el crear\\Insertar, actualizar y eliminar operaciones (CUD) de un tipo de entidad a procedimientos almacenados mediante el Diseñador de Entity Framework (EF Designer).  De forma predeterminada, Entity Framework genera automáticamente las instrucciones SQL para las operaciones CUD, pero también se pueden asignar procedimientos almacenados a estas operaciones.  

Tenga en cuenta que Code First no admite la asignación a procedimientos almacenados o funciones. Sin embargo, puede llamar a procedimientos almacenados o funciones mediante el método System.Data.Entity.DbSet.SqlQuery. Por ejemplo:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Consideraciones al asignar las operaciones CUD en procedimientos almacenados

Al asignar las operaciones CUD a procedimientos almacenados, se aplican las consideraciones siguientes: 

-   Si una de las operaciones CUD va a asignar a un procedimiento almacenado, se asignan todas ellas. Si no asigna las tres, las operaciones no asignadas se producirá un error si se ejecuta y un **UpdateException** se iniciará.
-   Todos los parámetros del procedimiento almacenado deben asignar las propiedades de entidad.
-   Si el servidor genera el valor de clave principal para la fila insertada, debe asignar este valor a la propiedad de clave de la entidad. En el ejemplo siguiente, la **InsertPerson** procedimiento almacenado devuelve la clave principal recién creada como parte del conjunto de resultados del procedimiento almacenado. La clave principal se asigna a la clave de entidad (**PersonID**) mediante el **&lt;Agregar Result Binding&gt;** característica de EF Designer.
-   Las llamadas de procedimiento almacenado son asignado 1:1 con las entidades del modelo conceptual. Por ejemplo, si implementa una jerarquía de herencia en el modelo conceptual y después asigne el CUD procedimientos almacenados para la **primario** (base) y el **secundarios** entidades (derivadas), guardar el **Secundarios** cambios solo se llamará el **secundarios**de procedimientos almacenados, no se desencadenará la **primario**de llamadas a procedimientos almacenados.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

-   Abra Visual Studio 2012.
-   Seleccione **archivo -&gt; nuevo:&gt; proyecto**
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.
-   Escriba **CUDSProcsSample** como el nombre.
-   Seleccione **Aceptar**.

## <a name="create-a-model"></a>Crear un modelo

-   Haga clic en el nombre del proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **CUDSProcs.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.
-   Haga clic en **nueva conexión**. En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el elija los objetos de base de datos cuadro de diálogo el **tablas** nodo, seleccione el **persona** tabla.
-   Además, seleccione los siguientes procedimientos almacenados en el **procedimientos almacenados y funciones** nodo: **DeletePerson**, **InsertPerson**, y **UpdatePerson** . 
-   A partir de Visual Studio 2012, EF Designer admite la importación masiva de los procedimientos almacenados. El **Importar seleccionada procedimientos almacenados y funciones en el modelo de entidad** está activada de forma predeterminada. Puesto que en este ejemplo nos hemos procedimientos almacenados que insertarán, actualización y eliminación tipos de entidad, quiere importarlos y se desactiva esta casilla de verificación. 

    ![Importar S procedimientos](~/ef6/media/importsprocs.jpg)

-   Haga clic en **Finalizar**.
    EF Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.

## <a name="map-the-person-entity-to-stored-procedures"></a>Asignar la entidad Person a procedimientos almacenados

-   Haga clic en el **persona** tipo de entidad y seleccione **asignación de procedimientos almacenados**.
-   Las asignaciones de procedimiento almacenado que aparecen en la **detalles de Mapping** ventana.
-   Haga clic en  **&lt;seleccione Insertar función&gt;**.
    El campo se convierte en una lista desplegable de los procedimientos almacenados del modelo de almacenamiento que se puede asignar a tipos de entidad del modelo conceptual.
    Seleccione **InsertPerson** en la lista desplegable.
-   Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad. Observe que las flechas indican la dirección de la asignación: se proporcionan valores de propiedad como parámetros de procedimiento almacenado.
-   Haga clic en  **&lt;Agregar Result Binding&gt;**.
-   Tipo **NewPersonID**, el nombre del parámetro devuelto por la **InsertPerson** procedimiento almacenado. Asegúrese de no escriba espacios iniciales o finales.
-   Presione **ENTRAR**.
-   De forma predeterminada, **NewPersonID** se asigna a la clave de entidad **PersonID**. Observe que una flecha indica la dirección de la asignación: el valor de la columna de resultado se proporciona para la propiedad.

    ![Detalles de la asignación](~/ef6/media/mappingdetails.png)

-   Haga clic en **&lt;seleccionar Update Function&gt;** y seleccione **UpdatePerson** en la lista desplegable resultante.
-   Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.
-   Haga clic en **&lt;Seleccionar Delete Function&gt;** y seleccione **DeletePerson** en la lista desplegable resultante.
-   Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.

La inserción, actualizar y eliminar operaciones de la **persona** tipo de entidad están asignadas ahora a procedimientos almacenados.

Si desea habilitar la comprobación de concurrencia al actualizar o eliminar una entidad con procedimientos almacenados, utilice una de las opciones siguientes:

-   Use un **salida** afectados de parámetro para devolver el número de filas desde el procedimiento almacenado y compruebe el **Rowsaffectedparameter** casilla de verificación junto al nombre del parámetro. Si el valor devuelto es cero cuando se llama a la operación, un [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) se iniciará.
-   Compruebe el **usar Original Value** casilla de verificación junto a una propiedad que se va a usar para la comprobación de simultaneidad. Cuando se intenta realizar una actualización, se usará el valor de la propiedad que se leyó originalmente de la base de datos cuando realiza una escritura de datos en la base de datos. Si el valor no coincide con el valor de la base de datos, un **OptimisticConcurrencyException** se iniciará.

## <a name="use-the-model"></a>Usar el modelo

Abra el **Program.cs** donde el **Main** se define el método. Agregue el código siguiente en la función Main.

El código crea un nuevo **persona** objeto, a continuación, actualiza el objeto y finalmente elimina el objeto.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   Compile y ejecute la aplicación. El programa produce el siguiente resultado *
    >[!NOTE]
> PersonID es generado automáticamente por el servidor, por lo que verá más probable es que un número diferente *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Si está trabajando con la versión de Visual Studio Ultimate, puede utilizar Intellitrace con el depurador para ver las instrucciones SQL que se ejecutan.

![IntelliTrace](~/ef6/media/intellitrace.png)
