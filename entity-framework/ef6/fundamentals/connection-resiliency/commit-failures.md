---
title: Control de errores de confirmación de transacción - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 71d5649dd993bb95e24165a55d812c71a37f03f3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489393"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="e96f2-102">Control de errores de confirmación de transacción</span><span class="sxs-lookup"><span data-stu-id="e96f2-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="e96f2-103">**EF6.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="e96f2-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="e96f2-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="e96f2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="e96f2-105">Como parte de 6.1 estamos introduciendo una nueva característica de resistencia de conexión para EF: la capacidad para detectar y recuperarse automáticamente cuando los errores transitorios de conexión afecta a la confirmación de transacciones confirmadas.</span><span class="sxs-lookup"><span data-stu-id="e96f2-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="e96f2-106">Los detalles completos del escenario se describe mejor en la entrada de blog [conectividad de base de datos de SQL y el problema de idempotencia](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="e96f2-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="e96f2-107">En resumen, el escenario es que, cuando se produce una excepción durante una confirmación de transacción hay dos causas posibles:</span><span class="sxs-lookup"><span data-stu-id="e96f2-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="e96f2-108">Error de la confirmación de transacción en el servidor</span><span class="sxs-lookup"><span data-stu-id="e96f2-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="e96f2-109">La confirmación de transacción se realizó correctamente en el servidor pero un problema de conectividad impidió la notificación correcta de alcanzar el cliente</span><span class="sxs-lookup"><span data-stu-id="e96f2-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="e96f2-110">Cuando produce la primera situación la aplicación o el usuario puede volver a intentar la operación, pero cuando se produce la segunda situación se deben evitar reintentos y la aplicación puede recuperarse automáticamente.</span><span class="sxs-lookup"><span data-stu-id="e96f2-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="e96f2-111">El desafío es que, sin la capacidad para detectar cuál fue el motivo real detectó una excepción durante la confirmación, la aplicación no puede elegir la forma de acción.</span><span class="sxs-lookup"><span data-stu-id="e96f2-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="e96f2-112">La nueva característica de EF 6.1 permite a EF comprobar de nuevo con la base de datos si la transacción se ha realizado correctamente y tomar la forma de acción de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="e96f2-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="e96f2-113">Uso de la característica</span><span class="sxs-lookup"><span data-stu-id="e96f2-113">Using the feature</span></span>  

<span data-ttu-id="e96f2-114">Con el fin de habilitar la característica debe incluir una llamada a [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) en el constructor de su **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="e96f2-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="e96f2-115">Si no está familiarizado con **DbConfiguration**, consulte [código configuración](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="e96f2-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="e96f2-116">Esta característica puede usarse en combinación con los reintentos automáticos se introdujeron en EF6, que ayudan en la situación en la que la transacción realmente no se pudo confirmar en el servidor debido a un error transitorio:</span><span class="sxs-lookup"><span data-stu-id="e96f2-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="e96f2-117">¿Cómo se realiza un seguimiento de las transacciones</span><span class="sxs-lookup"><span data-stu-id="e96f2-117">How transactions are tracked</span></span>  

<span data-ttu-id="e96f2-118">Cuando la característica está habilitada, EF agrega automáticamente una nueva tabla a la base de datos denominada **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="e96f2-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="e96f2-119">Cada vez que se crea una transacción por EF y esa fila se comprueba la existencia si se produce un error en la transacción durante la confirmación, se inserta una fila nueva en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="e96f2-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="e96f2-120">Aunque EF hará todo lo posible para que elimine las filas de la tabla cuando ya no se necesitan, la tabla puede crecer si la aplicación se cierra prematuramente y por ese motivo que debe purgar la tabla manualmente en algunos casos.</span><span class="sxs-lookup"><span data-stu-id="e96f2-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="e96f2-121">Cómo controlar los errores de confirmación con versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="e96f2-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="e96f2-122">Antes de EF 6.1 no hubo mecanismo para controlar los errores de confirmación en el producto EF.</span><span class="sxs-lookup"><span data-stu-id="e96f2-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="e96f2-123">Hay varias maneras de tratar esta situación que se puede aplicar a versiones anteriores de EF6:</span><span class="sxs-lookup"><span data-stu-id="e96f2-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="e96f2-124">Opción 1: no hacer nada</span><span class="sxs-lookup"><span data-stu-id="e96f2-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="e96f2-125">La probabilidad de un error de conexión durante la confirmación de transacción es baja, por lo que puede ser aceptable para su aplicación simplemente no funcione correctamente si esta condición se produce realmente.</span><span class="sxs-lookup"><span data-stu-id="e96f2-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="e96f2-126">Opción 2: usar la base de datos para restablecer estado</span><span class="sxs-lookup"><span data-stu-id="e96f2-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="e96f2-127">Descartar el DbContext actual</span><span class="sxs-lookup"><span data-stu-id="e96f2-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="e96f2-128">Crear una nueva clase de DbContext y restaurar el estado de la aplicación desde la base de datos</span><span class="sxs-lookup"><span data-stu-id="e96f2-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="e96f2-129">Informar al usuario que la última operación es posible que no se completaron correctamente</span><span class="sxs-lookup"><span data-stu-id="e96f2-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="e96f2-130">Opción 3: realizar un seguimiento manual de la transacción</span><span class="sxs-lookup"><span data-stu-id="e96f2-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="e96f2-131">Agregar una tabla no realiza un seguimiento a la base de datos utilizada para realizar un seguimiento del estado de las transacciones.</span><span class="sxs-lookup"><span data-stu-id="e96f2-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="e96f2-132">Insertar una fila en la tabla al principio de cada transacción.</span><span class="sxs-lookup"><span data-stu-id="e96f2-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="e96f2-133">Si se produce un error en la conexión durante la confirmación, compruebe la presencia de la fila correspondiente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e96f2-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="e96f2-134">Si la fila está presente, continúe con normalidad, como la transacción se confirmó correctamente</span><span class="sxs-lookup"><span data-stu-id="e96f2-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="e96f2-135">Si la fila no está presente, use una estrategia de ejecución para volver a intentar la operación actual.</span><span class="sxs-lookup"><span data-stu-id="e96f2-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="e96f2-136">Si la confirmación se realiza correctamente, elimine la fila correspondiente para evitar el crecimiento de la tabla.</span><span class="sxs-lookup"><span data-stu-id="e96f2-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="e96f2-137">[Esta entrada de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contiene código de ejemplo para llevar a cabo esto en SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="e96f2-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
