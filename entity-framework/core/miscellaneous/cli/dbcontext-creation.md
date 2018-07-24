---
title: Creación de DbContext en tiempo de diseño - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 648ca990252fb32d8cf181a7ae672d07a81f56bb
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201924"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="31d4a-102">Creación de DbContext en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="31d4a-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="31d4a-103">Algunos de los comandos de herramientas de EF Core (por ejemplo, el [migraciones] [ 1] comandos) requieren una derivada `DbContext` creación en tiempo de diseño con el fin de recopilar detalles acerca de la aplicación de instancia tipos de entidad y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="31d4a-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="31d4a-104">En la mayoría de los casos, es deseable que la `DbContext` creado, por tanto, se configura de forma similar a cómo sería [configurado en tiempo de ejecución][2].</span><span class="sxs-lookup"><span data-stu-id="31d4a-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="31d4a-105">Hay varias maneras de probar las herramientas para crear el `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="31d4a-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="31d4a-106">Servicios de aplicación</span><span class="sxs-lookup"><span data-stu-id="31d4a-106">From application services</span></span>
-------------------------
<span data-ttu-id="31d4a-107">Si su proyecto de inicio es una aplicación ASP.NET Core, las herramientas intentan obtener el objeto DbContext de proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="31d4a-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="31d4a-108">La herramienta primero intenta obtener el proveedor de servicios mediante la invocación `Program.BuildWebHost()` y tener acceso a la `IWebHost.Services` propiedad.</span><span class="sxs-lookup"><span data-stu-id="31d4a-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="31d4a-109">Cuando se crea una nueva aplicación de ASP.NET Core 2.0, este enlace se incluye de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="31d4a-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="31d4a-110">En versiones anteriores de EF Core y ASP.NET Core, intentan invocar las herramientas `Startup.ConfigureServices` directamente con el fin de obtener el proveedor de servicios de la aplicación, pero este patrón ya no funciona correctamente en las aplicaciones de ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="31d4a-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="31d4a-111">Si va a actualizar una aplicación de ASP.NET Core 1.x a 2.0, puede [modificar su `Program` clase seguir el patrón nuevo][3].</span><span class="sxs-lookup"><span data-stu-id="31d4a-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="31d4a-112">El `DbContext` propio y las dependencias en su constructor es necesario registrar como servicios en el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="31d4a-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="31d4a-113">Esto puede lograrse fácilmente al tener [un constructor en el `DbContext` que toma una instancia de `DbContextOptions<TContext>` como argumento] [ 4] y el uso de la [ `AddDbContext<TContext>` (método)] [5].</span><span class="sxs-lookup"><span data-stu-id="31d4a-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="31d4a-114">Usar un constructor sin parámetros</span><span class="sxs-lookup"><span data-stu-id="31d4a-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="31d4a-115">Si la clase DbContext no se puede obtener desde el proveedor de servicios de aplicación, las herramientas de buscar la clase derivada `DbContext` tipo dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="31d4a-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="31d4a-116">A continuación, intenta crear una instancia mediante un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="31d4a-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="31d4a-117">Esto puede ser el constructor predeterminado si el `DbContext` se configura mediante el [ `OnConfiguring` ] [ 6] método.</span><span class="sxs-lookup"><span data-stu-id="31d4a-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="31d4a-118">Desde una fábrica de tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="31d4a-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="31d4a-119">También puede indicar las herramientas de creación de DbContext implementando la `IDesignTimeDbContextFactory<TContext>` interfaz: si una clase que implementa esta interfaz se encuentre en el mismo proyecto que la clase derivada `DbContext` o bien en el proyecto de inicio de la aplicación, omiten las herramientas otras formas de crear el DbContext y uso el generador de tiempo de diseño en su lugar.</span><span class="sxs-lookup"><span data-stu-id="31d4a-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="31d4a-120">El `args` parámetro se utiliza actualmente.</span><span class="sxs-lookup"><span data-stu-id="31d4a-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="31d4a-121">No hay [un problema] [ 7] la capacidad para especificar los argumentos de tiempo de diseño de las herramientas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="31d4a-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="31d4a-122">Una fábrica de tiempo de diseño puede ser especialmente útil si tiene que configurar la clase DbContext de manera diferente para el tiempo de diseño que en tiempo de ejecución si el `DbContext` toma constructor parámetros adicionales no están registrados en la inserción de dependencias, si no utiliza DI en absoluto, o si para cada motivo prefiere no tener un `BuildWebHost` método en la aplicación de ASP.NET Core `Main` clase.</span><span class="sxs-lookup"><span data-stu-id="31d4a-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
