---
title: Trabajar con DbContext - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489068"
---
# <a name="working-with-dbcontext"></a>Trabajar con DbContext

Para usar Entity Framework para consultar, insertar, actualizar y eliminar datos mediante objetos. NET, primero deberá [crear un modelo](~/ef6/modeling/index.md) que asigna las entidades y relaciones que se definen en el modelo en las tablas de una base de datos.

Una vez tenga un modelo, la clase principal con la que tu aplicación interactúa es `System.Data.Entity.DbContext` (a menudo denominado como la clase de contexto). Puede usar un DbContext asociado a un modelo para:
- Escribir y ejecutar consultas   
- Materializar resultados de una consulta como objetos entidad
- Realizar un seguimiento de los cambios realizados en esos objetos
- Almacenar los cambios del objeto en la base de datos
- Enlazar objetos en memoria a los controles de interfaz de usuario

Esta página proporciona algunas instrucciones sobre cómo administrar la clase de contexto.  

## <a name="defining-a-dbcontext-derived-class"></a>Definir una clase derivada de DbContext  

La manera recomendada para trabajar con el contexto es definir una clase que derive de DbContext y exponga las propiedades DbSet que representan colecciones de las entidades especificadas en el contexto. Si está trabajando con EF Designer, se generará el contexto para usted. Si está trabajando con Code First, normalmente usted mismo escribirá el contexto.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Una vez que tenga un contexto, podría consultar, agregar (con los métodos `Add` o `Attach`) o eliminar (con `Remove`) entidades en el contexto a través de estas propiedades. Obtener acceso a un `DbSet` propiedad en un objeto de contexto representa una consulta inicial que devuelve todas las entidades del tipo especificado. Tenga en cuenta acceder a la propiedad no ejecutará la consulta. Se ejecuta una consulta cuando:  

- Se enumera mediante una instrucción `foreach` (C#) o `For Each` (Visual Basic).  
- Se enumera mediante una operación de colección como `ToArray`, `ToDictionary`, o `ToList`.  
- Se especifican operadores LINQ, como `First` o `Any` en la parte exterior de la consulta.  
- Se llama a uno de los métodos siguientes: el método de extensión `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, y `DbSet<T>.Find`, si una entidad con la clave especificada no se encuentra ya cargada en el contexto.  

## <a name="lifetime"></a>Período de duración  

La duración del contexto comienza cuando se crea y finaliza cuando la instancia se elimina o se recopilan de elementos no utilizados de la instancia. Use **mediante** si desea que todos los recursos que controla el contexto sean eliminados al final del bloque. Cuando usas **mediante**, el compilador crea automáticamente un bloque try/finally y llama a dispose en el **finalmente** bloque.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Estas son algunas directrices generales al decidir la duración del contexto:  

- Cuando se trabaja con aplicaciones Web, utilice una instancia de contexto por solicitud.  
- Cuando se trabaja con Windows Presentation Foundation (WPF) o Windows Forms, use una instancia de contexto por formulario. Esto le permite usar la funcionalidad de seguimiento de cambios proporciona ese contexto.  
- Si la instancia de contexto se crea un contenedor de inyección de dependencia, normalmente es responsabilidad del contenedor el eliminar el contexto.
- Si se crea el contexto en el código de aplicación, recuerde eliminar el contexto cuando ya no sea necesario.  
- Cuando se trabaja con el contexto de ejecución prolongada, tenga en cuenta lo siguiente:  
    - La carga de más objetos y sus referencias en la memoria puede aumentar rápidamente el consumo de memoria del contexto. lo que puede ocasionar problemas de rendimiento.  
    - El contexto no es seguro para subprocesos, por lo tanto no debe compartirse entre varios subprocesos que realizan el trabajo en él al mismo tiempo.
    - Si una excepción provoca que el contexto entre en un estado irrecuperable, puede finalizar toda la aplicación.  
    - Las posibilidades de que haya problemas relacionados con la concurrencia se incrementan a medida que aumenta la distancia entre el momento en que se consultan los datos y el momento en que se actualizan.  

## <a name="connections"></a>Conexiones  

De forma predeterminada, el contexto administra las conexiones a la base de datos. El contexto se abre y cierra las conexiones según sea necesario. Por ejemplo, el contexto abre una conexión para ejecutar una consulta y, a continuación, cierra la conexión cuando se han procesado todos los conjuntos de resultados.  

Hay casos en los que se desea más control sobre el momento en que se abre y cierra la conexión. Por ejemplo, cuando se trabaja con SQL Server Compact, a menudo se recomienda mantener una conexión abierta independiente a la base de datos durante la vigencia de la aplicación para mejorar el rendimiento. Puede administrar este proceso manualmente mediante la propiedad `Connection`.  
