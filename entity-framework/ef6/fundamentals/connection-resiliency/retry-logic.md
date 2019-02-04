---
title: Lógica de reintento y resistencia de conexión - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 7d6aa870cc32a2b344457fbb04525a7c2c8d1c61
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668770"
---
# <a name="connection-resiliency-and-retry-logic"></a>Lógica de reintento y resistencia de conexión
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Las aplicaciones se conectan a un servidor de base de datos siempre han sido vulnerables a los saltos de conexión debido a errores de back-end y la inestabilidad en la red. Sin embargo, en un entorno basada en LAN, trabajar con servidores de base de datos dedicada estos errores son lo bastante raros que lógica adicional para controlar dichos errores con frecuencia no es necesario. Con el aumento de la nube según los servidores de base de datos, como Windows Azure SQL Database y conexiones a través de redes menos confiables ahora es más común para los saltos de conexión que se produzca. Esto puede deberse a que las técnicas defensivas que en la nube de uso de las bases de datos para garantizar la imparcialidad del servicio, como la limitación de la conexión o inestabilidad en la red provocando tiempos de espera intermitentes y otros errores transitorios.  

Resistencia de conexión hace referencia a la capacidad de EF volver a intentar automáticamente todos los comandos que se producirá un error debido a estos saltos de conexión.  

## <a name="execution-strategies"></a>Estrategias de ejecución  

Reintento de conexión se ocupa de una implementación de la interfaz IDbExecutionStrategy. Las implementaciones de la IDbExecutionStrategy será responsables de aceptar una operación y, si se produce una excepción, determinar si un reintento es adecuado y volver a intentar si es. Hay cuatro estrategias de ejecución que se suministran con EF:  

1. **DefaultExecutionStrategy**: esta estrategia de ejecución no reintenta las operaciones, es el valor predeterminado para las bases de datos distintos de sql server.  
2. **DefaultSqlExecutionStrategy**: se trata de una estrategia de ejecución interno que se usa de forma predeterminada. Esta estrategia no vuelva a intentar en absoluto, sin embargo, ajustarán las excepciones que pueden ser transitorias para informar a los usuarios que desean habilitar la resistencia de conexión.  
3. **DbExecutionStrategy**: esta clase es adecuada como una clase base para otras estrategias de ejecución, incluidas sus propias plantillas personalizadas. Implementa una directiva de reintentos exponencial, donde el reintento inicial produce con retraso de cero y el retraso aumenta exponencialmente hasta que se alcance el número máximo de reintentos. Esta clase tiene un método abstracto ShouldRetryOn que se puede implementar en las estrategias de ejecución derivados para controlar las excepciones que se deben Reintentar.  
4. **SqlAzureExecutionStrategy**: esta estrategia de ejecución se hereda de DbExecutionStrategy y se volverá a intentar en las excepciones que se sabe que posiblemente transitorio al trabajar con Azure SQL Database.

> [!NOTE]
> Estrategias de ejecución 2 y 4 se incluyen en el proveedor de Sql Server que se suministra con EF, que se encuentra en el ensamblado EntityFramework.SqlServer y están diseñadas para trabajar con SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Habilitación de una estrategia de ejecución  

Es la manera más fácil para indicar a EF que use una estrategia de ejecución con el método SetExecutionStrategy de la [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) clase:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Este código indica a EF que use el SqlAzureExecutionStrategy al conectarse a SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Configuración de la estrategia de ejecución  

El constructor de SqlAzureExecutionStrategy puede aceptar dos parámetros, MaxRetryCount y MaxDelay. Recuento de MaxRetry es el número máximo de veces que se reintentará la estrategia. El MaxDelay es un objeto TimeSpan que representa el retraso máximo entre reintentos que se va a usar la estrategia de ejecución.  

Para establecer el número máximo de reintentos a 1 y el retraso máximo en 30 segundos debería execue lo siguiente:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

El SqlAzureExecutionStrategy volverá a intentar al instante se supera la primera vez que se produce un error transitorio, pero se retrasará ya entre cada reintento hasta que el máximo de cualquier límite de reintentos o el tiempo total alcanza el retraso máximo.  

Las estrategias de ejecución solo volverá a intentar un número limitado de las excepciones que suelen ser tansient, todavía deberá controlar otros errores, así como detectar la excepción RetryLimitExceeded para el caso de que un error no es transitorio o tarda demasiado tiempo resolver Sí.  

Hay algunos conocidos de limitaciones cuando se usa una estrategia de ejecución de reintento:  

## <a name="streaming-queries-are-not-supported"></a>No se admiten consultas de streaming  

De forma predeterminada, EF6 y versiones posteriores mantiene los resultados de la consulta en lugar de streaming en ellos. Si desea tener resultados transmite por secuencias puede usar el método AsStreaming para cambiar un LINQ para consultar las entidades en streaming.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

No se admite la transmisión por secuencias cuando se registra una estrategia de ejecución de reintento. Esta limitación existe porque la conexión se puede quitar parte a través de los resultados devueltos. Cuando esto ocurre, EF debe volver a ejecutar la consulta completa, pero no tiene ninguna manera de saber qué resultados ya se han devuelto confiable (datos que han cambiado desde que se envió la consulta inicial, los resultados pueden proceder de vuelta en un orden diferente, los resultados no pueden tener un identificador único etcetera.).  

## <a name="user-initiated-transactions-are-not-supported"></a>No se admiten las transacciones iniciadas por el usuario  

Cuando se ha configurado una estrategia de ejecución que da como resultado reintentos, existen algunas limitaciones respecto al uso de transacciones.  

De forma predeterminada, EF llevará a cabo las actualizaciones de base de datos dentro de una transacción. No es necesario hacer nada para habilitar esta opción, EF siempre lo hace automáticamente.  

Por ejemplo, en el código siguiente SaveChanges se realiza automáticamente dentro de una transacción. Si SaveChanges fuera a producir un error después de insertar uno de la nueva carpeta del sitio, a continuación, se revertirán la transacción y no hay cambios aplicados a la base de datos. El contexto también se deja en un estado que permita SaveChanges vuelva a llamar para volver a intentar aplicar los cambios.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Cuando no se utiliza una estrategia de ejecución de reintento puede encapsular varias operaciones en una sola transacción. Por ejemplo, el código siguiente incluye dos llamadas de SaveChanges en una sola transacción. Si cualquier parte de cualquiera de las operaciones se produce un error, a continuación, ninguno de los cambios se aplican.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Esto no se admite cuando se usa una estrategia de ejecución de reintento, dado que EF no tiene en cuenta todas las operaciones anteriores y cómo reintentarlos. Por ejemplo, si el segundo error de SaveChanges, a continuación, EF ya no tiene la información necesaria para volver a intentar la primera llamada a SaveChanges.  

### <a name="workaround-suspend-execution-strategy"></a>Solución: Suspender la estrategia de ejecución  

Una posible solución es suspender la estrategia de ejecución de reintento para el fragmento de código que se debe utilizar un usuario inició la transacción. La manera más fácil de hacerlo es agregar una marca SuspendExecutionStrategy para el código en función de clase de configuración y cambie la expresión lambda de estrategia de ejecución para devolver la estrategia de ejecución (no retying) predeterminada cuando se establece la marca.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy") ?? false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Tenga en cuenta que estamos usando CallContext para almacenar el valor de marca. Esto proporciona una funcionalidad similar a almacenamiento local de subprocesos, pero es seguro usar con el código asincrónico - incluidos consulta asincrónica y guardar con Entity Framework.  

Ahora nos podemos suspender la estrategia de ejecución de la sección de código que usa una transacción iniciada por el usuario.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Solución: Llamar manualmente a la estrategia de ejecución  

Otra opción es usar la estrategia de ejecución manualmente y asignarle el conjunto completo de la lógica que se ejecutará; por lo que puede reintentar todo lo que si se produce un error en una de las operaciones. Todavía es necesario suspender la estrategia de ejecución - mediante la técnica anterior - para que los contextos utilizados dentro del bloque de código que se puede reintentar no intentará volver a realizar.  

Tenga en cuenta que los contextos deben crearse dentro del bloque de código que se vuelva a intentar. Esto garantiza que estamos empezando con un estado limpio para cada reintento.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
