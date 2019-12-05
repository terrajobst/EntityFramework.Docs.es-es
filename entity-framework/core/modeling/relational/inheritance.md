---
title: Herencia (base de datos relacional)-EF Core
description: Cómo configurar la herencia de tipos de entidad en una base de datos relacional mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824743"
---
# <a name="inheritance-relational-database"></a>Herencia (base de datos relacional)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

> [!NOTE]  
> Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core. Otros patrones comunes como tabla por tipo (TPT) y tabla por tipo específico (TPC) no están disponibles todavía.

## <a name="conventions"></a>Convenciones

De forma predeterminada, la herencia se asignará mediante el patrón de tabla por jerarquía (TPH). TPH usa una sola tabla para almacenar los datos de todos los tipos de la jerarquía. Una columna de discriminador se utiliza para identificar el tipo que representa cada fila.

EF Core solo configurará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo (consulte [herencia](../inheritance.md) para obtener más detalles).

A continuación se muestra un ejemplo en el que se muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH. La columna *Discriminator* identifica el tipo de *blog* que se almacena en cada fila.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Las columnas de la base de datos se convierten en NULL automáticamente según sea necesario al usar la asignación TPH.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre y el tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configurar la propiedad Discriminator

En los ejemplos anteriores, el discriminador se crea como una [propiedad Shadow](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía. Dado que es una propiedad del modelo, se puede configurar igual que otras propiedades. Por ejemplo, para establecer la longitud máxima cuando se usa el discriminador predeterminado, por Convención:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

El discriminador también se puede asignar a una propiedad de .NET en la entidad y configurarlo. Por ejemplo:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Columnas compartidas

Cuando dos tipos de entidad relacionados tienen una propiedad con el mismo nombre, se asignarán a dos columnas independientes de forma predeterminada. Pero si son compatibles, se pueden asignar a la misma columna:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]