---
title: Pruebas con InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900267"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="7be91-102">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="7be91-102">Testing with InMemory</span></span>

<span data-ttu-id="7be91-103">El proveedor InMemory es útil cuando desea probar componentes con algo que se aproxima al conectarse a la base de datos real, sin la sobrecarga de operaciones de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="7be91-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="7be91-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="7be91-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="7be91-105">InMemory no es una base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="7be91-105">InMemory is not a relational database</span></span>

<span data-ttu-id="7be91-106">Proveedores de base de datos EF Core no tiene que ser bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="7be91-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="7be91-107">InMemory está diseñado para ser una base de datos de uso general para las pruebas y no está diseñado para imitar una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="7be91-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="7be91-108">Algunos ejemplos de esto son:</span><span class="sxs-lookup"><span data-stu-id="7be91-108">Some examples of this include:</span></span>

* <span data-ttu-id="7be91-109">InMemory, podrá guardar los datos que infringen las restricciones de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="7be91-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="7be91-110">Si usa DefaultValueSql(string) para una propiedad en el modelo, esto es una API de base de datos relacional y no tendrá efecto cuando se ejecuta en InMemory.</span><span class="sxs-lookup"><span data-stu-id="7be91-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="7be91-111">[Simultaneidad a través de la versión de fila o marca de tiempo](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` o `IsRowVersion`) no se admite.</span><span class="sxs-lookup"><span data-stu-id="7be91-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="7be91-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) se producirá si una actualización se realiza mediante un token de simultaneidad antiguo.</span><span class="sxs-lookup"><span data-stu-id="7be91-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="7be91-113">Para muchos fines de prueba no se importan estas diferencias.</span><span class="sxs-lookup"><span data-stu-id="7be91-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="7be91-114">Sin embargo, si desea probarlo con algo que se comporta más como una base de datos relacional es true, considere la posibilidad de usar [modo en memoria de SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="7be91-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="7be91-115">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7be91-115">Example testing scenario</span></span>

<span data-ttu-id="7be91-116">Considere el siguiente servicio que permite que el código de aplicación realizar algunas operaciones relacionadas con blogs.</span><span class="sxs-lookup"><span data-stu-id="7be91-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="7be91-117">Lo utiliza internamente un `DbContext` que se conecta a una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7be91-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="7be91-118">Sería útil cambiar este contexto para conectarse a una base de datos InMemory, por lo que podemos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer una gran cantidad de trabajo para crear una prueba doble del contexto.</span><span class="sxs-lookup"><span data-stu-id="7be91-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="7be91-119">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="7be91-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="7be91-120">Evite configurar dos proveedores de base de datos</span><span class="sxs-lookup"><span data-stu-id="7be91-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="7be91-121">En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory.</span><span class="sxs-lookup"><span data-stu-id="7be91-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="7be91-122">Si va a configurar un proveedor de base de datos invalidando `OnConfiguring` en su contexto, a continuación, deberá agregar algún código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.</span><span class="sxs-lookup"><span data-stu-id="7be91-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="7be91-123">Si usa ASP.NET Core, a continuación, no deberían necesitar este código puesto que ya esté configurado el proveedor de base de datos fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="7be91-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="7be91-124">Agregue un constructor para las pruebas</span><span class="sxs-lookup"><span data-stu-id="7be91-124">Add a constructor for testing</span></span>

<span data-ttu-id="7be91-125">Es la manera más sencilla de habilitar las pruebas con otra base de datos modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7be91-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="7be91-126">`DbContextOptions<TContext>` indica el contexto de todos sus valores, por ejemplo, para conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7be91-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="7be91-127">Este es el mismo objeto que se compila mediante la ejecución del método OnConfiguring en su contexto.</span><span class="sxs-lookup"><span data-stu-id="7be91-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="7be91-128">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="7be91-128">Writing tests</span></span>

<span data-ttu-id="7be91-129">La clave para las pruebas con este proveedor es la capacidad de decirle el contexto para usar el proveedor InMemory y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="7be91-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="7be91-130">Normalmente desea limpiar una base de datos de cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="7be91-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="7be91-131">Este es un ejemplo de una clase de prueba que usa la base de datos InMemory.</span><span class="sxs-lookup"><span data-stu-id="7be91-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="7be91-132">Cada método de prueba especifica un nombre de base de datos único, lo que significa que cada método tiene su propia base de datos InMemory.</span><span class="sxs-lookup"><span data-stu-id="7be91-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="7be91-133">Para usar el `.UseInMemoryDatabase()` método de extensión, el paquete NuGet de referencia `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="7be91-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
