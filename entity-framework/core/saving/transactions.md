---
title: Transacciones - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>Uso de transacciones

Las transacciones permiten varias operaciones de base de datos debe procesarse de forma atómica. Si la transacción se confirma, todas las operaciones se aplican correctamente a la base de datos. Si la transacción se revierte, ninguna de las operaciones se aplican a la base de datos.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) en GitHub.

## <a name="default-transaction-behavior"></a>Comportamiento de transacciones predeterminado

De forma predeterminada, si el proveedor de base de datos admite transacciones, todos los cambios en una sola llamada a `SaveChanges()` se aplican en una transacción. Si se produce un error en cualquiera de los cambios, la transacción se revierte y ninguno de los cambios se aplican a la base de datos. Esto significa que `SaveChanges()` se garantiza que terminen completamente correctamente o dejar la base de datos sin modificar si se produce un error.

Para la mayoría de las aplicaciones, este comportamiento predeterminado es suficiente. Debe controlar solo manualmente las transacciones si sus requisitos de aplicación consideren necesario.

## <a name="controlling-transactions"></a>Control de transacciones

Puede usar el `DbContext.Database` API para iniciar, confirmar y deshacer las transacciones. En el ejemplo siguiente se muestra dos `SaveChanges()` operaciones y un LINQ de consulta que se está ejecutando en una sola transacción.

No todos los proveedores de base de datos admiten transacciones. Algunos proveedores pueden producir o no-op cuando la transacción se denominan API.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Transacción de contexto cruzado (sólo bases de datos relacionales)

También puede compartir una transacción en varias instancias de contexto. Esta funcionalidad solo está disponible cuando se utiliza un proveedor de base de datos relacional porque requiere el uso de `DbTransaction` y `DbConnection`, que son específicos de bases de datos relacionales.

Para compartir una transacción, los contextos deben compartir tanto un `DbConnection` y `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Permitir conexión proporcionarse externamente

Compartir un `DbConnection` requiere la capacidad de pasar una conexión a un contexto cuando se construye.

La manera más fácil para permitir `DbConnection` proporcionarse externamente, consiste en dejar de usar el `DbContext.OnConfiguring` método para configurar el contexto y crear externamente `DbContextOptions` y pasarlas al constructor de contexto.

> [!TIP]  
> `DbContextOptionsBuilder`es la API que se usan en `DbContext.OnConfiguring` para configurar el contexto, ahora va a usar de forma externa para crear `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

Una alternativa consiste en seguir usando `DbContext.OnConfiguring`, pero que aceptan un `DbConnection` que se guarda y, a continuación, se usa en `DbContext.OnConfiguring`.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Conexión de recurso compartido y transacciones

Ahora puede crear varias instancias de contexto que comparten la misma conexión. A continuación, utilice el `DbContext.Database.UseTransaction(DbTransaction)` API para dar de alta ambos contextos en la misma transacción.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a>Usar DbTransactions externos (solo bases de datos relacionales)

Si usas varias tecnologías de acceso a datos para tener acceso a una base de datos relacional, puede que desee compartir un a transacción entre las operaciones realizadas por estas tecnologías diferentes.

El ejemplo siguiente, se muestra cómo realizar una operación de SqlClient de ADO.NET y una operación de Entity Framework Core en la misma transacción.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
