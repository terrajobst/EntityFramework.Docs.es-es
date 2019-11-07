---
title: Novedades de EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656010"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="b27e9-102">Características nuevas en EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="b27e9-102">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="b27e9-103">Modelado</span><span class="sxs-lookup"><span data-stu-id="b27e9-103">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="b27e9-104">Asignación de campos</span><span class="sxs-lookup"><span data-stu-id="b27e9-104">Field mapping</span></span>

<span data-ttu-id="b27e9-105">Permite configurar un campo de respaldo para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="b27e9-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="b27e9-106">Puede resultar útil en las propiedades de solo lectura o en los datos que tienen métodos Get/Set en lugar de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="b27e9-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="b27e9-107">Asignación a tablas optimizadas para memoria en SQL Server</span><span class="sxs-lookup"><span data-stu-id="b27e9-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="b27e9-108">Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria.</span><span class="sxs-lookup"><span data-stu-id="b27e9-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="b27e9-109">Cuando use EF Core para crear y mantener una base de datos basada en el modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada para memoria para estas entidades.</span><span class="sxs-lookup"><span data-stu-id="b27e9-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="b27e9-110">Seguimiento de cambios</span><span class="sxs-lookup"><span data-stu-id="b27e9-110">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="b27e9-111">API adicionales de seguimiento de cambios de EF6</span><span class="sxs-lookup"><span data-stu-id="b27e9-111">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="b27e9-112">Como `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span><span class="sxs-lookup"><span data-stu-id="b27e9-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="b27e9-113">Consulta</span><span class="sxs-lookup"><span data-stu-id="b27e9-113">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="b27e9-114">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="b27e9-114">Explicit Loading</span></span>

<span data-ttu-id="b27e9-115">Permite desencadenar el rellenado de una propiedad de navegación o una entidad que se cargó anteriormente a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b27e9-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="b27e9-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="b27e9-116">DbSet.Find</span></span>

<span data-ttu-id="b27e9-117">Proporciona una manera sencilla de capturar una entidad en función de su valor de clave principal.</span><span class="sxs-lookup"><span data-stu-id="b27e9-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="b27e9-118">Otros</span><span class="sxs-lookup"><span data-stu-id="b27e9-118">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="b27e9-119">Resistencia de conexión</span><span class="sxs-lookup"><span data-stu-id="b27e9-119">Connection resiliency</span></span>

<span data-ttu-id="b27e9-120">Reintenta automáticamente los comandos de base de datos erróneos.</span><span class="sxs-lookup"><span data-stu-id="b27e9-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="b27e9-121">Esto resulta especialmente útil cuando se realizan conexiones a SQL Azure, donde los errores transitorios son comunes.</span><span class="sxs-lookup"><span data-stu-id="b27e9-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="b27e9-122">Reemplazo de servicio simplificado</span><span class="sxs-lookup"><span data-stu-id="b27e9-122">Simplified service replacement</span></span>

<span data-ttu-id="b27e9-123">Facilita el reemplazo de servicios internos que EF usa.</span><span class="sxs-lookup"><span data-stu-id="b27e9-123">Makes it easier to replace internal services that EF uses.</span></span>
