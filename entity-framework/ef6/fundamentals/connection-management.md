---
title: Administración de la conexión - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: dc405e1876edc850ae6e4ce4649da52635d316ae
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997910"
---
# <a name="connection-management"></a>Administración de conexiones
Esta página describe el comportamiento de Entity Framework con respecto al pasar las conexiones de contexto y la funcionalidad de la **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Conexiones de pasar al contexto  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamiento de EF5 y versiones anteriores  

Hay dos constructores que aceptan conexiones:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Es posible usar estas pero tiene que solucionar un par de limitaciones:  

1. Si se pasa una conexión abierta a cualquiera de estos, a continuación, la primera vez que el marco intenta usarlo que se produce una excepción InvalidOperationException que dice que volver a no puede abrir una conexión ya abierta.  
2. La marca contextOwnsConnection se interpreta como si se debe desechar la conexión del almacén subyacente cuando se desecha el contexto. Sin embargo, independientemente de ese valor, siempre se cierra la conexión del almacén cuando se desecha el contexto. Por lo que si tiene más de un DbContext con la misma conexión de cualquier contexto se elimine en primer lugar cerrará la conexión (de forma similar si ha mezclado una conexión ADO.NET existente con una clase DbContext, DbContext siempre cerrará la conexión cuando se elimina) .  

Es posible evitar la primera limitación anteriormente pasando una conexión cerrada y sólo se ejecuta código que se abre una vez que se han creado todos los contextos:  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

La segunda limitación significa simplemente que necesita no elimine ninguno de los objetos DbContext hasta que esté listo para la conexión a punto de cerrarse.  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamiento en EF6 y versiones futuras  

En EF6 y versiones futuras DbContext tiene el mismos dos constructores, pero ya no requiere que se cierra la conexión que se pasó al constructor cuando se recibe. Así que ahora es posible:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

También la marca contextOwnsConnection ahora controla si la conexión es tanto cierra y se elimina cuando se desecha DbContext. Por lo que en el ejemplo anterior no se cierra la conexión cuando el contexto es eliminado (línea 32) como sucedía en versiones anteriores de EF, sino cuando se elimina la propia conexión (línea 40).  

Desde luego resulta todavía posible para la clase DbContext para tomar el control de la conexión (solo el conjunto contextOwnsConnection en true o use uno de los otros constructores) si se desea.  

> [!NOTE]
> Hay algunas consideraciones adicionales cuando se usan transacciones con este nuevo modelo. Para obtener información detallada, consulte [trabajar con transacciones](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamiento de EF5 y versiones anteriores  

En EF5 y versiones anteriores, hay un error que el **ObjectContext.Connection.State** no se ha actualizado para reflejar el estado real de la conexión del almacén subyacente. Por ejemplo, si ejecuta el código siguiente se puede devolver el estado **cerrado** , aunque en realidad subyacentes de la conexión del almacén es **abierto**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Por separado, si abre la conexión de base de datos mediante una llamada a Database.Connection.Open() será abierto hasta la próxima vez que ejecute una consulta o llamar a cualquier cosa que requiere una conexión de base de datos (por ejemplo, SaveChanges()) pero después que subyacente almacén se cerrará la conexión. El contexto se luego vuelva a abrir y volver a cerrar la conexión siempre que se requiere otra operación de base de datos:  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamiento en EF6 y versiones futuras  

Para EF6 y versiones futuras hemos tomado el enfoque que si elige el código de llamada para abrir la conexión mediante el contexto de la llamada. Database.Connection.Open(), a continuación, se tiene una buena razón para hacerlo y supone que el marco de trabajo que desea un control sobre la apertura y cierre de la conexión y ya no se cerrará automáticamente la conexión.  

> [!NOTE]
> Esto puede dañar las conexiones que están abiertas para por lo que use mucho tiempo con cuidado.  

También hemos actualizado el código para que ObjectContext.Connection.State ahora realiza un seguimiento del estado de la conexión subyacente correctamente.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
