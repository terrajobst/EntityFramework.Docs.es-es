---
title: 'Campos de respaldo: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811878"
---
# <a name="backing-fields"></a><span data-ttu-id="899aa-102">Campos de respaldo</span><span class="sxs-lookup"><span data-stu-id="899aa-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="899aa-103">Esta característica es nueva en EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="899aa-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="899aa-104">Los campos de respaldo permiten a EF leer o escribir en un campo en lugar de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="899aa-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="899aa-105">Esto puede ser útil cuando se usa la encapsulación en la clase para restringir el uso de y/o mejorar la semántica en torno al acceso a los datos por código de aplicación, pero el valor debe leerse o escribirse en la base de datos sin usar esas restricciones mejoras.</span><span class="sxs-lookup"><span data-stu-id="899aa-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="899aa-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="899aa-106">Conventions</span></span>

<span data-ttu-id="899aa-107">Por Convención, se detectarán los campos siguientes como campos de respaldo para una propiedad determinada (que se muestra en orden de prioridad).</span><span class="sxs-lookup"><span data-stu-id="899aa-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="899aa-108">Los campos solo se detectan para las propiedades que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="899aa-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="899aa-109">Para obtener más información sobre las propiedades que se incluyen en el modelo, vea [incluir & excluyendo las propiedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="899aa-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="899aa-110">Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar instancias de entidad de la base de datos (en lugar de usar el establecedor de propiedad).</span><span class="sxs-lookup"><span data-stu-id="899aa-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="899aa-111">Si EF necesita leer o escribir el valor en otros momentos, usará la propiedad si es posible.</span><span class="sxs-lookup"><span data-stu-id="899aa-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="899aa-112">Por ejemplo, si EF necesita actualizar el valor de una propiedad, usará el establecedor de la propiedad si se ha definido uno.</span><span class="sxs-lookup"><span data-stu-id="899aa-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="899aa-113">Si la propiedad es de solo lectura, se escribirá en el campo.</span><span class="sxs-lookup"><span data-stu-id="899aa-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="899aa-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="899aa-114">Data Annotations</span></span>

<span data-ttu-id="899aa-115">Los campos de respaldo no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="899aa-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="899aa-116">API fluida</span><span class="sxs-lookup"><span data-stu-id="899aa-116">Fluent API</span></span>

<span data-ttu-id="899aa-117">Puede usar la API fluida para configurar un campo de respaldo para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="899aa-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="899aa-118">Controlar cuándo se usa el campo</span><span class="sxs-lookup"><span data-stu-id="899aa-118">Controlling when the field is used</span></span>

<span data-ttu-id="899aa-119">Puede configurar Cuándo EF utiliza el campo o la propiedad.</span><span class="sxs-lookup"><span data-stu-id="899aa-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="899aa-120">Vea la [enumeración PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para ver las opciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="899aa-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="899aa-121">Campos sin propiedad</span><span class="sxs-lookup"><span data-stu-id="899aa-121">Fields without a property</span></span>

<span data-ttu-id="899aa-122">También puede crear una propiedad conceptual en el modelo que no tiene una propiedad de CLR correspondiente en la clase de entidad, sino que usa un campo para almacenar los datos en la entidad.</span><span class="sxs-lookup"><span data-stu-id="899aa-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="899aa-123">Esto es diferente de [las propiedades de las instantáneas](shadow-properties.md), donde los datos se almacenan en el seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="899aa-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="899aa-124">Normalmente se utilizaría si la clase de entidad usa métodos para obtener o establecer valores.</span><span class="sxs-lookup"><span data-stu-id="899aa-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="899aa-125">Puede dar a EF el nombre del campo en el `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="899aa-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="899aa-126">Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.</span><span class="sxs-lookup"><span data-stu-id="899aa-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="899aa-127">Cuando no hay ninguna propiedad en la clase de entidad, puede usar el método `EF.Property(...)` en una consulta LINQ para hacer referencia a la propiedad que forma conceptualmente parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="899aa-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
