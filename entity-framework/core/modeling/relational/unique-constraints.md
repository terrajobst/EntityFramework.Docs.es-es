---
title: Claves alternativas (restricciones únicas)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197614"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="d2333-102">Claves alternativas (restricciones UNIQUE)</span><span class="sxs-lookup"><span data-stu-id="d2333-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="d2333-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="d2333-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d2333-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="d2333-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d2333-105">Se introduce una restricción UNIQUE para cada clave alternativa del modelo.</span><span class="sxs-lookup"><span data-stu-id="d2333-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="d2333-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="d2333-106">Conventions</span></span>

<span data-ttu-id="d2333-107">Por Convención, el índice y la restricción que se introducen para una clave alternativa se `AK_<type name>_<property name>`denominarán.</span><span class="sxs-lookup"><span data-stu-id="d2333-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="d2333-108">Para las claves `<property name>` alternativas compuestas se convierte en una lista de nombres de propiedad separados por guiones bajos.</span><span class="sxs-lookup"><span data-stu-id="d2333-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d2333-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="d2333-109">Data Annotations</span></span>

<span data-ttu-id="d2333-110">Las restricciones UNIQUE no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d2333-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d2333-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="d2333-111">Fluent API</span></span>

<span data-ttu-id="d2333-112">Puede usar la API fluida para configurar el índice y el nombre de la restricción para una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="d2333-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
