---
title: Transacciones - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: fe4c0d6ad7ccb2e97dc94fbf2eb26a41e7fbcb19
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="using-transactions"></a>Uso de transacciones

Las transacciones permiten varias operaciones de base de datos debe procesarse de forma atómica. Si la transacción se confirma, todas las operaciones se aplican correctamente a la base de datos. Si la transacción se revierte, ninguna de las operaciones se aplican a la base de datos.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) de este artículo en GitHub.

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
> `DbContextOptionsBuilder` es la API que utiliza en `DbContext.OnConfiguring` para configurar el contexto, ahora va a utilizarlo externamente para crear `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

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

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Usar DbTransactions externos (solo bases de datos relacionales)

Si usas varias tecnologías de acceso a datos para tener acceso a una base de datos relacional, puede que desee compartir un a transacción entre las operaciones realizadas por estas tecnologías diferentes.

El ejemplo siguiente, se muestra cómo realizar una operación de SqlClient de ADO.NET y una operación de Entity Framework Core en la misma transacción.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Uso de System.Transactions

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

Es posible utilizar transacciones de ambiente si necesita coordinar a través de un ámbito mayor.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

También es posible dar de alta en una transacción explícita.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>Limitaciones de System.Transactions  

1. Núcleo EF se basa en proveedores de base de datos para implementar la compatibilidad de System.Transactions. Aunque el soporte es bastante común entre los proveedores de ADO.NET para.NET Framework, la API sólo se ha agregado recientemente a núcleo de .NET y, por tanto, soporte técnico no es ser tan extendido. Si un proveedor no implementa compatibilidad con System.Transactions, es posible que las llamadas a estas API se omitirán completamente. SqlClient para .NET Core admitirlo 2.1 en adelante. SqlClient para .NET 2.0 de núcleo producirá una excepción de intenta utilizar la característica. 

   > [!IMPORTANT]  
   > Se recomienda que pruebe que la API se comporta correctamente con su proveedor antes de confiar en él para administrar las transacciones. Se recomienda ponerse en contacto con el encargado del proveedor de base de datos si no es así. 

2. A partir de la versión 2.1, la implementación de System.Transactions en .NET Core no incluye compatibilidad con transacciones distribuidas, por lo tanto, no se puede usar `TransactionScope` o `CommitableTransaction` para coordinar las transacciones a través de varios administradores de recursos. 
