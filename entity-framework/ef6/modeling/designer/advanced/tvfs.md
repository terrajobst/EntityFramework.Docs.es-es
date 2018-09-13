---
title: Funciones con valores de tabla (TVF) - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489328"
---
# <a name="table-valued-functions-tvfs"></a>Funciones con valores de tabla (TVF)
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

El tutorial de vídeo y paso a paso muestra cómo asignar funciones con valores de tabla (TVF) mediante el Diseñador de Entity Framework. También se muestra cómo llamar a una función TVF desde una consulta LINQ.

TVF están actualmente solo se admite en la base de datos primer flujo de trabajo.

Compatibilidad TVF se introdujo en Entity Framework versión 5. Tenga en cuenta que para usar las nuevas características, como funciones con valores de tabla, enumeraciones y tipos espaciales debe tener como destino .NET Framework 4.5. Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.

TVF son muy similares a los procedimientos almacenados con una diferencia importante: el resultado de una función TVF es ajustable. Esto significa que los resultados de una función TVF pueden usarse en una consulta LINQ, mientras que los resultados de un procedimiento almacenado no.

## <a name="watch-the-video"></a>Vea el vídeo

**Presentado por**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Instalar el [base de datos School](~/ef6/resources/school-database.md).

- Tiene una versión reciente de Visual Studio

## <a name="set-up-the-project"></a>Configurar el proyecto

1.  Apertura de Visual Studio
2.  En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**
3.  En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla
4.  Escriba **TVF** como el nombre del proyecto y haga clic en **Aceptar**

## <a name="add-a-tvf-to-the-database"></a>Agregar una función TVF a la base de datos

-   Seleccione **vista -&gt; Explorador de objetos SQL Server**
-   Si LocalDB no está en la lista de servidores: haga doble clic en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse al servidor de LocalDB
-   Expanda el nodo de LocalDB
-   En el nodo bases de datos, haga clic en el nodo de base de datos School y seleccione **nueva consulta...**
-   En el Editor de Transact-SQL, pegue la siguiente definición de función TVF

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Haga clic en el botón secundario del mouse en el editor de Transact-SQL y seleccione **Execute**
-   La función GetStudentGradesForCourse se agrega a la base de datos School

 

## <a name="create-a-model"></a>Crear un modelo

1.  Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**
2.  Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el **plantillas** panel
3.  Escriba **TVFModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**
4.  En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**
5.  Haga clic en **nueva conexión** ENTRAR **(localdb)\\mssqllocaldb** en el texto de nombre de servidor cuadro ENTRAR **School** haga clic en el nombre de la base de datos **Aceptar**
6.  En el elija los objetos de base de datos cuadro de diálogo el **tablas** nodo, seleccione el **persona**, **StudentGrade**, y **curso** tablas
7.  Seleccione el **GetStudentGradesForCourse** función ubicado en el **procedimientos almacenados y funciones** nodo tenga en cuenta que a partir de Visual Studio 2012, Entity Designer le permite importar por lotes los procedimientos almacenados y funciones
8.  Haga clic en **finalizar**
9.  Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra. Todos los objetos que seleccionó en el **elija los objetos de base de datos** cuadro de diálogo se agregan al modelo.
10. De forma predeterminada, la forma del resultado de cada procedimiento almacenado importada o la función se convertirá automáticamente en un nuevo tipo complejo en el modelo de entidad. Pero queremos asignar los resultados de la función GetStudentGradesForCourse a la entidad StudentGrade: haga clic en la superficie de diseño y seleccione **Explorador de modelos** en el Explorador de modelos, seleccione **Function Imports**y, a continuación, haga doble clic en el **GetStudentGradesForCourse** función en el Editar función cuadro de diálogo Importar, seleccione **entidades** y elija **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Conservar y recuperar datos

Abra el archivo donde se define el método Main. Agregue el código siguiente en la función Main.

El código siguiente muestra cómo crear una consulta que utiliza una función con valores de tabla. La consulta proyecta los resultados en un tipo anónimo que contiene el título del curso relacionado y los alumnos relacionados con un grado mayor o igual a la versión 3.5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Compile y ejecute la aplicación. El programa produce el siguiente resultado:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Resumen

En este tutorial hemos examinado cómo asignar funciones con valores de tabla (TVF) mediante el Diseñador de Entity Framework. También muestra cómo llamar a una función TVF desde una consulta LINQ.
