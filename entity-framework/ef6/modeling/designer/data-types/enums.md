---
title: Compatibilidad de enumeraciones - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 331182c4311565c94cf072eb9b9ad372ac76180a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283945"
---
# <a name="enum-support---ef-designer"></a>Compatibilidad con enum - EF Designer
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

En este tutorial de vídeo y paso a paso muestra cómo usar los tipos de enumeración con Entity Framework Designer. También se muestra cómo usar las enumeraciones en una consulta LINQ.

En este tutorial usará Model First para crear una nueva base de datos, pero también se puede usar el Diseñador de EF con el [Database First](~/ef6/modeling/designer/workflows/database-first.md) flujo de trabajo para asignar a una base de datos existente.

Compatibilidad de la enumeración se introdujo en Entity Framework 5. Para usar las nuevas características como enumeraciones, tipos de datos espaciales y funciones con valores de tabla, debe tener como destino .NET Framework 4.5. Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.

En Entity Framework, una enumeración puede tener los siguientes tipos subyacentes: **bytes**, **Int16**, **Int32**, **Int64** , o **SByte**.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo muestra cómo usar los tipos de enumeración con Entity Framework Designer. También se muestra cómo usar las enumeraciones en una consulta LINQ.

**Presentado por**: Julia Kornich

**Vídeo**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Requisitos previos

Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.

## <a name="set-up-the-project"></a>Configurar el proyecto

1.  Abra Visual Studio 2012
2.  En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**
3.  En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla
4.  Escriba **EnumEFDesigner** como el nombre del proyecto y haga clic en **Aceptar**

## <a name="create-a-new-model-using-the-ef-designer"></a>Crear un nuevo modelo con EF Designer

1.  Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**
2.  Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel de plantillas
3.  Escriba **EnumTestModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**
4.  En la página del Asistente para Entity Data Model, seleccione **modelo vacío** en el cuadro de diálogo Elegir contenido del modelo
5.  Haga clic en **finalizar**

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.

El asistente realiza las siguientes acciones:

-   Genera el archivo EnumTestModel.edmx que define el modelo conceptual, el modelo de almacenamiento y la asignación entre ellos. Establece la propiedad Metadata Artifact Processing del archivo .edmx para insertar en el ensamblado de salida para que los archivos de metadatos generados se incrustan en el ensamblado.
-   Agrega una referencia a los ensamblados siguientes: Entity Framework, System.ComponentModel.DataAnnotations y System.Data.Entity.
-   Crea los archivos EnumTestModel.tt y EnumTestModel.Context.tt y los agrega en el archivo .edmx. Estos archivos de plantilla T4 generan el código que define el tipo de DbContext derivado y tipos POCO que se asignan a las entidades en el modelo de edmx.

## <a name="add-a-new-entity-type"></a>Agregar un nuevo tipo de entidad

1.  Haga clic en un área vacía de la superficie de diseño, seleccione **Add -&gt; entidad**, aparece el cuadro de diálogo New Entity
2.  Especificar **departamento** para el tipo de nombre y especificar **DepartmentID** para el nombre de propiedad de clave, deje el tipo como **Int32**
3.  Haga clic en **Aceptar**.
4.  Haga clic en la entidad y seleccione **Add New -&gt; propiedad escalar**
5.  Cambiar el nombre de la nueva propiedad a **nombre**
6.  Cambiar el tipo de la nueva propiedad a **Int32** (de forma predeterminada, la nueva propiedad es de tipo cadena) para cambiar el tipo, abra la ventana Propiedades y cambie la propiedad de tipo a **Int32**
7.  Agregar otra propiedad escalar y cambie su nombre a **presupuesto**, cambie el tipo a **Decimal**

## <a name="add-an-enum-type"></a>Agregar un tipo de enumeración

1.  En Entity Framework Designer, haga clic en la propiedad Name, seleccione **convertir para una enumeración**

    ![Convertir en Enum](~/ef6/media/converttoenum.png)

2.  En el **agregar Enum** tipo de cuadro de diálogo **DepartmentNames** para el nombre del tipo Enum, cambie el tipo subyacente a **Int32**, y, a continuación, agregue los siguientes miembros para el tipo: inglés, Matemáticas y ahorro

    ![Agregar tipo de enumeración](~/ef6/media/addenumtype.png)

3.  Presione **Aceptar**
4.  Guardar el modelo y compile el proyecto
    > [!NOTE]
    > Cuando se compila, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores. Puede omitir estas advertencias porque una vez que se elija Generar la base de datos del modelo, los errores desaparecerán.

Si observa la ventana Propiedades, observará que el tipo de la propiedad Name se cambiara a **DepartmentNames** y el tipo de enumeración recién agregada se agregó a la lista de tipos.

Si cambia a la ventana del explorador de modelos, verá que el tipo también se ha agregado al nodo de tipos de enumeración.

![Explorador de modelos](~/ef6/media/modelbrowser.png)

>[!NOTE]
> También puede agregar nuevos tipos de enumeración desde esta ventana, haga clic en el botón secundario del mouse y seleccione **Agregar tipo de enumeración**. Una vez creado el tipo aparecerá en la lista de tipos y podrá asociar a una propiedad

## <a name="generate-database-from-model"></a>Generar la base de datos de modelo

Ahora podemos generar una base de datos que se basa en el modelo.

1.  Haga clic en un espacio vacío en el Entity Designer superficie y seleccione **generar base de datos de modelo**
2.  Haga clic en mostrará la elija el cuadro de diálogo de conexión de datos del Asistente para generar base de datos es el **nueva conexión** botón especifique **(localdb)\\mssqllocaldb** para el nombre del servidor y  **EnumTest** para la base de datos y haga clic en **Aceptar**
3.  Se abrirá, haga clic en un cuadro de diálogo que le pregunta si desea crear una nueva base de datos **Sí**.
4.  Haga clic en **siguiente** y Asistente para crear base de datos genera el lenguaje de definición de datos (DDL) para crear una base de datos de la DDL generada se muestra en el resumen y una nota de cuadro de diálogo Configuración, el DDL no contiene una definición para un tabla que se asigna al tipo de enumeración
5.  Haga clic en **finalizar** no ejecuta el script DDL al hacer clic en Finalizar.
6.  El Asistente para crear base de datos hace lo siguiente: abre el **EnumTest.edmx.sql** en T-SQL, Editor genera las secciones de esquema y asignación de almacén de EDMX archivo agrega información de la cadena de conexión al archivo App.config
7.  Haga clic en el botón secundario del mouse en el Editor de Transact-SQL y seleccione **Execute** aparece el diálogo Conectar a servidor, escriba la información de conexión del paso 2 y haga clic en **Connect**
8.  Para ver el esquema generado, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**

## <a name="persist-and-retrieve-data"></a>Conservar y recuperar datos

Abra el archivo Program.cs, donde se define el método Main. Agregue el código siguiente en la función Main. El código agrega un nuevo objeto de departamento en el contexto. A continuación, guarda los datos. El código también ejecuta una consulta LINQ que devuelve un departamento donde el nombre es DepartmentNames.English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compile y ejecute la aplicación. El programa produce el siguiente resultado:

```
DepartmentID: 1 Name: English
```

Para ver los datos en la base de datos, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**. A continuación, haga clic en el botón secundario del mouse en la tabla y seleccione **ver datos**.

## <a name="summary"></a>Resumen

En este tutorial explicamos cómo asignar tipos de enumeración mediante el Diseñador de Entity Framework y cómo usar las enumeraciones en el código. 
