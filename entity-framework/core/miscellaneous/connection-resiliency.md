---
title: Resistencia de conexión - Core EF
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053535"
---
# <a name="connection-resiliency"></a>Resistencia de conexión

Resistencia de conexión reintenta automáticamente comandos de base de datos. La característica se puede utilizar con cualquier base de datos proporcionando una "estrategia de ejecución," que encapsula la lógica necesaria para detectar errores y vuelva a intentar comandos. Proveedores principales EF pueden proporcionar estrategias de ejecución adaptadas a sus condiciones de error de base de datos específica y directivas de reintento óptimo.

Por ejemplo, el proveedor de SQL Server incluye una estrategia de ejecución que se adapta específicamente a SQL Server (incluido SQL Azure). Es compatible con los tipos de excepción que se pueden recuperar y tiene valores predeterminados razonables para el número máximo de reintentos, retraso entre reintentos, etcetera.

Una estrategia de ejecución se especifica al configurar las opciones para el contexto. Esto se encuentra normalmente en el `OnConfiguring` método de su contexto derivado o en `Startup.cs` para una aplicación de ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

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

## <a name="execution-strategies-and-transactions"></a>Las transacciones y las estrategias de ejecución

Una estrategia de ejecución que realice reintentos automáticamente cuando se produzcan errores debe ser capaz de reproducir cada operación en un bloque de reintentos que se produce un error. Cuando se habilitan los reintentos, cada operación que se realiza a través de EF Core se convierte en su propia operación reintentable, es decir, cada consulta y cada llamada a `SaveChanges()` se volverá a intentar como una unidad si se produce un error transitorio.

Sin embargo, si el código inicia una transacción mediante `BeginTransaction()` va a definir su propio grupo de operaciones que se deben tratar como una unidad, es decir, todo dentro de la transacción necesitaría reproducirse producirá un error. Recibirá una excepción similar al siguiente si intenta hacer esto si usa una estrategia de ejecución.

> InvalidOperationException: La estrategia de ejecución configurada 'SqlServerRetryingExecutionStrategy' no es compatible con las transacciones iniciadas por el usuario. Utilice la estrategia de ejecución devuelta por 'DbContext.Database.CreateExecutionStrategy()' para ejecutar todas las operaciones en la transacción como una unidad reintentable.

La solución consiste en invocar manualmente la estrategia de ejecución con un delegado que representa todos los elementos que se debe ejecutar. Si se produce un error transitorio, la estrategia de ejecución invoca al delegado de nuevo.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Error de confirmación de transacción y el problema de idempotencia

En general, cuando se produce un error de conexión se revierte la transacción actual. Sin embargo, si la conexión se interrumpe mientras la transacción que se ha confirmado resultante se conoce el estado de la transacción. Consulte esta [entrada de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) para obtener más detalles.

De forma predeterminada, la estrategia de ejecución volverá a intentar la operación como si se revierte la transacción, pero si no es el caso esto dará como resultado una excepción si el nuevo estado de la base de datos no es compatible o podría dar lugar a **daños en los datos** si el operación no se basa en un estado determinado, por ejemplo, cuando se inserta una nueva fila con valores de clave generada automáticamente.

Hay varias formas de solucionar esto.

### <a name="option-1---do-almost-nothing"></a>Opción 1: hacer nada (casi)

La probabilidad de un error de conexión durante la confirmación de la transacción es baja, por lo que puede ser aceptable para su aplicación simplemente un error si esta condición se produce realmente.

Sin embargo, debe evitar el uso de claves generadas por el almacén con el fin de asegurarse de que se produce una excepción en lugar de agregar una fila duplicada. Considere el uso de un valor GUID generado por el cliente o un generador de valores de lado cliente.

### <a name="option-2---rebuild-application-state"></a>Opción 2: estado de la aplicación de regeneración

1. Descartar actual `DbContext`.
2. Crear un nuevo `DbContext` y restaurar el estado de la aplicación de la base de datos.
3. Informar al usuario que la última operación podría no hayan finalizado correctamente.

### <a name="option-3---add-state-verification"></a>Opción 3: agregar la comprobación de estado

Para la mayoría de las operaciones que cambian el estado de la base de datos es posible agregar código que compruebe si se ha realizado correctamente. EF proporciona un método de extensión para facilitar esta tarea - `IExecutionStrategy.ExecuteInTransaction`.

Este método comienza y confirma una transacción y también acepta una función en la `verifySucceeded` parámetro que se invoca cuando se produce un error transitorio durante la confirmación de transacción.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Aquí `SaveChanges` se invoca con `acceptAllChangesOnSuccess` establecido en `false` para evitar cambiar el estado de la `Blog` entidad a `Unchanged` si `SaveChanges` se realiza correctamente. Esto permite para volver a intentar la misma operación si se produce un error en la confirmación y la transacción se revierte.

### <a name="option-4---manually-track-the-transaction"></a>Opción 4: realizar un seguimiento de la transacción manual

Si necesita usar claves generadas por el almacén o necesita una forma genérica de control de errores de confirmación que no depende de la operación se ejecuta cada transacción se pudo asignar un identificador que se comprueba cuando se produce un error en la confirmación.

1. Agregar una tabla a la base de datos que se utiliza para realizar un seguimiento del estado de las transacciones.
2. Inserta una fila en la tabla al principio de cada transacción.
3. Si se produce un error en la conexión durante la confirmación, compruebe la presencia de la fila correspondiente en la base de datos.
4. Si la confirmación es correcta, elimine la fila correspondiente para evitar el crecimiento de la tabla.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Asegúrese de que el contexto utilizado para la comprobación tiene una estrategia de ejecución que se define como la conexión es probable que produzca un error durante la comprobación si se produjo un error durante la confirmación de la transacción.
