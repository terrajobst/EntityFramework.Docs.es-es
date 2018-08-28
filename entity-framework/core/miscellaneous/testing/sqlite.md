---
title: Pruebas con SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996873"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="379b7-102">Pruebas con SQLite</span><span class="sxs-lookup"><span data-stu-id="379b7-102">Testing with SQLite</span></span>

<span data-ttu-id="379b7-103">SQLite tiene un modo en memoria que le permite usar SQLite para escribir pruebas de una base de datos relacional, sin la sobrecarga de operaciones de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="379b7-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="379b7-104">Puede ver en este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) en GitHub</span><span class="sxs-lookup"><span data-stu-id="379b7-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="379b7-105">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="379b7-105">Example testing scenario</span></span>

<span data-ttu-id="379b7-106">Considere el siguiente servicio que permite que el código de aplicación realizar algunas operaciones relacionadas con blogs.</span><span class="sxs-lookup"><span data-stu-id="379b7-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="379b7-107">Lo utiliza internamente un `DbContext` que se conecta a una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="379b7-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="379b7-108">Sería útil cambiar este contexto para conectarse a una base de datos de SQLite en memoria, por lo que podemos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer una gran cantidad de trabajo para crear una prueba doble del contexto.</span><span class="sxs-lookup"><span data-stu-id="379b7-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="379b7-109">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="379b7-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="379b7-110">Evite configurar dos proveedores de base de datos</span><span class="sxs-lookup"><span data-stu-id="379b7-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="379b7-111">En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory.</span><span class="sxs-lookup"><span data-stu-id="379b7-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="379b7-112">Si va a configurar un proveedor de base de datos invalidando `OnConfiguring` en su contexto, a continuación, deberá agregar algún código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.</span><span class="sxs-lookup"><span data-stu-id="379b7-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="379b7-113">Si usa ASP.NET Core, a continuación, no deberían necesitar este código ya que esté configurado el proveedor de base de datos fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="379b7-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="379b7-114">Agregue un constructor para las pruebas</span><span class="sxs-lookup"><span data-stu-id="379b7-114">Add a constructor for testing</span></span>

<span data-ttu-id="379b7-115">Es la manera más sencilla de habilitar las pruebas con otra base de datos modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="379b7-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="379b7-116">`DbContextOptions<TContext>` indica el contexto de todos sus valores, por ejemplo, para conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="379b7-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="379b7-117">Este es el mismo objeto que se compila mediante la ejecución del método OnConfiguring en su contexto.</span><span class="sxs-lookup"><span data-stu-id="379b7-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="379b7-118">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="379b7-118">Writing tests</span></span>

<span data-ttu-id="379b7-119">La clave para las pruebas con este proveedor es la capacidad de decirle el contexto para usar SQLite y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="379b7-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="379b7-120">El ámbito de la base de datos se controla mediante la apertura y cierre la conexión.</span><span class="sxs-lookup"><span data-stu-id="379b7-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="379b7-121">La base de datos se limita a la duración de la conexión está abierta.</span><span class="sxs-lookup"><span data-stu-id="379b7-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="379b7-122">Normalmente desea limpiar una base de datos de cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="379b7-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
