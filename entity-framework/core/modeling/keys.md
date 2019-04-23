---
title: Claves (principal) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929841"
---
# <a name="keys-primary"></a><span data-ttu-id="722c7-102">Claves (principal)</span><span class="sxs-lookup"><span data-stu-id="722c7-102">Keys (primary)</span></span>

<span data-ttu-id="722c7-103">Una clave actúa como identificador único para cada instancia de la entidad principal.</span><span class="sxs-lookup"><span data-stu-id="722c7-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="722c7-104">Cuando se usa una base de datos relacional se asigna al concepto de un *clave principal*.</span><span class="sxs-lookup"><span data-stu-id="722c7-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="722c7-105">También puede configurar un identificador único que no es la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="722c7-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="722c7-106">Uno de los métodos siguientes se puede usar el programa de instalación o crear una clave principal.</span><span class="sxs-lookup"><span data-stu-id="722c7-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="722c7-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="722c7-107">Conventions</span></span>

<span data-ttu-id="722c7-108">Por convención, una propiedad denominada `Id` o `<type name>Id` se configurarán como la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="722c7-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="722c7-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="722c7-109">Data Annotations</span></span>

<span data-ttu-id="722c7-110">Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="722c7-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="722c7-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="722c7-111">Fluent API</span></span>

<span data-ttu-id="722c7-112">Puede usar la API Fluent para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="722c7-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="722c7-113">También puede usar la API Fluent para configurar varias propiedades para que sea la clave de una entidad (conocida como una clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="722c7-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="722c7-114">Las claves compuestas solo se pueden configurar mediante la API de Fluent: convenciones nunca realizará la instalación de una clave compuesta y no se pueden usar anotaciones de datos para configurar uno.</span><span class="sxs-lookup"><span data-stu-id="722c7-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
