---
title: Compatibilidad con enum - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 529370ef7aba3975a36cbfdf5c439ee87b563c7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997239"
---
# <a name="enum-support---code-first"></a>Compatibilidad con enum - Code First
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

En este tutorial de vídeo y paso a paso muestra cómo usar los tipos de enumeración con Entity Framework Code First. También se muestra cómo usar las enumeraciones en una consulta LINQ.

En este tutorial utilizará Code First para crear una nueva base de datos, pero también puede usar [Code First para asignar a una base de datos](~/ef6/modeling/code-first/workflows/existing-database.md).

Compatibilidad de la enumeración se introdujo en Entity Framework 5. Para usar las nuevas características como enumeraciones, tipos de datos espaciales y funciones con valores de tabla, debe tener como destino .NET Framework 4.5. Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.

En Entity Framework, una enumeración puede tener los siguientes tipos subyacentes: **bytes**, **Int16**, **Int32**, **Int64** , o **SByte**.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo muestra cómo usar los tipos de enumeración con Entity Framework Code First. También se muestra cómo usar las enumeraciones en una consulta LINQ.

**Presentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Requisitos previos

Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.

 

## <a name="set-up-the-project"></a>Configurar el proyecto

1.  Abra Visual Studio 2012
2.  En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**
3.  En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla
4.  Escriba **EnumCodeFirst** como el nombre del proyecto y haga clic en **Aceptar**

## <a name="define-a-new-model-using-code-first"></a>Definir un modelo nuevo mediante Code First

Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio). El código siguiente define la clase de departamento.

El código también define la enumeración DepartmentNames. De forma predeterminada, la enumeración es de **int** tipo. La propiedad Name de la clase departamento es del tipo DepartmentNames.

Abra el archivo Program.cs y pegue las siguientes definiciones de clase.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Definir la clase DbContext tipo derivado

Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades. La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.

Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.

Los tipos de DbContext y DbSet se definen en el ensamblado de Entity Framework. Se agregará una referencia a este archivo DLL mediante el paquete EntityFramework NuGet.

1.  En el Explorador de soluciones, haga doble clic en el nombre del proyecto.
2.  Seleccione **administrar paquetes NuGet...**
3.  En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete.
4.  Haga clic en **instalar**

Tenga en cuenta que el ensamblado de Entity Framework, además de las referencias a ensamblados System.ComponentModel.DataAnnotations y System.Data.Entity se agregan también.

En la parte superior del archivo Program.cs, agregue la siguiente instrucción using:

``` csharp
using System.Data.Entity;
```

En el archivo Program.cs, agregue la definición del contexto. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Conservar y recuperar datos

Abra el archivo Program.cs, donde se define el método Main. Agregue el código siguiente en la función Main. El código agrega un nuevo objeto de departamento en el contexto. A continuación, guarda los datos. El código también ejecuta una consulta LINQ que devuelve un departamento donde el nombre es DepartmentNames.English.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compile y ejecute la aplicación. El programa produce el siguiente resultado:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Vista de la base de datos generado

Al ejecutar la aplicación por primera vez, Entity Framework crea una base de datos. Dado que tenemos instalado Visual Studio 2012, se creará la base de datos en la instancia de LocalDB. De forma predeterminada, Entity Framework los nombres de la base de datos después del nombre completo del contexto derivado (para este ejemplo, es **EnumCodeFirst.EnumTestContext**). Las siguientes veces que se usará la base de datos existente.  

Tenga en cuenta que si realiza cualquier cambio en el modelo una vez creada la base de datos, debe usar migraciones de Code First para actualizar el esquema de base de datos. Consulte [Code First para una base de datos](~/ef6/modeling/code-first/workflows/new-database.md) para obtener un ejemplo del uso de las migraciones.

Para ver la base de datos y los datos, realice lo siguiente:

1.  En el menú principal de Visual Studio 2012, seleccione **vista**  - &gt; **Explorador de objetos de SQL Server**.
2.  Si LocalDB no está en la lista de servidores, haga clic en el botón secundario del mouse en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse a la Instancia de LocalDB
3.  Expanda el nodo de LocalDB
4.  Expandir el **bases de datos** carpeta para ver la nueva base de datos y busque el **departamento** tabla tenga en cuenta que Code First no crea una tabla que asigna al tipo de enumeración
5.  Para ver los datos, haga doble clic en la tabla y seleccione **ver datos**

## <a name="summary"></a>Resumen

En este tutorial analizamos cómo usar los tipos de enumeración con Entity Framework Code First. 
