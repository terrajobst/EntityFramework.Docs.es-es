---
title: Las relaciones, las propiedades de navegación y las claves externas - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 416eb1fb590330ba292a858347e26b83dddc74df
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829205"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Las relaciones, las propiedades de navegación y las claves externas
En este tema se proporciona información general de cómo Entity Framework administra las relaciones entre entidades. También se ofrece orientación sobre cómo asignar y manipular las relaciones.

## <a name="relationships-in-ef"></a>Relaciones de EF

En las bases de datos relacionales, se definen relaciones (también denominadas asociaciones) entre las tablas a través de las claves externas. Una clave externa (FK) es una columna o combinación de columnas que se usa para establecer y exigir un vínculo entre los datos de dos tablas. Generalmente, hay tres tipos de relaciones: uno a uno, uno a varios y varios a varios. En una relación uno a varios, la clave externa se define en la tabla que representa el extremo de la relación de varios. La relación de varios a varios implica definir una tercera tabla (denominada tabla de unión o de combinación), cuya clave principal se compone de las claves externas de ambas tablas relacionadas. En una relación uno a uno, la clave principal actúa además como una clave externa y no hay ninguna columna de clave externa independiente para cualquiera de las tablas.

La siguiente imagen muestra dos tablas que participan en la relación de uno a varios. El **curso** tabla es la tabla dependiente porque contiene la **DepartmentID** columna que se vincula a la **departamento** tabla.

![Tablas de departamento y en curso](~/ef6/media/database2.png)

En Entity Framework, una entidad puede deberse a otras entidades a través de una asociación o relación. Cada relación contiene dos extremos que describen el tipo de entidad y la multiplicidad del tipo (uno, cero o uno o muchos) para las dos entidades en dicha relación. La relación puede venir gobernada por una restricción referencial, que describe qué extremo de la relación es un rol principal y que es un rol dependiente.

Las propiedades de navegación proporcionan una manera de navegar por una asociación entre dos tipos de entidad. Cada objeto puede tener una propiedad de navegación para cada relación en la que participa. Propiedades de navegación permiten navegar y administrar relaciones en ambas direcciones, devolviendo un objeto de referencia (si la multiplicidad es uno o cero o uno) o una colección (si la multiplicidad es varios). También puede tener una navegación unidireccional, en cuyo caso se define la propiedad de navegación solo en uno de los tipos que participa en la relación y no en ambos.

Se recomienda incluir propiedades en el modelo que se asignan a las claves externas de la base de datos. Con las propiedades de clave externa incluidas, puede crear o cambiar una relación modificando el valor de clave externa sobre un objeto dependiente. Este tipo de asociación se denomina asociación de clave externa. Mediante claves externas es incluso más esencial al trabajar con entidades desconectadas. Tenga en cuenta que, cuando trabaja con 1 a 1 o 1 a 0.. relaciones 1, no hay ninguna columna de clave externa independiente, la propiedad de clave principal actúa como la clave externa y siempre se incluye en el modelo.

Cuando las columnas de clave externas no se incluyen en el modelo, la información de asociación se administra como un objeto independiente. Se realiza un seguimiento de las relaciones a través de referencias de objeto en lugar de propiedades de clave externa. Este tipo de asociación se denomina un *asociación independiente*. La manera más común para modificar un *asociación independiente* consiste en modificar las propiedades de navegación que se generan para cada entidad que participa en la asociación.

Puede decidir entre utilizar uno o ambos tipos de asociaciones en su modelo. Sin embargo, si tiene una relación muchos a muchos pura que está conectada mediante una tabla de combinación que solo contiene claves externas, EF utilizará una asociación independiente para administrar este tipo de relación muchos a muchos.   

La siguiente imagen muestra un modelo conceptual que se creó con Entity Framework Designer. El modelo contiene dos entidades que participan en la relación de uno a varios. Ambas entidades tienen propiedades de navegación. **Curso** es la entidad de dependencia y tiene la **DepartmentID** propiedad de clave externa definida.

![Tablas de departamento y curso con las propiedades de navegación](~/ef6/media/relationshipefdesigner.png)

El fragmento de código siguiente muestra el mismo modelo que se creó con Code First.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Configurar o asignación de relaciones

El resto de esta página cubre cómo obtener acceso y manipular datos mediante relaciones. Para obtener información acerca de cómo configurar las relaciones en el modelo, vea las páginas siguientes.

-   Para configurar las relaciones en Code First, consulte [anotaciones de datos](~/ef6/modeling/code-first/data-annotations.md) y [API Fluent-relaciones](~/ef6/modeling/code-first/fluent/relationships.md).
-   Para configurar las relaciones mediante Entity Framework Designer, vea [relaciones con EF Designer](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Crear y modificar relaciones

En un *asociación de clave externa*, al cambiar la relación, el estado de un objeto dependiente con un `EntityState.Unchanged` estado cambia a `EntityState.Modified`. En una relación independiente, al cambiar la relación no actualiza el estado del objeto dependiente.

Los ejemplos siguientes muestran cómo usar las propiedades de clave externa y las propiedades de navegación para asociar los objetos relacionados. Con asociaciones de clave externa, puede usar cualquiera de estos métodos para cambiar, crear o modificar las relaciones. Con asociaciones independientes, no puede utilizar la propiedad de clave externa.

- Asignando un nuevo valor a una propiedad de clave externa, como en el ejemplo siguiente.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- El código siguiente quita una relación estableciendo la clave externa en **null**. Tenga en cuenta que la propiedad de clave externa deben aceptar valores NULL.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Si la referencia está en el estado agregado (en este ejemplo, el objeto de curso), la propiedad de navegación de referencia no se sincronizarán con los valores de clave de un nuevo objeto hasta que se llama a SaveChanges. La sincronización no se produce porque el contexto del objeto no contiene claves permanentes para objetos agregados hasta que se guardan. Si debe tener los nuevos objetos totalmente sincronizados tan pronto como establecer la relación, utilice uno de los siguientes methods.*

- Asignando un nuevo objeto a una propiedad de navegación. El código siguiente crea una relación entre un curso y un `department`. Si los objetos se asocian al contexto, el `course` también se agrega a la `department.Courses` colección y la clave externa correspondiente propiedad en el `course` objeto se establece en el valor de propiedad de clave del departamento.  
  ``` csharp
  course.Department = department;
  ```

- Para eliminar la relación, establezca la propiedad de navegación en `null`. Si trabaja con Entity Framework que se basa en .NET 4.0, el extremo relacionado debe cargarse antes de que se establece en null. Por ejemplo:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  A partir de Entity Framework 5.0, que se basa en .NET 4.5, puede establecer la relación en null sin tener que cargar el extremo relacionado. También puede establecer el valor actual en null mediante el método siguiente.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Eliminando o agregando un objeto en una colección de entidades. Por ejemplo, puede agregar un objeto de tipo `Course` a la `department.Courses` colección. Esta operación crea una relación entre un determinado **curso** y un determinado `department`. Si los objetos se asocian para el contexto, la referencia de departamento y la propiedad de clave externa en el **curso** objeto se establecerá en adecuado `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Mediante el uso de la `ChangeRelationshipState` método para cambiar el estado de la relación entre dos objetos entidad especificado. Este método se usa con más frecuencia cuando se trabaja con aplicaciones de N niveles y una *asociación independiente* (no se puede usar con una asociación de clave externa). Además, para usar este método debe quitar hasta `ObjectContext`, tal y como se muestra en el ejemplo siguiente.  
En el ejemplo siguiente, hay una relación de varios a varios entre los instructores y a los cursos. Una llamada a la `ChangeRelationshipState` método y pasando el `EntityState.Added` parámetro, permite la `SchoolContext` saber que se ha agregado una relación entre los dos objetos:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Tenga en cuenta que si va a actualizar (no solo agregando) una relación, debe eliminar la relación anterior después de agregar una nueva:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Sincronizar los cambios entre las propiedades de navegación y las claves externas

Al cambiar la relación de los objetos asociados al contexto mediante uno de los métodos descritos anteriormente, Entity Framework necesita mantener sincronizadas las claves externas, referencias y colecciones. Entity Framework administra automáticamente esta sincronización (también conocido como relación corrección-arriba) para las entidades POCO con servidores proxy. Para obtener más información, consulte [trabajar con servidores proxy](~/ef6/fundamentals/proxies.md).

Si está utilizando entidades POCO sin objetos proxy, debe asegurarse que la **DetectChanges** método se llama para sincronizar los objetos relacionados en el contexto. Tenga en cuenta que las siguientes API desencadenan automáticamente un **DetectChanges** llamar.

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   Ejecutar un LINQ a consultas en un `DbSet`

## <a name="loading-related-objects"></a>Cargar objetos relacionados

En Entity Framework que usa con más frecuencia, utilice las propiedades de navegación para cargar entidades relacionadas con la entidad devuelta por la asociación definida. Para obtener más información, consulte [cargar objetos relacionados](~/ef6/querying/related-data.md).

> [!NOTE]
> En una asociación de clave externa, al cargar un extremo relacionado de un objeto dependiente, el objeto relacionado se cargará dependiendo del valor de clave externa del objeto dependiente actualmente en memoria:

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

En una asociación independiente, se consulta el extremo relacionado de un objeto dependiente de acuerdo con el valor de clave externa actualmente en la base de datos. Sin embargo, si se modificó la relación y la propiedad de referencia en el objeto dependiente señala a un objeto de entidad de seguridad diferente que se carga en el contexto del objeto, Entity Framework intentará crear una relación como se define en el cliente.

## <a name="managing-concurrency"></a>Administrar la simultaneidad

En la clave externa y asociaciones independientes, las comprobaciones de simultaneidad se basan en las claves de entidad y otras propiedades de entidad que se definen en el modelo. Cuando se usa EF Designer para crear un modelo, establecer el `ConcurrencyMode` atributo **fijo** para especificar que se debe comprobar la propiedad de simultaneidad. Cuando utilice Code First para definir un modelo, use el `ConcurrencyCheck` anotación en las propiedades que desea que se comprobará para simultaneidad. Cuando se trabaja con Code First también puede usar el `TimeStamp` anotación para especificar que se debe comprobar la propiedad de simultaneidad. Puede tener solo una propiedad de marca de tiempo en una clase determinada. Code First asigna esta propiedad a un campo que no aceptan valores NULL en la base de datos.

Se recomienda usar siempre la asociación de clave externa cuando se trabaja con entidades que participan en la comprobación de simultaneidad y la resolución.

Para obtener más información, consulte [controlar los conflictos de simultaneidad](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Trabajar con claves superpuestas

Las claves superpuestas son claves compuestas en las que algunas propiedades de la clave también forman parte de otra clave de la entidad. No es posible tener una clave superpuesta en una asociación independiente. Para cambiar una asociación de clave externa que incluya claves superpuestas, recomendamos modificar los valores de clave externa en lugar de utilizar las referencias a objetos.
