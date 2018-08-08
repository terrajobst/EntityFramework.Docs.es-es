---
title: Resistencia de conexión - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: dae646e39b4dbd96b34f47582f9b2aa531cf88a7
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614342"
---
# <a name="connection-resiliency"></a><span data-ttu-id="8dd58-102">Resistencia de las conexiones</span><span class="sxs-lookup"><span data-stu-id="8dd58-102">Connection Resiliency</span></span>

<span data-ttu-id="8dd58-103">Resistencia de la conexión reintenta automáticamente los comandos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8dd58-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="8dd58-104">La característica puede usarse con cualquier base de datos proporcionando una "estrategia de ejecución," que encapsula la lógica necesaria para detectar errores y vuelva a intentar los comandos.</span><span class="sxs-lookup"><span data-stu-id="8dd58-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="8dd58-105">Proveedores de EF Core pueden proporcionar estrategias de ejecución adaptadas a sus condiciones de error de base de datos específica y las directivas de reintento óptima.</span><span class="sxs-lookup"><span data-stu-id="8dd58-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="8dd58-106">Por ejemplo, el proveedor de SQL Server incluye una estrategia de ejecución que está específicamente adaptada a SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="8dd58-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="8dd58-107">Es compatible con los tipos de excepción que se pueden recuperar y tiene valores predeterminados razonables para el número máximo de reintentos, el retraso entre reintentos, etcetera.</span><span class="sxs-lookup"><span data-stu-id="8dd58-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="8dd58-108">Una estrategia de ejecución se especifica al configurar las opciones para el contexto.</span><span class="sxs-lookup"><span data-stu-id="8dd58-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="8dd58-109">Esto se encuentra normalmente en el `OnConfiguring` método de su contexto derivado, o en `Startup.cs` para una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8dd58-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="8dd58-110">Estrategia de ejecución personalizado</span><span class="sxs-lookup"><span data-stu-id="8dd58-110">Custom execution strategy</span></span>

<span data-ttu-id="8dd58-111">Hay un mecanismo para registrar una estrategia de ejecución personalizado propio si desea cambiar cualquiera de los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="8dd58-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="8dd58-112">Las transacciones y estrategias de ejecución</span><span class="sxs-lookup"><span data-stu-id="8dd58-112">Execution strategies and transactions</span></span>

<span data-ttu-id="8dd58-113">Una estrategia de ejecución reintenta automáticamente los errores de debe ser capaz de reproducir cada operación en un bloque de reintentos que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="8dd58-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="8dd58-114">Cuando se habilitan los reintentos, cada operación que se realiza a través de EF Core se convierte en su propia operación que se puede reintentar.</span><span class="sxs-lookup"><span data-stu-id="8dd58-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="8dd58-115">Es decir, cada consulta y cada llamada a `SaveChanges()` se reintentará como una unidad si se produce un error transitorio.</span><span class="sxs-lookup"><span data-stu-id="8dd58-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="8dd58-116">Sin embargo, si el código inicia una transacción utilizando `BeginTransaction()` va a definir su propio grupo de operaciones que se deben tratar como una unidad, y todo dentro de la transacción tendría que se reproducirá se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="8dd58-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="8dd58-117">Si se intenta hacer esto cuando se usa una estrategia de ejecución, recibirá una excepción similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8dd58-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="8dd58-118">InvalidOperationException: La estrategia de ejecución configurada "SqlServerRetryingExecutionStrategy" no es compatible con las transacciones iniciadas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="8dd58-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="8dd58-119">Use la estrategia de ejecución que devuelve "DbContext.Database.CreateExecutionStrategy()" para ejecutar todas las operaciones en la transacción como una unidad que se puede reintentar.</span><span class="sxs-lookup"><span data-stu-id="8dd58-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="8dd58-120">La solución consiste en invocar manualmente la estrategia de ejecución con un delegado que representa todo lo que se debe ejecutar.</span><span class="sxs-lookup"><span data-stu-id="8dd58-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="8dd58-121">Si se produce un error transitorio, la estrategia de ejecución vuelve a invocar al delegado.</span><span class="sxs-lookup"><span data-stu-id="8dd58-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="8dd58-122">Error de confirmación de transacción y el problema de idempotencia</span><span class="sxs-lookup"><span data-stu-id="8dd58-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="8dd58-123">En general, cuando se produce un error de conexión se revierte la transacción actual.</span><span class="sxs-lookup"><span data-stu-id="8dd58-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="8dd58-124">Sin embargo, si se interrumpe la conexión mientras la transacción está confirmada resultante se conoce el estado de la transacción.</span><span class="sxs-lookup"><span data-stu-id="8dd58-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="8dd58-125">Vea este [entrada de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="8dd58-125">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="8dd58-126">De forma predeterminada, la estrategia de ejecución volverá a intentar la operación como si se ha revertido la transacción, pero si no es así Esto provocará una excepción si el nuevo estado de la base de datos no es compatible o podría dar lugar a **daños en los datos** si el operación no se basa en un estado determinado, por ejemplo, cuando se inserta una nueva fila con valores de clave generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="8dd58-127">Hay varias maneras de tratar este asunto.</span><span class="sxs-lookup"><span data-stu-id="8dd58-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="8dd58-128">Opción 1: hacer nada (casi)</span><span class="sxs-lookup"><span data-stu-id="8dd58-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="8dd58-129">La probabilidad de un error de conexión durante la confirmación de transacción es baja, por lo que puede ser aceptable para su aplicación simplemente no funcione correctamente si esta condición se produce realmente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="8dd58-130">Sin embargo, deberá evitar el uso de claves generadas por el almacén con el fin de asegurarse de que se produce una excepción en lugar de agregar una fila duplicada.</span><span class="sxs-lookup"><span data-stu-id="8dd58-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="8dd58-131">Considere el uso de un valor GUID generado por el cliente o un generador de valor del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="8dd58-132">Opción 2: volver a generar el estado de aplicación</span><span class="sxs-lookup"><span data-stu-id="8dd58-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="8dd58-133">Descartar actual `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8dd58-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="8dd58-134">Cree un nuevo `DbContext` y restaurar el estado de la aplicación desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8dd58-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="8dd58-135">Informar al usuario que la última operación es posible que no se completaron correctamente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="8dd58-136">Opción 3: agregar comprobación de estado</span><span class="sxs-lookup"><span data-stu-id="8dd58-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="8dd58-137">Para la mayoría de las operaciones que cambian el estado de la base de datos es posible agregar código que comprueba si se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="8dd58-138">EF proporciona un método de extensión para facilitar esta tarea - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="8dd58-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="8dd58-139">Este método comienza y confirma una transacción y también acepta una función en el `verifySucceeded` parámetro que se invoca cuando se produce un error transitorio durante la confirmación de transacción.</span><span class="sxs-lookup"><span data-stu-id="8dd58-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="8dd58-140">Aquí `SaveChanges` se invoca con `acceptAllChangesOnSuccess` establecido en `false` para evitar cambiar el estado de la `Blog` entidad `Unchanged` si `SaveChanges` se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="8dd58-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="8dd58-141">Esto permite volver a intentar la misma operación si se produce un error en la confirmación y se revierte la transacción.</span><span class="sxs-lookup"><span data-stu-id="8dd58-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="8dd58-142">Opción 4: realizar un seguimiento manual de la transacción</span><span class="sxs-lookup"><span data-stu-id="8dd58-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="8dd58-143">Si necesita usar claves generadas por el almacén o necesita una manera genérica de control de errores de confirmación que no depende de la operación realizada cada transacción se podría asignar un identificador que se comprueba cuando se produce un error en la confirmación.</span><span class="sxs-lookup"><span data-stu-id="8dd58-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="8dd58-144">Agregar una tabla a la base de datos utilizada para realizar un seguimiento del estado de las transacciones.</span><span class="sxs-lookup"><span data-stu-id="8dd58-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="8dd58-145">Insertar una fila en la tabla al principio de cada transacción.</span><span class="sxs-lookup"><span data-stu-id="8dd58-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="8dd58-146">Si se produce un error en la conexión durante la confirmación, compruebe la presencia de la fila correspondiente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8dd58-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="8dd58-147">Si la confirmación se realiza correctamente, elimine la fila correspondiente para evitar el crecimiento de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8dd58-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="8dd58-148">Asegúrese de que el contexto utilizado para la comprobación tiene una estrategia de ejecución que se define como la conexión es propenso a errores durante la comprobación de si se produjo un error durante la confirmación de la transacción.</span><span class="sxs-lookup"><span data-stu-id="8dd58-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
