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
# <a name="inheritance-relational-database"></a><span data-ttu-id="8b90a-103">Herencia (base de datos relacional)</span><span class="sxs-lookup"><span data-stu-id="8b90a-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="8b90a-104">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="8b90a-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8b90a-105">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="8b90a-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8b90a-106">La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8b90a-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="8b90a-107">Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b90a-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="8b90a-108">Otros patrones comunes como tabla por tipo (TPT) y tabla por tipo específico (TPC) no están disponibles todavía.</span><span class="sxs-lookup"><span data-stu-id="8b90a-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="8b90a-109">Convenciones</span><span class="sxs-lookup"><span data-stu-id="8b90a-109">Conventions</span></span>

<span data-ttu-id="8b90a-110">De forma predeterminada, la herencia se asignará mediante el patrón de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="8b90a-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="8b90a-111">TPH usa una sola tabla para almacenar los datos de todos los tipos de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="8b90a-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="8b90a-112">Una columna de discriminador se utiliza para identificar el tipo que representa cada fila.</span><span class="sxs-lookup"><span data-stu-id="8b90a-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="8b90a-113">EF Core solo configurará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo (consulte [herencia](../inheritance.md) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="8b90a-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="8b90a-114">A continuación se muestra un ejemplo en el que se muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH.</span><span class="sxs-lookup"><span data-stu-id="8b90a-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="8b90a-115">La columna *Discriminator* identifica el tipo de *blog* que se almacena en cada fila.</span><span class="sxs-lookup"><span data-stu-id="8b90a-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="8b90a-117">Las columnas de la base de datos se convierten en NULL automáticamente según sea necesario al usar la asignación TPH.</span><span class="sxs-lookup"><span data-stu-id="8b90a-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8b90a-118">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="8b90a-118">Data Annotations</span></span>

<span data-ttu-id="8b90a-119">No se pueden usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="8b90a-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8b90a-120">API fluida</span><span class="sxs-lookup"><span data-stu-id="8b90a-120">Fluent API</span></span>

<span data-ttu-id="8b90a-121">Puede usar la API fluida para configurar el nombre y el tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="8b90a-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="8b90a-122">Configurar la propiedad Discriminator</span><span class="sxs-lookup"><span data-stu-id="8b90a-122">Configuring the discriminator property</span></span>

<span data-ttu-id="8b90a-123">En los ejemplos anteriores, el discriminador se crea como una [propiedad Shadow](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="8b90a-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="8b90a-124">Dado que es una propiedad del modelo, se puede configurar igual que otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="8b90a-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="8b90a-125">Por ejemplo, para establecer la longitud máxima cuando se usa el discriminador predeterminado, por Convención:</span><span class="sxs-lookup"><span data-stu-id="8b90a-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="8b90a-126">El discriminador también se puede asignar a una propiedad de .NET en la entidad y configurarlo.</span><span class="sxs-lookup"><span data-stu-id="8b90a-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="8b90a-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b90a-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="8b90a-128">Columnas compartidas</span><span class="sxs-lookup"><span data-stu-id="8b90a-128">Shared columns</span></span>

<span data-ttu-id="8b90a-129">Cuando dos tipos de entidad relacionados tienen una propiedad con el mismo nombre, se asignarán a dos columnas independientes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8b90a-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="8b90a-130">Pero si son compatibles, se pueden asignar a la misma columna:</span><span class="sxs-lookup"><span data-stu-id="8b90a-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]