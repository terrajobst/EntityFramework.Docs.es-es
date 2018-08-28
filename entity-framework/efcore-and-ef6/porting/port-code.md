---
title: 'Portabilidad de EF6 a EF Core: trasladar un modelo basado en código'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997054"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portabilidad de un modelo basado en código de EF6 a EF Core

Si ha leído todas las advertencias y está listo para el puerto, a continuación, estas son algunas directrices que le ayudarán a empezar a trabajar.

## <a name="install-ef-core-nuget-packages"></a>Instalar paquetes de NuGet de EF Core

Para usar EF Core, instale el paquete NuGet para el proveedor de base de datos que desea usar. Por ejemplo, cuando el destino es SQL Server, instalaría `Microsoft.EntityFrameworkCore.SqlServer`. Consulte [proveedores de base de datos](../../core/providers/index.md) para obtener más información.

Si planea usar migraciones, también debe instalar el `Microsoft.EntityFrameworkCore.Tools` paquete.

Es correcto dejar el paquete NuGet de EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser utilizado por en paralelo en la misma aplicación. Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete para obtener errores de compilación en fragmentos de código que necesitan atención.

## <a name="swap-namespaces"></a>Intercambiar los espacios de nombres

La mayoría de las API que se utiliza en EF6 está en el `System.Data.Entity` espacio de nombres (y relacionados con subespacios de nombres). El primer cambio de código consiste en cambiar a la `Microsoft.EntityFrameworkCore` espacio de nombres. ¿Suelen comenzar con el archivo de código del contexto derivado y, a continuación, calcular a partir de ahí, dirige a medida que se producen errores de compilación.

## <a name="context-configuration-connection-etc"></a>Configuración del contexto (conexión etcetera).

Como se describe en [asegurarse de EF Core le funcione para su aplicación](ensure-requirements.md), EF Core tiene menos magia en torno a detectar para conectarse a la base de datos. Deberá reemplazar el `OnConfiguring` método en su contexto derivado y usar la API específica del proveedor de base de datos para configurar la conexión a la base de datos.

La mayoría de las aplicaciones de EF6 almacena la cadena de conexión en las aplicaciones `App/Web.config` archivo. En EF Core, lea esta cadena de conexión mediante el `ConfigurationManager` API. Es posible que deba agregar una referencia a la `System.Configuration` ensamblado de marco de trabajo para poder usar esta API.

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

En este punto, es una cuestión de solucionar errores de compilación y revisión de código para ver si los cambios de comportamiento le afectará.

## <a name="existing-migrations"></a>Migraciones existentes

Realmente no es una manera sea factible de puerto existente de las migraciones de EF6 a EF Core.

Si es posible, es mejor suponer que todas las migraciones anteriores de EF6 se han aplicado a la base de datos y, a continuación, usar EF Core de punto de migración del esquema desde el el inicio. Para ello, usaría el `Add-Migration` comando para agregar una migración una vez que el modelo se ha trasladado a EF Core. A continuación, debe quitar todos los códigos de la `Up` y `Down` métodos de la migración con scaffolding. Las migraciones posteriores se compararán en el modelo cuando se ha aplicado scaffolding a que la migración inicial.

## <a name="test-the-port"></a>Probar el puerto

Simplemente porque se compila la aplicación, no significa que es portada correctamente a EF Core. Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento han afectado negativamente la aplicación.
