---
title: Migraciones en entornos de equipo - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053805"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="96e10-102">Migraciones en entornos de equipo</span><span class="sxs-lookup"><span data-stu-id="96e10-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="96e10-103">Cuando trabaje con migraciones en entornos de equipo, preste especial atención en el archivo de instantánea de modelo.</span><span class="sxs-lookup"><span data-stu-id="96e10-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="96e10-104">Este archivo puede indicarle si migración de su compañero limpiamente combina con la suya de si tiene que resolver un conflicto mediante la creación de volver a la migración antes de compartirlo.</span><span class="sxs-lookup"><span data-stu-id="96e10-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="96e10-105">Combinación</span><span class="sxs-lookup"><span data-stu-id="96e10-105">Merging</span></span>
-------
<span data-ttu-id="96e10-106">Cuando se mezclan las migraciones de sus compañeros de equipo, puede obtener conflictos en el archivo de instantánea de modelo.</span><span class="sxs-lookup"><span data-stu-id="96e10-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="96e10-107">Si no están relacionadas con los cambios, la combinación es trivial y pueden coexistir dos migraciones.</span><span class="sxs-lookup"><span data-stu-id="96e10-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="96e10-108">Por ejemplo, puede obtener un conflicto de combinación en la configuración del tipo de entidad customer este aspecto:</span><span class="sxs-lookup"><span data-stu-id="96e10-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="96e10-109">Puesto que ambas propiedades deben existir en el modelo final, completar la combinación mediante la adición de ambas propiedades.</span><span class="sxs-lookup"><span data-stu-id="96e10-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="96e10-110">En muchos casos, el sistema de control de versiones puede combinar dichos cambios automáticamente.</span><span class="sxs-lookup"><span data-stu-id="96e10-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="96e10-111">En estos casos, la migración y la migración de su compañero son independientes entre sí.</span><span class="sxs-lookup"><span data-stu-id="96e10-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="96e10-112">Puesto que ninguna de ellas puede aplicarse en primer lugar, no es necesario realizar ningún cambio adicional a la migración antes de compartirla con su equipo.</span><span class="sxs-lookup"><span data-stu-id="96e10-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="96e10-113">Resolución de conflictos</span><span class="sxs-lookup"><span data-stu-id="96e10-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="96e10-114">A veces se produzca un conflicto true cuando se mezclan los modelos de instantánea.</span><span class="sxs-lookup"><span data-stu-id="96e10-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="96e10-115">Por ejemplo, usted y su compañero pueden cada uno de ellos ha cambiado el nombre la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="96e10-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="96e10-116">Si se produce este tipo de conflicto, volver a crear la migración para resolverlo.</span><span class="sxs-lookup"><span data-stu-id="96e10-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="96e10-117">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="96e10-117">Follow these steps:</span></span>

1. <span data-ttu-id="96e10-118">Anular la mezcla y la reversión en su directorio de trabajo antes de la combinación</span><span class="sxs-lookup"><span data-stu-id="96e10-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="96e10-119">Quitar la migración (pero conservar los cambios de modelo)</span><span class="sxs-lookup"><span data-stu-id="96e10-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="96e10-120">Combinar los cambios de su compañero en el directorio de trabajo</span><span class="sxs-lookup"><span data-stu-id="96e10-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="96e10-121">Vuelva a agregar la migración</span><span class="sxs-lookup"><span data-stu-id="96e10-121">Re-add your migration</span></span>

<span data-ttu-id="96e10-122">Una vez hecho esto, pueden aplicarse las dos migraciones en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="96e10-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="96e10-123">Su migración se aplica en primer lugar, cambiar el nombre de la columna a *Alias*, a partir de ahí la migración cambia el nombre a *nombre de usuario*.</span><span class="sxs-lookup"><span data-stu-id="96e10-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="96e10-124">La migración puede ser comparte de forma segura con el resto del equipo.</span><span class="sxs-lookup"><span data-stu-id="96e10-124">Your migration can safely be shared with the rest of the team.</span></span>
