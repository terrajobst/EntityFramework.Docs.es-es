---
title: 'Creación de DbContext en tiempo de diseño: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655524"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="f692f-102">Creación de DbContext en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="f692f-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="f692f-103">Algunos de los comandos de herramientas de EF Core (por ejemplo, los comandos de [migración][1] ) requieren que se cree una instancia de `DbContext` derivada en tiempo de diseño para recopilar detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f692f-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="f692f-104">En la mayoría de los casos, es conveniente que el `DbContext` creado se configure de forma similar a como se [configuraría en tiempo de ejecución][2].</span><span class="sxs-lookup"><span data-stu-id="f692f-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="f692f-105">Hay varias maneras en las que las herramientas intentan crear el `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="f692f-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="f692f-106">De servicios de aplicación</span><span class="sxs-lookup"><span data-stu-id="f692f-106">From application services</span></span>

<span data-ttu-id="f692f-107">Si el proyecto de inicio usa el host de [Web ASP.net Core][3] o el [host genérico de .net Core][4], las herramientas intentan obtener el objeto DbContext del proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f692f-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="f692f-108">En primer lugar, las herramientas intentan obtener el proveedor de servicios invocando `Program.CreateHostBuilder()`, llamando a `Build()`y, a continuación, accediendo a la propiedad `Services`.</span><span class="sxs-lookup"><span data-stu-id="f692f-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="f692f-109">Cuando se crea una nueva aplicación de ASP.NET Core, este enlace se incluye de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f692f-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="f692f-110">El propio `DbContext` y las dependencias de su constructor deben registrarse como servicios en el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f692f-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="f692f-111">Esto se puede lograr fácilmente si se tiene [un constructor en el `DbContext` que toma una instancia de `DbContextOptions<TContext>` como argumento][5] y mediante el [método`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="f692f-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="f692f-112">Usar un constructor sin parámetros</span><span class="sxs-lookup"><span data-stu-id="f692f-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="f692f-113">Si DbContext no se puede obtener del proveedor de servicios de aplicación, las herramientas buscan el tipo de `DbContext` derivado dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f692f-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="f692f-114">A continuación, intentan crear una instancia mediante un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="f692f-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="f692f-115">Este puede ser el constructor predeterminado si el `DbContext` se configura mediante el método [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="f692f-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="f692f-116">Desde un generador en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="f692f-116">From a design-time factory</span></span>

<span data-ttu-id="f692f-117">También puede indicar a las herramientas cómo crear su DbContext implementando la interfaz de `IDesignTimeDbContextFactory<TContext>`: Si una clase que implementa esta interfaz se encuentra en el mismo proyecto que el `DbContext` derivado o en el proyecto de inicio de la aplicación, las herramientas omiten el otro. formas de crear DbContext y usar el generador en tiempo de diseño en su lugar.</span><span class="sxs-lookup"><span data-stu-id="f692f-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="f692f-118">El parámetro `args` no se usa actualmente.</span><span class="sxs-lookup"><span data-stu-id="f692f-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="f692f-119">Existe [un problema][8] al realizar el seguimiento de la capacidad de especificar argumentos en tiempo de diseño desde las herramientas.</span><span class="sxs-lookup"><span data-stu-id="f692f-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="f692f-120">Un generador en tiempo de diseño puede ser especialmente útil si necesita configurar DbContext de forma diferente en tiempo de diseño que en tiempo de ejecución, si el constructor `DbContext` toma parámetros adicionales que no están registrados en DI, si no usa DI en absoluto, o si por alguna razón prefiere no tener un método `BuildWebHost` en la clase `Main` de la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f692f-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
