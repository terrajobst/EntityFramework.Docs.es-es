---
title: "Creación de tiempo de diseño DbContext; EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="702ef-102">Creación de DbContext de tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="702ef-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="702ef-103">Algunas de las herramientas de EF comandos requieren una instancia de DbContext se creará al diseño de tiempo (por ejemplo, cuando se ejecuta comandos de migraciones).</span><span class="sxs-lookup"><span data-stu-id="702ef-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="702ef-104">Hay varias maneras de que las herramientas de intentar crearla.</span><span class="sxs-lookup"><span data-stu-id="702ef-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="702ef-105">Servicios de aplicación</span><span class="sxs-lookup"><span data-stu-id="702ef-105">From application services</span></span>
-------------------------
<span data-ttu-id="702ef-106">Si su proyecto de inicio es una aplicación de ASP.NET Core, las herramientas de intentar obtener el objeto de DbContext de proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="702ef-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="702ef-107">Se obtienen mediante la invocación de `Program.BuildWebHost()` y obtener acceso a la `IWebHost.Services` propiedad.</span><span class="sxs-lookup"><span data-stu-id="702ef-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="702ef-108">Cualquier DbContext registrarse mediante `IServiceCollection.AddDbContext<TContext>()` puede encontrarse y creadas de esta manera.</span><span class="sxs-lookup"><span data-stu-id="702ef-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="702ef-109">Este patrón era [presentadas en ASP.NET 2.0 de núcleo][1]</span><span class="sxs-lookup"><span data-stu-id="702ef-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="702ef-110">Utilizando el constructor predeterminado</span><span class="sxs-lookup"><span data-stu-id="702ef-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="702ef-111">Si no se puede obtener el DbContext desde el proveedor de servicios de aplicación, las herramientas de buscar el tipo de DbContext dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="702ef-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="702ef-112">Intentar crearlo mediante su constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="702ef-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="702ef-113">De un generador de tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="702ef-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="702ef-114">También puede indicar las herramientas de creación de su DbContext mediante la implementación de `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="702ef-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="702ef-115">Si una clase que implementa esta interfaz se encuentra dentro de su proyecto, las herramientas de omitan las otras maneras de crear DbContext.</span><span class="sxs-lookup"><span data-stu-id="702ef-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="702ef-116">Siempre usan el generador en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="702ef-116">They always use the factory at design time.</span></span> <span data-ttu-id="702ef-117">Un generador es especialmente útil si tiene que configurar de forma diferente DbContext para tiempo de diseño que en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="702ef-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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

> [!NOTE]
> <span data-ttu-id="702ef-118">El `args` parámetro no se utiliza actualmente.</span><span class="sxs-lookup"><span data-stu-id="702ef-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="702ef-119">No hay [un problema] [ 2] la capacidad para especificar los argumentos de tiempo de diseño de las herramientas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="702ef-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
