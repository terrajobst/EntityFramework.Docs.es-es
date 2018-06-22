---
title: Claves alternativas (restricciones únicas) - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052795"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="c19ec-102">Claves alternativas (restricciones únicas)</span><span class="sxs-lookup"><span data-stu-id="c19ec-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="c19ec-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="c19ec-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c19ec-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="c19ec-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c19ec-105">Una restricción unique se introdujo para cada clave alternativa en el modelo.</span><span class="sxs-lookup"><span data-stu-id="c19ec-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="c19ec-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="c19ec-106">Conventions</span></span>

<span data-ttu-id="c19ec-107">Por convención, se llamará el índice y la restricción que se introducen para una clave alternativa `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="c19ec-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="c19ec-108">Para las claves compuestas de alternativas `<property name>` se convierte en una lista de subrayado separado de los nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="c19ec-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c19ec-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c19ec-109">Data Annotations</span></span>

<span data-ttu-id="c19ec-110">Restricciones UNIQUE no se pueden configurar mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="c19ec-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c19ec-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="c19ec-111">Fluent API</span></span>

<span data-ttu-id="c19ec-112">Puede usar la API fluida para configurar el nombre de índice y de restricción para una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="c19ec-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
