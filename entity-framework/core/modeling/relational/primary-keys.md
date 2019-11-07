---
title: 'Claves principales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656049"
---
# <a name="primary-keys"></a><span data-ttu-id="16931-102">Claves principales</span><span class="sxs-lookup"><span data-stu-id="16931-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="16931-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="16931-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="16931-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="16931-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="16931-105">Se introduce una restricción primary key para la clave de cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="16931-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="16931-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="16931-106">Conventions</span></span>

<span data-ttu-id="16931-107">Por Convención, la clave principal de la base de datos se denominará `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="16931-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="16931-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="16931-108">Data Annotations</span></span>

<span data-ttu-id="16931-109">No se puede configurar ningún aspecto específico de la base de datos relacional con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="16931-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="16931-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="16931-110">Fluent API</span></span>

<span data-ttu-id="16931-111">Puede usar la API fluida para configurar el nombre de la restricción primary key en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16931-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
