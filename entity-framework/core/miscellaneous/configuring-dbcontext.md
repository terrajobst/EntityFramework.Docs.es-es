---
title: Configurar un DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="1b67c-102">Configurar un DbContext</span><span class="sxs-lookup"><span data-stu-id="1b67c-102">Configuring a DbContext</span></span>

<span data-ttu-id="1b67c-103">En este artículo se muestra patrones para configurar un `DbContext` con `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="1b67c-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="1b67c-104">Opciones se utilizan principalmente para seleccionar y configurar el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="1b67c-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="1b67c-105">Configurar DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="1b67c-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="1b67c-106">`DbContext`debe tener una instancia de `DbContextOptions` para ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="1b67c-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="1b67c-107">Esto se puede configurar mediante la invalidación `OnConfiguring`, o externamente proporcionado a través de un argumento de constructor.</span><span class="sxs-lookup"><span data-stu-id="1b67c-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="1b67c-108">Si se utilizan ambos, `OnConfiguring` se ejecuta en las opciones proporcionadas, lo que significa se suma y puede sobrescribir opciones proporcionadas para el argumento del constructor.</span><span class="sxs-lookup"><span data-stu-id="1b67c-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="1b67c-109">Argumentos de constructor</span><span class="sxs-lookup"><span data-stu-id="1b67c-109">Constructor argument</span></span>

<span data-ttu-id="1b67c-110">Código del contexto con el constructor</span><span class="sxs-lookup"><span data-stu-id="1b67c-110">Context code with constructor</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="1b67c-111">El constructor base de DbContext acepta también la versión no genérica de `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="1b67c-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="1b67c-112">No se recomienda usar la versión no genérica para aplicaciones con varios tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="1b67c-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="1b67c-113">Código de aplicación para inicializar desde el argumento del constructor</span><span class="sxs-lookup"><span data-stu-id="1b67c-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="1b67c-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="1b67c-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="1b67c-115">`OnConfiguring`se produce una último y pueden sobrescribir opciones obtenidos desde el constructor o DI.</span><span class="sxs-lookup"><span data-stu-id="1b67c-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="1b67c-116">Este enfoque no se presta a las pruebas (a menos que el destino es la base de datos completa).</span><span class="sxs-lookup"><span data-stu-id="1b67c-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="1b67c-117">Código del contexto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1b67c-117">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="1b67c-118">Código de la aplicación para inicializar con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1b67c-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="1b67c-119">Uso de DbContext con inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="1b67c-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="1b67c-120">EF admite el uso de `DbContext` con un contenedor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="1b67c-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="1b67c-121">El tipo de DbContext se puede agregar al contenedor de servicios mediante `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1b67c-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="1b67c-122">`AddDbContext`hará que tanto el tipo de DbContext, `TContext`, y `DbContextOptions<TContext>` disponibles para la inyección del contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="1b67c-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="1b67c-123">Vea [leer más](#more-reading) a continuación para obtener información sobre la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="1b67c-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="1b67c-124">Agregar dbcontext a la inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="1b67c-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="1b67c-125">Esto es necesario agregar una [argumento del constructor](#constructor-argument) a su tipo de DbContext que acepta `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="1b67c-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="1b67c-126">Código del contexto:</span><span class="sxs-lookup"><span data-stu-id="1b67c-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="1b67c-127">Código de la aplicación (en ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="1b67c-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="1b67c-128">Código de la aplicación (mediante ServiceProvider directamente, menos común):</span><span class="sxs-lookup"><span data-stu-id="1b67c-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="1b67c-129">Uso de`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="1b67c-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="1b67c-130">Como alternativa a las opciones anteriores, también puede proporcionar una implementación de `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1b67c-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="1b67c-131">Herramientas EF pueden utilizar este generador para crear una instancia de su DbContext.</span><span class="sxs-lookup"><span data-stu-id="1b67c-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="1b67c-132">Esto puede ser necesario para permitir que las experiencias de tiempo de diseño específicas, como las migraciones.</span><span class="sxs-lookup"><span data-stu-id="1b67c-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="1b67c-133">Implemente esta interfaz para habilitar los servicios de tiempo de diseño para los tipos de contexto que no tiene un constructor predeterminado público.</span><span class="sxs-lookup"><span data-stu-id="1b67c-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="1b67c-134">Servicios en tiempo de diseño detecta automáticamente implementaciones de esta interfaz que se encuentran en el mismo ensamblado que el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="1b67c-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="1b67c-135">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1b67c-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="1b67c-136">Leer más</span><span class="sxs-lookup"><span data-stu-id="1b67c-136">More reading</span></span>

* <span data-ttu-id="1b67c-137">Lectura [Introducción a ASP.NET Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b67c-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="1b67c-138">Lectura [inyección de dependencia](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) para obtener información adicional acerca del uso DI.</span><span class="sxs-lookup"><span data-stu-id="1b67c-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="1b67c-139">Lectura [pruebas](testing/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="1b67c-139">Read [Testing](testing/index.md) for more information.</span></span>
