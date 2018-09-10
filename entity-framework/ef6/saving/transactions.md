---
title: Trabajar con transacciones - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 26473e1e52a6044babc717d5b158ad73aac5c738
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250614"
---
# <a name="working-with-transactions"></a><span data-ttu-id="9d377-102">Trabajar con transacciones</span><span class="sxs-lookup"><span data-stu-id="9d377-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="9d377-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9d377-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9d377-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="9d377-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="9d377-105">Este documento se describirá mediante transacciones en EF6, incluidas las mejoras que hemos agregado desde EF5 para facilitar el trabajo con transacciones.</span><span class="sxs-lookup"><span data-stu-id="9d377-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="9d377-106">Lo que hace EF predeterminada</span><span class="sxs-lookup"><span data-stu-id="9d377-106">What EF does by default</span></span>  

<span data-ttu-id="9d377-107">En todas las versiones de Entity Framework, cada vez que ejecuta **SaveChanges()** Insertar, actualizar o eliminar en la base de datos, el marco de trabajo se encapsulará esa operación dentro de una transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="9d377-108">Esta transacción dura solo el tiempo suficiente para ejecutar la operación y, a continuación, se completa.</span><span class="sxs-lookup"><span data-stu-id="9d377-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="9d377-109">Cuando se ejecuta otra operación de este tipo se inicia una nueva transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="9d377-110">A partir de EF6 **Database.ExecuteSqlCommand()** de forma predeterminada se ajustará el comando en una transacción si uno ya no estaba presente.</span><span class="sxs-lookup"><span data-stu-id="9d377-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="9d377-111">Hay sobrecargas de este método que permiten invalidar este comportamiento si lo desea.</span><span class="sxs-lookup"><span data-stu-id="9d377-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="9d377-112">También en la ejecución de procedimientos almacenados incluidos en el modelo mediante las API como EF6 **ObjectContext.ExecuteFunction()** hace lo mismo (salvo que el comportamiento predeterminado no en el momento en que se puede invalidar).</span><span class="sxs-lookup"><span data-stu-id="9d377-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="9d377-113">En cualquier caso, el nivel de aislamiento de la transacción es cualquier nivel de aislamiento del proveedor de base de datos se considera que su valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9d377-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="9d377-114">De forma predeterminada, por ejemplo, en SQL Server trata READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="9d377-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="9d377-115">Entity Framework no ajusta a las consultas en una transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="9d377-116">Esta funcionalidad predeterminada es adecuada para un lote de usuarios y si por lo que no es necesario hacer algo diferente en EF6; escribir el código tal como lo hace habitualmente.</span><span class="sxs-lookup"><span data-stu-id="9d377-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="9d377-117">Sin embargo, algunos usuarios requieren mayor control sobre sus transacciones, este tema se trata en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="9d377-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="9d377-118">Cómo funcionan las API</span><span class="sxs-lookup"><span data-stu-id="9d377-118">How the APIs work</span></span>  

<span data-ttu-id="9d377-119">Antes de EF6 Entity Framework insistía en abrir la conexión de base de datos propio (produjo una excepción si se pasa una conexión que ya estaba abierta).</span><span class="sxs-lookup"><span data-stu-id="9d377-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="9d377-120">Puesto que sólo se puede iniciar una transacción en una conexión abierta, esto significaba que la única manera en que un usuario podría encapsular varias operaciones en una transacción era usar un [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) o usar el  **ObjectContext.Connection** propiedad y llamar a start **Open()** y **BeginTransaction()** directamente en el valor devuelto **EntityConnection** objeto.</span><span class="sxs-lookup"><span data-stu-id="9d377-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="9d377-121">Además, las llamadas de API que puede establecer contacto con la base de datos produciría un error si ha iniciado una transacción en la conexión de base de datos subyacente por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="9d377-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="9d377-122">Se ha quitado la limitación de aceptar únicamente conexiones cerradas en Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9d377-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="9d377-123">Para obtener más información, consulte [administración de conexiones](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="9d377-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="9d377-124">A partir de EF6 el marco de trabajo ahora proporciona:</span><span class="sxs-lookup"><span data-stu-id="9d377-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="9d377-125">**Database.BeginTransaction()** : un método más fácil para un usuario iniciar y completar las transacciones dentro de un elemento DbContext existente: permitir que varias operaciones que va a combinarse en la misma transacción y, por tanto, todos confirmada o todos revierte como uno.</span><span class="sxs-lookup"><span data-stu-id="9d377-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="9d377-126">También permite al usuario especificar más fácilmente el nivel de aislamiento de la transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="9d377-127">**Database.UseTransaction()** : lo que permite que la clase DbContext para utilizar una transacción que se inició fuera de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9d377-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="9d377-128">Combinar varias operaciones en una transacción dentro del mismo contexto</span><span class="sxs-lookup"><span data-stu-id="9d377-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="9d377-129">**Database.BeginTransaction()** tiene dos invalidaciones: uno que toma una explícita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) y uno que no toma ningún argumento y utiliza el valor predeterminado IsolationLevel desde el proveedor de base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="9d377-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="9d377-130">En ambos devuelven un **DbContextTransaction** objeto que proporciona **Commit()** y **Rollback()** métodos que realizan commit y rollback en el almacén subyacente transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="9d377-131">El **DbContextTransaction** está pensado para ser desechado una vez que se ha confirmado o revertido.</span><span class="sxs-lookup"><span data-stu-id="9d377-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="9d377-132">Una forma sencilla de lograrlo es el **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="9d377-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="9d377-133">la sintaxis que se llamará automáticamente **Dispose()** cuando el bloque se complete:</span><span class="sxs-lookup"><span data-stu-id="9d377-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
> <span data-ttu-id="9d377-134">A partir de una transacción requiere que la conexión del almacén subyacente está abierta.</span><span class="sxs-lookup"><span data-stu-id="9d377-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="9d377-135">Por lo que una llamada a Database.BeginTransaction() se abrirá la conexión si no está ya abierto.</span><span class="sxs-lookup"><span data-stu-id="9d377-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="9d377-136">Si DbContextTransaction abre la conexión, a continuación, cerrará, cuando se llama a Dispose().</span><span class="sxs-lookup"><span data-stu-id="9d377-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="9d377-137">Pasar una transacción existente al contexto</span><span class="sxs-lookup"><span data-stu-id="9d377-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="9d377-138">A veces desea que una transacción que es incluso más amplio en el ámbito y que incluye operaciones en la misma base de datos, pero fuera de EF completamente.</span><span class="sxs-lookup"><span data-stu-id="9d377-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="9d377-139">Para realizar esta acción abre la conexión e iniciar la transacción por sí mismo y, a continuación, indicar a EF a) para usar la conexión de base de datos ya abierto y (b) para usar la transacción existente en esa conexión.</span><span class="sxs-lookup"><span data-stu-id="9d377-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="9d377-140">Para ello debe definir y usar un constructor en la clase de contexto que se hereda de uno de los constructores de DbContext que toman i) un parámetro de conexión existente y ii) la contextOwnsConnection booleano.</span><span class="sxs-lookup"><span data-stu-id="9d377-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="9d377-141">La marca contextOwnsConnection debe establecerse en false cuando se llama en este escenario.</span><span class="sxs-lookup"><span data-stu-id="9d377-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="9d377-142">Esto es importante, ya que informa a Entity Framework que no debe cerrar la conexión cuando termine con él (por ejemplo, vea la línea 4 a continuación):</span><span class="sxs-lookup"><span data-stu-id="9d377-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="9d377-143">Además, debe iniciar la transacción usted mismo (incluida la IsolationLevel si desea evitar la configuración predeterminada) y dejar que Entity Framework sabe que hay una transacción existente que ya ha iniciado en la conexión (ver línea 33 a continuación).</span><span class="sxs-lookup"><span data-stu-id="9d377-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="9d377-144">A continuación, es libre para ejecutar operaciones de base de datos directamente en la instancia SqlConnection propio, o bien en DbContext.</span><span class="sxs-lookup"><span data-stu-id="9d377-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="9d377-145">Todas estas operaciones se ejecutan dentro de una transacción.</span><span class="sxs-lookup"><span data-stu-id="9d377-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="9d377-146">Asume la responsabilidad para confirmar o revertir la transacción y para llamar a Dispose() en él, así como para cerrar o desechar la conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9d377-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="9d377-147">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9d377-147">For example:</span></span>  

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

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="9d377-148">Borrar la transacción</span><span class="sxs-lookup"><span data-stu-id="9d377-148">Clearing up the transaction</span></span>

<span data-ttu-id="9d377-149">Puede pasar null para Database.UseTransaction() para borrar el conocimiento de Entity Framework de la transacción actual.</span><span class="sxs-lookup"><span data-stu-id="9d377-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="9d377-150">Entity Framework le ni confirmar ni revertir la transacción existente al hacerlo, así que use con cuidado y solo si está seguro de esto es lo que desea hacer.</span><span class="sxs-lookup"><span data-stu-id="9d377-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="9d377-151">Errores en UseTransaction</span><span class="sxs-lookup"><span data-stu-id="9d377-151">Errors in UseTransaction</span></span>

<span data-ttu-id="9d377-152">Verá una excepción desde Database.UseTransaction() si pasa una transacción cuando:</span><span class="sxs-lookup"><span data-stu-id="9d377-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="9d377-153">Entity Framework ya tiene una transacción existente</span><span class="sxs-lookup"><span data-stu-id="9d377-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="9d377-154">Entity Framework ya está trabajando dentro de un TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9d377-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="9d377-155">En la transacción pasado el objeto de conexión es null.</span><span class="sxs-lookup"><span data-stu-id="9d377-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="9d377-156">Es decir, la transacción no está asociada con una conexión, que normalmente es un indicio de que esa transacción ya se ha completado</span><span class="sxs-lookup"><span data-stu-id="9d377-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="9d377-157">En la transacción pasado el objeto de conexión no coincide con la conexión de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9d377-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="9d377-158">Usar transacciones con otras características</span><span class="sxs-lookup"><span data-stu-id="9d377-158">Using transactions with other features</span></span>  

<span data-ttu-id="9d377-159">Esta sección detalla cómo interactúan las transacciones anteriores:</span><span class="sxs-lookup"><span data-stu-id="9d377-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="9d377-160">Resistencia de conexión</span><span class="sxs-lookup"><span data-stu-id="9d377-160">Connection resiliency</span></span>  
- <span data-ttu-id="9d377-161">Métodos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="9d377-161">Asynchronous methods</span></span>  
- <span data-ttu-id="9d377-162">Transacciones de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9d377-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="9d377-163">Resistencia de las conexiones</span><span class="sxs-lookup"><span data-stu-id="9d377-163">Connection Resiliency</span></span>  

<span data-ttu-id="9d377-164">La nueva característica de resistencia de conexión no funciona con las transacciones iniciadas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="9d377-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="9d377-165">Para obtener más información, consulte [estrategias de ejecución reintentos](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="9d377-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="9d377-166">Programación asincrónica</span><span class="sxs-lookup"><span data-stu-id="9d377-166">Asynchronous Programming</span></span>  

<span data-ttu-id="9d377-167">El enfoque descrito en las secciones anteriores no necesita más opciones ni configuración para trabajar con el [asincrónica consultar y guardar los métodos](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="9d377-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="9d377-168">Pero tenga en cuenta que, según lo que lo hace dentro de los métodos asincrónicos, esto puede producir en transacciones de larga ejecución, lo que a su vez pueden provocar interbloqueos o bloqueo que es perjudicial para el rendimiento de la aplicación global.</span><span class="sxs-lookup"><span data-stu-id="9d377-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="9d377-169">Transacciones de TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9d377-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="9d377-170">Antes de EF6 era usar un objeto TransactionScope la manera recomendada de proporcionar más grandes transacciones de ámbito:</span><span class="sxs-lookup"><span data-stu-id="9d377-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="9d377-171">SqlConnection y Entity Framework se usan la transacción ambiente de TransactionScope y, por tanto, se confirmen juntos.</span><span class="sxs-lookup"><span data-stu-id="9d377-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="9d377-172">A partir de .NET 4.5.1 TransactionScope se ha actualizado para trabajar también con los métodos asincrónicos mediante el uso de la [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeración:</span><span class="sxs-lookup"><span data-stu-id="9d377-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="9d377-173">Todavía hay algunas limitaciones en el enfoque de TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="9d377-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="9d377-174">Requiere que .NET 4.5.1 o posterior, para trabajar con los métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="9d377-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="9d377-175">No se puede usar en escenarios de nube a menos que esté seguro de que tiene una y solo una conexión (escenarios de nube no admiten transacciones distribuidas).</span><span class="sxs-lookup"><span data-stu-id="9d377-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="9d377-176">No se puede combinar con el enfoque Database.UseTransaction() de las secciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="9d377-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="9d377-177">Producirán excepciones si se emite ninguna instrucción DDL y no se han habilitado las transacciones distribuidas a través del servicio MSDTC.</span><span class="sxs-lookup"><span data-stu-id="9d377-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="9d377-178">Ventajas del enfoque TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="9d377-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="9d377-179">Actualizarán automáticamente una transacción local a una transacción distribuida si se realiza más de una conexión a una base de datos o combine una conexión a una base de datos con una conexión a una base de datos diferentes en la misma transacción (Nota: debe tener el servicio MSDTC configurado para permitir las transacciones distribuidas para que funcione).</span><span class="sxs-lookup"><span data-stu-id="9d377-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="9d377-180">Facilidad de codificación.</span><span class="sxs-lookup"><span data-stu-id="9d377-180">Ease of coding.</span></span> <span data-ttu-id="9d377-181">Si prefiere que la transacción ambiente y tratarán de forma implícita en segundo plano en lugar de en forma explícita de control, a continuación, el enfoque de TransactionScope posible a sus necesidades mejor.</span><span class="sxs-lookup"><span data-stu-id="9d377-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="9d377-182">En resumen, con el nuevo Database.BeginTransaction() y Database.UseTransaction() API anterior, el enfoque de TransactionScope ya no es necesario para la mayoría de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="9d377-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="9d377-183">Si se sigue usando TransactionScope, a continuación, tenga en cuenta las limitaciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="9d377-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="9d377-184">Se recomienda usar el enfoque descrito en las secciones anteriores en su lugar, siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="9d377-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
