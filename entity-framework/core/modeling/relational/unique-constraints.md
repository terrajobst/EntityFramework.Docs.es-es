---
title: Claves alternativas (restricciones únicas) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994196"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="9f35a-102">Claves alternativas (restricciones únicas)</span><span class="sxs-lookup"><span data-stu-id="9f35a-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="9f35a-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="9f35a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9f35a-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="9f35a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9f35a-105">Se introdujo una restricción única para cada clave alternativa en el modelo.</span><span class="sxs-lookup"><span data-stu-id="9f35a-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="9f35a-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="9f35a-106">Conventions</span></span>

<span data-ttu-id="9f35a-107">Por convención, se denominará el índice y la restricción que se presentan para una clave alternativa `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="9f35a-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="9f35a-108">Para las claves alternativas compuestas `<property name>` se convierte en una lista separada por un carácter de subrayado de los nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f35a-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9f35a-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="9f35a-109">Data Annotations</span></span>

<span data-ttu-id="9f35a-110">Restricciones UNIQUE no se pueden configurar mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="9f35a-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9f35a-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="9f35a-111">Fluent API</span></span>

<span data-ttu-id="9f35a-112">Puede usar la API Fluent para configurar el nombre de índice y de restricción para una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="9f35a-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
