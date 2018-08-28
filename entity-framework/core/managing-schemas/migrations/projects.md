---
title: Migraciones con varios proyectos - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997992"
---
<a name="using-a-separate-project"></a><span data-ttu-id="ea6d4-102">Uso de un proyecto independiente</span><span class="sxs-lookup"><span data-stu-id="ea6d4-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="ea6d4-103">Es posible que desea almacenar las migraciones en un ensamblado diferente del que contiene su `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="ea6d4-104">También puede usar esta estrategia que mantener varios conjuntos de migraciones, por ejemplo, uno para el desarrollo y otro para las actualizaciones de versión a otra.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="ea6d4-105">Para...</span><span class="sxs-lookup"><span data-stu-id="ea6d4-105">To do this...</span></span>

1. <span data-ttu-id="ea6d4-106">Cree una nueva biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-106">Create a new class library.</span></span>

2. <span data-ttu-id="ea6d4-107">Agregue una referencia al ensamblado DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="ea6d4-108">Mueva los archivos de instantánea de modelo y las migraciones a la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="ea6d4-109">Si dispone de ninguna de las migraciones, generar uno en el proyecto que contiene la clase DbContext, a continuación, moverla.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="ea6d4-110">Esto es importante porque si el ensamblado de las migraciones no contiene una migración existente, el comando Add-Migration será no se puede encontrar la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="ea6d4-111">Configure el ensamblado de migraciones:</span><span class="sxs-lookup"><span data-stu-id="ea6d4-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="ea6d4-112">Agregue una referencia al ensamblado migraciones desde el ensamblado de inicio.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="ea6d4-113">Si esto hace que una dependencia circular, actualice la ruta de acceso de salida de la biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="ea6d4-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="ea6d4-114">Si hizo todo correctamente, debería poder agregar migraciones nueva al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea6d4-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
