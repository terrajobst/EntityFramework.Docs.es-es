---
title: Migrar desde EF6 EF Core - trasladar un modelo basado en código
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052955"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Trasladar un modelo basado en código de EF6 a EF Core

Si ha leído todas las observaciones y está listo para portar, estas son algunas directrices para ayudarle a empezar a trabajar.

## <a name="install-ef-core-nuget-packages"></a>Instalar paquetes de NuGet de núcleo de EF

Para usar EF Core, instalar el paquete NuGet para el proveedor de base de datos que desea usar. Por ejemplo, cuando el destino es SQL Server, se instalan `Microsoft.EntityFrameworkCore.SqlServer`. Vea [proveedores de base de datos](../../core/providers/index.md) para obtener más información.

Si va a usar migraciones, también debe instalar el `Microsoft.EntityFrameworkCore.Tools` paquete.

Es correcto dejar el paquete de NuGet EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser usado en paralelo en la misma aplicación. Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete le ayudarán a dar errores de compilación en fragmentos de código que requieren atención.

## <a name="swap-namespaces"></a>Intercambiar los espacios de nombres

La mayoría de las API que se utilizan en EF6 están en el `System.Data.Entity` espacio de nombres (y relacionados con subespacios de nombres). El primer cambio en el código consiste en cambiar a la `Microsoft.EntityFrameworkCore` espacio de nombres. ¿Normalmente empieza con el archivo de código de contexto derivada y, a continuación, calcular a partir de ahí, solucionar errores de compilación cuando se producen.

## <a name="context-configuration-connection-etc"></a>Configuración del contexto (conexión etcetera).

Como se describe en [Asegúrese EF Core le trabajo para su aplicación](ensure-requirements.md), Core EF tiene menos magia alrededor de detectar la base de datos para conectarse a. Debe invalidar el `OnConfiguring` método en su contexto derivada y use la API específica del proveedor de base de datos para configurar la conexión a la base de datos.

La mayoría de las aplicaciones de EF6 almacenan la cadena de conexión en las aplicaciones `App/Web.config` archivo. En el núcleo de EF, lea esta cadena de conexión mediante el `ConfigurationManager` API. Puede que necesite agregar una referencia a la `System.Configuration` ensamblado de framework para poder usar esta API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Actualizar el código

En este punto, es una cuestión de solucionar errores de compilación y revisión de código para ver si los cambios de comportamiento le afecta.

## <a name="existing-migrations"></a>Migraciones existentes

Realmente no es una manera factible de puerto existentes migraciones EF6 EF Core.

Si es posible, es mejor que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, a continuación, mediante el uso EF Core de punto de inicio migrar el esquema desde el se supone. Para ello, usaría el `Add-Migration` comando para agregar una migración una vez que el modelo se transfieren a EF Core. A continuación, se quitará todo el código de la `Up` y `Down` métodos de la migración con scaffolding. Migraciones posteriores comparará al modelo al que la migración inicial se scaffolding.

## <a name="test-the-port"></a>Probar el puerto

Solo porque se compila la aplicación, no significa que se transfieren correctamente a EF Core. Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento se vea afectado negativamente la aplicación.
