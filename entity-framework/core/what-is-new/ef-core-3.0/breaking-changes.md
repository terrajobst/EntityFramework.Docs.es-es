---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: a7e1a03bf1131cd53123f5cc39b07bed94619b44
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463383"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)

> [!IMPORTANT]
> Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.

Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.
Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).
Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Las consultas LINQ ya no se evalúan en el cliente

[Problema de seguimiento n.º 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

> [!IMPORTANT]
> Es el anuncio previo de este cambio.
Todavía no se ha incluido en ninguna versión preliminar 3.0.

**Comportamiento anterior**

Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.
De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.

**Comportamiento nuevo**

A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).
Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.

**Por qué**

La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.
Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.
Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.
Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.
Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.

Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.

**Mitigaciones**

Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core ya no forma parte del marco compartido ASP.NET Core

[Anuncios del problema de seguimiento n.º 325](https://github.com/aspnet/Announcements/issues/325)

Este cambio se ha introducido en ASP.NET Core 3.0, versión preliminar 1. 

**Comportamiento anterior**

Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.

**Comportamiento nuevo**

A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.

**Por qué**

Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no. Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.

Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.
Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.

**Mitigaciones**

Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.

## <a name="query-execution-is-logged-at-debug-level"></a>La ejecución de consultas se registra en el nivel de depuración

[Problema de seguimiento n.º 14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, la ejecución de consultas y otros comandos se registraba en el nivel `Info`.

**Comportamiento nuevo**

A partir de EF Core 3.0, el registro de ejecución de comandos y SQL se produce en el nivel `Debug`.

**Por qué**

Este cambio se ha realizado para reducir el ruido en el nivel de registro `Info`.

**Mitigaciones**

Este evento de registro se define mediante `RelationalEventId.CommandExecuting` con el id. de evento 20100.
Para volver a registrar SQL en el nivel `Info`, active el registro en el nivel `Debug` y filtre solo por este evento.


## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Los valores de clave temporal ya no se establecen en instancias de entidad

[Problema de seguimiento n.º 12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).

**Comportamiento anterior**

Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.
Normalmente, estos valores temporales eran números negativos grandes.

**Comportamiento nuevo**

A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.

**Por qué**

Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`. 

**Mitigaciones**

Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.
Esto se puede evitar con las siguientes situaciones:
* No se usan claves generadas por el almacén.
* Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.
* Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.
Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.

## <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges respeta los valores de clave generados por el almacén

[Problema de seguimiento n.º 14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.

**Comportamiento nuevo**

A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.
Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.
Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.

**Por qué**

Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.

**Mitigaciones**

Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.
La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.
Por ejemplo, con la API fluida:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

O bien con anotaciones de datos:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada

[Problema de seguimiento n.º 10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.

**Comportamiento nuevo**

A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.
Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.

**Por qué**

Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.

**Mitigaciones**

El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.
Por ejemplo:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>Los tipos de consulta se consolidan con tipos de entidad

[Problema de seguimiento n.º 14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.
Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).

**Comportamiento nuevo**

Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.
Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.

**Por qué**

Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.
En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.
Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.

**Mitigaciones**

Los elementos siguientes de la API ahora están obsoletos:
* **`ModelBuilder.Query<>()`**: en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.
Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.
* **`DbQuery<>`**: en su lugar se debe usar `DbSet<>`.
* **`DbContext.Query<>()`**: en su lugar se debe usar `DbContext.Set<>()`.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>La API de configuración para las relaciones de tipo de propiedad ha cambiado

[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`. 

**Comportamiento nuevo**

A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.
Por ejemplo:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.
Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.
Por ejemplo:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.

**Por qué**

Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.
A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.

**Mitigaciones**

Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad

[Problema de seguimiento n.º 13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Considere el modelo siguiente:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.
Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.

**Comportamiento nuevo**

A partir de la versión 3.0, EF Core no intentará usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.
Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.
Por ejemplo:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Por qué**

Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.

**Mitigaciones**

Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Cada propiedad usa la generación de claves enteras en memoria independiente

[Problema de seguimiento n.º 6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).

**Comportamiento anterior**

Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.

**Comportamiento nuevo**

A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.
Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.

**Por qué**

Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.

**Mitigaciones**

Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.
En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.

## <a name="backing-fields-are-used-by-default"></a>Los campos de respaldo se usan de forma predeterminada

[Problema de seguimiento n.º 12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).

**Comportamiento anterior**

Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.
La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.

**Comportamiento nuevo**

A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, esa propiedad siempre se leerá y escribirá mediante el campo de respaldo.
Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.

**Por qué**

Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.

**Mitigaciones**

El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad de la API fluida modelBuilder.
Por ejemplo:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Inicio de excepciones si se encuentran varios campos de respaldo compatibles

[Problema de seguimiento n.º 12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).

**Comportamiento anterior**

Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.
Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.

**Comportamiento nuevo**

A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.

**Por qué**

Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.

**Mitigaciones**

En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.
Por ejemplo, con la API fluida:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>Ahora DbContext.Entry realiza una operación DetectChanges local

[Problema de seguimiento n.º 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.
Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.

**Comportamiento nuevo**

A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.
Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.

Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.

Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.

**Por qué**

Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.

**Mitigaciones**

Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>El cliente no genera las claves de matriz de cadena y byte de forma predeterminada

[Problema de seguimiento n.º 14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).

**Comportamiento anterior**

Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.
En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.

**Comportamiento nuevo**

A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.

**Por qué**

Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.

**Mitigaciones**

Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.
Por ejemplo, con la API fluida:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

O bien con anotaciones de datos:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>Ahora ILoggerFactory es un servicio con ámbito

[Problema de seguimiento n.º 14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.

**Comportamiento nuevo**

A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.

**Por qué**

Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.

**Mitigaciones**

Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.
Esto no es habitual.
En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.

Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo se ha fusionado en IDbContextOptionsExtension

[Problema de seguimiento n.º 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

`IDbContextOptionsExtensionWithDebugInfo` era una interfaz opcional adicional ampliada de `IDbContextOptionsExtension` para evitar realizar cambios importantes en la interfaz durante el ciclo de versiones 2.x.

**Comportamiento nuevo**

Ahora las interfaces se combinan en `IDbContextOptionsExtension`.

**Por qué**

Este cambio se ha realizado porque las interfaces son conceptualmente una.

**Mitigaciones**

Cualquier implementación de `IDbContextOptionsExtension` tendrá que actualizarse para admitir el miembro nuevo.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas

[Problema de seguimiento n.º 12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).

**Comportamiento anterior**

Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.
En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.
En estos casos, el intento de carga diferida era no operativo.

**Comportamiento nuevo**

A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.
Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.
Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.
Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.

**Por qué**

Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.

**Mitigaciones**

Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada

[Problema de seguimiento n.º 10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.

**Comportamiento nuevo**

A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción. 

**Por qué**

Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.

**Mitigaciones**

Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.
Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.
Por ejemplo:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>La anotación Relational:TypeMapping ahora es simplemente TypeMapping

[Problema de seguimiento n.º 9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).

**Comportamiento anterior**

El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".

**Comportamiento nuevo**

Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".

**Por qué**

Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.

**Mitigaciones**

Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.
La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable en un tipo derivado inicia una excepción 

[Problema de seguimiento n.º 11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido. 

**Comportamiento nuevo**

A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.

**Por qué**

En la actualidad no se considera válido asignar un tipo derivado a otra tabla.
Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.

**Mitigaciones**

Quite todos los intentos de asignar tipos derivados a otras tablas.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex se ha reemplazado por HasIndex 

[Problema de seguimiento n.º 12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.

**Comportamiento nuevo**

A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.
Use `HasIndex().ForSqlServerInclude()`.

**Por qué**

Este cambio se ha realizado para consolidar la API para índices con `Includes` en un mismo lugar para todos los proveedores de base de datos.

**Mitigaciones**

Use la API nueva, como se ha mostrado anteriormente.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core ya no envía pragma para el cumplimiento de SQLite FK

[Problema de seguimiento n.º 12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).

**Comportamiento anterior**

Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.

**Comportamiento nuevo**

A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.

**Por qué**

Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.

**Mitigaciones**

Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.
Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3

**Comportamiento anterior**

Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.

**Comportamiento nuevo**

A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.

**Por qué**

Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.

**Mitigaciones**

Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.
