---
title: 'Restricciones Foreign Key: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655997"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="c2050-102">Restricciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="c2050-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="c2050-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="c2050-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c2050-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="c2050-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c2050-105">Se introduce una restricción FOREIGN KEY para cada relación del modelo.</span><span class="sxs-lookup"><span data-stu-id="c2050-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="c2050-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="c2050-106">Conventions</span></span>

<span data-ttu-id="c2050-107">Por Convención, las restricciones Foreign Key se denominan `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c2050-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="c2050-108">En el caso de las claves externas compuestas `<foreign key property name>` se convierte en una lista de nombres de propiedades de clave externa separadas por guiones bajos.</span><span class="sxs-lookup"><span data-stu-id="c2050-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c2050-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c2050-109">Data Annotations</span></span>

<span data-ttu-id="c2050-110">Los nombres de restricción de clave externa no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="c2050-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c2050-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="c2050-111">Fluent API</span></span>

<span data-ttu-id="c2050-112">Puede usar la API fluida para configurar el nombre de la restricción de clave externa para una relación.</span><span class="sxs-lookup"><span data-stu-id="c2050-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
