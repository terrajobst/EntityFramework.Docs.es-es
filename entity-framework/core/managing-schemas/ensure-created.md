---
title: Crear y quitar API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285644"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="47f38-102">Crear y quitar API</span><span class="sxs-lookup"><span data-stu-id="47f38-102">Create and Drop APIs</span></span>

<span data-ttu-id="47f38-103">Los métodos EnsureDeleted y de EnsureCreated proporcionan una alternativa ligera a [migraciones](migrations/index.md) para administrar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f38-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="47f38-104">Esto es útil en escenarios cuando los datos es transitorios y se pueden quitar cuando cambia el esquema.</span><span class="sxs-lookup"><span data-stu-id="47f38-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="47f38-105">Por ejemplo, durante la creación de prototipos, pruebas o en las memorias caché locales.</span><span class="sxs-lookup"><span data-stu-id="47f38-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="47f38-106">Algunos proveedores (especialmente los que no son relacionales) no admiten las migraciones.</span><span class="sxs-lookup"><span data-stu-id="47f38-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="47f38-107">En estos casos, EnsureCreated suele ser la manera más fácil para inicializar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f38-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="47f38-108">Migraciones y EnsureCreated no funcionan bien juntos.</span><span class="sxs-lookup"><span data-stu-id="47f38-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="47f38-109">Si usa las migraciones, no use EnsureCreated para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="47f38-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="47f38-110">Transición de EnsureCreated a las migraciones no es una experiencia sin problemas.</span><span class="sxs-lookup"><span data-stu-id="47f38-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="47f38-111">La simpelest para lograr esto consiste en colocar la base de datos y volver a crearla mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="47f38-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="47f38-112">Si prevé que usará las migraciones en el futuro, es mejor comenzar con las migraciones en lugar de usar EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="47f38-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="47f38-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="47f38-113">EnsureDeleted</span></span>

<span data-ttu-id="47f38-114">El método EnsureDeleted quitará la base de datos si existe.</span><span class="sxs-lookup"><span data-stu-id="47f38-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="47f38-115">Si no tiene los permisos adecuados, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="47f38-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="47f38-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="47f38-116">EnsureCreated</span></span>

<span data-ttu-id="47f38-117">EnsureCreated creará la base de datos si no existe e inicializar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f38-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="47f38-118">Si existen cualquier tablas (incluidas las tablas de otra clase DbContext), el esquema no inicializarse.</span><span class="sxs-lookup"><span data-stu-id="47f38-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="47f38-119">Versiones asincrónicas de estos métodos también están disponibles.</span><span class="sxs-lookup"><span data-stu-id="47f38-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="47f38-120">Secuencia de comandos SQL</span><span class="sxs-lookup"><span data-stu-id="47f38-120">SQL Script</span></span>

<span data-ttu-id="47f38-121">Para obtener el SQL que usa EnsureCreated, puede usar el método GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="47f38-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="47f38-122">Varias clases DbContext</span><span class="sxs-lookup"><span data-stu-id="47f38-122">Multiple DbContext classes</span></span>

<span data-ttu-id="47f38-123">EnsureCreated solo funciona cuando no hay tablas están presentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f38-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="47f38-124">Si es necesario, puede escribir su propia comprobación para ver si se debe inicializar el esquema y el servicio IRelationalDatabaseCreator subyacente para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="47f38-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
