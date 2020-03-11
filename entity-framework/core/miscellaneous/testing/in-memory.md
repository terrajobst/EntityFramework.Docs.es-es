---
title: Pruebas con inmemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414030"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="821c8-102">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="821c8-102">Testing with InMemory</span></span>

<span data-ttu-id="821c8-103">El proveedor de inmemory es útil cuando se desea probar los componentes mediante algo que se aproxima a la conexión a la base de datos real, sin la sobrecarga de las operaciones de base de datos reales.</span><span class="sxs-lookup"><span data-stu-id="821c8-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="821c8-104">Puede ver un [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="821c8-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="821c8-105">La inmemory no es una base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="821c8-105">InMemory is not a relational database</span></span>

<span data-ttu-id="821c8-106">EF Core proveedores de bases de datos no tienen que ser bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="821c8-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="821c8-107">La inmemory está diseñada para ser una base de datos de uso general para las pruebas y no está diseñada para imitar una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="821c8-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="821c8-108">Algunos ejemplos de esto son:</span><span class="sxs-lookup"><span data-stu-id="821c8-108">Some examples of this include:</span></span>

* <span data-ttu-id="821c8-109">La inmemory le permitirá guardar datos que infrinjan las restricciones de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="821c8-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="821c8-110">Si usa DefaultValueSql (String) para una propiedad en el modelo, se trata de una API de base de datos relacional y no tendrá ningún efecto cuando se ejecute en inmemory.</span><span class="sxs-lookup"><span data-stu-id="821c8-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="821c8-111">No se admite la [simultaneidad mediante la versión de marca](xref:core/modeling/concurrency#timestamprowversion) de tiempo o fila (`[Timestamp]` o `IsRowVersion`).</span><span class="sxs-lookup"><span data-stu-id="821c8-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="821c8-112">No se producirá [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) si se realiza una actualización mediante un token de simultaneidad antiguo.</span><span class="sxs-lookup"><span data-stu-id="821c8-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="821c8-113">Para muchos propósitos de prueba, estas diferencias no serán importantes.</span><span class="sxs-lookup"><span data-stu-id="821c8-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="821c8-114">Sin embargo, si desea probar algo que se comporta más como una verdadera base de datos relacional, considere el uso [del modo en memoria de SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="821c8-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="821c8-115">Escenario de prueba de ejemplo</span><span class="sxs-lookup"><span data-stu-id="821c8-115">Example testing scenario</span></span>

<span data-ttu-id="821c8-116">Considere el siguiente servicio que permite al código de la aplicación realizar algunas operaciones relacionadas con blogs.</span><span class="sxs-lookup"><span data-stu-id="821c8-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="821c8-117">Utiliza internamente un `DbContext` que se conecta a una base de datos SQL Server.</span><span class="sxs-lookup"><span data-stu-id="821c8-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="821c8-118">Sería útil intercambiar este contexto para conectarse a una base de datos inmemory de modo que podamos escribir pruebas eficaces para este servicio sin tener que modificar el código, o hacer mucho trabajo para crear un doble de prueba del contexto.</span><span class="sxs-lookup"><span data-stu-id="821c8-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="821c8-119">Preparar el contexto</span><span class="sxs-lookup"><span data-stu-id="821c8-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="821c8-120">Evite configurar dos proveedores de bases de datos</span><span class="sxs-lookup"><span data-stu-id="821c8-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="821c8-121">En las pruebas, va a configurar externamente el contexto para usar el proveedor de inmemory.</span><span class="sxs-lookup"><span data-stu-id="821c8-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="821c8-122">Si está configurando un proveedor de base de datos invalidando `OnConfiguring` en el contexto, deberá agregar código condicional para asegurarse de que solo se configura el proveedor de base de datos si aún no se ha configurado ninguno.</span><span class="sxs-lookup"><span data-stu-id="821c8-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="821c8-123">Si usa ASP.NET Core, no necesitará este código ya que el proveedor de base de datos ya está configurado fuera del contexto (en Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="821c8-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="821c8-124">Agregar un constructor para las pruebas</span><span class="sxs-lookup"><span data-stu-id="821c8-124">Add a constructor for testing</span></span>

<span data-ttu-id="821c8-125">La manera más sencilla de habilitar las pruebas en una base de datos diferente es modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="821c8-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="821c8-126">`DbContextOptions<TContext>` indica al contexto toda su configuración, como la base de datos a la que se va a conectar.</span><span class="sxs-lookup"><span data-stu-id="821c8-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="821c8-127">Este es el mismo objeto que se genera al ejecutar el método de configuración en el contexto.</span><span class="sxs-lookup"><span data-stu-id="821c8-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="821c8-128">Escribir pruebas</span><span class="sxs-lookup"><span data-stu-id="821c8-128">Writing tests</span></span>

<span data-ttu-id="821c8-129">La clave para probar con este proveedor es la capacidad de indicar al contexto que use el proveedor de inmemory y controlar el ámbito de la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="821c8-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="821c8-130">Normalmente, desea una base de datos limpia para cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="821c8-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="821c8-131">Este es un ejemplo de una clase de prueba que usa la base de datos inmemory.</span><span class="sxs-lookup"><span data-stu-id="821c8-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="821c8-132">Cada método de prueba especifica un nombre de base de datos único, lo que significa que cada método tiene su propia base de datos inmemory.</span><span class="sxs-lookup"><span data-stu-id="821c8-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="821c8-133">Para usar el método de extensión `.UseInMemoryDatabase()`, haga referencia al paquete NuGet [Microsoft. EntityFrameworkCore. inmemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="821c8-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
