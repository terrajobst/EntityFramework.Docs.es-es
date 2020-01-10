---
title: 'Tokens de simultaneidad: EF Core'
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781149"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

> [!NOTE]
> En esta página se documenta cómo configurar los tokens de simultaneidad. Vea [controlar los conflictos de simultaneidad](../saving/concurrency.md) para obtener una explicación detallada de cómo funciona el control de simultaneidad en EF Core y ejemplos de cómo controlar los conflictos de simultaneidad en la aplicación.

Las propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista.

## <a name="configuration"></a>Configuración de

### <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Marca de tiempo/rowversion

Timestamp/rowversion es una propiedad para la cual la base de datos genera automáticamente un nuevo valor cada vez que se inserta o se actualiza una fila. La propiedad también se trata como un token de simultaneidad, lo que garantiza que se obtiene una excepción si una fila que se está actualizando ha cambiado desde que se realizó la consulta. Los detalles precisos dependen del proveedor de base de datos utilizado; por SQL Server, normalmente se utiliza una propiedad *Byte []* , que se configurará como una columna *ROWVERSION* en la base de datos.

Puede configurar una propiedad para que sea una marca de tiempo o rowversion como se indica a continuación:

### <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api)

[! code-CSharp [Main] (.. /.. /.. /samples/core/Modeling/FluentAPI/Timestamp.cs? Name = timestamp & resalte = 9, 17]

***
