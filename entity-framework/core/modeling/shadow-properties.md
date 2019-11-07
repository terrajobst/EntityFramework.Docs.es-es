---
title: 'Propiedades de sombra: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655964"
---
# <a name="shadow-properties"></a>Propiedades reemplazadas

Las propiedades de sombra son propiedades que no están definidas en la clase de entidad de .NET pero que se definen para ese tipo de entidad en el modelo de EF Core. El valor y el estado de estas propiedades se mantienen únicamente en el seguimiento de cambios.

Las propiedades Shadow son útiles cuando hay datos en la base de datos que no se deben exponer en los tipos de entidad asignados. Se usan con mayor frecuencia para las propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación se administra en los tipos de entidad mediante las propiedades de navegación entre los tipos de entidad.

Los valores de las propiedades Shadow se pueden obtener y cambiar a través de la API de `ChangeTracker`.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Se puede hacer referencia a las propiedades Shadow en consultas LINQ a través del método estático `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenciones

Las propiedades de sombra se pueden crear por Convención cuando se detecta una relación, pero no se encuentra ninguna propiedad de clave externa en la clase de entidad dependiente. En este caso, se introducirá una propiedad de clave externa de sombra. La propiedad Shadow Foreign Key se denominará `<navigation property name><principal key property name>` (la navegación en la entidad dependiente, que apunta a la entidad principal, se utiliza para la nomenclatura). Si el nombre de la propiedad de clave principal incluye el nombre de la propiedad de navegación, el nombre simplemente se `<principal key property name>`. Si no hay ninguna propiedad de navegación en la entidad dependiente, el nombre del tipo de entidad de seguridad se usa en su lugar.

Por ejemplo, la siguiente lista de código dará como resultado la introducción de una `BlogId` propiedad Shadow en la entidad `Post`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden crear propiedades de sombra con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar las propiedades de las instantáneas. Una vez que haya llamado a la sobrecarga de la cadena de `Property` puede encadenar cualquiera de las llamadas de configuración que desee para otras propiedades.

Si el nombre proporcionado al método `Property` coincide con el nombre de una propiedad existente (una propiedad Shadow o una definida en la clase de entidad), el código configurará esa propiedad existente en lugar de introducir una nueva propiedad Shadow.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
