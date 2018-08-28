---
title: Personalización de la tabla de historial de migraciones - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996766"
---
# <a name="customizing-the-migrations-history-table"></a>Personalización de la tabla de historial de migraciones
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

> [!NOTE]
> En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos. Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="what-is-migrations-history-table"></a>¿Qué es la tabla de historial de migraciones?

Tabla de historial de migraciones es una tabla usada por migraciones de Code First para almacenar los detalles acerca de las migraciones que se aplica a la base de datos. De forma predeterminada, el nombre de la tabla en la base de datos es \_ \_MigrationHistory y se crea cuando se aplica la primera migración, haga lo la base de datos. En Entity Framework 5 esta tabla era una tabla del sistema si la aplicación utiliza la base de datos de Microsoft Sql Server. Esto ha cambiado pero en Entity Framework 6 y la tabla de historial de migraciones ya no se marca una tabla del sistema.

## <a name="why-customize-migrations-history-table"></a>¿Por qué personalizar la tabla de historial de migraciones?

Tabla de historial de migraciones se supone que debe usarse únicamente por migraciones de Code First y cambiarla manualmente puede interrumpir las migraciones. Sin embargo a veces la configuración predeterminada no es adecuada y la tabla debe personalizarse, por ejemplo:

-   Deberá cambiar los nombres o las facetas de las columnas para habilitar un 3<sup>rd</sup> proveedor migraciones
-   Para cambiar el nombre de la tabla
-   Debe usar un esquema no predeterminado para el \_ \_tabla MigrationHistory
-   Necesita almacenar datos adicionales para una versión concreta del contexto y, por tanto, deberá agregar una columna adicional a la tabla

## <a name="words-of-precaution"></a>Palabras de la medida de precaución

Cambiar la tabla de historial de migración es eficaz, pero debe tener cuidado de no abusar. En tiempo de ejecución EF actualmente no comprueba si la tabla de historial de migraciones personalizado es compatible con el tiempo de ejecución. Si la aplicación no es posible interrumpir en tiempo de ejecución o se comportan de manera impredecible. Esto es incluso más importante si usa varios contextos por base de datos en cuyo caso varios contextos pueden usar la misma tabla de historial de migración para almacenar información acerca de las migraciones.

## <a name="how-to-customize-migrations-history-table"></a>¿Cómo personalizar la tabla de historial de migraciones?

Antes de empezar necesita saber que puede personalizar la tabla de historial de migraciones antes de aplicar la primera migración. Ahora, en el código.

En primer lugar, deberá crear una clase derivada de la clase System.Data.Entity.Migrations.History.HistoryContext. La clase HistoryContext se deriva de la clase DbContext para que la configuración de la tabla de historial de migraciones es muy similar a la configuración de los modelos EF con la API fluida. Solo necesita invalidar el método OnModelCreating y usar la API fluida para configurar la tabla.

>[!NOTE]
> Normalmente, al configurar los modelos EF no es necesario llamar a base. OnModelCreating() desde el método OnModelCreating reemplazado, ya que el DbContext.OnModelCreating() tiene un cuerpo vacío. No es el caso cuando la configuración de la tabla de historial de migraciones. En este caso la primera cosa que hacer en la invalidación OnModelCreating() es realmente llame a la base. OnModelCreating(). Esto configurará la tabla de historial de migraciones en el modo predeterminado que, a continuación, ajustar en el método de reemplazo.

Supongamos que desea cambiar el nombre de la tabla de historial de migraciones y colocarlo en un esquema personalizado denominado "admin". Además el DBA desea cambiar el nombre de la columna MigrationId migración\_identificador.  Se puede conseguir mediante la creación de la siguiente clase que deriva de HistoryContext:

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Cuando esté listo el HistoryContext personalizado deberá asegurarse de que EF lo sepa registrarlo mediante [configuración basada en código](http://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

Eso es prácticamente todo. Ahora puede ir a la consola de administrador de paquetes, Enable-Migrations, Add-Migration y, finalmente, Update-Database. Esto debería producir agregar a la base de datos de una tabla de historial de migraciones configurada según los detalles especificados en la clase HistoryContext derivada.

![Base de datos](~/ef6/media/database.png)
