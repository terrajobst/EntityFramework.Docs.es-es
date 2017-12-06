---
title: Probar con SQLite - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="ad2ed-102">Pruebas de SQLite</span><span class="sxs-lookup"><span data-stu-id="ad2ed-102">Testing with SQLite</span></span>

<span data-ttu-id="ad2ed-103">SQLite tiene un modo en memoria que permite utilizar código para escribir pruebas con una base de datos relacional, sin la sobrecarga de operaciones de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="ad2ed-104">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) en GitHub</span><span class="sxs-lookup"><span data-stu-id="ad2ed-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="ad2ed-105">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ad2ed-105">Example testing scenario</span></span>

<span data-ttu-id="ad2ed-106">Tenga en cuenta el siguiente servicio que permite que el código de aplicación para realizar algunas operaciones relacionadas con los blogs.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="ad2ed-107">Usa internamente un `DbContext` que se conecta a una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="ad2ed-108">Podría ser útil intercambiar este contexto para conectarse a una base de datos de SQLite en memoria, por lo que podemos escribir pruebas eficaz para este servicio sin tener que modificar el código, o hacer una gran cantidad de trabajo que se va a crear una prueba de doble del contexto.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="ad2ed-109">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="ad2ed-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="ad2ed-110">Evite configurar dos proveedores de base de datos</span><span class="sxs-lookup"><span data-stu-id="ad2ed-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="ad2ed-111">En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="ad2ed-112">Si va a configurar un proveedor de base de datos mediante la invalidación `OnConfiguring` en el contexto, a continuación, debe agregar código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="ad2ed-113">Si utiliza ASP.NET Core, a continuación, no deberían necesitar este código desde el proveedor de base de datos se configuró fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="ad2ed-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="ad2ed-114">Agregue un constructor para pruebas</span><span class="sxs-lookup"><span data-stu-id="ad2ed-114">Add a constructor for testing</span></span>

<span data-ttu-id="ad2ed-115">La manera más sencilla de habilitar las pruebas en otra base de datos consiste en modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="ad2ed-116">`DbContextOptions<TContext>`indica el contexto de todos sus valores, como qué base de datos al que conectarse.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="ad2ed-117">Este es el mismo objeto que se crea mediante la ejecución del método OnConfiguring en el contexto.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="ad2ed-118">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="ad2ed-118">Writing tests</span></span>

<span data-ttu-id="ad2ed-119">La clave que se va a realizar las pruebas con este proveedor es la capacidad para indicar el contexto para utilizar código y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="ad2ed-120">El ámbito de la base de datos se controla mediante la apertura y cierre la conexión.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="ad2ed-121">La base de datos se limita a la duración de la conexión está abierta.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="ad2ed-122">Normalmente, desea una base de datos limpia para cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="ad2ed-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
