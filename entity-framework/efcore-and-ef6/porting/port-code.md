---
title: 'Portabilidad de EF6 a EF Core: trasladar un modelo basado en código'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997054"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="b87f5-102">Portabilidad de un modelo basado en código de EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="b87f5-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="b87f5-103">Si ha leído todas las advertencias y está listo para el puerto, a continuación, estas son algunas directrices que le ayudarán a empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="b87f5-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="b87f5-104">Instalar paquetes de NuGet de EF Core</span><span class="sxs-lookup"><span data-stu-id="b87f5-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="b87f5-105">Para usar EF Core, instale el paquete NuGet para el proveedor de base de datos que desea usar.</span><span class="sxs-lookup"><span data-stu-id="b87f5-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="b87f5-106">Por ejemplo, cuando el destino es SQL Server, instalaría `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="b87f5-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="b87f5-107">Consulte [proveedores de base de datos](../../core/providers/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b87f5-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="b87f5-108">Si planea usar migraciones, también debe instalar el `Microsoft.EntityFrameworkCore.Tools` paquete.</span><span class="sxs-lookup"><span data-stu-id="b87f5-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="b87f5-109">Es correcto dejar el paquete NuGet de EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser utilizado por en paralelo en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="b87f5-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="b87f5-110">Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete para obtener errores de compilación en fragmentos de código que necesitan atención.</span><span class="sxs-lookup"><span data-stu-id="b87f5-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="b87f5-111">Intercambiar los espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="b87f5-111">Swap namespaces</span></span>

<span data-ttu-id="b87f5-112">La mayoría de las API que se utiliza en EF6 está en el `System.Data.Entity` espacio de nombres (y relacionados con subespacios de nombres).</span><span class="sxs-lookup"><span data-stu-id="b87f5-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="b87f5-113">El primer cambio de código consiste en cambiar a la `Microsoft.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="b87f5-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="b87f5-114">¿Suelen comenzar con el archivo de código del contexto derivado y, a continuación, calcular a partir de ahí, dirige a medida que se producen errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="b87f5-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="b87f5-115">Configuración del contexto (conexión etcetera).</span><span class="sxs-lookup"><span data-stu-id="b87f5-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="b87f5-116">Como se describe en [asegurarse de EF Core le funcione para su aplicación](ensure-requirements.md), EF Core tiene menos magia en torno a detectar para conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b87f5-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="b87f5-117">Deberá reemplazar el `OnConfiguring` método en su contexto derivado y usar la API específica del proveedor de base de datos para configurar la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b87f5-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="b87f5-118">La mayoría de las aplicaciones de EF6 almacena la cadena de conexión en las aplicaciones `App/Web.config` archivo.</span><span class="sxs-lookup"><span data-stu-id="b87f5-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="b87f5-119">En EF Core, lea esta cadena de conexión mediante el `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="b87f5-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="b87f5-120">Es posible que deba agregar una referencia a la `System.Configuration` ensamblado de marco de trabajo para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="b87f5-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="b87f5-121">Actualizar el código</span><span class="sxs-lookup"><span data-stu-id="b87f5-121">Update your code</span></span>

<span data-ttu-id="b87f5-122">En este punto, es una cuestión de solucionar errores de compilación y revisión de código para ver si los cambios de comportamiento le afectará.</span><span class="sxs-lookup"><span data-stu-id="b87f5-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="b87f5-123">Migraciones existentes</span><span class="sxs-lookup"><span data-stu-id="b87f5-123">Existing migrations</span></span>

<span data-ttu-id="b87f5-124">Realmente no es una manera sea factible de puerto existente de las migraciones de EF6 a EF Core.</span><span class="sxs-lookup"><span data-stu-id="b87f5-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="b87f5-125">Si es posible, es mejor suponer que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, a continuación, usar EF Core de punto de migración del esquema desde el el inicio.</span><span class="sxs-lookup"><span data-stu-id="b87f5-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="b87f5-126">Para ello, usaría el `Add-Migration` comando para agregar una migración una vez que el modelo se ha trasladado a EF Core.</span><span class="sxs-lookup"><span data-stu-id="b87f5-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="b87f5-127">A continuación, debe quitar todos los códigos de la `Up` y `Down` métodos de la migración con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b87f5-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="b87f5-128">Las migraciones posteriores se compararán en el modelo cuando se ha aplicado scaffolding a que la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="b87f5-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="b87f5-129">Probar el puerto</span><span class="sxs-lookup"><span data-stu-id="b87f5-129">Test the port</span></span>

<span data-ttu-id="b87f5-130">Simplemente porque se compila la aplicación, no significa que es portada correctamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="b87f5-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="b87f5-131">Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento han afectado negativamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b87f5-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
