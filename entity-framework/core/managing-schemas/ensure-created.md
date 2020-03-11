---
title: 'Crear y quitar API: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414324"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="5ad37-102">Crear y quitar API</span><span class="sxs-lookup"><span data-stu-id="5ad37-102">Create and Drop APIs</span></span>

<span data-ttu-id="5ad37-103">Los métodos EnsureCreated y EnsureDeleted proporcionan una alternativa ligera a las [migraciones](migrations/index.md) para administrar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="5ad37-104">Estos métodos son útiles en escenarios en los que los datos son transitorios y se pueden quitar cuando cambia el esquema.</span><span class="sxs-lookup"><span data-stu-id="5ad37-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="5ad37-105">Por ejemplo, durante el prototipo, en las pruebas o en las memorias caché locales.</span><span class="sxs-lookup"><span data-stu-id="5ad37-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="5ad37-106">Algunos proveedores (especialmente los no relacionales) no admiten las migraciones.</span><span class="sxs-lookup"><span data-stu-id="5ad37-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="5ad37-107">Para estos proveedores, EnsureCreated suele ser la manera más fácil de inicializar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="5ad37-108">EnsureCreated y las migraciones no funcionan bien juntos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="5ad37-109">Si utiliza migraciones, no use EnsureCreated para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="5ad37-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="5ad37-110">La transición de EnsureCreated a migraciones no es una experiencia sin problemas.</span><span class="sxs-lookup"><span data-stu-id="5ad37-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="5ad37-111">La manera más sencilla de hacerlo es quitar la base de datos y volver a crearla con las migraciones.</span><span class="sxs-lookup"><span data-stu-id="5ad37-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="5ad37-112">Si prevé usar migraciones en el futuro, es mejor empezar con las migraciones en lugar de usar EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="5ad37-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="5ad37-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="5ad37-113">EnsureDeleted</span></span>

<span data-ttu-id="5ad37-114">El método EnsureDeleted quitará la base de datos si existe.</span><span class="sxs-lookup"><span data-stu-id="5ad37-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="5ad37-115">Si no tiene los permisos adecuados, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="5ad37-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="5ad37-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="5ad37-116">EnsureCreated</span></span>

<span data-ttu-id="5ad37-117">EnsureCreated creará la base de datos si no existe e inicializará el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="5ad37-118">Si existe alguna tabla (incluidas las tablas de otra clase DbContext), el esquema no se inicializará.</span><span class="sxs-lookup"><span data-stu-id="5ad37-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="5ad37-119">También hay disponibles versiones asincrónicas de estos métodos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="5ad37-120">Secuencia de comandos de SQL</span><span class="sxs-lookup"><span data-stu-id="5ad37-120">SQL Script</span></span>

<span data-ttu-id="5ad37-121">Para obtener el SQL que usa EnsureCreated, puede utilizar el método GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="5ad37-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="5ad37-122">Varias clases DbContext</span><span class="sxs-lookup"><span data-stu-id="5ad37-122">Multiple DbContext classes</span></span>

<span data-ttu-id="5ad37-123">EnsureCreated solo funciona cuando no hay ninguna tabla presente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ad37-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="5ad37-124">Si es necesario, puede escribir su propia comprobación para ver si es necesario inicializar el esquema y usar el servicio IRelationalDatabaseCreator subyacente para inicializar el esquema.</span><span class="sxs-lookup"><span data-stu-id="5ad37-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
