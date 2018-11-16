---
title: Crear y quitar API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688634"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="fe861-102">Crear y quitar API</span><span class="sxs-lookup"><span data-stu-id="fe861-102">Create and Drop APIs</span></span>

<span data-ttu-id="fe861-103">Los métodos EnsureDeleted y de EnsureCreated proporcionan una alternativa ligera a [migraciones](migrations/index.md) para administrar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fe861-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="fe861-104">Estos métodos son útiles en escenarios cuando los datos es transitorios y se pueden quitar cuando cambia el esquema.</span><span class="sxs-lookup"><span data-stu-id="fe861-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="fe861-105">Por ejemplo, durante la creación de prototipos, pruebas o en las memorias caché locales.</span><span class="sxs-lookup"><span data-stu-id="fe861-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="fe861-106">Algunos proveedores (especialmente los que no son relacionales) no admiten las migraciones.</span><span class="sxs-lookup"><span data-stu-id="fe861-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="fe861-107">Para estos proveedores, EnsureCreated suele ser la manera más fácil para inicializar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fe861-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="fe861-108">Migraciones y EnsureCreated no funcionan bien juntos.</span><span class="sxs-lookup"><span data-stu-id="fe861-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="fe861-109">Si usa las migraciones, no use EnsureCreated para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="fe861-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="fe861-110">Transición de EnsureCreated a las migraciones no es una experiencia sin problemas.</span><span class="sxs-lookup"><span data-stu-id="fe861-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="fe861-111">La manera más sencilla de hacerlo es quitar la base de datos y volver a crearla mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="fe861-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="fe861-112">Si prevé que usará las migraciones en el futuro, es mejor comenzar con las migraciones en lugar de usar EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="fe861-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="fe861-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="fe861-113">EnsureDeleted</span></span>

<span data-ttu-id="fe861-114">El método EnsureDeleted quitará la base de datos si existe.</span><span class="sxs-lookup"><span data-stu-id="fe861-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="fe861-115">Si no tiene los permisos adecuados, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="fe861-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="fe861-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="fe861-116">EnsureCreated</span></span>

<span data-ttu-id="fe861-117">EnsureCreated creará la base de datos si no existe e inicializar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fe861-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="fe861-118">Si existen cualquier tablas (incluidas las tablas de otra clase DbContext), el esquema no inicializarse.</span><span class="sxs-lookup"><span data-stu-id="fe861-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="fe861-119">Versiones asincrónicas de estos métodos también están disponibles.</span><span class="sxs-lookup"><span data-stu-id="fe861-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="fe861-120">Secuencia de comandos SQL</span><span class="sxs-lookup"><span data-stu-id="fe861-120">SQL Script</span></span>

<span data-ttu-id="fe861-121">Para obtener el SQL que usa EnsureCreated, puede usar el método GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="fe861-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="fe861-122">Varias clases DbContext</span><span class="sxs-lookup"><span data-stu-id="fe861-122">Multiple DbContext classes</span></span>

<span data-ttu-id="fe861-123">EnsureCreated solo funciona cuando no hay tablas están presentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fe861-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="fe861-124">Si es necesario, puede escribir su propia comprobación para ver si se debe inicializar el esquema y el servicio IRelationalDatabaseCreator subyacente para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="fe861-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
