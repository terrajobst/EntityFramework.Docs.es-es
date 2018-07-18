---
title: Trabajar con transacciones - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4238c88cc149458ed11b96a0bf9aaed9aac40b2d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122795"
---
# <a name="working-with-transactions"></a>Trabajar con transacciones
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Este documento se describirá mediante transacciones en EF6, incluidas las mejoras que hemos agregado desde EF5 para facilitar el trabajo con transacciones.  

## <a name="what-ef-does-by-default"></a>Lo que hace EF predeterminada  

En todas las versiones de Entity Framework, cada vez que ejecuta **SaveChanges()** Insertar, actualizar o eliminar en la base de datos, el marco de trabajo se encapsulará esa operación dentro de una transacción. Esta transacción dura solo el tiempo suficiente para ejecutar la operación y, a continuación, se completa. Cuando se ejecuta otra operación de este tipo se inicia una nueva transacción.  

A partir de EF6 **Database.ExecuteSqlCommand()** de forma predeterminada se ajustará el comando en una transacción si uno ya no estaba presente. Hay sobrecargas de este método que permiten invalidar este comportamiento si lo desea. También en la ejecución de procedimientos almacenados incluidos en el modelo mediante las API como EF6 **ObjectContext.ExecuteFunction()** hace lo mismo (salvo que el comportamiento predeterminado no en el momento en que se puede invalidar).  

En cualquier caso, el nivel de aislamiento de la transacción es cualquier nivel de aislamiento del proveedor de base de datos se considera que su valor predeterminado. De forma predeterminada, por ejemplo, en SQL Server trata READ COMMITTED.  

Entity Framework no ajusta a las consultas en una transacción.  

Esta funcionalidad predeterminada es adecuada para un lote de usuarios y si por lo que no es necesario hacer algo diferente en EF6; escribir el código tal como lo hace habitualmente.  

Sin embargo, algunos usuarios requieren mayor control sobre sus transacciones, este tema se trata en las secciones siguientes.  

## <a name="how-the-apis-work"></a>Cómo funcionan las API  

Antes de EF6 Entity Framework insistía en abrir la conexión de base de datos propio (produjo una excepción si se pasa una conexión que ya estaba abierta). Puesto que sólo se puede iniciar una transacción en una conexión abierta, esto significaba que la única manera en que un usuario podría encapsular varias operaciones en una transacción era usar un [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) o usar el  **ObjectContext.Connection** propiedad y llamar a start **Open()** y **BeginTransaction()** directamente en el valor devuelto **EntityConnection** objeto. Además, las llamadas de API que puede establecer contacto con la base de datos produciría un error si ha iniciado una transacción en la conexión de base de datos subyacente por su cuenta.  

> [!NOTE]
> Se ha quitado la limitación de aceptar únicamente conexiones cerradas en Entity Framework 6. Para obtener más información, consulte [administración de conexiones](~/ef6/fundamentals/connection-management.md).  

A partir de EF6 el marco de trabajo ahora proporciona:  

1. **Database.BeginTransaction()** : un método más fácil para un usuario iniciar y completar las transacciones dentro de un elemento DbContext existente: permitir que varias operaciones que va a combinarse en la misma transacción y, por tanto, todos confirmada o todos revierte como uno. También permite al usuario especificar más fácilmente el nivel de aislamiento de la transacción.  
2. **Database.UseTransaction()** : lo que permite que la clase DbContext para utilizar una transacción que se inició fuera de Entity Framework.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Combinar varias operaciones en una transacción dentro del mismo contexto  

**Database.BeginTransaction()** tiene dos invalidaciones: uno que toma una explícita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) y uno que no toma ningún argumento y utiliza el valor predeterminado IsolationLevel desde el proveedor de base de datos subyacente. En ambos devuelven un **DbContextTransaction** objeto que proporciona **Commit()** y **Rollback()** métodos que realizan commit y rollback en el almacén subyacente transacción.  

El **DbContextTransaction** está pensado para ser desechado una vez que se ha confirmado o revertido. Una forma sencilla de lograrlo es el **using(...) {...}** la sintaxis que se llamará automáticamente **Dispose()** cuando el bloque se complete:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> A partir de una transacción requiere que la conexión del almacén subyacente está abierta. Por lo que una llamada a Database.BeginTransaction() se abrirá la conexión si no está ya abierto. Si DbContextTransaction abre la conexión, a continuación, cerrará, cuando se llama a Dispose().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Pasar una transacción existente al contexto  

A veces desea que una transacción que es incluso más amplio en el ámbito y que incluye operaciones en la misma base de datos, pero fuera de EF completamente. Para realizar esta acción abre la conexión e iniciar la transacción por sí mismo y, a continuación, indicar a EF a) para usar la conexión de base de datos ya abierto y (b) para usar la transacción existente en esa conexión.  

Para ello debe definir y usar un constructor en la clase de contexto que se hereda de uno de los constructores de DbContext que toman i) un parámetro de conexión existente y ii) la contextOwnsConnection booleano.  

> [!NOTE]
> La marca contextOwnsConnection debe establecerse en false cuando se llama en este escenario. Esto es importante, ya que informa a Entity Framework que no debe cerrar la conexión cuando termine con él (por ejemplo, vea la línea 4 a continuación):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Además, debe iniciar la transacción usted mismo (incluida la IsolationLevel si desea evitar la configuración predeterminada) y dejar que Entity Framework sabe que hay una transacción existente que ya ha iniciado en la conexión (ver línea 33 a continuación).  

A continuación, es libre para ejecutar operaciones de base de datos directamente en la instancia SqlConnection propio, o bien en DbContext. Todas estas operaciones se ejecutan dentro de una transacción. Asume la responsabilidad para confirmar o revertir la transacción y para llamar a Dispose() en él, así como para cerrar o desechar la conexión de base de datos. Por ejemplo:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>Borrar la transacción

Puede pasar null para Database.UseTransaction() para borrar el conocimiento de Entity Framework de la transacción actual. Entity Framework le ni confirmar ni revertir la transacción existente al hacerlo, así que use con cuidado y solo si está seguro de esto es lo que desea hacer.  

### <a name="errors-in-usetransaction"></a>Errores en UseTransaction

Verá una excepción desde Database.UseTransaction() si pasa una transacción cuando:  
- Entity Framework ya tiene una transacción existente  
- Entity Framework ya está trabajando dentro de un TransactionScope  
- En la transacción pasado el objeto de conexión es null. Es decir, la transacción no está asociada con una conexión, que normalmente es un indicio de que esa transacción ya se ha completado  
- En la transacción pasado el objeto de conexión no coincide con la conexión de Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Usar transacciones con otras características  

Esta sección detalla cómo interactúan las transacciones anteriores:  

- Resistencia de conexión  
- Métodos asincrónicos  
- Transacciones de TransactionScope  

### <a name="connection-resiliency"></a>Resistencia de las conexiones  

La nueva característica de resistencia de conexión no funciona con las transacciones iniciadas por el usuario. Para obtener más información, consulte [limitaciones con estrategias de ejecución reintentos](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).  

### <a name="asynchronous-programming"></a>Programación asincrónica  

El enfoque descrito en las secciones anteriores no necesita más opciones ni configuración para trabajar con el [asincrónica consultar y guardar los métodos](~/ef6/fundamentals/async.md
). Pero tenga en cuenta que, según lo que lo hace dentro de los métodos asincrónicos, esto puede producir en transacciones de larga ejecución, lo que a su vez pueden provocar interbloqueos o bloqueo que es perjudicial para el rendimiento de la aplicación global.  

### <a name="transactionscope-transactions"></a>Transacciones de TransactionScope  

Antes de EF6 era usar un objeto TransactionScope la manera recomendada de proporcionar más grandes transacciones de ámbito:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

SqlConnection y Entity Framework se usan la transacción ambiente de TransactionScope y, por tanto, se confirmen juntos.  

A partir de .NET 4.5.1 TransactionScope se ha actualizado para trabajar también con los métodos asincrónicos mediante el uso de la [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeración:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

Todavía hay algunas limitaciones en el enfoque de TransactionScope:  

- Requiere que .NET 4.5.1 o posterior, para trabajar con los métodos asincrónicos.  
- No se puede usar en escenarios de nube a menos que esté seguro de que tiene una y solo una conexión (escenarios de nube no admiten transacciones distribuidas).  
- No se puede combinar con el enfoque Database.UseTransaction() de las secciones anteriores.  
- Producirán excepciones si se emite ninguna instrucción DDL y no se han habilitado las transacciones distribuidas a través del servicio MSDTC.  

Ventajas del enfoque TransactionScope:  

- Actualizarán automáticamente una transacción local a una transacción distribuida si se realiza más de una conexión a una base de datos o combine una conexión a una base de datos con una conexión a una base de datos diferentes en la misma transacción (Nota: debe tener el servicio MSDTC configurado para permitir las transacciones distribuidas para que funcione).  
- Facilidad de codificación. Si prefiere que la transacción ambiente y tratarán de forma implícita en segundo plano en lugar de en forma explícita de control, a continuación, el enfoque de TransactionScope posible a sus necesidades mejor.  

En resumen, con el nuevo Database.BeginTransaction() y Database.UseTransaction() API anterior, el enfoque de TransactionScope ya no es necesario para la mayoría de los usuarios. Si se sigue usando TransactionScope, a continuación, tenga en cuenta las limitaciones anteriores. Se recomienda usar el enfoque descrito en las secciones anteriores en su lugar, siempre que sea posible.  
