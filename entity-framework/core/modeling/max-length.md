---
title: 'Longitud máxima: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197228"
---
# <a name="maximum-length"></a>Longitud máxima

La configuración de una longitud máxima proporciona una sugerencia al almacén de datos sobre el tipo de datos adecuado que se va a usar para una propiedad determinada. La longitud máxima solo se aplica a los tipos de datos `string` de `byte[]`matriz, como y.

> [!NOTE]  
> Entity Framework no realiza ninguna validación de la longitud máxima antes de pasar datos al proveedor. Depende del proveedor o del almacén de datos que se valide si es necesario. Por ejemplo, cuando el destino es SQL Server, si se supera la longitud máxima, se producirá una excepción, ya que el tipo de datos de la columna subyacente no permitirá que se almacenen los datos sobrantes.

## <a name="conventions"></a>Convenciones

Por Convención, se deja al proveedor de base de datos para elegir un tipo de datos adecuado para las propiedades. En el caso de las propiedades que tienen una longitud, el proveedor de base de datos suele elegir un tipo de datos que permite la mayor longitud de los datos. Por ejemplo, Microsoft SQL Server utilizará `nvarchar(max)` para `string` las propiedades ( `nvarchar(450)` o si la columna se utiliza como clave).

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar las anotaciones de datos para configurar una longitud máxima para una propiedad. En este ejemplo, al establecer como destino SQL Server esto daría lugar `nvarchar(500)` a que se usara el tipo de datos.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una longitud máxima para una propiedad. En este ejemplo, al establecer como destino SQL Server esto daría lugar `nvarchar(500)` a que se usara el tipo de datos.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
