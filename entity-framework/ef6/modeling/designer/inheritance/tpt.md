---
title: Herencia de TPT diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996353"
---
# <a name="designer-tpt-inheritance"></a>Herencia de TPT Diseñador
Este tutorial paso a paso muestra cómo implementar la herencia de tabla por tipo (TPT) en el modelo usando el Diseñador de Entity Framework (EF Designer). La herencia de tabla por tipo utiliza una tabla independiente de la base de datos para mantener los datos de las propiedades no heredadas y de las propiedades de clave para cada tipo de la jerarquía de herencia.

En este tutorial se asignará el **curso** (tipo base), **OnsiteCourse** (se deriva de curso), y **OnsiteCourse** (se deriva de **curso**) entidades a tablas con los mismos nombres. Crearemos un modelo de la base de datos y, a continuación, modificar el modelo para implementar la herencia de TPT.

Puede también iniciar con Model First y, a continuación, generar la base de datos del modelo. EF Designer usa la estrategia de TPT de forma predeterminada y, por lo que cualquier herencia en el modelo se asignan a tablas independientes.

## <a name="other-inheritance-options"></a>Otras opciones de herencia

Tabla por jerarquía (TPH) es otro tipo de herencia en la base de datos de una tabla se utiliza para mantener los datos de todos los tipos de entidad en una jerarquía de herencia.  Para obtener información acerca de cómo asignar la herencia de tabla por jerarquía con Entity Designer, vea [la herencia de TPH de EF Designer](~/ef6/modeling/designer/inheritance/tph.md). 

Tenga en cuenta que, la tabla concreta por tipo de herencia (TPC) y herencia mixta modelos son compatibles con el tiempo de ejecución de Entity Framework, pero no son compatibles con EF Designer. Si desea usar TPC o herencia mixta, tiene dos opciones: usar Code First o editar manualmente el archivo EDMX. Si decide trabajar con el archivo EDMX, la ventana de detalles de asignación se colocará en "modo seguro" y no podrá usar el diseñador para cambiar las asignaciones.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

-   Abra Visual Studio 2012.
-   Seleccione **archivo -&gt; nuevo:&gt; proyecto**
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.
-   Escriba **TPTDBFirstSample** como el nombre.
-   Seleccione **Aceptar**.

## <a name="create-a-model"></a>Crear un modelo

-   Haga clic en el proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **TPTModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione ** Generar desde la base de datos ** y, a continuación, haga clic en **siguiente**.
-   Haga clic en **nueva conexión**.
    En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el cuadro de diálogo Elija los objetos de base de datos en el nodo tablas, seleccione el **departamento**, **curso OnsiteCourse y OnsiteCourse** tablas.
-   Haga clic en **Finalizar**.

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra. Todos los objetos que seleccionó en el cuadro de diálogo Elija los objetos de base de datos se agregan al modelo.

## <a name="implement-table-per-type-inheritance"></a>Implementar la herencia de tabla por tipo

-   En la superficie de diseño, haga clic en el **OnsiteCourse** tipo de entidad y seleccione **propiedades**.
-   En el **propiedades** ventana, establezca la propiedad de tipo Base en **curso**.
-   Haga clic en el **OnsiteCourse** tipo de entidad y seleccione **propiedades**.
-   En el **propiedades** ventana, establezca la propiedad de tipo Base en **curso**.
-   Haga clic en la asociación (la línea) entre el **OnsiteCourse** y **curso** tipos de entidad.
    Seleccione **eliminar del modelo**.
-   Haga clic en la asociación entre el **OnsiteCourse** y **curso** tipos de entidad.
    Seleccione **eliminar del modelo**.

Ahora eliminaremos el **CourseID** propiedad desde **OnsiteCourse** y **OnsiteCourse** porque estas clases heredan **CourseID** desde el **curso** tipo base.

-   Haga clic en el **CourseID** propiedad de la **OnsiteCourse** tipo de entidad y, a continuación, seleccione **eliminar del modelo**.
-   Haga clic en el **CourseID** propiedad de la **OnsiteCourse** tipo de entidad y, a continuación, seleccione **eliminar del modelo**
-   Ya está implementada la herencia de tabla por tipo.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Usar el modelo

Abra el **Program.cs** donde el **Main** se define el método. Pegue el código siguiente en el **Main** función. El código ejecuta tres consultas. La primera consulta devuelve todos los **cursos** relacionadas con el departamento especificado. La segunda consulta utiliza la **OfType** método devuelva **Online** relacionadas con el departamento especificado. Devuelve la tercera consulta **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
