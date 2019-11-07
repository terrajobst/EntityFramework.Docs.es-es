---
title: 'Relaciones: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655671"
---
# <a name="relationships"></a>Relaciones

Una relación define el modo en que dos entidades se relacionan entre sí. En una base de datos relacional, se representa mediante una restricción FOREIGN KEY.

> [!NOTE]  
> La mayoría de los ejemplos de este artículo usan una relación de uno a varios para demostrar los conceptos. Para obtener ejemplos de relaciones de uno a uno y de varios a varios, consulte la sección [otros patrones de relación](#other-relationship-patterns) al final del artículo.

## <a name="definition-of-terms"></a>Definición de términos

Hay una serie de términos que se usan para describir las relaciones

* **Entidad dependiente:** Esta es la entidad que contiene las propiedades de clave externa. A veces se conoce como "secundario" de la relación.

* **Entidad de entidad** de seguridad: Esta es la entidad que contiene las propiedades de clave principal/alternativa. A veces se denomina "primario" de la relación.

* **Clave externa:** Las propiedades de la entidad dependiente que se usa para almacenar los valores de la propiedad de clave principal con la que está relacionada la entidad.

* **Clave principal:** Las propiedades que identifican de forma única la entidad principal. Puede ser la clave principal o una clave alternativa.

* **Propiedad de navegación:** Propiedad definida en la entidad de seguridad o dependiente que contiene una o más referencias a las entidades relacionadas.

  * **Propiedad de navegación de colección:** Propiedad de navegación que contiene referencias a muchas entidades relacionadas.

  * **Propiedad de navegación de referencia:** Propiedad de navegación que contiene una referencia a una sola entidad relacionada.

  * **Propiedad de navegación inversa:** Al discutir una propiedad de navegación determinada, este término hace referencia a la propiedad de navegación en el otro extremo de la relación.

En la lista de código siguiente se muestra una relación de uno a varios entre `Blog` y `Post`

* `Post` es la entidad dependiente

* `Blog` es la entidad principal

* `Post.BlogId` es la clave externa

* `Blog.BlogId` es la clave principal (en este caso, es una clave principal en lugar de una clave alternativa)

* `Post.Blog` es una propiedad de navegación de referencia

* `Blog.Posts` es una propiedad de navegación de colección

* `Post.Blog` es la propiedad de navegación inversa de `Blog.Posts` (y viceversa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenciones

Por Convención, se creará una relación cuando se detecte una propiedad de navegación en un tipo. Una propiedad se considera una propiedad de navegación si el tipo al que señala no se puede asignar como un tipo escalar por el proveedor de base de datos actual.

> [!NOTE]  
> Las relaciones detectadas por la Convención siempre tendrán como destino la clave principal de la entidad principal. Para elegir como destino una clave alternativa, se debe realizar una configuración adicional mediante la API fluida.

### <a name="fully-defined-relationships"></a>Relaciones totalmente definidas

El patrón más común para las relaciones es tener propiedades de navegación definidas en ambos extremos de la relación y una propiedad de clave externa definida en la clase de entidad dependiente.

* Si se encuentra un par de propiedades de navegación entre dos tipos, se configurarán como propiedades de navegación inversa de la misma relación.

* Si la entidad dependiente contiene una propiedad denominada `<primary key property name>`, `<navigation property name><primary key property name>`o `<principal entity name><primary key property name>`, se configurará como la clave externa.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Si hay varias propiedades de navegación definidas entre dos tipos (es decir, más de un par de navegación distinto que apuntan entre sí), no se creará ninguna relación por Convención y tendrá que configurarlas manualmente para identificar cómo pareja de propiedades de navegación.

### <a name="no-foreign-key-property"></a>No hay propiedad de clave externa

Aunque se recomienda tener una propiedad de clave externa definida en la clase de entidad dependiente, no es necesario. Si no se encuentra ninguna propiedad de clave externa, se introducirá una propiedad de clave externa de sombra con el nombre `<navigation property name><principal key property name>` (consulte [propiedades de sombra](shadow-properties.md) para obtener más información).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propiedad de navegación única

Incluir solo una propiedad de navegación (sin navegación inversa y sin propiedad de clave externa) es suficiente para tener una relación definida por Convención. También puede tener una propiedad de navegación única y una propiedad de clave externa.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Eliminación en cascada

Por Convención, la eliminación en cascada se establecerá en *Cascade* para las relaciones necesarias y *ClientSetNull* para las relaciones opcionales. *Cascade* significa que las entidades dependientes también se eliminan. *ClientSetNull* significa que las entidades dependientes que no se cargan en la memoria permanecerán sin cambios y deben eliminarse manualmente o actualizarse para que apunten a una entidad principal válida. En el caso de las entidades que se cargan en memoria, EF Core intentará establecer las propiedades de clave externa en NULL.

Vea la sección [relaciones obligatorias y opcionales](#required-and-optional-relationships) para ver la diferencia entre las relaciones obligatorias y opcionales.

Consulte [eliminación en cascada](../saving/cascade-delete.md) para obtener más detalles sobre los distintos comportamientos de eliminación y los valores predeterminados que usa la Convención.

## <a name="data-annotations"></a>Anotaciones de datos

Hay dos anotaciones de datos que se pueden usar para configurar relaciones, `[ForeignKey]` y `[InverseProperty]`. Están disponibles en el espacio de nombres `System.ComponentModel.DataAnnotations.Schema`.

### <a name="foreignkey"></a>ForeignKey

Puede usar las anotaciones de datos para configurar qué propiedad se debe usar como la propiedad de clave externa para una relación determinada. Normalmente, esto se hace cuando la Convención no detecta la propiedad de clave externa.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> La anotación `[ForeignKey]` se puede colocar en cualquier propiedad de navegación de la relación. No es necesario que vaya a la propiedad de navegación en la clase de entidad dependiente.

### <a name="inverseproperty"></a>[InverseProperty]

Puede usar las anotaciones de datos para configurar cómo se emparejan las propiedades de navegación en las entidades de entidad de seguridad y dependencias. Normalmente, esto se hace cuando hay más de un par de propiedades de navegación entre dos tipos de entidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>API fluida

Para configurar una relación en la API fluida, empiece por identificar las propiedades de navegación que componen la relación. `HasOne` o `HasMany` identifica la propiedad de navegación en el tipo de entidad en el que se está iniciando la configuración. A continuación, encadenar una llamada a `WithOne` o `WithMany` para identificar la navegación inversa. `HasOne`/`WithOne` se utilizan para las propiedades de navegación de referencia y `HasMany`/`WithMany` se utilizan para las propiedades de navegación de la colección.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Propiedad de navegación única

Si solo tiene una propiedad de navegación, hay sobrecargas sin parámetros de `WithOne` y `WithMany`. Esto indica que hay conceptualmente una referencia o una colección en el otro extremo de la relación, pero no hay ninguna propiedad de navegación incluida en la clase de entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Clave externa

Puede usar la API fluida para configurar qué propiedad se debe usar como la propiedad de clave externa para una relación determinada.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

En la lista de código siguiente se muestra cómo configurar una clave externa compuesta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Puede usar la sobrecarga de cadena de `HasForeignKey(...)` para configurar una propiedad Shadow como clave externa (consulte [propiedades de sombra](shadow-properties.md) para obtener más información). Se recomienda agregar explícitamente la propiedad Shadow al modelo antes de usarla como clave externa (como se muestra a continuación).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Sin propiedad de navegación

No es necesario proporcionar una propiedad de navegación. Simplemente puede proporcionar una clave externa en un lado de la relación.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Clave principal

Si desea que la clave externa haga referencia a una propiedad que no sea la clave principal, puede usar la API fluida para configurar la propiedad de clave principal de la relación. La propiedad que configure como clave principal se configurará automáticamente como clave alternativa (consulte [claves alternativas](alternate-keys.md) para obtener más información).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

En la lista de código siguiente se muestra cómo configurar una clave principal compuesta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> El orden en el que se especifican las propiedades de clave principal debe coincidir con el orden en que se especifican para la clave externa.

### <a name="required-and-optional-relationships"></a>Relaciones obligatorias y opcionales

Puede usar la API fluida para configurar si la relación es obligatoria u opcional. En última instancia, controla si la propiedad de clave externa es obligatoria u opcional. Esto es muy útil cuando se usa una clave externa de estado de sombra. Si tiene una propiedad de clave externa en la clase de entidad, la necesidad de la relación se determina en función de si la propiedad de clave externa es necesaria u opcional (vea [propiedades obligatorias y opcionales](required-optional.md) para obtener más información).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>Eliminación en cascada

Puede usar la API fluida para configurar explícitamente el comportamiento de eliminación en cascada para una relación determinada.

Consulte [eliminación en cascada](../saving/cascade-delete.md) en la sección guardar datos para obtener una explicación detallada de cada opción.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Otros patrones de relación

### <a name="one-to-one"></a>Uno a uno

Una relación de uno a uno tiene una propiedad de navegación de referencia en ambos lados. Siguen las mismas convenciones que las relaciones uno a varios, pero se incluye un índice único en la propiedad de clave externa para asegurarse de que solo un dependiente esté relacionado con cada entidad de seguridad.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF elegirá una de las entidades como dependiente en función de su capacidad para detectar una propiedad de clave externa. Si se elige la entidad equivocada como dependiente, puede usar la API fluida para corregir este problema.

Al configurar la relación con la API fluida, se usan los métodos `HasOne` y `WithOne`.

Al configurar la clave externa, debe especificar el tipo de entidad dependiente: Observe que el parámetro genérico proporcionado a `HasForeignKey` en la siguiente lista. En una relación uno a varios, es evidente que la entidad con la navegación de referencia es el dependiente y el que tiene la colección es la entidad de seguridad. Pero esto no es así en una relación uno a uno; por lo tanto, la necesidad de definirla explícitamente.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Varios a varios

Todavía no se admiten las relaciones de varios a varios sin una clase de entidad para representar la tabla de combinación. Sin embargo, puede representar una relación de varios a varios incluyendo una clase de entidad para la tabla de combinación y asignando dos relaciones uno a varios independientes.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
