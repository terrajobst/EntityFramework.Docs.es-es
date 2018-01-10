---
title: Migraciones con varios proyectos - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="1c55c-102">Uso de un proyecto independiente</span><span class="sxs-lookup"><span data-stu-id="1c55c-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="1c55c-103">Puede que desee almacenar las migraciones en un ensamblado diferente del que contiene el `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1c55c-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="1c55c-104">También puede utilizar esta estrategia para mantener varios conjuntos de migraciones, por ejemplo, uno para el desarrollo y otro para las actualizaciones de versión a otra.</span><span class="sxs-lookup"><span data-stu-id="1c55c-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="1c55c-105">Para...</span><span class="sxs-lookup"><span data-stu-id="1c55c-105">To do this...</span></span>

1. <span data-ttu-id="1c55c-106">Cree una nueva biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="1c55c-106">Create a new class library.</span></span>

2. <span data-ttu-id="1c55c-107">Agregue una referencia al ensamblado DbContext.</span><span class="sxs-lookup"><span data-stu-id="1c55c-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="1c55c-108">Mueva los archivos de instantánea de modelo y las migraciones a la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="1c55c-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="1c55c-109">Si aún no lo ha agregado, agregue una al proyecto de DbContext, a continuación, moverlo.</span><span class="sxs-lookup"><span data-stu-id="1c55c-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="1c55c-110">Configure el ensamblado de migraciones:</span><span class="sxs-lookup"><span data-stu-id="1c55c-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="1c55c-111">Agregue una referencia al ensamblado migraciones desde el ensamblado de inicio.</span><span class="sxs-lookup"><span data-stu-id="1c55c-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="1c55c-112">Si esto hace que una dependencia circular, actualice la ruta de acceso de salida de la biblioteca de clases:</span><span class="sxs-lookup"><span data-stu-id="1c55c-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="1c55c-113">Si hizo todo correctamente, podrá agregar las migraciones nuevas al proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c55c-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
