---
title: Base de datos de SQLite proveedor - limitaciones - EF Core
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="25bd1-102">Limitaciones del proveedor de base de datos de SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="25bd1-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="25bd1-103">El proveedor de código tiene una serie de limitaciones de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="25bd1-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="25bd1-104">La mayoría de estas limitaciones es el resultado de las limitaciones en el motor de base de datos de código subyacente y no es específica de EF.</span><span class="sxs-lookup"><span data-stu-id="25bd1-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="25bd1-105">Limitaciones de modelado</span><span class="sxs-lookup"><span data-stu-id="25bd1-105">Modeling limitations</span></span>

<span data-ttu-id="25bd1-106">La biblioteca relacional común (compartida por los proveedores de base de datos relacional de Entity Framework) define las API para la modelización conceptos que son comunes a la mayoría de los motores de base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="25bd1-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="25bd1-107">Un par de estos conceptos no se admiten por el proveedor de código.</span><span class="sxs-lookup"><span data-stu-id="25bd1-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="25bd1-108">Esquemas</span><span class="sxs-lookup"><span data-stu-id="25bd1-108">Schemas</span></span>
* <span data-ttu-id="25bd1-109">Secuencias</span><span class="sxs-lookup"><span data-stu-id="25bd1-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="25bd1-110">Limitaciones de las migraciones</span><span class="sxs-lookup"><span data-stu-id="25bd1-110">Migrations limitations</span></span>

<span data-ttu-id="25bd1-111">El motor de base de datos de SQLite no es compatible con un número de operaciones de esquema que son compatibles con la mayoría de las otras bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="25bd1-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="25bd1-112">Si intenta aplicar una de las operaciones no admitidas en una base de datos de SQLite un `NotSupportedException` se iniciará.</span><span class="sxs-lookup"><span data-stu-id="25bd1-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="25bd1-113">Operación</span><span class="sxs-lookup"><span data-stu-id="25bd1-113">Operation</span></span>            | <span data-ttu-id="25bd1-114">¿Es compatible?</span><span class="sxs-lookup"><span data-stu-id="25bd1-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="25bd1-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="25bd1-115">AddColumn</span></span>            | <span data-ttu-id="25bd1-116">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-116">✔</span></span>          |
| <span data-ttu-id="25bd1-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="25bd1-117">AddForeignKey</span></span>        | <span data-ttu-id="25bd1-118">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-118">✗</span></span>          |
| <span data-ttu-id="25bd1-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="25bd1-119">AddPrimaryKey</span></span>        | <span data-ttu-id="25bd1-120">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-120">✗</span></span>          |
| <span data-ttu-id="25bd1-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="25bd1-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="25bd1-122">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-122">✗</span></span>          |
| <span data-ttu-id="25bd1-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="25bd1-123">AlterColumn</span></span>          | <span data-ttu-id="25bd1-124">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-124">✗</span></span>          |
| <span data-ttu-id="25bd1-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="25bd1-125">CreateIndex</span></span>          | <span data-ttu-id="25bd1-126">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-126">✔</span></span>          |
| <span data-ttu-id="25bd1-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="25bd1-127">CreateTable</span></span>          | <span data-ttu-id="25bd1-128">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-128">✔</span></span>          |
| <span data-ttu-id="25bd1-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="25bd1-129">DropColumn</span></span>           | <span data-ttu-id="25bd1-130">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-130">✗</span></span>          |
| <span data-ttu-id="25bd1-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="25bd1-131">DropForeignKey</span></span>       | <span data-ttu-id="25bd1-132">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-132">✗</span></span>          |
| <span data-ttu-id="25bd1-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="25bd1-133">DropIndex</span></span>            | <span data-ttu-id="25bd1-134">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-134">✔</span></span>          |
| <span data-ttu-id="25bd1-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="25bd1-135">DropPrimaryKey</span></span>       | <span data-ttu-id="25bd1-136">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-136">✗</span></span>          |
| <span data-ttu-id="25bd1-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="25bd1-137">DropTable</span></span>            | <span data-ttu-id="25bd1-138">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-138">✔</span></span>          |
| <span data-ttu-id="25bd1-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="25bd1-139">DropUniqueConstraint</span></span> | <span data-ttu-id="25bd1-140">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-140">✗</span></span>          |
| <span data-ttu-id="25bd1-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="25bd1-141">RenameColumn</span></span>         | <span data-ttu-id="25bd1-142">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-142">✗</span></span>          |
| <span data-ttu-id="25bd1-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="25bd1-143">RenameIndex</span></span>          | <span data-ttu-id="25bd1-144">✗</span><span class="sxs-lookup"><span data-stu-id="25bd1-144">✗</span></span>          |
| <span data-ttu-id="25bd1-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="25bd1-145">RenameTable</span></span>          | <span data-ttu-id="25bd1-146">✔</span><span class="sxs-lookup"><span data-stu-id="25bd1-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="25bd1-147">Solución alternativa de las limitaciones de las migraciones</span><span class="sxs-lookup"><span data-stu-id="25bd1-147">Migrations limitations workaround</span></span>

<span data-ttu-id="25bd1-148">Puede solucionar algunas de estas limitaciones, escriba manualmente el código en las migraciones para realizar una tabla volver a generar.</span><span class="sxs-lookup"><span data-stu-id="25bd1-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="25bd1-149">Una recompilación de tabla implica cambiar el nombre de la tabla existente, crea una nueva tabla, copiar los datos a la nueva tabla y eliminación de la tabla anterior.</span><span class="sxs-lookup"><span data-stu-id="25bd1-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="25bd1-150">Debe utilizar el `Sql(string)` método para realizar algunos de estos pasos.</span><span class="sxs-lookup"><span data-stu-id="25bd1-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="25bd1-151">Vea [realizar otros tipos de tabla de cambios de esquema](http://sqlite.org/lang_altertable.html#otheralter) en la documentación de SQLite para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="25bd1-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="25bd1-152">En el futuro, EF puede admitir algunas de estas operaciones mediante el enfoque de regeneración de tabla en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="25bd1-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="25bd1-153">También puede [realizar el seguimiento de esta característica en nuestro proyecto de GitHub](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="25bd1-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
