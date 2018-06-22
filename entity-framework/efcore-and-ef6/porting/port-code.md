---
title: Migrar desde EF6 EF Core - trasladar un modelo basado en código
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052955"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="19325-102">Trasladar un modelo basado en código de EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="19325-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="19325-103">Si ha leído todas las observaciones y está listo para portar, estas son algunas directrices para ayudarle a empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="19325-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="19325-104">Instalar paquetes de NuGet de núcleo de EF</span><span class="sxs-lookup"><span data-stu-id="19325-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="19325-105">Para usar EF Core, instalar el paquete NuGet para el proveedor de base de datos que desea usar.</span><span class="sxs-lookup"><span data-stu-id="19325-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="19325-106">Por ejemplo, cuando el destino es SQL Server, se instalan `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="19325-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="19325-107">Vea [proveedores de base de datos](../../core/providers/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="19325-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="19325-108">Si va a usar migraciones, también debe instalar el `Microsoft.EntityFrameworkCore.Tools` paquete.</span><span class="sxs-lookup"><span data-stu-id="19325-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="19325-109">Es correcto dejar el paquete de NuGet EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser usado en paralelo en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="19325-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="19325-110">Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete le ayudarán a dar errores de compilación en fragmentos de código que requieren atención.</span><span class="sxs-lookup"><span data-stu-id="19325-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="19325-111">Intercambiar los espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="19325-111">Swap namespaces</span></span>

<span data-ttu-id="19325-112">La mayoría de las API que se utilizan en EF6 están en el `System.Data.Entity` espacio de nombres (y relacionados con subespacios de nombres).</span><span class="sxs-lookup"><span data-stu-id="19325-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="19325-113">El primer cambio en el código consiste en cambiar a la `Microsoft.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="19325-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="19325-114">¿Normalmente empieza con el archivo de código de contexto derivada y, a continuación, calcular a partir de ahí, solucionar errores de compilación cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="19325-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="19325-115">Configuración del contexto (conexión etcetera).</span><span class="sxs-lookup"><span data-stu-id="19325-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="19325-116">Como se describe en [Asegúrese EF Core le trabajo para su aplicación](ensure-requirements.md), Core EF tiene menos magia alrededor de detectar la base de datos para conectarse a.</span><span class="sxs-lookup"><span data-stu-id="19325-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="19325-117">Debe invalidar el `OnConfiguring` método en su contexto derivada y use la API específica del proveedor de base de datos para configurar la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19325-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="19325-118">La mayoría de las aplicaciones de EF6 almacenan la cadena de conexión en las aplicaciones `App/Web.config` archivo.</span><span class="sxs-lookup"><span data-stu-id="19325-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="19325-119">En el núcleo de EF, lea esta cadena de conexión mediante el `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="19325-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="19325-120">Puede que necesite agregar una referencia a la `System.Configuration` ensamblado de framework para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="19325-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="19325-121">Actualizar el código</span><span class="sxs-lookup"><span data-stu-id="19325-121">Update your code</span></span>

<span data-ttu-id="19325-122">En este punto, es una cuestión de solucionar errores de compilación y revisión de código para ver si los cambios de comportamiento le afecta.</span><span class="sxs-lookup"><span data-stu-id="19325-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="19325-123">Migraciones existentes</span><span class="sxs-lookup"><span data-stu-id="19325-123">Existing migrations</span></span>

<span data-ttu-id="19325-124">Realmente no es una manera factible de puerto existentes migraciones EF6 EF Core.</span><span class="sxs-lookup"><span data-stu-id="19325-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="19325-125">Si es posible, es mejor que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, a continuación, mediante el uso EF Core de punto de inicio migrar el esquema desde el se supone.</span><span class="sxs-lookup"><span data-stu-id="19325-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="19325-126">Para ello, usaría el `Add-Migration` comando para agregar una migración una vez que el modelo se transfieren a EF Core.</span><span class="sxs-lookup"><span data-stu-id="19325-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="19325-127">A continuación, se quitará todo el código de la `Up` y `Down` métodos de la migración con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="19325-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="19325-128">Migraciones posteriores comparará al modelo al que la migración inicial se scaffolding.</span><span class="sxs-lookup"><span data-stu-id="19325-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="19325-129">Probar el puerto</span><span class="sxs-lookup"><span data-stu-id="19325-129">Test the port</span></span>

<span data-ttu-id="19325-130">Solo porque se compila la aplicación, no significa que se transfieren correctamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="19325-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="19325-131">Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento se vea afectado negativamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="19325-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
