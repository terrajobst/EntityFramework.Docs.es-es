---
title: Controlar los conflictos de simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="0ab04-102">Administrar los conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="0ab04-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="0ab04-103">Esta página documenta cómo funciona la simultaneidad en EF Core y cómo tratar los conflictos de simultaneidad en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ab04-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="0ab04-104">Vea [Tokens de simultaneidad](xref:core/modeling/concurrency) para obtener más información sobre cómo configurar los tokens de simultaneidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="0ab04-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="0ab04-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="0ab04-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="0ab04-106">_Simultaneidad de base de datos_ hace referencia a situaciones en las que varios procesos o a los usuarios tener acceso o cambiar los mismos datos en una base de datos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="0ab04-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="0ab04-107">_Control de simultaneidad_ hace referencia a mecanismos específicos utilizados para garantizar la coherencia de datos en presencia de cambios simultáneos.</span><span class="sxs-lookup"><span data-stu-id="0ab04-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="0ab04-108">Núcleo EF implementa _control de simultaneidad optimista_, lo que significa que permitirá a varios procesos o los usuarios realizar cambios de forma independiente sin la sobrecarga de sincronización o de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="0ab04-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="0ab04-109">En la situación ideal, estos cambios no interfieran entre sí y, por tanto, se podrán realizar correctamente.</span><span class="sxs-lookup"><span data-stu-id="0ab04-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="0ab04-110">En el peor de los caso, dos o más procesos intentará realizar cambios en conflicto y solo uno de ellos debe ejecutarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="0ab04-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="0ab04-111">Cómo funciona el control de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="0ab04-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="0ab04-112">Propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista: cada vez que se realiza una operación update o delete durante `SaveChanges`, el valor del token de simultaneidad en la base de datos se compara con la versión original valor leído por núcleo de EF.</span><span class="sxs-lookup"><span data-stu-id="0ab04-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="0ab04-113">Si los valores coinciden, puede completar la operación.</span><span class="sxs-lookup"><span data-stu-id="0ab04-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="0ab04-114">Si los valores no coinciden, se da por supuesto en EF Core que otro usuario ha realizado una operación en conflicto y anula la transacción actual.</span><span class="sxs-lookup"><span data-stu-id="0ab04-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="0ab04-115">La situación cuando otro usuario ha realizado una operación que entra en conflicto con la operación actual se conoce como _conflicto de simultaneidad_.</span><span class="sxs-lookup"><span data-stu-id="0ab04-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="0ab04-116">Proveedores de base de datos son responsables de implementar la comparación de valores de token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="0ab04-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="0ab04-117">En bases de datos relacionales EF Core incluye una comprobación para el valor del token de simultaneidad en el `WHERE` cláusula de cualquier `UPDATE` o `DELETE` las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="0ab04-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="0ab04-118">Después de ejecutar las instrucciones, Core EF lee el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="0ab04-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="0ab04-119">Si ninguna fila afectada, se detecta un conflicto de simultaneidad y produce EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0ab04-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="0ab04-120">Por ejemplo, queremos podemos configurar `LastName` en `Person` un token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="0ab04-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="0ab04-121">Cualquier operación de actualización en persona incluirá la comprobación de simultaneidad en el `WHERE` cláusula:</span><span class="sxs-lookup"><span data-stu-id="0ab04-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="0ab04-122">Resolver conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="0ab04-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="0ab04-123">Siguiendo con el ejemplo anterior, si un usuario intenta guardar los cambios a un `Person`, pero otro usuario ya ha cambiado el `LastName` el se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="0ab04-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="0ab04-124">En este punto, la aplicación simplemente podría informar al usuario que la actualización no tuvo éxito debido a cambios en conflicto y mover.</span><span class="sxs-lookup"><span data-stu-id="0ab04-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="0ab04-125">Pero puede ser deseable para pedir al usuario para asegurarse de que este registro representa siguen la misma persona real y vuelva a intentar la operación.</span><span class="sxs-lookup"><span data-stu-id="0ab04-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="0ab04-126">Este proceso es un ejemplo de _resolver un conflicto de simultaneidad_.</span><span class="sxs-lookup"><span data-stu-id="0ab04-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="0ab04-127">Resolver un conflicto de simultaneidad implica combinar los cambios pendientes actual `DbContext` con los valores de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ab04-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="0ab04-128">Combinarán los valores que varía en función de la aplicación y puede ser dirigido por proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="0ab04-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="0ab04-129">**Hay tres conjuntos de valores disponibles para ayudar a resolver un conflicto de simultaneidad:**</span><span class="sxs-lookup"><span data-stu-id="0ab04-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="0ab04-130">**Valores actuales** son los valores que la aplicación estaba intentando escribir en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ab04-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="0ab04-131">**Valores originales** son los valores que se recuperaron originalmente desde la base de datos antes de que se realizaron las modificaciones.</span><span class="sxs-lookup"><span data-stu-id="0ab04-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="0ab04-132">**Valores de la base de datos** son los valores almacenados actualmente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ab04-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="0ab04-133">Es el enfoque general para controlar los conflictos de simultaneidad:</span><span class="sxs-lookup"><span data-stu-id="0ab04-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="0ab04-134">Detectar `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="0ab04-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="0ab04-135">Use `DbUpdateConcurrencyException.Entries` para preparar un nuevo conjunto de cambios para las entidades afectadas.</span><span class="sxs-lookup"><span data-stu-id="0ab04-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="0ab04-136">Actualice los valores originales de lo token de simultaneidad para reflejar los valores actuales de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0ab04-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="0ab04-137">Vuelva a intentar el proceso hasta que no se produzca ningún conflicto.</span><span class="sxs-lookup"><span data-stu-id="0ab04-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="0ab04-138">En el ejemplo siguiente, `Person.FirstName` y `Person.LastName` están configurados como tokens de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="0ab04-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="0ab04-139">Hay un `// TODO:` comentario en la ubicación donde se incluye lógica específica de aplicación para elegir el valor que se guarde.</span><span class="sxs-lookup"><span data-stu-id="0ab04-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
