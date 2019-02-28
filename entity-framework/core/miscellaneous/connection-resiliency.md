---
title: Resistencia de conexión - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 6d8cf117dfd94524a53e10bb4a23c2a44c4c8e7b
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459177"
---
# <a name="connection-resiliency"></a>Resistencia de las conexiones

Resistencia de la conexión reintenta automáticamente los comandos de base de datos. La característica puede usarse con cualquier base de datos proporcionando una "estrategia de ejecución," que encapsula la lógica necesaria para detectar errores y vuelva a intentar los comandos. Los proveedores de EF Core pueden proporcionar estrategias de ejecución adaptándose a sus condiciones de error de base de datos específica y las directivas de reintento óptima.

Por ejemplo, el proveedor de SQL Server incluye una estrategia de ejecución que está específicamente adaptada a SQL Server (incluido SQL Azure). Es compatible con los tipos de excepción que se pueden recuperar y tiene valores predeterminados razonables para el número máximo de reintentos, el retraso entre reintentos, etcetera.

Una estrategia de ejecución se especifica al configurar las opciones para el contexto. Esto se encuentra normalmente en el `OnConfiguring` método de su contexto derivado:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

o bien en `Startup.cs` para una aplicación ASP.NET Core:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>Estrategia de ejecución personalizado

Hay un mecanismo para registrar una estrategia de ejecución personalizado propio si desea cambiar cualquiera de los valores predeterminados.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Las transacciones y estrategias de ejecución

Una estrategia de ejecución reintenta automáticamente los errores de debe ser capaz de reproducir cada operación en un bloque de reintentos que se produce un error. Cuando se habilitan los reintentos, cada operación que se realiza a través de EF Core se convierte en su propia operación que se puede reintentar. Es decir, cada consulta y cada llamada a `SaveChanges()` se reintentará como una unidad si se produce un error transitorio.

Sin embargo, si el código inicia una transacción utilizando `BeginTransaction()` va a definir su propio grupo de operaciones que se deben tratar como una unidad, y todo dentro de la transacción tendría que se reproducirá se producirá un error. Si se intenta hacer esto cuando se usa una estrategia de ejecución, recibirá una excepción similar al siguiente:

> InvalidOperationException: la estrategia de ejecución configurada "SqlServerRetryingExecutionStrategy" no es compatible con las transacciones que el usuario inicie. Use la estrategia de ejecución que devuelve "DbContext.Database.CreateExecutionStrategy()" para ejecutar todas las operaciones en la transacción como una unidad que se puede reintentar.

La solución consiste en invocar manualmente la estrategia de ejecución con un delegado que representa todo lo que se debe ejecutar. Si se produce un error transitorio, la estrategia de ejecución vuelve a invocar al delegado.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

Este enfoque también se puede utilizar con transacciones de ambiente.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Error de confirmación de transacción y el problema de idempotencia

En general, cuando se produce un error de conexión se revierte la transacción actual. Sin embargo, si se interrumpe la conexión mientras la transacción está confirmada resultante se conoce el estado de la transacción. Vea este [entrada de blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obtener más detalles.

De forma predeterminada, la estrategia de ejecución volverá a intentar la operación como si se ha revertido la transacción, pero si no es así Esto provocará una excepción si el nuevo estado de la base de datos no es compatible o podría dar lugar a **daños en los datos** si el operación no se basa en un estado determinado, por ejemplo, cuando se inserta una nueva fila con valores de clave generados automáticamente.

Hay varias maneras de tratar este asunto.

### <a name="option-1---do-almost-nothing"></a>Opción 1: hacer nada (casi)

La probabilidad de un error de conexión durante la confirmación de transacción es baja, por lo que puede ser aceptable para su aplicación simplemente no funcione correctamente si esta condición se produce realmente.

Sin embargo, deberá evitar el uso de claves generadas por el almacén con el fin de asegurarse de que se produce una excepción en lugar de agregar una fila duplicada. Considere el uso de un valor GUID generado por el cliente o un generador de valor del lado cliente.

### <a name="option-2---rebuild-application-state"></a>Opción 2: volver a generar el estado de aplicación

1. Descartar actual `DbContext`.
2. Cree un nuevo `DbContext` y restaurar el estado de la aplicación desde la base de datos.
3. Informar al usuario que la última operación es posible que no se completaron correctamente.

### <a name="option-3---add-state-verification"></a>Opción 3: agregar comprobación de estado

Para la mayoría de las operaciones que cambian el estado de la base de datos es posible agregar código que comprueba si se ha ejecutado correctamente. EF proporciona un método de extensión para facilitar esta tarea - `IExecutionStrategy.ExecuteInTransaction`.

Este método comienza y confirma una transacción y también acepta una función en el `verifySucceeded` parámetro que se invoca cuando se produce un error transitorio durante la confirmación de transacción.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Aquí `SaveChanges` se invoca con `acceptAllChangesOnSuccess` establecido en `false` para evitar cambiar el estado de la `Blog` entidad `Unchanged` si `SaveChanges` se realiza correctamente. Esto permite volver a intentar la misma operación si se produce un error en la confirmación y se revierte la transacción.

### <a name="option-4---manually-track-the-transaction"></a>Opción 4: realizar un seguimiento manual de la transacción

Si necesita usar claves generadas por el almacén o necesita una manera genérica de control de errores de confirmación que no depende de la operación realizada cada transacción se podría asignar un identificador que se comprueba cuando se produce un error en la confirmación.

1. Agregar una tabla a la base de datos utilizada para realizar un seguimiento del estado de las transacciones.
2. Insertar una fila en la tabla al principio de cada transacción.
3. Si se produce un error en la conexión durante la confirmación, compruebe la presencia de la fila correspondiente en la base de datos.
4. Si la confirmación se realiza correctamente, elimine la fila correspondiente para evitar el crecimiento de la tabla.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Asegúrese de que el contexto utilizado para la comprobación tiene una estrategia de ejecución que se define como la conexión es propenso a errores durante la comprobación de si se produjo un error durante la confirmación de la transacción.
