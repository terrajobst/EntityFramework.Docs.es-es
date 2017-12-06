---
title: Realizar una copia de los campos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a><span data-ttu-id="af4f0-102">Campos de respaldo</span><span class="sxs-lookup"><span data-stu-id="af4f0-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="af4f0-103">Esta característica es nueva en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="af4f0-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="af4f0-104">Campos de respaldo permiten EF leer o escribir en un campo en lugar de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="af4f0-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="af4f0-105">Esto puede resultar útil cuando se utiliza para mejorar la semántica de relación con el acceso a los datos o restringir el uso de encapsulación de la clase mediante código de aplicación, pero el valor se debe leer desde o escribir en la base de datos sin utilizar esas restricciones / mejoras.</span><span class="sxs-lookup"><span data-stu-id="af4f0-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="af4f0-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="af4f0-106">Conventions</span></span>

<span data-ttu-id="af4f0-107">Por convención, se detectarán los siguientes campos como realizar una copia de los campos de una propiedad determinada (que se muestran en orden de prioridad).</span><span class="sxs-lookup"><span data-stu-id="af4f0-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="af4f0-108">Solo se detectan los campos para las propiedades que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="af4f0-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="af4f0-109">Para obtener más información en el que se incluyen propiedades en el modelo, vea [incluidos & Excluir propiedades](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="af4f0-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="af4f0-110">Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar instancias de entidad desde la base de datos (en lugar de usar el establecedor de propiedades).</span><span class="sxs-lookup"><span data-stu-id="af4f0-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="af4f0-111">Si necesita EF leer o escribir el valor en otras ocasiones, utilizará la propiedad si es posible.</span><span class="sxs-lookup"><span data-stu-id="af4f0-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="af4f0-112">Por ejemplo, si EF debe actualizar el valor de una propiedad, usará el establecedor de propiedad si se ha definido uno.</span><span class="sxs-lookup"><span data-stu-id="af4f0-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="af4f0-113">Si la propiedad es de solo lectura, se escribirá en el campo.</span><span class="sxs-lookup"><span data-stu-id="af4f0-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="af4f0-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="af4f0-114">Data Annotations</span></span>

<span data-ttu-id="af4f0-115">Realizar una copia de los campos no se puede configurar con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="af4f0-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="af4f0-116">API fluida</span><span class="sxs-lookup"><span data-stu-id="af4f0-116">Fluent API</span></span>

<span data-ttu-id="af4f0-117">Puede usar la API fluida para configurar un campo de respaldo para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="af4f0-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="af4f0-118">Controlar cuando se usa el campo</span><span class="sxs-lookup"><span data-stu-id="af4f0-118">Controlling when the field is used</span></span>

<span data-ttu-id="af4f0-119">Puede configurar cuando EF usa el campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="af4f0-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="af4f0-120">Consulte la [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para las opciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="af4f0-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="af4f0-121">Campos sin una propiedad</span><span class="sxs-lookup"><span data-stu-id="af4f0-121">Fields without a property</span></span>

<span data-ttu-id="af4f0-122">También puede crear una propiedad conceptual en el modelo que no tiene una propiedad CLR correspondiente en la clase de entidad, sino que utiliza un campo para almacenar los datos de la entidad.</span><span class="sxs-lookup"><span data-stu-id="af4f0-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="af4f0-123">Esto es diferente de [reemplazar propiedades](shadow-properties.md), donde se almacenan los datos en la herramienta de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="af4f0-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="af4f0-124">Esto lo que normalmente se utiliza si la clase de entidad utiliza métodos para obtener o establecer valores.</span><span class="sxs-lookup"><span data-stu-id="af4f0-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="af4f0-125">Puede asignar EF el nombre del campo en el `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="af4f0-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="af4f0-126">Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.</span><span class="sxs-lookup"><span data-stu-id="af4f0-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="af4f0-127">También puede asignar un nombre distinto del nombre de campo a la propiedad.</span><span class="sxs-lookup"><span data-stu-id="af4f0-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="af4f0-128">Este nombre se utiliza al crear el modelo, sobre todo se usará para el nombre de columna que se asigna a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="af4f0-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="af4f0-129">Cuando no hay ninguna propiedad en la clase de entidad, puede usar el `EF.Property(...)` método en una consulta LINQ para hacer referencia a la propiedad que es conceptualmente parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="af4f0-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
