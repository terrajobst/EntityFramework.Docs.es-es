---
title: Pruebas con SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414636"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="ea02a-102">Pruebas con SQLite</span><span class="sxs-lookup"><span data-stu-id="ea02a-102">Testing with SQLite</span></span>

<span data-ttu-id="ea02a-103">SQLite tiene un modo en memoria que le permite usar SQLite para escribir pruebas en una base de datos relacional, sin la sobrecarga de las operaciones de base de datos reales.</span><span class="sxs-lookup"><span data-stu-id="ea02a-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="ea02a-104">Puede ver el [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en github</span><span class="sxs-lookup"><span data-stu-id="ea02a-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="ea02a-105">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ea02a-105">Example testing scenario</span></span>

<span data-ttu-id="ea02a-106">Considere el siguiente servicio que permite al código de la aplicación realizar algunas operaciones relacionadas con blogs.</span><span class="sxs-lookup"><span data-stu-id="ea02a-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="ea02a-107">Utiliza internamente un `DbContext` que se conecta a una base de datos SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ea02a-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="ea02a-108">Sería útil intercambiar este contexto para conectarse a una base de datos de SQLite en memoria, de modo que podamos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer mucho trabajo para crear un doble de prueba del contexto.</span><span class="sxs-lookup"><span data-stu-id="ea02a-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="ea02a-109">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="ea02a-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="ea02a-110">Evite configurar dos proveedores de bases de datos</span><span class="sxs-lookup"><span data-stu-id="ea02a-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="ea02a-111">En las pruebas, va a configurar externamente el contexto para usar el proveedor de inmemory.</span><span class="sxs-lookup"><span data-stu-id="ea02a-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="ea02a-112">Si está configurando un proveedor de base de datos invalidando `OnConfiguring` en el contexto, deberá agregar código condicional para asegurarse de que solo se configura el proveedor de base de datos si aún no se ha configurado ninguno.</span><span class="sxs-lookup"><span data-stu-id="ea02a-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="ea02a-113">Si usa ASP.NET Core, no necesitará este código, ya que el proveedor de la base de datos se configura fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="ea02a-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="ea02a-114">Agregar un constructor para las pruebas</span><span class="sxs-lookup"><span data-stu-id="ea02a-114">Add a constructor for testing</span></span>

<span data-ttu-id="ea02a-115">La manera más sencilla de habilitar las pruebas en una base de datos diferente es modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ea02a-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="ea02a-116">`DbContextOptions<TContext>` indica al contexto toda su configuración, como la base de datos a la que se va a conectar.</span><span class="sxs-lookup"><span data-stu-id="ea02a-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="ea02a-117">Este es el mismo objeto que se genera al ejecutar el método de configuración en el contexto.</span><span class="sxs-lookup"><span data-stu-id="ea02a-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="ea02a-118">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="ea02a-118">Writing tests</span></span>

<span data-ttu-id="ea02a-119">La clave para realizar pruebas con este proveedor es la posibilidad de indicar al contexto que use SQLite y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="ea02a-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="ea02a-120">El ámbito de la base de datos se controla mediante la apertura y el cierre de la conexión.</span><span class="sxs-lookup"><span data-stu-id="ea02a-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="ea02a-121">La base de datos está en el ámbito de la duración de la conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="ea02a-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="ea02a-122">Normalmente, desea una base de datos limpia para cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="ea02a-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="ea02a-123">Para usar `SqliteConnection()` y el método de extensión `.UseSqlite()`, haga referencia al paquete NuGet [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="ea02a-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
