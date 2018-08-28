---
title: Creación de DbContext en tiempo de diseño - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993723"
---
<a name="design-time-dbcontext-creation"></a>Creación de DbContext en tiempo de diseño
==============================
Algunos de los comandos de herramientas de EF Core (por ejemplo, el [migraciones] [ 1] comandos) requieren una derivada `DbContext` creación en tiempo de diseño con el fin de recopilar detalles acerca de la aplicación de instancia tipos de entidad y cómo se asignan a un esquema de base de datos. En la mayoría de los casos, es deseable que la `DbContext` creado, por tanto, se configura de forma similar a cómo sería [configurado en tiempo de ejecución][2].

Hay varias maneras de probar las herramientas para crear el `DbContext`:

<a name="from-application-services"></a>Servicios de aplicación
-------------------------
Si su proyecto de inicio es una aplicación ASP.NET Core, las herramientas intentan obtener el objeto DbContext de proveedor de servicios de la aplicación.

Las herramientas primero intentan obtener el proveedor de servicios mediante la invocación `Program.BuildWebHost()` y tener acceso a la `IWebHost.Services` propiedad.

> [!NOTE]
> Cuando se crea una nueva aplicación de ASP.NET Core 2.0, este enlace se incluye de forma predeterminada. En versiones anteriores de EF Core y ASP.NET Core, intentan invocar las herramientas `Startup.ConfigureServices` directamente con el fin de obtener el proveedor de servicios de la aplicación, pero este patrón ya no funciona correctamente en las aplicaciones de ASP.NET Core 2.0. Si va a actualizar una aplicación de ASP.NET Core 1.x a 2.0, puede [modificar su `Program` clase seguir el patrón nuevo][3].

El `DbContext` propio y las dependencias en su constructor es necesario registrar como servicios en el proveedor de servicios de la aplicación. Esto puede lograrse fácilmente al tener [un constructor en el `DbContext` que toma una instancia de `DbContextOptions<TContext>` como argumento] [ 4] y el uso de la [ `AddDbContext<TContext>` (método)] [5].

<a name="using-a-constructor-with-no-parameters"></a>Usar un constructor sin parámetros
--------------------------------------
Si la clase DbContext no se puede obtener desde el proveedor de servicios de aplicación, las herramientas de buscar la clase derivada `DbContext` tipo dentro del proyecto. A continuación, intenta crear una instancia mediante un constructor sin parámetros. Esto puede ser el constructor predeterminado si el `DbContext` se configura mediante el [ `OnConfiguring` ] [ 6] método.

<a name="from-a-design-time-factory"></a>Desde una fábrica de tiempo de diseño
--------------------------
También puede indicar las herramientas de creación de DbContext implementando la `IDesignTimeDbContextFactory<TContext>` interfaz: si una clase que implementa esta interfaz se encuentre en el mismo proyecto que la clase derivada `DbContext` o bien en el proyecto de inicio de la aplicación, omiten las herramientas otras formas de crear el DbContext y uso el generador de tiempo de diseño en su lugar.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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
> El `args` parámetro se utiliza actualmente. No hay [un problema] [ 7] la capacidad para especificar los argumentos de tiempo de diseño de las herramientas de seguimiento.

Una fábrica de tiempo de diseño puede ser especialmente útil si tiene que configurar la clase DbContext de manera diferente para el tiempo de diseño que en tiempo de ejecución si el `DbContext` toma constructor parámetros adicionales no están registrados en la inserción de dependencias, si no utiliza DI en absoluto, o si para cada motivo prefiere no tener un `BuildWebHost` método en la aplicación de ASP.NET Core `Main` clase.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
