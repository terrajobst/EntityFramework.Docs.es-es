---
title: Probar con InMemory - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="13b4c-102">Probar con InMemory</span><span class="sxs-lookup"><span data-stu-id="13b4c-102">Testing with InMemory</span></span>

<span data-ttu-id="13b4c-103">El proveedor de InMemory es útil cuando desea probar los componentes con algo que se aproxima al conectarse a la base de datos real, sin la sobrecarga de operaciones de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="13b4c-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="13b4c-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="13b4c-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="13b4c-105">InMemory no es una base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="13b4c-105">InMemory is not a relational database</span></span>

<span data-ttu-id="13b4c-106">Proveedores de base de datos principales EF no tiene que ser bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="13b4c-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="13b4c-107">InMemory está diseñado para ser una base de datos de uso general para realizar pruebas y no está diseñado para imitar una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="13b4c-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="13b4c-108">Algunos ejemplos de este comportamiento son:</span><span class="sxs-lookup"><span data-stu-id="13b4c-108">Some examples of this include:</span></span>
* <span data-ttu-id="13b4c-109">InMemory le permitirá guardar los datos que infringen las restricciones de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="13b4c-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="13b4c-110">Si usa DefaultValueSql(string) para una propiedad en el modelo, esto es una API de base de datos relacional y no tiene ningún efecto cuando se ejecuta en InMemory.</span><span class="sxs-lookup"><span data-stu-id="13b4c-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="13b4c-111">Para fines de prueba no se importan estas diferencias.</span><span class="sxs-lookup"><span data-stu-id="13b4c-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="13b4c-112">Sin embargo, si desea probar con algo que se comporta más como una base de datos relacional es true, a continuación, considere el uso de [modo in-memory de SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="13b4c-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="13b4c-113">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="13b4c-113">Example testing scenario</span></span>

<span data-ttu-id="13b4c-114">Tenga en cuenta el siguiente servicio que permite que el código de aplicación para realizar algunas operaciones relacionadas con los blogs.</span><span class="sxs-lookup"><span data-stu-id="13b4c-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="13b4c-115">Usa internamente un `DbContext` que se conecta a una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="13b4c-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="13b4c-116">Podría ser útil intercambiar este contexto para conectarse a una base de datos InMemory, por lo que podemos escribir pruebas eficaz para este servicio sin tener que modificar el código, o hacer una gran cantidad de trabajo que se va a crear una prueba de doble del contexto.</span><span class="sxs-lookup"><span data-stu-id="13b4c-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="13b4c-117">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="13b4c-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="13b4c-118">Evite configurar dos proveedores de base de datos</span><span class="sxs-lookup"><span data-stu-id="13b4c-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="13b4c-119">En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory.</span><span class="sxs-lookup"><span data-stu-id="13b4c-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="13b4c-120">Si va a configurar un proveedor de base de datos mediante la invalidación `OnConfiguring` en el contexto, a continuación, debe agregar código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.</span><span class="sxs-lookup"><span data-stu-id="13b4c-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="13b4c-121">Si utiliza ASP.NET Core, a continuación, no deberían necesitar este código desde el proveedor de base de datos ya se configuró fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="13b4c-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="13b4c-122">Agregue un constructor para pruebas</span><span class="sxs-lookup"><span data-stu-id="13b4c-122">Add a constructor for testing</span></span>

<span data-ttu-id="13b4c-123">La manera más sencilla de habilitar las pruebas en otra base de datos consiste en modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="13b4c-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="13b4c-124">`DbContextOptions<TContext>`indica el contexto de todos sus valores, como qué base de datos al que conectarse.</span><span class="sxs-lookup"><span data-stu-id="13b4c-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="13b4c-125">Este es el mismo objeto que se crea mediante la ejecución del método OnConfiguring en el contexto.</span><span class="sxs-lookup"><span data-stu-id="13b4c-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="13b4c-126">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="13b4c-126">Writing tests</span></span>

<span data-ttu-id="13b4c-127">La clave que se va a realizar las pruebas con este proveedor es la capacidad para indicar el contexto para usar el proveedor InMemory y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="13b4c-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="13b4c-128">Normalmente, desea una base de datos limpia para cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="13b4c-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="13b4c-129">Este es un ejemplo de una clase de prueba que usa la base de datos InMemory.</span><span class="sxs-lookup"><span data-stu-id="13b4c-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="13b4c-130">Cada método de prueba especifica un nombre único de la base de datos, lo que significa que cada método tiene su propia base de datos InMemory.</span><span class="sxs-lookup"><span data-stu-id="13b4c-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="13b4c-131">Para usar el `.UseInMemoryDatabase()` método de extensión, el paquete NuGet de referencia `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="13b4c-131">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
