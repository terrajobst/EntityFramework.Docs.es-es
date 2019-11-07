---
title: 'Creación de DbContext en tiempo de diseño: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655524"
---
# <a name="design-time-dbcontext-creation"></a>Creación de DbContext en tiempo de diseño

Algunos de los comandos de herramientas de EF Core (por ejemplo, los comandos de [migración][1] ) requieren que se cree una instancia de `DbContext` derivada en tiempo de diseño para recopilar detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos. En la mayoría de los casos, es conveniente que el `DbContext` creado se configure de forma similar a como se [configuraría en tiempo de ejecución][2].

Hay varias maneras en las que las herramientas intentan crear el `DbContext`:

## <a name="from-application-services"></a>De servicios de aplicación

Si el proyecto de inicio usa el host de [Web ASP.net Core][3] o el [host genérico de .net Core][4], las herramientas intentan obtener el objeto DbContext del proveedor de servicios de la aplicación.

En primer lugar, las herramientas intentan obtener el proveedor de servicios invocando `Program.CreateHostBuilder()`, llamando a `Build()`y, a continuación, accediendo a la propiedad `Services`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> Cuando se crea una nueva aplicación de ASP.NET Core, este enlace se incluye de forma predeterminada.

El propio `DbContext` y las dependencias de su constructor deben registrarse como servicios en el proveedor de servicios de la aplicación. Esto se puede lograr fácilmente si se tiene [un constructor en el `DbContext` que toma una instancia de `DbContextOptions<TContext>` como argumento][5] y mediante el [método`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Usar un constructor sin parámetros

Si DbContext no se puede obtener del proveedor de servicios de aplicación, las herramientas buscan el tipo de `DbContext` derivado dentro del proyecto. A continuación, intentan crear una instancia mediante un constructor sin parámetros. Este puede ser el constructor predeterminado si el `DbContext` se configura mediante el método [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>Desde un generador en tiempo de diseño

También puede indicar a las herramientas cómo crear su DbContext implementando la interfaz de `IDesignTimeDbContextFactory<TContext>`: Si una clase que implementa esta interfaz se encuentra en el mismo proyecto que el `DbContext` derivado o en el proyecto de inicio de la aplicación, las herramientas omiten el otro. formas de crear DbContext y usar el generador en tiempo de diseño en su lugar.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> El parámetro `args` no se usa actualmente. Existe [un problema][8] al realizar el seguimiento de la capacidad de especificar argumentos en tiempo de diseño desde las herramientas.

Un generador en tiempo de diseño puede ser especialmente útil si necesita configurar DbContext de forma diferente en tiempo de diseño que en tiempo de ejecución, si el constructor `DbContext` toma parámetros adicionales que no están registrados en DI, si no usa DI en absoluto, o si por alguna razón prefiere no tener un método `BuildWebHost` en la clase `Main` de la aplicación ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
