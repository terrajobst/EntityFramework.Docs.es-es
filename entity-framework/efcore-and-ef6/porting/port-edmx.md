---
title: Migrar desde EF6 EF Core - trasladar un modelo basado en EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="05b41-102">Trasladar un modelo basado en EDMX de EF6 a EF Core</span><span class="sxs-lookup"><span data-stu-id="05b41-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="05b41-103">Núcleo EF no admite el formato de archivo EDMX para los modelos.</span><span class="sxs-lookup"><span data-stu-id="05b41-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="05b41-104">La mejor opción migrar estos modelos, consiste en generar un nuevo modelo basado en código de la base de datos para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b41-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="05b41-105">Instalar paquetes de NuGet de núcleo de EF</span><span class="sxs-lookup"><span data-stu-id="05b41-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="05b41-106">Instalar el `Microsoft.EntityFrameworkCore.Tools` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="05b41-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="05b41-107">Volver a generar el modelo</span><span class="sxs-lookup"><span data-stu-id="05b41-107">Regenerate the model</span></span>

<span data-ttu-id="05b41-108">Ahora puede usar la funcionalidad de ingeniería inversa para crear un modelo basado en la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="05b41-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="05b41-109">Ejecute el siguiente comando en la consola de administrador de paquetes (Herramientas –> Administrador de paquetes de NuGet –> Package Manager Console).</span><span class="sxs-lookup"><span data-stu-id="05b41-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="05b41-110">Vea [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opciones de comando para aplicar la técnica scaffolding un subconjunto de tablas etcetera.</span><span class="sxs-lookup"><span data-stu-id="05b41-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="05b41-111">Por ejemplo, este es el comando para aplicar la técnica scaffolding un modelo de la base de datos de creación de blogs en la instancia de SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="05b41-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="05b41-112">Quitar EF6 modelo</span><span class="sxs-lookup"><span data-stu-id="05b41-112">Remove EF6 model</span></span>

<span data-ttu-id="05b41-113">Ahora se quitará el modelo EF6 desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b41-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="05b41-114">Es correcto dejar el paquete de NuGet EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser usado en paralelo en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b41-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="05b41-115">Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete le ayudarán a dar errores de compilación en fragmentos de código que requieren atención.</span><span class="sxs-lookup"><span data-stu-id="05b41-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="05b41-116">Actualizar el código</span><span class="sxs-lookup"><span data-stu-id="05b41-116">Update your code</span></span>

<span data-ttu-id="05b41-117">En este punto, es una cuestión de direccionamiento errores de compilación y revisión de código para ver si los cambios de comportamiento entre EF6 y EF Core afectarán al usuario.</span><span class="sxs-lookup"><span data-stu-id="05b41-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="05b41-118">Probar el puerto</span><span class="sxs-lookup"><span data-stu-id="05b41-118">Test the port</span></span>

<span data-ttu-id="05b41-119">Solo porque se compila la aplicación, no significa que se transfieren correctamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="05b41-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="05b41-120">Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento se vea afectado negativamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05b41-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="05b41-121">Vea [Introducción a EF principales en el núcleo de ASP.NET con una base de datos](xref:core/get-started/aspnetcore/existing-db) como una referencia adicional sobre cómo trabajar con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="05b41-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
