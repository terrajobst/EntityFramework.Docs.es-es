---
title: Lógica de reintento y resistencia de conexión - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
caps.latest.revision: 3
ms.openlocfilehash: 4c6e296ecb8b43468408d359f44fd1d1a6edf864
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122575"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="b0cc7-102">Lógica de reintento y resistencia de conexión</span><span class="sxs-lookup"><span data-stu-id="b0cc7-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="b0cc7-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b0cc7-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b0cc7-105">Las aplicaciones se conectan a un servidor de base de datos siempre han sido vulnerables a los saltos de conexión debido a errores de back-end y la inestabilidad en la red.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="b0cc7-106">Sin embargo, en un entorno basada en LAN, trabajar con servidores de base de datos dedicada estos errores son lo bastante raros que lógica adicional para controlar dichos errores con frecuencia no es necesario.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="b0cc7-107">Con el aumento de la nube según los servidores de base de datos, como Windows Azure SQL Database y conexiones a través de redes menos confiables ahora es más común para los saltos de conexión que se produzca.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="b0cc7-108">Esto puede deberse a que las técnicas defensivas que en la nube de uso de las bases de datos para garantizar la imparcialidad del servicio, como la limitación de la conexión o inestabilidad en la red provocando tiempos de espera intermitentes y otros errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="b0cc7-109">Resistencia de conexión hace referencia a la capacidad de EF volver a intentar automáticamente todos los comandos que se producirá un error debido a estos saltos de conexión.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="b0cc7-110">Estrategias de ejecución</span><span class="sxs-lookup"><span data-stu-id="b0cc7-110">Execution Strategies</span></span>  

<span data-ttu-id="b0cc7-111">Reintento de conexión se ocupa de una implementación de la interfaz IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="b0cc7-112">Las implementaciones de la IDbExecutionStrategy será responsables de aceptar una operación y, si se produce una excepción, determinar si un reintento es adecuado y volver a intentar si es.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="b0cc7-113">Hay cuatro estrategias de ejecución que se suministran con EF:</span><span class="sxs-lookup"><span data-stu-id="b0cc7-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="b0cc7-114">**DefaultExecutionStrategy**: esta estrategia de ejecución no reintenta las operaciones, es el valor predeterminado para las bases de datos distintos de sql server.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="b0cc7-115">**DefaultSqlExecutionStrategy**: se trata de una estrategia de ejecución interno que se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="b0cc7-116">Esta estrategia no vuelva a intentar en absoluto, sin embargo, ajustarán las excepciones que pueden ser transitorias para informar a los usuarios que desean habilitar la resistencia de conexión.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="b0cc7-117">**DbExecutionStrategy**: esta clase es adecuada como una clase base para otras estrategias de ejecución, incluidas sus propias plantillas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="b0cc7-118">Implementa una directiva de reintentos exponencial, donde el reintento inicial produce con retraso de cero y el retraso aumenta exponencialmente hasta que se alcance el número máximo de reintentos.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="b0cc7-119">Esta clase tiene un método abstracto ShouldRetryOn que se puede implementar en las estrategias de ejecución derivados para controlar las excepciones que se deben Reintentar.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="b0cc7-120">**SqlAzureExecutionStrategy**: esta estrategia de ejecución se hereda de DbExecutionStrategy y se volverá a intentar en las excepciones que se sabe que posiblemente transitorio al trabajar con Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="b0cc7-121">Estrategias de ejecución 2 y 4 se incluyen en el proveedor de Sql Server que se suministra con EF, que se encuentra en el ensamblado EntityFramework.SqlServer y están diseñadas para trabajar con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="b0cc7-122">Habilitación de una estrategia de ejecución</span><span class="sxs-lookup"><span data-stu-id="b0cc7-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="b0cc7-123">Es la manera más fácil para indicar a EF que use una estrategia de ejecución con el método SetExecutionStrategy de la [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) clase:</span><span class="sxs-lookup"><span data-stu-id="b0cc7-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="b0cc7-124">Este código indica a EF que use el SqlAzureExecutionStrategy al conectarse a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="b0cc7-125">Configuración de la estrategia de ejecución</span><span class="sxs-lookup"><span data-stu-id="b0cc7-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="b0cc7-126">El constructor de SqlAzureExecutionStrategy puede aceptar dos parámetros, MaxRetryCount y MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="b0cc7-127">Recuento de MaxRetry es el número máximo de veces que se reintentará la estrategia.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="b0cc7-128">El MaxDelay es un objeto TimeSpan que representa el retraso máximo entre reintentos que se va a usar la estrategia de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="b0cc7-129">Para establecer el número máximo de reintentos a 1 y el retraso máximo en 30 segundos debería execue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b0cc7-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="b0cc7-130">El SqlAzureExecutionStrategy volverá a intentar al instante se supera la primera vez que se produce un error transitorio, pero se retrasará ya entre cada reintento hasta que el máximo de cualquier límite de reintentos o el tiempo total alcanza el retraso máximo.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="b0cc7-131">Las estrategias de ejecución solo volverá a intentar un número limitado de las excepciones que suelen ser tansient, todavía deberá controlar otros errores, así como detectar la excepción RetryLimitExceeded para el caso de que un error no es transitorio o tarda demasiado tiempo resolver Sí.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="b0cc7-132">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="b0cc7-132">Limitations</span></span>  

<span data-ttu-id="b0cc7-133">Hay algunos conocidos de limitaciones cuando se usa una estrategia de ejecución de reintento:</span><span class="sxs-lookup"><span data-stu-id="b0cc7-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="b0cc7-134">No se admiten consultas de streaming</span><span class="sxs-lookup"><span data-stu-id="b0cc7-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="b0cc7-135">De forma predeterminada, EF6 y versiones posteriores mantiene los resultados de la consulta en lugar de streaming en ellos.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="b0cc7-136">Si desea tener resultados transmite por secuencias puede usar el método AsStreaming para cambiar un LINQ para consultar las entidades en streaming.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="b0cc7-137">No se admite la transmisión por secuencias cuando se registra una estrategia de ejecución de reintento.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="b0cc7-138">Esta limitación existe porque la conexión se puede quitar parte a través de los resultados devueltos.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="b0cc7-139">Cuando esto ocurre, EF debe volver a ejecutar la consulta completa, pero no tiene ninguna manera de saber qué resultados ya se han devuelto confiable (datos que han cambiado desde que se envió la consulta inicial, los resultados pueden proceder de vuelta en un orden diferente, los resultados no pueden tener un identificador único etcetera.).</span><span class="sxs-lookup"><span data-stu-id="b0cc7-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="b0cc7-140">Las transacciones no admitidas iniciada por el usuario</span><span class="sxs-lookup"><span data-stu-id="b0cc7-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="b0cc7-141">Cuando se ha configurado una estrategia de ejecución que da como resultado reintentos, existen algunas limitaciones respecto al uso de transacciones.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="b0cc7-142">¿Qué es compatible: de EF el comportamiento de transacciones predeterminado</span><span class="sxs-lookup"><span data-stu-id="b0cc7-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="b0cc7-143">De forma predeterminada, EF llevará a cabo las actualizaciones de base de datos dentro de una transacción.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="b0cc7-144">No es necesario hacer nada para habilitar esta opción, EF siempre lo hace automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="b0cc7-145">Por ejemplo, en el código siguiente SaveChanges se realiza automáticamente dentro de una transacción.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="b0cc7-146">Si SaveChanges fuera a producir un error después de insertar uno de la nueva carpeta del sitio, a continuación, se revertirán la transacción y no hay cambios aplicados a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="b0cc7-147">El contexto también se deja en un estado que permita SaveChanges vuelva a llamar para volver a intentar aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="b0cc7-148">Lo que no se admite: las transacciones iniciada por el usuario</span><span class="sxs-lookup"><span data-stu-id="b0cc7-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="b0cc7-149">Cuando no se utiliza una estrategia de ejecución de reintento puede encapsular varias operaciones en una sola transacción.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="b0cc7-150">Por ejemplo, el código siguiente incluye dos llamadas de SaveChanges en una sola transacción.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="b0cc7-151">Si cualquier parte de cualquiera de las operaciones se produce un error, a continuación, ninguno de los cambios se aplican.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-151">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="b0cc7-152">Esto no se admite cuando se usa una estrategia de ejecución de reintento, dado que EF no tiene en cuenta todas las operaciones anteriores y cómo reintentarlos.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="b0cc7-153">Por ejemplo, si el segundo error de SaveChanges, a continuación, EF ya no tiene la información necesaria para volver a intentar la primera llamada a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="b0cc7-154">Posibles soluciones</span><span class="sxs-lookup"><span data-stu-id="b0cc7-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="b0cc7-155">Suspender la estrategia de ejecución</span><span class="sxs-lookup"><span data-stu-id="b0cc7-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="b0cc7-156">Una posible solución es suspender la estrategia de ejecución de reintento para el fragmento de código que se debe utilizar un usuario inició la transacción.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="b0cc7-157">La manera más fácil de hacerlo es agregar una marca SuspendExecutionStrategy para el código en función de clase de configuración y cambie la expresión lambda de estrategia de ejecución para devolver la estrategia de ejecución (no retying) predeterminada cuando se establece la marca.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

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
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="b0cc7-158">Tenga en cuenta que estamos usando CallContext para almacenar el valor de marca.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="b0cc7-159">Esto proporciona una funcionalidad similar a almacenamiento local de subprocesos, pero es seguro usar con el código asincrónico - incluidos consulta asincrónica y guardar con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="b0cc7-160">Ahora nos podemos suspender la estrategia de ejecución de la sección de código que usa una transacción iniciada por el usuario.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

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

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="b0cc7-161">Llamar manualmente a la estrategia de ejecución</span><span class="sxs-lookup"><span data-stu-id="b0cc7-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="b0cc7-162">Otra opción es usar la estrategia de ejecución manualmente y asignarle el conjunto completo de la lógica que se ejecutará; por lo que puede reintentar todo lo que si se produce un error en una de las operaciones.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="b0cc7-163">Todavía es necesario suspender la estrategia de ejecución - mediante la técnica anterior - para que los contextos utilizados dentro del bloque de código que se puede reintentar no intentará volver a realizar.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="b0cc7-164">Tenga en cuenta que los contextos deben crearse dentro del bloque de código que se vuelva a intentar.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="b0cc7-165">Esto garantiza que estamos empezando con un estado limpio para cada reintento.</span><span class="sxs-lookup"><span data-stu-id="b0cc7-165">This ensures that we are starting with a clean state for each retry.</span></span>  

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
