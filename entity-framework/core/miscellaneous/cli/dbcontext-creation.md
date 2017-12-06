---
title: "Creación de tiempo de diseño DbContext; EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Creación de DbContext de tiempo de diseño
==============================
Algunas de las herramientas de EF comandos requieren una instancia de DbContext se creará al diseño de tiempo (por ejemplo, cuando se ejecuta comandos de migraciones). Hay varias maneras de que las herramientas de intentar crearla.

<a name="from-application-services"></a>Servicios de aplicación
-------------------------
Si su proyecto de inicio es una aplicación de ASP.NET Core, las herramientas de intentar obtener el objeto de DbContext de proveedor de servicios de la aplicación. Se obtienen mediante la invocación de `Program.BuildWebHost()` y obtener acceso a la `IWebHost.Services` propiedad. Cualquier DbContext registrarse mediante `IServiceCollection.AddDbContext<TContext>()` puede encontrarse y creadas de esta manera. Este patrón era [presentadas en ASP.NET 2.0 de núcleo][1]

<a name="using-the-default-constructor"></a>Utilizando el constructor predeterminado
-----------------------------
Si no se puede obtener el DbContext desde el proveedor de servicios de aplicación, las herramientas de buscar el tipo de DbContext dentro del proyecto. Intentar crearlo mediante su constructor predeterminado.

<a name="from-a-design-time-factory"></a>De un generador de tiempo de diseño
--------------------------
También puede indicar las herramientas de creación de su DbContext mediante la implementación de `IDesignTimeDbContextFactory`. Si una clase que implementa esta interfaz se encuentra dentro de su proyecto, las herramientas de omitan las otras maneras de crear DbContext.
Siempre usan el generador en tiempo de diseño. Un generador es especialmente útil si tiene que configurar de forma diferente DbContext para tiempo de diseño que en tiempo de ejecución.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> El `args` parámetro no se utiliza actualmente. No hay [un problema] [ 2] la capacidad para especificar los argumentos de tiempo de diseño de las herramientas de seguimiento.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
