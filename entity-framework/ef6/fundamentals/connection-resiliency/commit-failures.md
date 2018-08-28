---
title: Control de errores de confirmación de transacción - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: a22a651851bc46e2bf1fe652b3b9a921ec22b70b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996842"
---
# <a name="handling-transaction-commit-failures"></a>Control de errores de confirmación de transacción
> [!NOTE]
> **EF6.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 6.1. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Como parte de 6.1 estamos introduciendo una nueva característica de resistencia de conexión para EF: la capacidad para detectar y recuperarse automáticamente cuando los errores transitorios de conexión afecta a la confirmación de transacciones confirmadas. Los detalles completos del escenario se describe mejor en la entrada de blog [conectividad de base de datos de SQL y el problema de idempotencia](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  En resumen, el escenario es que, cuando se produce una excepción durante una confirmación de transacción hay dos causas posibles:  

1. Error de la confirmación de transacción en el servidor
2. La confirmación de transacción se realizó correctamente en el servidor pero un problema de conectividad impidió la notificación correcta de alcanzar el cliente  

Cuando produce la primera situación la aplicación o el usuario puede volver a intentar la operación, pero cuando se produce la segunda situación se deben evitar reintentos y la aplicación puede recuperarse automáticamente. El desafío es que, sin la capacidad para detectar cuál fue el motivo real detectó una excepción durante la confirmación, la aplicación no puede elegir la forma de acción. La nueva característica de EF 6.1 permite a EF comprobar de nuevo con la base de datos si la transacción se ha realizado correctamente y tomar la forma de acción de forma transparente.  

## <a name="using-the-feature"></a>Uso de la característica  

Con el fin de habilitar la característica debe incluir una llamada a [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) en el constructor de su **DbConfiguration**. Si no está familiarizado con **DbConfiguration**, consulte [código configuración](~/ef6/fundamentals/configuring/code-based.md). Esta característica puede usarse en combinación con los reintentos automáticos se introdujeron en EF6, que ayudan en la situación en la que la transacción realmente no se pudo confirmar en el servidor debido a un error transitorio:  

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

## <a name="how-transactions-are-tracked"></a>¿Cómo se realiza un seguimiento de las transacciones  

Cuando la característica está habilitada, EF agrega automáticamente una nueva tabla a la base de datos denominada **__Transactions**. Cada vez que se crea una transacción por EF y esa fila se comprueba la existencia si se produce un error en la transacción durante la confirmación, se inserta una fila nueva en esta tabla.  

Aunque EF hará todo lo posible para que elimine las filas de la tabla cuando ya no se necesitan, la tabla puede crecer si la aplicación se cierra prematuramente y por ese motivo que debe purgar la tabla manualmente en algunos casos.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Cómo controlar los errores de confirmación con versiones anteriores

Antes de EF 6.1 no hubo mecanismo para controlar los errores de confirmación en el producto EF. Hay varias maneras de tratar esta situación que se puede aplicar a versiones anteriores de EF6:  

### <a name="option-1---do-nothing"></a>Opción 1: no hacer nada  

La probabilidad de un error de conexión durante la confirmación de transacción es baja, por lo que puede ser aceptable para su aplicación simplemente no funcione correctamente si esta condición se produce realmente.  

## <a name="option-2---use-the-database-to-reset-state"></a>Opción 2: usar la base de datos para restablecer estado  

1. Descartar el DbContext actual  
2. Crear una nueva clase de DbContext y restaurar el estado de la aplicación desde la base de datos  
3. Informar al usuario que la última operación es posible que no se completaron correctamente  

## <a name="option-3---manually-track-the-transaction"></a>Opción 3: realizar un seguimiento manual de la transacción  

1. Agregar una tabla no realiza un seguimiento a la base de datos utilizada para realizar un seguimiento del estado de las transacciones.  
2. Insertar una fila en la tabla al principio de cada transacción.  
3. Si se produce un error en la conexión durante la confirmación, compruebe la presencia de la fila correspondiente en la base de datos.  
    - Si la fila está presente, continúe con normalidad, como la transacción se confirmó correctamente  
    - Si la fila no está presente, use una estrategia de ejecución para volver a intentar la operación actual.  
4. Si la confirmación se realiza correctamente, elimine la fila correspondiente para evitar el crecimiento de la tabla.  

[Esta entrada de blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contiene código de ejemplo para llevar a cabo esto en SQL Azure.  
