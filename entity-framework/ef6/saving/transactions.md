---
title: 'Trabajo con transacciones: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416244"
---
# <a name="working-with-transactions"></a><span data-ttu-id="6ace7-102">Trabajar con transacciones</span><span class="sxs-lookup"><span data-stu-id="6ace7-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="6ace7-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6ace7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="6ace7-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="6ace7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="6ace7-105">En este documento se describe el uso de transacciones en EF6, incluidas las mejoras que hemos agregado desde EF5 para facilitar el trabajo con transacciones.</span><span class="sxs-lookup"><span data-stu-id="6ace7-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="6ace7-106">Qué hace EF de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="6ace7-106">What EF does by default</span></span>  

<span data-ttu-id="6ace7-107">En todas las versiones de Entity Framework, siempre que ejecute **SaveChanges ()** para insertar, actualizar o eliminar en la base de datos, el marco de trabajo encapsulará esa operación en una transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="6ace7-108">Esta transacción dura solo el tiempo suficiente para ejecutar la operación y, a continuación, se completa.</span><span class="sxs-lookup"><span data-stu-id="6ace7-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="6ace7-109">Al ejecutar otra operación de este tipo, se inicia una nueva transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="6ace7-110">A partir de la **base de datos EF6. ExecuteSqlCommand ()** , de forma predeterminada, ajustará el comando en una transacción si aún no está presente.</span><span class="sxs-lookup"><span data-stu-id="6ace7-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="6ace7-111">Hay sobrecargas de este método que le permiten invalidar este comportamiento si lo desea.</span><span class="sxs-lookup"><span data-stu-id="6ace7-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="6ace7-112">Además, en la ejecución EF6 de procedimientos almacenados incluidos en el modelo a través de API como **ObjectContext. ExecuteFunction ()** hace lo mismo (salvo que no se puede reemplazar el comportamiento predeterminado en ese momento).</span><span class="sxs-lookup"><span data-stu-id="6ace7-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="6ace7-113">En cualquier caso, el nivel de aislamiento de la transacción es cualquier nivel de aislamiento en el que el proveedor de base de datos considere su configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6ace7-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="6ace7-114">De forma predeterminada, por ejemplo, en SQL Server este es READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="6ace7-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="6ace7-115">Entity Framework no encapsula las consultas en una transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="6ace7-116">Esta funcionalidad predeterminada es adecuada para muchos usuarios y, si es así, no es necesario hacer nada diferente en EF6; solo tiene que escribir el código como lo hizo siempre.</span><span class="sxs-lookup"><span data-stu-id="6ace7-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="6ace7-117">Sin embargo, algunos usuarios requieren un mayor control sobre sus transacciones; esto se trata en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="6ace7-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="6ace7-118">Cómo funcionan las API</span><span class="sxs-lookup"><span data-stu-id="6ace7-118">How the APIs work</span></span>  

<span data-ttu-id="6ace7-119">Antes de EF6 Entity Framework insista en abrir la propia conexión de base de datos (se produjo una excepción si se pasa una conexión que ya estaba abierta).</span><span class="sxs-lookup"><span data-stu-id="6ace7-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="6ace7-120">Puesto que una transacción solo se puede iniciar en una conexión abierta, esto significaba que la única forma en que un usuario podía encapsular varias operaciones en una transacción era usar [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) o usar la propiedad **ObjectContext. Connection** y comenzar a llamar a **Open ()** y **BeginTransaction ()** directamente en el objeto **EntityConnection** devuelto.</span><span class="sxs-lookup"><span data-stu-id="6ace7-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="6ace7-121">Además, las llamadas API que contacten con la base de datos generarán un error si hubiera iniciado una transacción en la conexión de base de datos subyacente por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="6ace7-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="6ace7-122">La limitación de aceptar solo conexiones cerradas se quitó en Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6ace7-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="6ace7-123">Para obtener más información, consulte [Administración de conexiones](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="6ace7-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="6ace7-124">A partir de EF6, el marco de trabajo ahora proporciona:</span><span class="sxs-lookup"><span data-stu-id="6ace7-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="6ace7-125">**Database. BeginTransaction ()** : método más sencillo para que un usuario inicie y complete transacciones en un DbContext existente, lo que permite combinar varias operaciones dentro de la misma transacción y, por lo tanto, todas confirmadas o revertidas como una.</span><span class="sxs-lookup"><span data-stu-id="6ace7-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="6ace7-126">También permite al usuario especificar más fácilmente el nivel de aislamiento para la transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="6ace7-127">**Database. UseTransaction ()** : permite que DbContext use una transacción que se inició fuera del Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6ace7-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="6ace7-128">Combinar varias operaciones en una transacción dentro del mismo contexto</span><span class="sxs-lookup"><span data-stu-id="6ace7-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="6ace7-129">**Database. BeginTransaction ()** tiene dos invalidaciones: una que toma un [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) explícito y otra que no toma ningún argumento y usa el valor de IsolationLevel predeterminado del proveedor de base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="6ace7-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="6ace7-130">Ambas invalidaciones devuelven un objeto **DbContextTransaction** que proporciona los métodos **Commit ()** y **Rollback ()** que realizan la confirmación y reversión en la transacción del almacén subyacente.</span><span class="sxs-lookup"><span data-stu-id="6ace7-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="6ace7-131">La **DbContextTransaction** está pensada para ser eliminada una vez que se ha confirmado o revertido.</span><span class="sxs-lookup"><span data-stu-id="6ace7-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="6ace7-132">Una manera fácil de lograrlo es **usar (...) {.** ..}</span><span class="sxs-lookup"><span data-stu-id="6ace7-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="6ace7-133">sintaxis que llamará automáticamente a **Dispose ()** cuando se complete el bloque Using:</span><span class="sxs-lookup"><span data-stu-id="6ace7-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="6ace7-134">Iniciar una transacción requiere que la conexión del almacén subyacente esté abierta.</span><span class="sxs-lookup"><span data-stu-id="6ace7-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="6ace7-135">Por tanto, si se llama a Database. BeginTransaction (), se abrirá la conexión si aún no está abierta.</span><span class="sxs-lookup"><span data-stu-id="6ace7-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="6ace7-136">Si DbContextTransaction abrió la conexión, se cerrará cuando se llame a Dispose ().</span><span class="sxs-lookup"><span data-stu-id="6ace7-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="6ace7-137">Pasar una transacción existente al contexto</span><span class="sxs-lookup"><span data-stu-id="6ace7-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="6ace7-138">A veces, desea una transacción que es incluso más amplia en el ámbito y que incluye operaciones en la misma base de datos pero fuera de EF completamente.</span><span class="sxs-lookup"><span data-stu-id="6ace7-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="6ace7-139">Para ello, debe abrir la conexión e iniciar la transacción usted mismo y, a continuación, indicar a EF a) que use la conexión de base de datos ya abierta y b) para usar la transacción existente en esa conexión.</span><span class="sxs-lookup"><span data-stu-id="6ace7-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="6ace7-140">Para ello, debe definir y usar un constructor en la clase de contexto que herede de uno de los constructores DbContext que llevan i) un parámetro de conexión existente y II) el valor booleano contextOwnsConnection.</span><span class="sxs-lookup"><span data-stu-id="6ace7-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="6ace7-141">La marca contextOwnsConnection debe establecerse en false cuando se llama en este escenario.</span><span class="sxs-lookup"><span data-stu-id="6ace7-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="6ace7-142">Esto es importante a medida que informa Entity Framework que no debería cerrar la conexión cuando se realiza con ella (por ejemplo, vea la línea 4 a continuación):</span><span class="sxs-lookup"><span data-stu-id="6ace7-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="6ace7-143">Además, debe iniciar la transacción usted mismo (incluido el valor de IsolationLevel si desea evitar la configuración predeterminada) y dejar que Entity Framework sepa que hay una transacción existente ya iniciada en la conexión (consulte la línea 33 a continuación).</span><span class="sxs-lookup"><span data-stu-id="6ace7-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="6ace7-144">Después, puede ejecutar operaciones de base de datos directamente en el SqlConnection o en DbContext.</span><span class="sxs-lookup"><span data-stu-id="6ace7-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="6ace7-145">Todas estas operaciones se ejecutan dentro de una transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="6ace7-146">Usted asume la responsabilidad de confirmar o revertir la transacción y de llamar a Dispose () en ella, así como para cerrar y eliminar la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6ace7-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="6ace7-147">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6ace7-147">For example:</span></span>  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="6ace7-148">Borrar la transacción</span><span class="sxs-lookup"><span data-stu-id="6ace7-148">Clearing up the transaction</span></span>

<span data-ttu-id="6ace7-149">Puede pasar null a Database. UseTransaction () para borrar el conocimiento de Entity Framework de la transacción actual.</span><span class="sxs-lookup"><span data-stu-id="6ace7-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="6ace7-150">Entity Framework no confirmará ni revertirá la transacción existente al hacerlo, por lo que debe usar con cuidado y solo si está seguro de que es lo que desea hacer.</span><span class="sxs-lookup"><span data-stu-id="6ace7-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="6ace7-151">Errores en UseTransaction</span><span class="sxs-lookup"><span data-stu-id="6ace7-151">Errors in UseTransaction</span></span>

<span data-ttu-id="6ace7-152">Verá una excepción de Database. UseTransaction () si pasa una transacción cuando:</span><span class="sxs-lookup"><span data-stu-id="6ace7-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="6ace7-153">Entity Framework ya tiene una transacción existente</span><span class="sxs-lookup"><span data-stu-id="6ace7-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="6ace7-154">Entity Framework ya está funcionando en TransactionScope</span><span class="sxs-lookup"><span data-stu-id="6ace7-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="6ace7-155">El objeto de conexión en la transacción pasada es NULL.</span><span class="sxs-lookup"><span data-stu-id="6ace7-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="6ace7-156">Es decir, la transacción no está asociada a una conexión; normalmente es un signo de que ya se ha completado la transacción.</span><span class="sxs-lookup"><span data-stu-id="6ace7-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="6ace7-157">El objeto de conexión de la transacción pasada no coincide con la conexión del Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6ace7-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="6ace7-158">Usar transacciones con otras características</span><span class="sxs-lookup"><span data-stu-id="6ace7-158">Using transactions with other features</span></span>  

<span data-ttu-id="6ace7-159">En esta sección se describe cómo interactúan las transacciones anteriores con:</span><span class="sxs-lookup"><span data-stu-id="6ace7-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="6ace7-160">Resistencia de la conexión</span><span class="sxs-lookup"><span data-stu-id="6ace7-160">Connection resiliency</span></span>  
- <span data-ttu-id="6ace7-161">Métodos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="6ace7-161">Asynchronous methods</span></span>  
- <span data-ttu-id="6ace7-162">Transacciones TransactionScope</span><span class="sxs-lookup"><span data-stu-id="6ace7-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="6ace7-163">Resistencia de conexión</span><span class="sxs-lookup"><span data-stu-id="6ace7-163">Connection Resiliency</span></span>  

<span data-ttu-id="6ace7-164">La nueva característica de resistencia de conexión no funciona con las transacciones iniciadas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="6ace7-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="6ace7-165">Para obtener más información, consulte [reintento de estrategias de ejecución](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="6ace7-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="6ace7-166">Programación asincrónica</span><span class="sxs-lookup"><span data-stu-id="6ace7-166">Asynchronous Programming</span></span>  

<span data-ttu-id="6ace7-167">El enfoque descrito en las secciones anteriores no necesita más opciones ni valores de configuración para trabajar con los [métodos de consulta y guardado asíncronos](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="6ace7-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="6ace7-168">Pero tenga en cuenta que, en función de lo que haga dentro de los métodos asincrónicos, esto puede dar lugar a transacciones de ejecución prolongada, lo que a su vez puede provocar interbloqueos o bloqueos que son incorrectos para el rendimiento de la aplicación global.</span><span class="sxs-lookup"><span data-stu-id="6ace7-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="6ace7-169">Transacciones TransactionScope</span><span class="sxs-lookup"><span data-stu-id="6ace7-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="6ace7-170">Antes de EF6, la manera recomendada de proporcionar transacciones de ámbito mayor era usar un objeto TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="6ace7-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="6ace7-171">SqlConnection y Entity Framework usarían ambas transacciones ambiente TransactionScope y, por tanto, se confirmarán juntas.</span><span class="sxs-lookup"><span data-stu-id="6ace7-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="6ace7-172">A partir de .NET 4.5.1 TransactionScope, se ha actualizado para que también funcione con métodos asincrónicos mediante el uso de la enumeración [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :</span><span class="sxs-lookup"><span data-stu-id="6ace7-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="6ace7-173">Todavía existen algunas limitaciones en el enfoque de TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="6ace7-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="6ace7-174">Requiere .NET 4.5.1 o superior para trabajar con métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="6ace7-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="6ace7-175">No se puede usar en escenarios en la nube a menos que esté seguro de que tiene una sola conexión (los escenarios en la nube no admiten transacciones distribuidas).</span><span class="sxs-lookup"><span data-stu-id="6ace7-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="6ace7-176">No se puede combinar con el enfoque Database. UseTransaction () de las secciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="6ace7-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="6ace7-177">Se producirán excepciones si se emite cualquier DDL y no se han habilitado las transacciones distribuidas a través del servicio MSDTC.</span><span class="sxs-lookup"><span data-stu-id="6ace7-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="6ace7-178">Ventajas del enfoque de TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="6ace7-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="6ace7-179">Actualizará automáticamente una transacción local a una transacción distribuida si realiza más de una conexión a una base de datos determinada o combina una conexión con una base de datos con una conexión a una base de datos diferente dentro de la misma transacción (Nota: debe tener el servicio MSDTC configurado para permitir que funcionen las transacciones distribuidas.</span><span class="sxs-lookup"><span data-stu-id="6ace7-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="6ace7-180">Facilidad de codificación.</span><span class="sxs-lookup"><span data-stu-id="6ace7-180">Ease of coding.</span></span> <span data-ttu-id="6ace7-181">Si prefiere que la transacción sea ambiente y se trate implícitamente en segundo plano en lugar de hacerlo explícitamente bajo el control, el enfoque de TransactionScope puede adaptarse mejor.</span><span class="sxs-lookup"><span data-stu-id="6ace7-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="6ace7-182">En Resumen, con las nuevas API Database. BeginTransaction () y Database. UseTransaction () anteriores, el enfoque de TransactionScope ya no es necesario para la mayoría de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="6ace7-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="6ace7-183">Si sigue usando TransactionScope, tenga en cuenta las limitaciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="6ace7-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="6ace7-184">Se recomienda usar el enfoque descrito en las secciones anteriores, siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="6ace7-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
