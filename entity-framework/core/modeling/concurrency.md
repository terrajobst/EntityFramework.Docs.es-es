---
title: 'Tokens de simultaneidad: EF Core'
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: bfeb611f222f7195fe22d920b452b40cc4addf90
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414624"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

> [!NOTE]
> En esta página se documenta cómo configurar los tokens de simultaneidad. Vea [controlar los conflictos de simultaneidad](../saving/concurrency.md) para obtener una explicación detallada de cómo funciona el control de simultaneidad en EF Core y ejemplos de cómo controlar los conflictos de simultaneidad en la aplicación.

Las propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista.

## <a name="configuration"></a>Configuración

### <a name="data-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Marca de tiempo/rowversion

Timestamp/rowversion es una propiedad para la cual la base de datos genera automáticamente un nuevo valor cada vez que se inserta o se actualiza una fila. La propiedad también se trata como un token de simultaneidad, lo que garantiza que se obtiene una excepción si una fila que se está actualizando ha cambiado desde que se realizó la consulta. Los detalles precisos dependen del proveedor de base de datos utilizado; por SQL Server, normalmente se utiliza una propiedad *Byte []* , que se configurará como una columna *ROWVERSION* en la base de datos.

Puede configurar una propiedad para que sea una marca de tiempo o rowversion como se indica a continuación:

### <a name="data-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
