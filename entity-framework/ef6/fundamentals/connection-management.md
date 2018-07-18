---
title: Administración de la conexión - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122765"
---
# <a name="connection-management"></a><span data-ttu-id="5373c-102">Administración de conexiones</span><span class="sxs-lookup"><span data-stu-id="5373c-102">Connection management</span></span>
<span data-ttu-id="5373c-103">Esta página describe el comportamiento de Entity Framework con respecto al pasar las conexiones de contexto y la funcionalidad de la **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="5373c-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="5373c-104">Conexiones de pasar al contexto</span><span class="sxs-lookup"><span data-stu-id="5373c-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="5373c-105">Comportamiento de EF5 y versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="5373c-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="5373c-106">Hay dos constructores que aceptan conexiones:</span><span class="sxs-lookup"><span data-stu-id="5373c-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="5373c-107">Es posible usar estas pero tiene que solucionar un par de limitaciones:</span><span class="sxs-lookup"><span data-stu-id="5373c-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="5373c-108">Si se pasa una conexión abierta a cualquiera de estos, a continuación, la primera vez que el marco intenta usarlo que se produce una excepción InvalidOperationException que dice que volver a no puede abrir una conexión ya abierta.</span><span class="sxs-lookup"><span data-stu-id="5373c-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="5373c-109">La marca contextOwnsConnection se interpreta como si se debe desechar la conexión del almacén subyacente cuando se desecha el contexto.</span><span class="sxs-lookup"><span data-stu-id="5373c-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="5373c-110">Sin embargo, independientemente de ese valor, siempre se cierra la conexión del almacén cuando se desecha el contexto.</span><span class="sxs-lookup"><span data-stu-id="5373c-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="5373c-111">Por lo que si tiene más de un DbContext con la misma conexión de cualquier contexto se elimine en primer lugar cerrará la conexión (de forma similar si ha mezclado una conexión ADO.NET existente con una clase DbContext, DbContext siempre cerrará la conexión cuando se elimina) .</span><span class="sxs-lookup"><span data-stu-id="5373c-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="5373c-112">Es posible evitar la primera limitación anteriormente pasando una conexión cerrada y sólo se ejecuta código que se abre una vez que se han creado todos los contextos:</span><span class="sxs-lookup"><span data-stu-id="5373c-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="5373c-113">La segunda limitación significa simplemente que necesita no elimine ninguno de los objetos DbContext hasta que esté listo para la conexión a punto de cerrarse.</span><span class="sxs-lookup"><span data-stu-id="5373c-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="5373c-114">Comportamiento en EF6 y versiones futuras</span><span class="sxs-lookup"><span data-stu-id="5373c-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="5373c-115">En EF6 y versiones futuras DbContext tiene el mismos dos constructores, pero ya no requiere que se cierra la conexión que se pasó al constructor cuando se recibe.</span><span class="sxs-lookup"><span data-stu-id="5373c-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="5373c-116">Así que ahora es posible:</span><span class="sxs-lookup"><span data-stu-id="5373c-116">So this is now possible:</span></span>  

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

<span data-ttu-id="5373c-117">También la marca contextOwnsConnection ahora controla si la conexión es tanto cierra y se elimina cuando se desecha DbContext.</span><span class="sxs-lookup"><span data-stu-id="5373c-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="5373c-118">Por lo que en el ejemplo anterior no se cierra la conexión cuando el contexto es eliminado (línea 32) como sucedía en versiones anteriores de EF, sino cuando se elimina la propia conexión (línea 40).</span><span class="sxs-lookup"><span data-stu-id="5373c-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="5373c-119">Desde luego resulta todavía posible para la clase DbContext para tomar el control de la conexión (solo el conjunto contextOwnsConnection en true o use uno de los otros constructores) si se desea.</span><span class="sxs-lookup"><span data-stu-id="5373c-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="5373c-120">Hay algunas consideraciones adicionales cuando se usan transacciones con este nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="5373c-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="5373c-121">Para obtener información detallada, consulte [trabajar con transacciones](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="5373c-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="5373c-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="5373c-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="5373c-123">Comportamiento de EF5 y versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="5373c-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="5373c-124">En EF5 y versiones anteriores, hay un error que el **ObjectContext.Connection.State** no se ha actualizado para reflejar el estado real de la conexión del almacén subyacente.</span><span class="sxs-lookup"><span data-stu-id="5373c-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="5373c-125">Por ejemplo, si ejecuta el código siguiente se puede devolver el estado **cerrado** , aunque en realidad subyacentes de la conexión del almacén es **abierto**.</span><span class="sxs-lookup"><span data-stu-id="5373c-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="5373c-126">Por separado, si abre la conexión de base de datos mediante una llamada a Database.Connection.Open() será abierto hasta la próxima vez que ejecute una consulta o llamar a cualquier cosa que requiere una conexión de base de datos (por ejemplo, SaveChanges()) pero después que subyacente almacén se cerrará la conexión.</span><span class="sxs-lookup"><span data-stu-id="5373c-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="5373c-127">El contexto se luego vuelva a abrir y volver a cerrar la conexión siempre que se requiere otra operación de base de datos:</span><span class="sxs-lookup"><span data-stu-id="5373c-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="5373c-128">Comportamiento en EF6 y versiones futuras</span><span class="sxs-lookup"><span data-stu-id="5373c-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="5373c-129">Para EF6 y versiones futuras hemos tomado el enfoque que si elige el código de llamada para abrir la conexión mediante el contexto de la llamada. Database.Connection.Open(), a continuación, se tiene una buena razón para hacerlo y supone que el marco de trabajo que desea un control sobre la apertura y cierre de la conexión y ya no se cerrará automáticamente la conexión.</span><span class="sxs-lookup"><span data-stu-id="5373c-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="5373c-130">Esto puede dañar las conexiones que están abiertas para por lo que use mucho tiempo con cuidado.</span><span class="sxs-lookup"><span data-stu-id="5373c-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="5373c-131">También hemos actualizado el código para que ObjectContext.Connection.State ahora realiza un seguimiento del estado de la conexión subyacente correctamente.</span><span class="sxs-lookup"><span data-stu-id="5373c-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
