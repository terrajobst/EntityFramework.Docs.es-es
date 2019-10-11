---
title: Migrar de EF6 a EF Core-Porting de un modelo basado en EDMX-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182063"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="8d61e-102">Portabilidad de un modelo basado en EF6 EDMX a EF Core</span><span class="sxs-lookup"><span data-stu-id="8d61e-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="8d61e-103">EF Core no admite el formato de archivo EDMX para los modelos.</span><span class="sxs-lookup"><span data-stu-id="8d61e-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="8d61e-104">La mejor opción para portar estos modelos es generar un nuevo modelo basado en código a partir de la base de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d61e-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="8d61e-105">Instalación de EF Core paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="8d61e-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="8d61e-106">Instale el paquete NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="8d61e-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="8d61e-107">Regeneración del modelo</span><span class="sxs-lookup"><span data-stu-id="8d61e-107">Regenerate the model</span></span>

<span data-ttu-id="8d61e-108">Ahora puede usar la funcionalidad de ingeniería inversa para crear un modelo basado en la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="8d61e-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="8d61e-109">Ejecute el siguiente comando en la consola del administrador de paquetes (Herramientas – > Administrador de paquetes NuGet – > consola del administrador de paquetes).</span><span class="sxs-lookup"><span data-stu-id="8d61e-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="8d61e-110">Vea [consola del administrador de paquetes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para ver las opciones de comando para aplicar scaffolding a un subconjunto de tablas, etc.</span><span class="sxs-lookup"><span data-stu-id="8d61e-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="8d61e-111">Por ejemplo, este es el comando para aplicar scaffolding a un modelo a partir de la base de datos de blogs en la SQL Server instancia de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="8d61e-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="8d61e-112">Quitar el modelo EF6</span><span class="sxs-lookup"><span data-stu-id="8d61e-112">Remove EF6 model</span></span>

<span data-ttu-id="8d61e-113">Ahora se quitará el modelo EF6 de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d61e-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="8d61e-114">Es adecuado dejar el paquete NuGet de EF6 (EntityFramework) instalado, ya que EF Core y EF6 se pueden usar en paralelo en la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d61e-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="8d61e-115">Sin embargo, si no va a usar EF6 en ninguna de las áreas de la aplicación, la desinstalación del paquete le ayudará a dar errores de compilación en fragmentos de código que requieren atención.</span><span class="sxs-lookup"><span data-stu-id="8d61e-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="8d61e-116">Actualización del código</span><span class="sxs-lookup"><span data-stu-id="8d61e-116">Update your code</span></span>

<span data-ttu-id="8d61e-117">En este punto, es cuestión de solucionar los errores de compilación y revisar el código para ver si el comportamiento cambia entre EF6 y EF Core afectará a su caso.</span><span class="sxs-lookup"><span data-stu-id="8d61e-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="8d61e-118">Prueba del puerto</span><span class="sxs-lookup"><span data-stu-id="8d61e-118">Test the port</span></span>

<span data-ttu-id="8d61e-119">El hecho de que la aplicación se compile, no significa que se haya trasladado correctamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="8d61e-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="8d61e-120">Tendrá que probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento afecte negativamente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d61e-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
