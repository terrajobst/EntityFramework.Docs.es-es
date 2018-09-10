---
title: Separación de entidad diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250603"
---
# <a name="designer-entity-splitting"></a>División de entidades de diseñador
En este tutorial se muestra cómo asignar un tipo de entidad a dos tablas modificando un modelo con Entity Framework Designer (Diseñador de EF). Puede asignar una entidad a varias tablas cuando estas comparten una clave común. Los conceptos que se aplican en la asignación de un tipo de entidad a dos tablas se extienden con facilidad a la asignación de un tipo de entidad a más de dos tablas.

La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Requisitos previos

Edición visual de Studio 2012 o Visual Studio 2010, Ultimate, Premium, Professional o Web Express.

## <a name="create-the-database"></a>Crear la base de datos

El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:

-   Si usa Visual Studio 2012, a continuación, se creará una base de datos LocalDB.
-   Si usa Visual Studio 2010 se creará una base de datos de SQL Express.

En primer lugar, vamos a crear una base de datos con dos tablas que vamos a combinar en una única entidad.

-   Apertura de Visual Studio
-   **Vista -&gt; Explorador de servidores**
-   Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**
-   Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar **Microsoft SQL Server** como origen de datos
-   Conectarse a LocalDB o Express de SQL, según lo que se ha instalado
-   Escriba **EntitySplitting** como el nombre de la base de datos
-   Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**
-   La nueva base de datos aparecerán en el Explorador de servidores
-   Si usa Visual Studio 2012
    -   Haga doble clic en la base de datos en el Explorador de servidores y seleccione **nueva consulta**
    -   Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**
-   Si usa Visual Studio 2010
    -   Seleccione **datos -&gt; Transact SQL Editor -&gt; nueva conexión de consulta...**
    -   Escriba **.\\ SQLEXPRESS** como el nombre del servidor y haga clic en **Aceptar**
    -   Seleccione el **EntitySplitting** de base de datos en la lista desplegable en la parte superior del editor de consultas
    -   Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **ejecutar SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Crear el proyecto

-   En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **aplicación de consola** plantilla.
-   Escriba **MapEntityToTablesSample** como el nombre del proyecto y haga clic en **Aceptar**.
-   Haga clic en **n** si se le pide que guarde la consulta SQL que creó en la primera sección.

## <a name="create-a-model-based-on-the-database"></a>Crear un modelo basado en la base de datos

-   Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **MapEntityToTablesModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente.**
-   Seleccione el **EntitySplitting** conexión en la lista desplegable y haga clic en **siguiente**.
-   En el cuadro de diálogo Elija los objetos de base de datos, active la casilla junto a la **tablas** nodo.
    Esta acción agregará todas las tablas de la **EntitySplitting** base de datos para el modelo.
-   Haga clic en **Finalizar**.

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.

## <a name="map-an-entity-to-two-tables"></a>Asignar una entidad a dos tablas

En este paso se actualizará el **persona** tipo de entidad para combinar datos de la **persona** y **PersonInfo** tablas.

-   Seleccione el **correo electrónico** y **teléfono** propiedades de la ** PersonInfo ** entidad y presione **CTRL+x** claves.
-   Seleccione el ** persona ** entidad y presione **CTRL+v** claves.
-   En la superficie de diseño, seleccione el **PersonInfo** entidad y presione **eliminar** botón en el teclado.
-   Haga clic en **No** cuando se le pregunte si desea quitar el **PersonInfo** tabla del modelo, que se van a asignar a la **persona** entidad.

    ![Eliminar tablas](~/ef6/media/deletetables.png)

Los pasos siguientes requieren la **detalles de Mapping** ventana. Si no puede ver esta ventana, haga clic en la superficie de diseño y seleccione **detalles de Mapping**.

-   Seleccione el **persona** tipo de entidad y haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.
-   Seleccione ** PersonInfo ** en la lista desplegable.
    El **detalles de Mapping** ventana se actualiza con las asignaciones de columnas de forma predeterminada, estos están bien para nuestro escenario.

El **persona** tipo de entidad está asignado ahora a la **persona** y **PersonInfo** tablas.

![Asignación de 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Usar el modelo

-   Pegue el código siguiente en el método Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Compile y ejecute la aplicación.

Las siguientes instrucciones de T-SQL se ejecutaron en la base de datos como resultado de ejecutar esta aplicación. 

-   Las dos siguientes **insertar** las instrucciones que se ejecutaron como resultado de ejecutar el contexto. SaveChanges(). Obtienen los datos el **persona** entidad y dividirla entre el **persona** y **PersonInfo** tablas.

    ![Insertar 1](~/ef6/media/insert1.png)

    ![Insertar 2](~/ef6/media/insert2.png)
-   La siguiente **seleccione** se ejecutó como resultado de la enumeración de las personas de la base de datos. Combina los datos de la **persona** y **PersonInfo** tabla.

    ![Seleccionar](~/ef6/media/select.png)
