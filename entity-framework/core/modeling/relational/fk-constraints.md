---
title: 'Restricciones Foreign Key: EF Core'
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824584"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="942ce-102">Restricciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="942ce-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="942ce-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="942ce-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="942ce-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="942ce-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="942ce-105">Se introduce una restricción FOREIGN KEY para cada relación del modelo.</span><span class="sxs-lookup"><span data-stu-id="942ce-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="942ce-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="942ce-106">Conventions</span></span>

<span data-ttu-id="942ce-107">Por Convención, las restricciones Foreign Key se denominan `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="942ce-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="942ce-108">En el caso de las claves externas compuestas `<foreign key property name>` se convierte en una lista de nombres de propiedades de clave externa separadas por guiones bajos.</span><span class="sxs-lookup"><span data-stu-id="942ce-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="942ce-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="942ce-109">Data Annotations</span></span>

<span data-ttu-id="942ce-110">Los nombres de restricción de clave externa no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="942ce-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="942ce-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="942ce-111">Fluent API</span></span>

<span data-ttu-id="942ce-112">Puede usar la API fluida para configurar el nombre de la restricción de clave externa para una relación.</span><span class="sxs-lookup"><span data-stu-id="942ce-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
