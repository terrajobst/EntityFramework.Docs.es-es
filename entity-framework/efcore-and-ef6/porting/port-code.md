---
title: Migración de EF6 a EF Core-Porting de un modelo basado en código-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181212"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portabilidad de un modelo basado en código EF6 a EF Core

Si ha leído todas las advertencias y está listo para migrar, estas son algunas directrices que le ayudarán a empezar.

## <a name="install-ef-core-nuget-packages"></a>Instalación de EF Core paquetes NuGet

Para usar EF Core, instale el paquete NuGet para el proveedor de base de datos que desea usar. Por ejemplo, cuando el destino es SQL Server, instalaría `Microsoft.EntityFrameworkCore.SqlServer`. Consulte [proveedores de bases de datos](../../core/providers/index.md) para obtener más información.

Si tiene previsto usar migraciones, debe instalar también el paquete `Microsoft.EntityFrameworkCore.Tools`.

Es adecuado dejar el paquete NuGet de EF6 (EntityFramework) instalado, ya que EF Core y EF6 se pueden usar en paralelo en la misma aplicación. Sin embargo, si no va a usar EF6 en ninguna de las áreas de la aplicación, la desinstalación del paquete le ayudará a dar errores de compilación en fragmentos de código que requieren atención.

## <a name="swap-namespaces"></a>Intercambiar espacios de nombres

La mayoría de las API que se usan en EF6 se encuentran en el espacio de nombres `System.Data.Entity` (y los subespacios de nombres relacionados). El primer cambio de código consiste en intercambiar el espacio de nombres `Microsoft.EntityFrameworkCore`. Normalmente, se iniciará con el archivo de código de contexto derivado y, a continuación, se realizará el trabajo desde allí, con lo que se solucionarán los errores de compilación.

## <a name="context-configuration-connection-etc"></a>Configuración de contexto (conexión, etc.)

Tal y como se describe en Asegúrese de que [EF Core funcionará para su aplicación](ensure-requirements.md), EF Core tiene menos magia en torno a la detección de la base de datos a la que conectarse. Tendrá que reemplazar el método `OnConfiguring` en el contexto derivado y usar la API específica del proveedor de base de datos para configurar la conexión a la base de datos.

La mayoría de las aplicaciones EF6 almacenan la cadena de conexión en el archivo Applications `App/Web.config`. En EF Core, lea esta cadena de conexión con la API `ConfigurationManager`. Es posible que tenga que agregar una referencia al ensamblado de .NET Framework `System.Configuration` para poder usar esta API.

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

## <a name="update-your-code"></a>Actualización del código

En este punto, es cuestión de solucionar los errores de compilación y revisar el código para ver si los cambios de comportamiento afectarán.

## <a name="existing-migrations"></a>Migraciones existentes

No es realmente una manera viable de migrar las migraciones de EF6 existentes a EF Core.

Si es posible, es mejor suponer que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, a continuación, iniciar la migración del esquema desde ese punto mediante EF Core. Para ello, use el comando `Add-Migration` para agregar una migración una vez que el modelo se haya trasladado a EF Core. A continuación, se quitará todo el código de los métodos `Up` y `Down` de la migración con scaffolding. Las migraciones posteriores se compararán con el modelo cuando se haya aplicado scaffolding a la migración inicial.

## <a name="test-the-port"></a>Prueba del puerto

El hecho de que la aplicación se compile, no significa que se haya trasladado correctamente a EF Core. Tendrá que probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento afecte negativamente a la aplicación.
