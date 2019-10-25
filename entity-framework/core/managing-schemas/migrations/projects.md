---
title: 'Uso de un proyecto de migración independiente: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812034"
---
# <a name="using-a-separate-migrations-project"></a>Uso de un proyecto de migración independiente

Es posible que desee almacenar las migraciones en un ensamblado diferente del que contiene el `DbContext`. También puede usar esta estrategia para mantener varios conjuntos de migraciones, por ejemplo, una para el desarrollo y otra para las actualizaciones de lanzamiento a lanzamiento.

Para...

1. Cree una nueva biblioteca de clases.

2. Agregue una referencia al ensamblado DbContext.

3. Mueva las migraciones y los archivos de instantáneas de modelo a la biblioteca de clases.
   > [!TIP]
   > Si no tiene ninguna migración existente, genere una en el proyecto que contiene el DbContext y muévala.
   > Esto es importante porque si el ensamblado de migraciones no contiene una migración existente, el comando Add-Migration no podrá encontrar DbContext.

4. Configure el ensamblado de migraciones:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Agregue una referencia al ensamblado de migraciones desde el ensamblado de inicio.
   * Si esto produce una dependencia circular, actualice la ruta de acceso de salida de la biblioteca de clases:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Si lo hizo todo correctamente, debería poder agregar nuevas migraciones al proyecto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
