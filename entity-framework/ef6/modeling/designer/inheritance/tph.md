---
title: Herencia de TPH diseñador - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490134"
---
# <a name="designer-tph-inheritance"></a>Herencia de TPH Diseñador
En este tutorial paso a paso muestra cómo implementar la herencia de tabla por jerarquía (TPH) en el modelo conceptual con Entity Framework Designer (Diseñador de EF). Herencia de TPH emplea una tabla de base de datos para mantener los datos para todos los tipos de entidad en una jerarquía de herencia.

En este tutorial se asignará la tabla Person a tres tipos de entidad: persona (el tipo base), Student (que se deriva de persona) e Instructor (que se deriva de persona). Crearemos un modelo conceptual de la base de datos (Database First) y, a continuación, modificar el modelo para implementar la herencia de TPH con EF Designer.

Es posible asignar a una herencia de TPH con Model First, pero tendría que escribir su propio flujo de trabajo de generación de base de datos que es compleja. A continuación, podría asignar este flujo de trabajo la **flujo de trabajo de generación de base de datos** propiedad en EF Designer. Una alternativa más sencilla es usar Code First.

## <a name="other-inheritance-options"></a>Otras opciones de herencia

Tabla por tipo (TPT) es otro tipo de herencia en el que las tablas independientes en la base de datos se asignan a entidades que participan en la herencia.  Para obtener información acerca de cómo asignar la herencia de tabla por tipo con EF Designer, vea [herencia de TPT de EF Designer](~/ef6/modeling/designer/inheritance/tpt.md).

Herencia de tipo de tabla por concreto (TPC) y modelos de herencia mixto son compatibles con el tiempo de ejecución de Entity Framework, pero no son compatibles con EF Designer. Si desea usar TPC o herencia mixta, tiene dos opciones: usar Code First o editar manualmente el archivo EDMX. Si decide trabajar con el archivo EDMX, la ventana de detalles de asignación se colocará en "modo seguro" y no podrá usar el diseñador para cambiar las asignaciones.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

-   Abra Visual Studio 2012.
-   Seleccione **archivo -&gt; nuevo:&gt; proyecto**
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.
-   Escriba **TPHDBFirstSample** como el nombre.
-   Seleccione **Aceptar**.

## <a name="create-a-model"></a>Crear un modelo

-   Haga clic en el nombre del proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **TPHModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.
-   Haga clic en **nueva conexión**.
    En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el cuadro de diálogo Elija los objetos de base de datos en el nodo tablas, seleccione el **persona** tabla.
-   Haga clic en **Finalizar**.

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra. Todos los objetos que seleccionó en el cuadro de diálogo Elija los objetos de base de datos se agregan al modelo.

Que es el modo **persona** tabla busca en la base de datos.

![Tabla Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementar la herencia de tabla por jerarquía

El **persona** tabla tiene el **discriminador** columna, que puede tener uno de dos valores: "Student" e "Instructor". Dependiendo del valor el **persona** tabla se asignará a la **estudiante** entidad o la **Instructor** entidad. El **persona** tabla también tiene dos columnas, **HireDate** y **EnrollmentDate**, que debe ser **que acepta valores NULL** porque no puede ser una persona un Student e instructor al mismo tiempo (al menos no en este tutorial).

### <a name="add-new-entities"></a>Agregar nuevas entidades

-   Agregar una nueva entidad.
    Para ello, haga doble clic en un área vacía de la superficie de diseño de Entity Framework Designer y seleccione **Add -&gt;entidad**.
-   Tipo **Instructor** para el **nombre de entidad** y seleccione **persona** en la lista desplegable para la **tipo Base**.
-   Haga clic en **Aceptar**.
-   Agregue otra nueva entidad. Tipo **estudiante** para el **nombre de entidad** y seleccione **persona** en la lista desplegable para la **tipo Base**.

Se agregaron dos nuevos tipos de entidad a la superficie de diseño. Una flecha señala desde los nuevos tipos de entidad a la **persona** tipo de entidad; esto indica que **persona** es el tipo base para los nuevos tipos de entidad.

-   Haga clic en el **HireDate** propiedad de la **persona** entidad. Seleccione **cortar** (o utilizar la tecla Ctrl-X).
-   Haga clic en el **Instructor** entidad y seleccione **pegar** (o utilizar la tecla Ctrl-V).
-   Haga clic en el **HireDate** propiedad y seleccione **propiedades**.
-   En el **propiedades** ventana, establezca el **Nullable** propiedad **false**.
-   Haga clic en el **EnrollmentDate** propiedad de la **persona** entidad. Seleccione **cortar** (o utilizar la tecla Ctrl-X).
-   Haga clic en el **estudiante** entidad y seleccione **pegar (o clave de uso de la combinación Ctrl-V).**
-   Seleccione el **EnrollmentDate** propiedad y establezca el **Nullable** propiedad **false**.
-   Seleccione el **persona** tipo de entidad. En el **propiedades** ventana, establezca su **abstracta** propiedad **true**.
-   Eliminar el **discriminador** propiedad desde **persona**. En la sección siguiente, se explica el motivo debe eliminarse.

### <a name="map-the-entities"></a>Asigne las entidades

-   Haga clic en el **Instructor** y seleccione **asignación de tabla.**
    La entidad Instructor está seleccionada en la ventana Detalles de la asignación.
-   Haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.
    El **&lt;agregar una tabla o vista&gt;** campo se convierte en una lista desplegable de tablas o vistas a que se puede asignar la entidad seleccionada.
-   Seleccione **persona** en la lista desplegable.
-   El **detalles de Mapping** ventana se actualiza con las asignaciones de columna predeterminadas y una opción para agregar una condición.
-   Haga clic en  **&lt;agregar una condición&gt;**.
    El **&lt;agregar una condición&gt;** campo se convierte en una lista desplegable de columnas para el que se pueden establecer condiciones.
-   Seleccione **discriminador** en la lista desplegable.
-   En el **operador** columna de la **detalles de Mapping** ventana, seleccione = de la lista desplegable.
-   En el **valor/propiedad** columna, escriba **Instructor**. El resultado final debe tener este aspecto:

    ![Detalles de la asignación](~/ef6/media/mappingdetails2.png)

-   Repita estos pasos para la **estudiante** tipo de entidad, pero asegúrese de la condición igual a **estudiante** valor.  
    *La razón por la que desea quitar el **discriminador** propiedad, es porque no se puede asignar una columna de tabla de más de una vez. Esta columna se utilizará para la asignación condicional, por lo que no se puede usar para la asignación de propiedad así. La única manera que puede usarse en ambos casos, si utiliza una condición de un **Is Null** o **Is Not Null** comparación.*

Ahora se implementa la herencia de tabla por jerarquía.

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Usar el modelo

Abra el **Program.cs** donde el **Main** se define el método. Pegue el código siguiente en el **Main** función. El código ejecuta tres consultas. La primera consulta devuelve todos los **persona** objetos. La segunda consulta utiliza la **OfType** método devuelva **Instructor** objetos. La tercera consulta utiliza la **OfType** método devuelva **estudiante** objetos.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
