---
title: EF Core de esquema predeterminados
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655965"
---
# <a name="default-schema"></a><span data-ttu-id="05453-102">Esquema predeterminado</span><span class="sxs-lookup"><span data-stu-id="05453-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="05453-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="05453-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="05453-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="05453-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="05453-105">El esquema predeterminado es el esquema de la base de datos en el que se crearán los objetos si no se configura explícitamente un esquema para ese objeto.</span><span class="sxs-lookup"><span data-stu-id="05453-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="05453-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="05453-106">Conventions</span></span>

<span data-ttu-id="05453-107">Por Convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado.</span><span class="sxs-lookup"><span data-stu-id="05453-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="05453-108">Por ejemplo, Microsoft SQL Server usará el esquema de `dbo` y SQLite no usará un esquema (dado que los esquemas no se admiten en SQLite).</span><span class="sxs-lookup"><span data-stu-id="05453-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="05453-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="05453-109">Data Annotations</span></span>

<span data-ttu-id="05453-110">No se puede establecer el esquema predeterminado con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="05453-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="05453-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="05453-111">Fluent API</span></span>

<span data-ttu-id="05453-112">Puede usar la API fluida para especificar un esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="05453-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
