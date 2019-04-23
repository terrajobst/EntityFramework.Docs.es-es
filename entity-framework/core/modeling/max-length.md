---
title: Longitud máxima - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929854"
---
# <a name="maximum-length"></a>Longitud máxima

Configurar una longitud máxima, proporciona una sugerencia al almacén de datos sobre el tipo de datos apropiados que se usará para una propiedad determinada. Longitud máxima solo se aplica a tipos de datos de matriz, como `string` y `byte[]`.

> [!NOTE]  
> Entity Framework no realiza ninguna validación de longitud máxima antes de pasar datos al proveedor. Es el almacén de datos o proveedor para validar si es necesario. Por ejemplo, cuando el destino es SQL Server, si se supera la longitud máxima se producirán una excepción como el tipo de datos de la columna subyacente no permitirá exceso de datos que se almacenará.

## <a name="conventions"></a>Convenciones

Por convención, se deja hasta el proveedor de base de datos para elegir un tipo de datos adecuado para las propiedades. Para las propiedades que tienen una longitud, el proveedor de base de datos generalmente elegirá un tipo de datos que permite que la mayor longitud de datos. Por ejemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` si la columna se utiliza como clave).

## <a name="data-annotations"></a>Anotaciones de datos

Puede utilizar las anotaciones de datos para configurar una longitud máxima para una propiedad. En este ejemplo, como el destino de SQL Server, esto daría lugar a la `nvarchar(500)` tipo de datos que se va a usar.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar una longitud máxima para una propiedad. En este ejemplo, como el destino de SQL Server, esto daría lugar a la `nvarchar(500)` tipo de datos que se va a usar.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
