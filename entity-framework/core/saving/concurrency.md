---
title: 'Administración de los conflictos de simultaneidad: EF Core'
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413652"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="09080-102">Administrar los conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="09080-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="09080-103">En esta página se documenta cómo funciona la simultaneidad en EF Core y cómo administrar los conflictos de simultaneidad en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09080-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="09080-104">Consulte [Concurrency Tokens](xref:core/modeling/concurrency) (Tokens de simultaneidad) para detalles sobre cómo configurar los tokens de simultaneidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="09080-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="09080-105">Puede ver un [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="09080-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="09080-106">La _simultaneidad de base de datos_ se refiere a las situaciones en las que varios procesos o usuarios acceden o cambian los mismos datos de una base de datos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="09080-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="09080-107">El _control de simultaneidad_ se refiere a los mecanismos específicos que se usan para garantizar la coherencia de los datos en presencia de cambios simultáneos.</span><span class="sxs-lookup"><span data-stu-id="09080-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="09080-108">EF Core implementa el _control de simultaneidad optimista_, lo que significa que permitirá que varios procesos o usuarios hagan cambios de manera independiente sin la sobrecarga que implica la sincronización o el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="09080-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="09080-109">En la situación ideal, estos cambios no interferirán entre sí y, por tanto, se realizarán correctamente.</span><span class="sxs-lookup"><span data-stu-id="09080-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="09080-110">En el peor escenario, dos o más procesos intentarán hacer cambios conflictivos y solo uno de ellos se completará correctamente.</span><span class="sxs-lookup"><span data-stu-id="09080-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="09080-111">Funcionamiento del control de simultaneidad en EF Core</span><span class="sxs-lookup"><span data-stu-id="09080-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="09080-112">Las propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista: cada vez que se realiza una operación de actualización o eliminación durante `SaveChanges`, el valor del token de simultaneidad en la base de datos se compara con el valor original leído por EF Core.</span><span class="sxs-lookup"><span data-stu-id="09080-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="09080-113">Si los valores coinciden, la operación se puede completar.</span><span class="sxs-lookup"><span data-stu-id="09080-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="09080-114">Si no coinciden, EF Core supone que otro usuario realizó una operación en conflicto y anula la transacción actual.</span><span class="sxs-lookup"><span data-stu-id="09080-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="09080-115">La situación en que otro usuario realizó una operación que entra en conflicto con la operación actual se conoce como _conflicto de simultaneidad_.</span><span class="sxs-lookup"><span data-stu-id="09080-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="09080-116">Los proveedores de base de datos son responsable de implementar la comparación de los valores de los tokens de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="09080-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="09080-117">En las bases de datos relacionales, EF Core incluye una comprobación del valor del token de simultaneidad en la cláusula `WHERE` de cualquier instrucción `UPDATE` o `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="09080-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="09080-118">Después de ejecutar las instrucciones, EF Core lee el número de filas que se vieron afectadas.</span><span class="sxs-lookup"><span data-stu-id="09080-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="09080-119">Si no se afectó ninguna fila, se detecta un conflicto de simultaneidad y EF Core genera la excepción `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="09080-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="09080-120">Por ejemplo, queremos configurar `LastName` en `Person` como token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="09080-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="09080-121">Luego, toda operación de actualización en Person incluirá la comprobación de la simultaneidad en la cláusula `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="09080-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="09080-122">Resolución de los conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="09080-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="09080-123">Siguiendo con el ejemplo anterior, si un usuario intenta guardar algunos cambios en una `Person` pero otro usuario ya cambió `LastName`, se generará una excepción.</span><span class="sxs-lookup"><span data-stu-id="09080-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="09080-124">En este punto, la aplicación simplemente podría informar al usuario que la actualización no se realizó correctamente debido a cambios en conflicto y siguió adelante.</span><span class="sxs-lookup"><span data-stu-id="09080-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="09080-125">Pero podría ser conveniente pedirle al usuario que se asegure de que este registro sigue representando a la misma persona real y que reintente la operación.</span><span class="sxs-lookup"><span data-stu-id="09080-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="09080-126">Este proceso es un ejemplo de cómo _resolver un conflicto de simultaneidad_.</span><span class="sxs-lookup"><span data-stu-id="09080-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="09080-127">Resolver un conflicto de simultaneidad implica combinar los cambios pendientes del `DbContext` actual con los valores de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="09080-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="09080-128">Los valores que se van a combinar variarán en función de la aplicación y los puede dirigir el mismo usuario.</span><span class="sxs-lookup"><span data-stu-id="09080-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="09080-129">**Existen tres conjuntos de valores disponibles para ayudar a resolver un conflicto de simultaneidad:**</span><span class="sxs-lookup"><span data-stu-id="09080-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="09080-130">Los **valores actuales** son los valores que la aplicación intentó escribir en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="09080-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="09080-131">Los **valores originales** son los valores que se recuperaron originalmente de la base de datos, antes de realizar cualquier edición.</span><span class="sxs-lookup"><span data-stu-id="09080-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="09080-132">Los **valores de base de datos** son los valores actualmente almacenados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="09080-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="09080-133">El enfoque general para controlar un conflicto de simultaneidad es:</span><span class="sxs-lookup"><span data-stu-id="09080-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="09080-134">Detecte `DbUpdateConcurrencyException` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="09080-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="09080-135">Use `DbUpdateConcurrencyException.Entries` para preparar un nuevo conjunto de cambios para las entidades afectadas.</span><span class="sxs-lookup"><span data-stu-id="09080-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="09080-136">Actualice los valores originales del token de simultaneidad para reflejar los valores actuales en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="09080-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="09080-137">Reintente el proceso hasta que no haya ningún conflicto.</span><span class="sxs-lookup"><span data-stu-id="09080-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="09080-138">En el ejemplo siguiente, `Person.FirstName` y `Person.LastName` están configurados como tokens de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="09080-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="09080-139">Hay un comentario `// TODO:` en la ubicación donde se incluye la lógica específica de la aplicación para elegir el valor que se guardará.</span><span class="sxs-lookup"><span data-stu-id="09080-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
