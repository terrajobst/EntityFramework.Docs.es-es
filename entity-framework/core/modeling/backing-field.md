---
title: Seguridad de los campos - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994098"
---
# <a name="backing-fields"></a><span data-ttu-id="642e6-102">Campos de respaldo</span><span class="sxs-lookup"><span data-stu-id="642e6-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="642e6-103">Esta característica es nueva en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="642e6-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="642e6-104">Los campos de respaldo para permitir EF leer o escribir en un campo en lugar de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="642e6-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="642e6-105">Esto puede ser útil cuando la encapsulación de la clase se utiliza para mejorar la semántica de relación con el acceso a los datos y restringir el uso de código de aplicación, pero el valor se debería leen o escriben en la base de datos sin usar esas restricciones / mejoras.</span><span class="sxs-lookup"><span data-stu-id="642e6-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="642e6-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="642e6-106">Conventions</span></span>

<span data-ttu-id="642e6-107">Por convención, se detectarán los siguientes campos como campos para una propiedad determinada (que se muestran en orden de prioridad) de respaldo.</span><span class="sxs-lookup"><span data-stu-id="642e6-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="642e6-108">Solo se detectan los campos para las propiedades que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="642e6-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="642e6-109">Para obtener más información en el que se incluyan las propiedades en el modelo, vea [inclusión y exclusión de propiedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="642e6-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="642e6-110">Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar las instancias de entidad desde la base de datos (en lugar de utilizar el establecedor de propiedad).</span><span class="sxs-lookup"><span data-stu-id="642e6-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="642e6-111">Si EF necesita para leer o escribir el valor en otras ocasiones, usará la propiedad si es posible.</span><span class="sxs-lookup"><span data-stu-id="642e6-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="642e6-112">Por ejemplo, si necesita actualizar el valor de una propiedad de EF, usará el establecedor de propiedad si hay alguno definido.</span><span class="sxs-lookup"><span data-stu-id="642e6-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="642e6-113">Si la propiedad es de solo lectura, escribirá en el campo.</span><span class="sxs-lookup"><span data-stu-id="642e6-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="642e6-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="642e6-114">Data Annotations</span></span>

<span data-ttu-id="642e6-115">Campos de respaldo no pueden configurarse con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="642e6-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="642e6-116">API fluida</span><span class="sxs-lookup"><span data-stu-id="642e6-116">Fluent API</span></span>

<span data-ttu-id="642e6-117">Puede usar la API Fluent para configurar un campo de respaldo para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="642e6-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="642e6-118">Cuando se usa el campo de control</span><span class="sxs-lookup"><span data-stu-id="642e6-118">Controlling when the field is used</span></span>

<span data-ttu-id="642e6-119">Puede configurar cuando EF usa el campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="642e6-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="642e6-120">Consulte la [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para las opciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="642e6-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="642e6-121">Campos sin una propiedad</span><span class="sxs-lookup"><span data-stu-id="642e6-121">Fields without a property</span></span>

<span data-ttu-id="642e6-122">También puede crear una propiedad conceptual en el modelo que no tiene una propiedad CLR correspondiente en la clase de entidad, sino que usa un campo para almacenar los datos de la entidad.</span><span class="sxs-lookup"><span data-stu-id="642e6-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="642e6-123">Esto es diferente de [reemplazar propiedades](shadow-properties.md), donde los datos se almacenan en el seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="642e6-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="642e6-124">Esto normalmente se usaría si la clase de entidad usa los métodos para obtener o establecer valores.</span><span class="sxs-lookup"><span data-stu-id="642e6-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="642e6-125">Puede dar el nombre del campo en EF la `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="642e6-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="642e6-126">Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.</span><span class="sxs-lookup"><span data-stu-id="642e6-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="642e6-127">También puede dar un nombre distinto del nombre de campo a la propiedad.</span><span class="sxs-lookup"><span data-stu-id="642e6-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="642e6-128">Este nombre, a continuación, se usa al crear el modelo, sobre todo se usará para el nombre de columna que se asigna a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="642e6-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="642e6-129">Cuando no hay ninguna propiedad en la clase de entidad, puede usar el `EF.Property(...)` método en una consulta LINQ para hacer referencia a la propiedad que conceptualmente es parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="642e6-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
