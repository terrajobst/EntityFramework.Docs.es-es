---
title: 'Portabilidad de EF6 a EF Core: portabilidad de un modelo basado en código - EF'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413862"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="a68b8-102">Portabilidad de un modelo basado en código de EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="a68b8-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="a68b8-103">Si ha leído todas las advertencias y está a punto para realizar la portabilidad, estas son algunas instrucciones que le ayudarán a empezar.</span><span class="sxs-lookup"><span data-stu-id="a68b8-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="a68b8-104">Instalación de los paquetes NuGet de EF Core</span><span class="sxs-lookup"><span data-stu-id="a68b8-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="a68b8-105">Para usar EF Core, instale el paquete NuGet correspondiente al proveedor de base de datos que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="a68b8-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="a68b8-106">Por ejemplo, cuando el destino es SQL Server, tendría que instalar `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="a68b8-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="a68b8-107">Para obtener más información, vea [Proveedores de bases de datos](../../core/providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a68b8-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="a68b8-108">Si tiene previsto usar migraciones, también debe instalar el paquete `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a68b8-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="a68b8-109">No hay problema por dejar el paquete NuGet de EF6 (EntityFramework) instalado, ya que EF Core y EF6 se pueden usar en paralelo en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="a68b8-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="a68b8-110">Sin embargo, si no va a usar EF6 en ninguna de las áreas de la aplicación, la desinstalación del paquete le ayudará a dar errores de compilación en fragmentos de código que requieren atención.</span><span class="sxs-lookup"><span data-stu-id="a68b8-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="a68b8-111">Intercambio de espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="a68b8-111">Swap namespaces</span></span>

<span data-ttu-id="a68b8-112">La mayoría de las API que se usan en EF6 se encuentran en el espacio de nombres `System.Data.Entity` (y los subespacios de nombres relacionados).</span><span class="sxs-lookup"><span data-stu-id="a68b8-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="a68b8-113">El primer cambio de código consiste en cambiar al espacio de nombres `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a68b8-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="a68b8-114">Normalmente, empezará con el archivo de código de contexto derivado y, después, avanzará desde allí y solucionará los errores de compilación a medida que aparezcan.</span><span class="sxs-lookup"><span data-stu-id="a68b8-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="a68b8-115">Configuración del contexto (conexión, etc.)</span><span class="sxs-lookup"><span data-stu-id="a68b8-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="a68b8-116">Como se describe en [Asegurarse de que EF Core funcionará para la aplicación](ensure-requirements.md), la detección de la base de datos a la que se va a conectar EF Core tiene menos secretos.</span><span class="sxs-lookup"><span data-stu-id="a68b8-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="a68b8-117">Tendrá que reemplazar el método `OnConfiguring` en el contexto derivado y usar la API específica del proveedor de base de datos para configurar la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a68b8-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="a68b8-118">La mayoría de las aplicaciones EF6 almacenan la cadena de conexión en el archivo `App/Web.config` de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a68b8-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="a68b8-119">En EF Core, esta cadena de conexión se lee mediante la API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="a68b8-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="a68b8-120">Es posible que tenga que agregar una referencia al ensamblado del marco `System.Configuration` para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="a68b8-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="a68b8-121">Actualización del código</span><span class="sxs-lookup"><span data-stu-id="a68b8-121">Update your code</span></span>

<span data-ttu-id="a68b8-122">En este punto, es cuestión de solucionar los errores de compilación y de revisar el código para ver si los cambios de comportamiento le afectarán.</span><span class="sxs-lookup"><span data-stu-id="a68b8-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="a68b8-123">Migraciones existentes</span><span class="sxs-lookup"><span data-stu-id="a68b8-123">Existing migrations</span></span>

<span data-ttu-id="a68b8-124">Realmente no existe una manera viable de realizar la portabilidad de las migraciones de EF6 existentes a EF Core.</span><span class="sxs-lookup"><span data-stu-id="a68b8-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="a68b8-125">Si es posible, es mejor suponer que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, después, iniciar la migración del esquema desde ese punto mediante EF Core.</span><span class="sxs-lookup"><span data-stu-id="a68b8-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="a68b8-126">Para ello, use el comando `Add-Migration` para agregar una migración una vez que el modelo se haya trasladado a EF Core.</span><span class="sxs-lookup"><span data-stu-id="a68b8-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="a68b8-127">Después, podría quitar todo el código de los métodos `Up` y `Down` de la migración con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a68b8-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="a68b8-128">Las migraciones posteriores se compararán con el modelo cuando se haya aplicado scaffolding a la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="a68b8-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="a68b8-129">Prueba del puerto</span><span class="sxs-lookup"><span data-stu-id="a68b8-129">Test the port</span></span>

<span data-ttu-id="a68b8-130">El hecho de que la aplicación se compile no significa que se haya trasladado correctamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="a68b8-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="a68b8-131">Tendrá que probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento afecte de forma negativa a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a68b8-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
