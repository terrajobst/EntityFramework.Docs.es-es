---
title: Migraciones en entornos de equipo - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997700"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="02072-102">Migraciones en entornos de equipo</span><span class="sxs-lookup"><span data-stu-id="02072-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="02072-103">Cuando se trabaja con las migraciones en entornos de equipo, debe poner atención en el archivo de instantánea del modelo.</span><span class="sxs-lookup"><span data-stu-id="02072-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="02072-104">Este archivo puede indicarle si la migración de su compañero combina sin problemas con el suyo o si tiene que resolver un conflicto, vuelva a crear la migración antes de compartirlo.</span><span class="sxs-lookup"><span data-stu-id="02072-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="02072-105">Combinación</span><span class="sxs-lookup"><span data-stu-id="02072-105">Merging</span></span>
-------
<span data-ttu-id="02072-106">Cuando se mezclan las migraciones de sus compañeros de equipo, puede aparecer conflictos en el archivo de instantánea del modelo.</span><span class="sxs-lookup"><span data-stu-id="02072-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="02072-107">Si ambos cambios están relacionados, la combinación es trivial y las dos migraciones pueden coexistir.</span><span class="sxs-lookup"><span data-stu-id="02072-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="02072-108">Por ejemplo, es posible que obtenga un conflicto de combinación en la configuración de tipo de entidad customer que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="02072-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="02072-109">Puesto que ambas propiedades deben existir en el modelo final, complete la combinación agregando ambas propiedades.</span><span class="sxs-lookup"><span data-stu-id="02072-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="02072-110">En muchos casos, el sistema de control de versiones puede combinar estos cambios automáticamente.</span><span class="sxs-lookup"><span data-stu-id="02072-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="02072-111">En estos casos, la migración y migración de su compañero son independientes entre sí.</span><span class="sxs-lookup"><span data-stu-id="02072-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="02072-112">Dado que cualquiera de ellos podría aplicarse primero, no es necesario realizar ningún cambio adicional en la migración antes de compartirlos con su equipo.</span><span class="sxs-lookup"><span data-stu-id="02072-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="02072-113">Resolución de conflictos</span><span class="sxs-lookup"><span data-stu-id="02072-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="02072-114">A veces se produzca un conflicto es true cuando se mezclan los modelos de instantánea.</span><span class="sxs-lookup"><span data-stu-id="02072-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="02072-115">Por ejemplo, usted y su compañero de equipo es posible que cada uno de ellos ha cambiado el nombre la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="02072-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="02072-116">Si se produce este tipo de conflicto, volver a crear la migración para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="02072-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="02072-117">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="02072-117">Follow these steps:</span></span>

1. <span data-ttu-id="02072-118">Anular la mezcla y la reversión al directorio de trabajo antes de la combinación</span><span class="sxs-lookup"><span data-stu-id="02072-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="02072-119">Quitar la migración (pero mantener los cambios de modelo)</span><span class="sxs-lookup"><span data-stu-id="02072-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="02072-120">Combinar los cambios de su compañero en el directorio de trabajo</span><span class="sxs-lookup"><span data-stu-id="02072-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="02072-121">Volver a agregar a la migración</span><span class="sxs-lookup"><span data-stu-id="02072-121">Re-add your migration</span></span>

<span data-ttu-id="02072-122">Una vez hecho esto, se pueden aplicar las dos migraciones en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="02072-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="02072-123">La migración se aplica en primer lugar, cambiar el nombre de la columna a *Alias*, a partir de entonces la migración cambia su nombre a *Username*.</span><span class="sxs-lookup"><span data-stu-id="02072-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="02072-124">La migración puede compartirse con seguridad con el resto del equipo.</span><span class="sxs-lookup"><span data-stu-id="02072-124">Your migration can safely be shared with the rest of the team.</span></span>
