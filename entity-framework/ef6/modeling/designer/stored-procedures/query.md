---
title: 'Consulta del diseñador procedimientos almacenados: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 6284b10261e6f3b9bf69d1c15e121988e4976d48
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489952"
---
# <a name="designer-query-stored-procedures"></a>Consulta del Diseñador de procedimientos almacenados
En este tutorial paso a paso muestra cómo usar el Diseñador de Entity Framework (EF Designer) para importar los procedimientos almacenados en un modelo y, a continuación, llamar a los procedimientos almacenados importados para recuperar los resultados. 

Tenga en cuenta que Code First no admite la asignación a procedimientos almacenados o funciones. Sin embargo, puede llamar a procedimientos almacenados o funciones mediante el método System.Data.Entity.DbSet.SqlQuery. Por ejemplo:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

-   Abra Visual Studio 2012.
-   Seleccione **archivo -&gt; nuevo:&gt; proyecto**
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.
-   Escriba **EFwithSProcsSample** como el nombre.
-   Seleccione **Aceptar**.

## <a name="create-a-model"></a>Crear un modelo

-   Haga clic en el proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **EFwithSProcsModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.
-   Haga clic en **nueva conexión**.  
    En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.  
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el cuadro de diálogo Elija los objetos de base de datos, compruebe el **tablas** casilla de verificación para seleccionar todas las tablas.  
    Además, seleccione los siguientes procedimientos almacenados en el **procedimientos almacenados y funciones** nodo: **GetStudentGrades** y **GetDepartmentName**. 

    ![Importar](~/ef6/media/import.jpg)

    *A partir de Visual Studio 2012, EF Designer admite la importación masiva de los procedimientos almacenados. El **Importar seleccionada procedimientos almacenados y funciones en el modelo de theentity** está activada de forma predeterminada.*
-   Haga clic en **Finalizar**.

De forma predeterminada, la forma del resultado de cada procedimiento almacenado importada o la función que devuelve más de una columna se convertirá automáticamente en un nuevo tipo complejo. En este ejemplo desea asignar los resultados de la **GetStudentGrades** función a la **StudentGrade** entidad y los resultados de la **GetDepartmentName** a **ninguno** (**ninguno** es el valor predeterminado).

Para que una importación de función devolver un tipo de entidad, las columnas devueltas por el procedimiento almacenado correspondiente deben coincidir exactamente con las propiedades escalares del tipo de entidad devuelta. Una importación de función también puede devolver colecciones de tipos simples, tipos complejos o ningún valor.

-   Haga clic en la superficie de diseño y seleccione **Explorador de modelos**.
-   En **Explorador de modelos**, seleccione **Function Imports**y, a continuación, haga doble clic en el **GetStudentGrades** función.
-   En el cuadro de diálogo Editar importación de función, seleccione **entidades** y elija **StudentGrade**.  
    *El **importación de función es ajustable** casilla en la parte superior de la **Function Imports** cuadro de diálogo le permitirá asignar a las funciones que admite composición. Si activa esta casilla, solo las funciones que se pueden componer (funciones con valores de tabla) se mostrarán en el **Stored Procedure / nombre de la función** lista desplegable. Si no activa esta casilla, se mostrará solo las funciones que no se pueden componer en la lista.*

## <a name="use-the-model"></a>Usar el modelo

Abra el **Program.cs** donde el **Main** se define el método. Agregue el código siguiente en la función Main.

El código llama a dos procedimientos almacenados: **GetStudentGrades** (devuelve **StudentGrades** especificado *StudentId*) y **GetDepartmentName** (devuelve el nombre del departamento en el parámetro de salida).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Compile y ejecute la aplicación. El programa produce el siguiente resultado:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parámetros de salida
-----------------

Si se usan parámetros de salida, sus valores no estará disponibles hasta que se han leído completamente los resultados. Esto es debido al comportamiento subyacente de DbDataReader, vea [recuperar datos mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obtener más detalles.
