---
title: "Creación de tiempo de diseño DbContext; EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a899c474cc45437bff7c82ce5bddeb915b15c3b0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
<a name="design-time-dbcontext-creation"></a>Creación de DbContext de tiempo de diseño
==============================
Algunos de los comandos de herramientas de núcleo de EF (por ejemplo, el [migraciones] [ 1] comandos) requieren un derivada `DbContext` instancia se cree en tiempo de diseño para recopilar detalles acerca de la aplicación tipos de entidad y cómo se asignan a un esquema de base de datos. En la mayoría de los casos, es conveniente que el `DbContext` creado, por tanto, se configura de forma similar a cómo sería [configurado en tiempo de ejecución][2].

Hay varias maneras de las herramientas de intentar crear el `DbContext`:

<a name="from-application-services"></a>Servicios de aplicación
-------------------------
Si su proyecto de inicio es una aplicación de ASP.NET Core, las herramientas de intentar obtener el objeto de DbContext de proveedor de servicios de la aplicación.

La herramienta primero intenta obtener el proveedor de servicios mediante la invocación de `Program.BuildWebHost()` y obtener acceso a la `IWebHost.Services` propiedad.

> [!NOTE]
> Cuando se crea una nueva aplicación de ASP.NET Core 2.0, este enlace se incluye de forma predeterminada. En versiones anteriores de ASP.NET y de núcleo de EF, las herramientas intentan invocar `Startup.ConfigureServices` directamente con el fin de obtener el proveedor de servicios de la aplicación, pero este patrón ya no funciona correctamente en las aplicaciones de ASP.NET Core 2.0. Si va a actualizar una aplicación de ASP.NET Core 1.x a 2.0, puede [modificar su `Program` clase para seguir el patrón nuevo][3].

El `DbContext` sí mismo y a todas las dependencias en su constructor deben registrarse como servicios en el proveedor de servicios de la aplicación. Esto puede lograrse fácilmente manteniendo [un constructor en el `DbContext` que toma una instancia de `DbContextOptions<TContext>` como un argumento de] [ 4] y el uso de la [ `AddDbContext<TContext>` (método)] [5].

<a name="using-a-constructor-with-no-parameters"></a>Usar un constructor sin parámetros
--------------------------------------
Si no se puede obtener el DbContext desde el proveedor de servicios de aplicación, las herramientas de buscar la clase derivada `DbContext` tipo dentro del proyecto. A continuación, intenta crear una instancia mediante un constructor sin parámetros. Esto puede ser el constructor predeterminado si el `DbContext` se configura mediante la [ `OnConfiguring` ] [ 6] método.

<a name="from-a-design-time-factory"></a>De un generador de tiempo de diseño
--------------------------
También puede indicar las herramientas de cómo crear su DbContext implementando la `IDesignTimeDbContextFactory<TContext>` interfaz: si una clase que implementa esta interfaz se encuentre en el mismo proyecto que la clase derivada `DbContext` o en el proyecto de inicio de la aplicación, omisión las herramientas otras maneras de crear el DbContext y use el generador de tiempo de diseño en su lugar.

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
> El `args` parámetro no se utiliza actualmente. No hay [un problema] [ 7] la capacidad para especificar los argumentos de tiempo de diseño de las herramientas de seguimiento.

Un generador de tiempo de diseño puede ser especialmente útil si tiene que configurar de forma diferente DbContext para tiempo de diseño que en tiempo de ejecución si el `DbContext` toma constructor parámetros adicionales no están registrados en DI, si no utilizas DI en absoluto, o si para algunos motivo no desea tener un `BuildWebHost` método de la aplicación de ASP.NET Core  
Clase `Main`.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
