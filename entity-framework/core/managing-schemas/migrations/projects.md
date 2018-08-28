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
<a name="using-a-separate-project"></a>Uso de un proyecto independiente
========================
Es posible que desea almacenar las migraciones en un ensamblado diferente del que contiene su `DbContext`. También puede usar esta estrategia que mantener varios conjuntos de migraciones, por ejemplo, uno para el desarrollo y otro para las actualizaciones de versión a otra.

Para...

1. Cree una nueva biblioteca de clases.

2. Agregue una referencia al ensamblado DbContext.

3. Mueva los archivos de instantánea de modelo y las migraciones a la biblioteca de clases.
   > [!TIP]
   > Si dispone de ninguna de las migraciones, generar uno en el proyecto que contiene la clase DbContext, a continuación, moverla. Esto es importante porque si el ensamblado de las migraciones no contiene una migración existente, el comando Add-Migration será no se puede encontrar la clase DbContext.

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
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si hizo todo correctamente, debería poder agregar migraciones nueva al proyecto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
