---
title: Tokens de simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="3cfdf-102">Tokens de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="3cfdf-102">Concurrency Tokens</span></span>

<span data-ttu-id="3cfdf-103">Si una propiedad está configurada como un token de simultaneidad EF comprobará que ningún otro usuario modificó ese valor en la base de datos al guardar los cambios a ese registro.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="3cfdf-104">EF usa un modelo de simultaneidad optimista, lo que significa que se suponen que no ha cambiado el valor e intente guardar los datos, pero producir si encuentra que el valor ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="3cfdf-105">Por ejemplo que podríamos desear configurar `LastName` en `Person` un token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="3cfdf-106">Esto significa que si un usuario intenta guardar los cambios a un `Person`, pero otro usuario ha cambiado el `LastName` , a continuación, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="3cfdf-107">Esto puede ser deseable para que la aplicación puede solicitar al usuario para asegurarse de que este registro representa siguen la misma persona real antes de guardar sus cambios.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="3cfdf-108">Esta página documenta cómo configurar los tokens de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="3cfdf-109">Vea [control de simultaneidad](../saving/concurrency.md) para obtener ejemplos de cómo usar simultaneidad optimista en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="3cfdf-110">Cómo funcionan los tokens de simultaneidad en EF</span><span class="sxs-lookup"><span data-stu-id="3cfdf-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="3cfdf-111">Almacenes de datos pueden exigir tokens de simultaneidad mediante la comprobación de que todos los registros que se va a actualizar o eliminar aún tiene el mismo valor para el token de simultaneidad que se asignó cuando el contexto de carga originalmente los datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="3cfdf-112">Por ejemplo, bases de datos relacionales para ello, incluido el token de simultaneidad en el `WHERE` cláusula de cualquier `UPDATE` o `DELETE` comandos y comprobando el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="3cfdf-113">Si el token de simultaneidad sigue coincidiendo con una fila se actualizará.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="3cfdf-114">Si ha cambiado el valor de la base de datos, no se actualiza ninguna fila.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="3cfdf-115">Convenciones</span><span class="sxs-lookup"><span data-stu-id="3cfdf-115">Conventions</span></span>

<span data-ttu-id="3cfdf-116">Por convención, propiedades nunca se configuran como tokens de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3cfdf-117">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="3cfdf-117">Data Annotations</span></span>

<span data-ttu-id="3cfdf-118">Puede usar las anotaciones de datos para configurar una propiedad como un token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="3cfdf-119">API fluida</span><span class="sxs-lookup"><span data-stu-id="3cfdf-119">Fluent API</span></span>

<span data-ttu-id="3cfdf-120">Puede usar la API fluida para configurar una propiedad como un token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="3cfdf-121">Versión de fila/marca de tiempo</span><span class="sxs-lookup"><span data-stu-id="3cfdf-121">Timestamp/row version</span></span>

<span data-ttu-id="3cfdf-122">Una marca de tiempo es una propiedad que se genera un nuevo valor de la base de datos cada vez que se inserta o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="3cfdf-123">La propiedad también se trata como un token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="3cfdf-124">Esto garantiza que obtendrá una excepción si nadie ha modificado una fila que está intentando actualizar ya que la consulta para los datos.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="3cfdf-125">Cómo esto se consigue es responsabilidad del proveedor de base de datos que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="3cfdf-126">Para SQL Server, marca de tiempo se utiliza normalmente en un *byte []* propiedad, que será el programa de instalación como un *ROWVERSION* columna en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="3cfdf-127">Convenciones</span><span class="sxs-lookup"><span data-stu-id="3cfdf-127">Conventions</span></span>

<span data-ttu-id="3cfdf-128">Por convención, propiedades nunca se configuran como marcas de tiempo.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="3cfdf-129">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="3cfdf-129">Data Annotations</span></span>

<span data-ttu-id="3cfdf-130">Puede usar anotaciones de datos para configurar una propiedad como una marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="3cfdf-131">API fluida</span><span class="sxs-lookup"><span data-stu-id="3cfdf-131">Fluent API</span></span>

<span data-ttu-id="3cfdf-132">Puede usar la API fluida para configurar una propiedad como una marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="3cfdf-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
