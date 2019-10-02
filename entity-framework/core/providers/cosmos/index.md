---
title: 'Proveedor de Azure Cosmos DB: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813546"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Proveedor de Azure Cosmos DB para EF Core

>[!NOTE]
> Este proveedor es nuevo en EF Core 3.0.

Este proveedor de base de datos permite usar Entity Framework Core con Azure Cosmos DB. Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Se recomienda encarecidamente que se familiarice con la [documentación sobre Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) antes de leer esta sección.

## <a name="install"></a>Instalar

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

# <a name="net-core-clitabdotnet-core-cli"></a>[CLI de .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Primeros pasos

> [!TIP]  
> Puede ver en [GitHub un ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) de este artículo.

Al igual que para otros proveedores, el primer paso es llamar a `UseCosmos`:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> El punto de conexión y la clave aquí se codifican de forma rígida por motivos de simplicidad pero, en una aplicación de producción, se deben [almacenar de manera segura](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

En este ejemplo, `Order` es una entidad sencilla con una referencia al [tipo en propiedad](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

La acción de guardar y consultar datos sigue el patrón de EF normal:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Llamar a `EnsureCreated` es necesario para crear las colecciones requeridas e insertar los [datos de inicialización](../../modeling/data-seeding.md) si están presentes en el modelo. Sin embargo, se debe llamar a `EnsureCreated` solo durante la implementación y no durante la operación normal, porque podría provocar problemas de rendimiento.

## <a name="cosmos-specific-model-customization"></a>Personalización del modelo específico para Cosmos

De manera predeterminada, todos los tipos de entidad están asignados al mismo contenedor, con un nombre que depende del contexto derivado (`"OrderContext"` en este caso). Para cambiar el nombre del contenedor predeterminado, use `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Para asignar un tipo de entidad a un contenedor distinto, use `ToContainer`:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Para identificar el tipo de entidad que un elemento determinado representa, EF Core agrega un valor de discriminador incluso si no hay tipos de entidad derivados. El nombre y el valor del discriminador [se pueden modificar](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Entidades insertadas

En Cosmos, las entidades en propiedad se insertan en el mismo elemento que el propietario. Para cambiar el nombre de una propiedad, use `ToJsonProperty`:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Con esta configuración, el pedido del ejemplo anterior se almacena de esta manera:

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

Las colecciones de entidades en propiedad también se insertan. En el ejemplo siguiente, usaremos la clase `Distributor` con una colección de `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

No es necesario que las entidades en propiedad proporcionen valores de clave explícitos que se deban almacenar:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Se conservarán de esta manera:

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

De manera interna, EF Core siempre debe tener valores de clave únicos para todas las entidades sometidas a seguimiento. La clave principal creada de manera predeterminada para las colecciones de tipos en propiedad consta de las propiedades de clave externa que apuntan al propietario y una propiedad `int` correspondiente al índice de la matriz JSON. Para recuperar estos valores, se podría usar la API de entrada:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Cuando sea necesario, se puede cambiar la clave principal predeterminada para los tipos de entidad en propiedad, pero los valores de clave se deben proporcionar de manera explícita.

## <a name="working-with-disconnected-entities"></a>Trabajo con entidades desconectadas

Cada elemento debe tener un valor `id` único para la clave de partición específica. De manera predeterminada, EF Core genera el valor mediante la concatenación de los valores de discriminador y de clave principal, con "|" como delimitador. Los valores de clave solo se generan cuando una entidad entra en el estado `Added`. Esto podría suponer un problema al [adjuntar entidades](../../saving/disconnected-entities.md) si no tienen una propiedad `id` en el tipo .NET para almacenar el valor.

Para evitar esta limitación, se podría crear y establecer el valor `id` de manera manual o marcar la entidad como agregada y, luego, cambiarla al estado deseado:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Este es el JSON resultante:

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
