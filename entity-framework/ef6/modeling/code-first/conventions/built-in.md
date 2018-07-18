---
title: Convenciones de Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121872"
---
# <a name="code-first-conventions"></a>Convenciones de Code First
Código primero le permite describir un modelo mediante las clases de C# o Visual Basic. NET. La forma básica del modelo se detecta mediante convenciones. Las convenciones son conjuntos de reglas que se usan para configurar automáticamente un modelo conceptual basándose en las definiciones de clase cuando se trabaja con Code First. Las convenciones se definen en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions.  

Puede configurar aún más el modelo mediante el uso de anotaciones de datos o la API fluida. Configuración a través de la API fluida, seguida de las convenciones y, a continuación, las anotaciones de datos tiene prioridad. Para obtener más información, consulte [anotaciones de datos](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relaciones](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - tipos y propiedades](~/ef6/modeling/code-first/fluent/types-and-properties.md) y [API Fluent con VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Está disponible en una lista detallada de las convenciones de Code First la [documentación de API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). En este tema se proporciona información general de las convenciones utilizadas por Code First.  

## <a name="type-discovery"></a>Detección de tipos  

Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio). Además de definir las clases, también debe permitir que **DbContext** saber los tipos que van a incluir en el modelo. Para ello, defina una clase de contexto que se derive de **DbContext** y expone **DbSet** propiedades para los tipos que se desean que forme parte del modelo. Código primero incluirá estos tipos y también se extrae en todos los tipos que se hace referencia, incluso si se definen los tipos que se hace referencia en un ensamblado diferente.  

Si los tipos que participan en una jerarquía de herencia, es suficiente para definir un **DbSet** propiedad para la clase base y los tipos derivados se incluirá automáticamente, si se encuentran en el mismo ensamblado que la clase base.  

En el ejemplo siguiente, solo hay un **DbSet** propiedad definida en el **SchoolEntities** clase (**departamentos**). Código primero usa esta propiedad para detectar y extraer en todos los tipos que se hace referencia.  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

Si desea excluir un tipo de modelo, utilice el **NotMapped** atributo o la **DbModelBuilder.Ignore** API fluida.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Convención de clave principal  

Código infiere en primer lugar que una propiedad es una clave principal si una propiedad en una clase se denomina "ID" (no distingue mayúsculas de minúsculas) o el nombre de la clase seguido por "ID". Si el tipo de la propiedad de clave principal es numérico o GUID estará configurado como una columna de identidad.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Convención de relación  

En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad. Cada objeto puede tener una propiedad de navegación para cada relación en la que participa. Propiedades de navegación permiten navegar y administrar relaciones en ambas direcciones, devolviendo un objeto de referencia (si la multiplicidad es uno o cero o uno) o una colección (si la multiplicidad es varios). Código infiere en primer lugar en función de las propiedades de navegación definidas en los tipos de relaciones.  

Además de las propiedades de navegación, se recomienda que incluya las propiedades de clave externa en los tipos que representan los objetos dependientes. Cualquier propiedad con el mismo tipo de datos como la principal propiedad de clave principal y con un nombre que se ajustan a uno de los siguientes formatos representa una clave externa para la relación: '\<nombre de la propiedad de navegación\>\<principal nombre de propiedad de clave principal\>','\<nombre de la clase de entidad de seguridad\>\<nombre de propiedad de clave principal\>', o '\<nombre de entidad de propiedad de clave principal\>'. Si se encuentran varias coincidencias en el orden mostrado anteriormente tiene prioridad. Detección de clave externa no distingue mayúsculas de minúsculas. Cuando se detecta una propiedad de clave externa, Code First deduce la multiplicidad de la relación según la nulabilidad de la clave externa. Si la propiedad acepta valores NULL, a continuación, se registra la relación como opcional; en caso contrario, se registra la relación según sea necesario.  

Si una clave externa en la entidad dependiente no admite valores NULL, a continuación, Code First establece eliminación en cascada en la relación. Si una clave externa en la entidad dependiente es que acepta valores NULL, Code First no establecido eliminación en cascada en la relación y, cuando se elimina la entidad de seguridad de la clave externa se establecerá en null. La multiplicidad y cascade delete comportamiento detectado por convención puede invalidarse mediante el uso de la API fluida.  

En el ejemplo siguiente se usan las propiedades de navegación y una clave externa para definir la relación entre las clases de departamento y curso.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> Si tiene varias relaciones entre los mismos tipos (por ejemplo, supongamos que define el **persona** y **libro** clases, donde el **persona** clase contiene la  **ReviewedBooks** y **AuthoredBooks** las propiedades de navegación y la **libro** clase contiene la **autor** y  **Revisor** las propiedades de navegación) deberá configurar manualmente las relaciones mediante el uso de anotaciones de datos o la API fluida. Para obtener más información, consulte [anotaciones de datos - relaciones](~/ef6/modeling/code-first/data-annotations.md) y [API Fluent - relaciones](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Convención de tipos complejos  

Cuando Code First detecta una definición de clase donde no se puede deducir una clave principal y no se registra ninguna clave principal a través de las anotaciones de datos o la API fluida, el tipo se registra automáticamente como un tipo complejo. Detección de tipos complejos también requiere que el tipo no tiene propiedades que hacen referencia a tipos de entidad y no se hace referencia desde una propiedad de colección en otro tipo. Dadas las siguientes definiciones de clase Code First podría deducir que **detalles** es un tipo complejo, porque no tiene ninguna clave principal.  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a>Convención de cadena de conexión  

Para obtener información sobre las convenciones que DbContext se usa para detectar la conexión con el uso del [modelos y las conexiones](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Quitar las convenciones  

Puede quitar cualquiera de las convenciones definidas en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions. En el ejemplo siguiente se quita **PluralizingTableNameConvention**.  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a>Convenciones personalizadas  

Convenciones personalizadas se admiten en EF6 y versiones posteriores. Para obtener más información, consulte [convenciones de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).
