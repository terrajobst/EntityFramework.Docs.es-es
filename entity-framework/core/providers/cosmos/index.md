---
title: 'Proveedor de Azure Cosmos DB: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317564"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="a8124-102">Proveedor de Azure Cosmos DB para EF Core</span><span class="sxs-lookup"><span data-stu-id="a8124-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="a8124-103">Este proveedor es nuevo en EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="a8124-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="a8124-104">Este proveedor de base de datos permite usar Entity Framework Core con Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a8124-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="a8124-105">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="a8124-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="a8124-106">Se recomienda encarecidamente que se familiarice con la [documentación sobre Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) antes de leer esta sección.</span><span class="sxs-lookup"><span data-stu-id="a8124-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="a8124-107">Instalar</span><span class="sxs-lookup"><span data-stu-id="a8124-107">Install</span></span>

<span data-ttu-id="a8124-108">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="a8124-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="a8124-109">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="a8124-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="a8124-110">Puede ver en [GitHub un ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) de este artículo.</span><span class="sxs-lookup"><span data-stu-id="a8124-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="a8124-111">Al igual que para otros proveedores, el primer paso es llamar a `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="a8124-111">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="a8124-112">El punto de conexión y la clave aquí se codifican de forma rígida por motivos de simplicidad pero, en una aplicación de producción, se deben [almacenar de manera segura](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="a8124-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="a8124-113">En este ejemplo, `Order` es una entidad sencilla con una referencia al [tipo en propiedad](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="a8124-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="a8124-114">La acción de guardar y consultar datos sigue el patrón de EF normal:</span><span class="sxs-lookup"><span data-stu-id="a8124-114">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="a8124-115">Llamar a `EnsureCreated` es necesario para crear las colecciones requeridas e insertar los [datos de inicialización](../../modeling/data-seeding.md) si están presentes en el modelo.</span><span class="sxs-lookup"><span data-stu-id="a8124-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="a8124-116">Sin embargo, se debe llamar a `EnsureCreated` solo durante la implementación y no durante la operación normal, porque podría provocar problemas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a8124-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="a8124-117">Personalización del modelo específico para Cosmos</span><span class="sxs-lookup"><span data-stu-id="a8124-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="a8124-118">De manera predeterminada, todos los tipos de entidad están asignados al mismo contenedor, con un nombre que depende del contexto derivado (`"OrderContext"` en este caso).</span><span class="sxs-lookup"><span data-stu-id="a8124-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="a8124-119">Para cambiar el nombre del contenedor predeterminado, use `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="a8124-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="a8124-120">Para asignar un tipo de entidad a un contenedor distinto, use `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="a8124-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="a8124-121">Para identificar el tipo de entidad que un elemento determinado representa, EF Core agrega un valor de discriminador incluso si no hay tipos de entidad derivados.</span><span class="sxs-lookup"><span data-stu-id="a8124-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="a8124-122">El nombre y el valor del discriminador [se pueden modificar](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="a8124-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="a8124-123">Entidades insertadas</span><span class="sxs-lookup"><span data-stu-id="a8124-123">Embedded Entities</span></span>

<span data-ttu-id="a8124-124">En Cosmos, las entidades en propiedad se insertan en el mismo elemento que el propietario.</span><span class="sxs-lookup"><span data-stu-id="a8124-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="a8124-125">Para cambiar el nombre de una propiedad, use `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="a8124-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="a8124-126">Con esta configuración, el pedido del ejemplo anterior se almacena de esta manera:</span><span class="sxs-lookup"><span data-stu-id="a8124-126">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="a8124-127">Las colecciones de entidades en propiedad también se insertan.</span><span class="sxs-lookup"><span data-stu-id="a8124-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="a8124-128">En el ejemplo siguiente, usaremos la clase `Distributor` con una colección de `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="a8124-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="a8124-129">No es necesario que las entidades en propiedad proporcionen valores de clave explícitos que se deban almacenar:</span><span class="sxs-lookup"><span data-stu-id="a8124-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="a8124-130">Se conservarán de esta manera:</span><span class="sxs-lookup"><span data-stu-id="a8124-130">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="a8124-131">De manera interna, EF Core siempre debe tener valores de clave únicos para todas las entidades sometidas a seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a8124-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="a8124-132">La clave principal creada de manera predeterminada para las colecciones de tipos en propiedad consta de las propiedades de clave externa que apuntan al propietario y una propiedad `int` correspondiente al índice de la matriz JSON.</span><span class="sxs-lookup"><span data-stu-id="a8124-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="a8124-133">Para recuperar estos valores, se podría usar la API de entrada:</span><span class="sxs-lookup"><span data-stu-id="a8124-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="a8124-134">Cuando sea necesario, se puede cambiar la clave principal predeterminada para los tipos de entidad en propiedad, pero los valores de clave se deben proporcionar de manera explícita.</span><span class="sxs-lookup"><span data-stu-id="a8124-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="a8124-135">Trabajo con entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="a8124-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="a8124-136">Cada elemento debe tener un valor `id` único para la clave de partición específica.</span><span class="sxs-lookup"><span data-stu-id="a8124-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="a8124-137">De manera predeterminada, EF Core genera el valor mediante la concatenación de los valores de discriminador y de clave principal, con "|" como delimitador.</span><span class="sxs-lookup"><span data-stu-id="a8124-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="a8124-138">Los valores de clave solo se generan cuando una entidad entra en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="a8124-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="a8124-139">Esto podría suponer un problema al [adjuntar entidades](../../saving/disconnected-entities.md) si no tienen una propiedad `id` en el tipo .NET para almacenar el valor.</span><span class="sxs-lookup"><span data-stu-id="a8124-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="a8124-140">Para evitar esta limitación, se podría crear y establecer el valor `id` de manera manual o marcar la entidad como agregada y, luego, cambiarla al estado deseado:</span><span class="sxs-lookup"><span data-stu-id="a8124-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="a8124-141">Este es el JSON resultante:</span><span class="sxs-lookup"><span data-stu-id="a8124-141">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```