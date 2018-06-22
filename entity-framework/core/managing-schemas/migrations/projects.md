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
ms.locfileid: "27161232"
---
<a name="using-a-separate-project"></a>Uso de un proyecto independiente
========================
Puede que desee almacenar las migraciones en un ensamblado diferente del que contiene el `DbContext`. También puede utilizar esta estrategia para mantener varios conjuntos de migraciones, por ejemplo, uno para el desarrollo y otro para las actualizaciones de versión a otra.

Para...

1. Cree una nueva biblioteca de clases.

2. Agregue una referencia al ensamblado DbContext.

3. Mueva los archivos de instantánea de modelo y las migraciones a la biblioteca de clases.
   * Si aún no lo ha agregado, agregue una al proyecto de DbContext, a continuación, moverlo.

4. Configure el ensamblado de migraciones:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Agregue una referencia al ensamblado migraciones desde el ensamblado de inicio.
   * Si esto hace que una dependencia circular, actualice la ruta de acceso de salida de la biblioteca de clases:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si hizo todo correctamente, podrá agregar las migraciones nuevas al proyecto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
