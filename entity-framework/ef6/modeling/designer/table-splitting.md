---
title: La división de tablas diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f07aeb0aa679f6fa8131c667ac808f17c3f03f20
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250990"
---
# <a name="designer-table-splitting"></a>División de tabla del diseñador
En este tutorial se muestra cómo asignar varios tipos de entidad a una única tabla mediante la modificación de un modelo con Entity Framework Designer (Diseñador de EF).

Una razón que desea utilizar la división de tablas está retrasando la carga de algunas propiedades cuando se usa la carga para los objetos de la carga diferida. Puede separar las propiedades que es posible que contienen gran cantidad de datos en una entidad independiente y la carga solo cuando sea necesario.

La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

En este tutorial se usa Visual Studio 2012.

-   Abra Visual Studio 2012.
-   En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.
-   En el panel izquierdo, haga clic en Visual C\#y, a continuación, seleccione la plantilla de aplicación de consola.
-   Escriba **TableSplittingSample** como el nombre del proyecto y haga clic en **Aceptar**.

## <a name="create-a-model-based-on-the-school-database"></a>Crear un modelo basado en la base de datos School

-   Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **TableSplittingModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente.**
-   Haga clic en nueva conexión. En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el cuadro de diálogo Elija los objetos de base de datos, expandir el **tablas** nodo y compruebe el **persona** tabla. Esto agregará la tabla especificada en el **School** modelo.
-   Haga clic en **Finalizar**.

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra. Todos los objetos que seleccionó en el **elija los objetos de base de datos** cuadro de diálogo se agregan al modelo.

## <a name="map-two-entities-to-a-single-table"></a>Dos entidades se asignan a una sola tabla

En esta sección dividirá el **persona** entidad a dos entidades y, a continuación, se asignan a una única tabla.

> [!NOTE]
> El **persona** entidad no contiene todas las propiedades que pueden contener grandes cantidades de datos; simplemente sirve como ejemplo.

-   Haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **entidad**.
    El **nueva entidad** aparece el cuadro de diálogo.
-   Tipo **HireInfo** para el **nombre de entidad** y **PersonID** para el **propiedad Key** nombre.
-   Haga clic en **Aceptar**.
-   Se crea y se muestra un nuevo tipo de entidad en la superficie de diseño.
-   Seleccione el **HireDate** propiedad de la **persona** tipo de entidad y presione **CTRL+x** claves.
-   Seleccione el **HireInfo** entidad y presione **CTRL+v** claves.
-   Crear una asociación entre **persona** y **HireInfo**. Para ello, haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **asociación**.
-   El **Agregar asociación** aparece el cuadro de diálogo. El **PersonHireInfo** se asigna el nombre de forma predeterminada.
-   Especificar la multiplicidad **1(One)** en ambos extremos de la relación.
-   Presione **Aceptar**.

El paso siguiente requiere la **detalles de Mapping** ventana. Si no puede ver esta ventana, haga clic en la superficie de diseño y seleccione **detalles de Mapping**.

-   Seleccione el **HireInfo** tipo de entidad y haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.
-   Seleccione **persona** desde el **&lt;agregar una tabla o vista&gt;** lista de campos de lista desplegable. La lista contiene las tablas o vistas a la que se puede asignar la entidad seleccionada.
    Las propiedades adecuadas deben asignarse de forma predeterminada.

    ![Asignación](~/ef6/media/mapping.png)

-   Seleccione el **PersonHireInfo** asociación en la superficie de diseño.
-   Haga clic en la asociación en la superficie de diseño y seleccione **propiedades**.
-   En el **propiedades** ventana, seleccione el **las restricciones referenciales** propiedad y haga clic en el botón de puntos suspensivos.
-   Seleccione **persona** desde el **Principal** lista desplegable.
-   Presione **Aceptar**.

 

## <a name="use-the-model"></a>Usar el modelo

-   Pegue el código siguiente en el método Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Compile y ejecute la aplicación.

Las siguientes instrucciones de T-SQL se ejecutaron en el **School** base de datos como resultado de ejecutar esta aplicación. 

-   La siguiente **insertar** se ejecutó como resultado de ejecutar el contexto. SaveChanges() y combina datos de la **persona** y **HireInfo** entidades

    ![Insertar](~/ef6/media/insert.png)

-   La siguiente **seleccione** se ejecutó como resultado de ejecutar el contexto. People.FirstOrDefault() y selecciona solo las columnas asignadas a **persona**

    ![Seleccione 1](~/ef6/media/select1.png)

-   La siguiente **seleccione** se ejecutó como consecuencia de tener acceso a la existingPerson.Instructor de propiedad de navegación y selecciona solo las columnas asignadas a **HireInfo**

    ![Seleccione 2](~/ef6/media/select2.png)
