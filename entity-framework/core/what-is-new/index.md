---
title: Novedades de EF Core 2.0 - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
ms.technology: entity-framework-core
uid: core/what-is-new/index
ms.openlocfilehash: a0e54fe85e810793913b9c5fb5e745419a0983b2
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="0540a-102">Nuevas características de EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="0540a-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="0540a-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="0540a-103">.NET Standard 2.0</span></span>
<span data-ttu-id="0540a-104">EF Core tiene ahora como destino .NET Standard 2.0, lo que significa que puede trabajar con .NET Core 2.0, .NET Framework 4.6.1 y otras bibliotecas que implementan .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="0540a-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="0540a-105">Vea [Implementaciones de .NET compatibles](../platforms/index.md) para obtener más detalles sobre lo que se admite.</span><span class="sxs-lookup"><span data-stu-id="0540a-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="0540a-106">Modelado</span><span class="sxs-lookup"><span data-stu-id="0540a-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="0540a-107">División de tablas</span><span class="sxs-lookup"><span data-stu-id="0540a-107">Table splitting</span></span>

<span data-ttu-id="0540a-108">Ahora es posible asignar dos o más tipos de entidad a la misma tabla en la que se van a compartir las columnas de clave principal y cada fila va a corresponder a dos o más entidades.</span><span class="sxs-lookup"><span data-stu-id="0540a-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="0540a-109">Para usar la división de tabla, debe configurarse una relación de identificación (donde las propiedades de clave externa forman la clave principal) entre todos los tipos de entidad que comparten la tabla:</span><span class="sxs-lookup"><span data-stu-id="0540a-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="0540a-110">Tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="0540a-110">Owned types</span></span>

<span data-ttu-id="0540a-111">Un tipo de entidad de propiedad puede compartir el mismo tipo CLR con otro tipo de entidad de propiedad, pero dado que no se puede identificar simplemente por el tipo CLR, debe haber una navegación a él desde otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="0540a-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="0540a-112">La entidad que contiene la navegación definitoria es el propietario.</span><span class="sxs-lookup"><span data-stu-id="0540a-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="0540a-113">Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0540a-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="0540a-114">Por convención, se crea una clave principal paralela para el tipo de propiedad y se asigna a la misma tabla que el propietario mediante la división de tabla.</span><span class="sxs-lookup"><span data-stu-id="0540a-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="0540a-115">Esto permite usar tipos de propiedad de forma similar al modo en que se usan los tipos complejos en EF6:</span><span class="sxs-lookup"><span data-stu-id="0540a-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

### <a name="model-level-query-filters"></a><span data-ttu-id="0540a-116">Filtros de consulta de nivel de modelo</span><span class="sxs-lookup"><span data-stu-id="0540a-116">Model-level query filters</span></span>

<span data-ttu-id="0540a-117">EF Core 2.0 incluye una nueva característica que se denomina filtros de consulta de nivel de modelo.</span><span class="sxs-lookup"><span data-stu-id="0540a-117">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="0540a-118">Esta característica permite que los predicados de consulta LINQ (una expresión booleana que normalmente se pasa al operador de consulta Where de LINQ) se definan directamente en tipos de entidad del modelo de metadatos (normalmente en OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="0540a-118">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="0540a-119">Estos filtros se aplican automáticamente a las consultas LINQ que implican a esos tipos de entidad, incluidos aquellos a los que se hace referencia de forma indirecta, por ejemplo mediante el uso de Include o de referencias de propiedad de navegación directas.</span><span class="sxs-lookup"><span data-stu-id="0540a-119">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="0540a-120">Algunas aplicaciones comunes de esta característica son:</span><span class="sxs-lookup"><span data-stu-id="0540a-120">Some common applications of this feature are:</span></span>

- <span data-ttu-id="0540a-121">Eliminación temporal: un tipo de entidad define una propiedad IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="0540a-121">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="0540a-122">Servicios multiinquilino: un tipo de entidad define una propiedad TenantId.</span><span class="sxs-lookup"><span data-stu-id="0540a-122">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="0540a-123">Este es un ejemplo sencillo que muestra la característica para los dos escenarios mencionados arriba:</span><span class="sxs-lookup"><span data-stu-id="0540a-123">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="0540a-124">Se define un filtro de nivel de modelo que implementa los servicios multiinquilino y la eliminación temporal para instancias del tipo de entidad ```Post```.</span><span class="sxs-lookup"><span data-stu-id="0540a-124">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="0540a-125">Observe el uso de una propiedad de nivel de instancia DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="0540a-125">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="0540a-126">Los filtros de nivel de modelo usan el valor de la instancia de contexto correcta.</span><span class="sxs-lookup"><span data-stu-id="0540a-126">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="0540a-127">Es decir, la que está ejecutando la consulta.</span><span class="sxs-lookup"><span data-stu-id="0540a-127">I.e. the one that is executing the query.</span></span>

<span data-ttu-id="0540a-128">Los filtros se pueden deshabilitar para consultas LINQ individuales mediante el operador IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="0540a-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="0540a-129">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="0540a-129">Limitations</span></span>

- <span data-ttu-id="0540a-130">No se permiten las referencias de navegación.</span><span class="sxs-lookup"><span data-stu-id="0540a-130">Navigation references are not allowed.</span></span> <span data-ttu-id="0540a-131">Esta característica se puede agregar en función de los comentarios.</span><span class="sxs-lookup"><span data-stu-id="0540a-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="0540a-132">Solo se pueden definir filtros en el tipo de entidad raíz de una jerarquía.</span><span class="sxs-lookup"><span data-stu-id="0540a-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="0540a-133">Asignación de función escalar de base de datos</span><span class="sxs-lookup"><span data-stu-id="0540a-133">Database scalar function mapping</span></span>

<span data-ttu-id="0540a-134">La versión Preview 2 incluye una importante contribución de [Paul Middleton](https://github.com/pmiddleton) que permite la asignación de funciones escalares de base de datos a stubs de método para que se puedan usar en consultas LINQ y convertirse a SQL.</span><span class="sxs-lookup"><span data-stu-id="0540a-134">Preview 2 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="0540a-135">Esta es una breve descripción de cómo se puede usar la característica:</span><span class="sxs-lookup"><span data-stu-id="0540a-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="0540a-136">Declare un método estático en `DbContext` y anótelo con `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="0540a-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="0540a-137">Los métodos como este se registran automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0540a-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="0540a-138">Una vez registrado un método, puede usarlo en cualquier lugar de las consultas:</span><span class="sxs-lookup"><span data-stu-id="0540a-138">Once a method has been registered you can use it anywhere in your queries:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="0540a-139">Algunas observaciones:</span><span class="sxs-lookup"><span data-stu-id="0540a-139">A few things to note:</span></span>

- <span data-ttu-id="0540a-140">Por convención, el nombre del método se usa como nombre de una función (en este caso una función definida por el usuario) al generar el SQL, pero puede invalidar el nombre y el esquema durante el registro del método.</span><span class="sxs-lookup"><span data-stu-id="0540a-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="0540a-141">Actualmente solo se admiten las funciones escalares.</span><span class="sxs-lookup"><span data-stu-id="0540a-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="0540a-142">Debe crear la función asignada en la base de datos, por ejemplo, las migraciones de EF Core no se encargan de crearla.</span><span class="sxs-lookup"><span data-stu-id="0540a-142">You must create the mapped function in the database, e.g. EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="0540a-143">Configuración de tipo independiente para Code First</span><span class="sxs-lookup"><span data-stu-id="0540a-143">Self-contained type configuration for code first</span></span>

<span data-ttu-id="0540a-144">En EF6 era posible encapsular la configuración de Code First de un tipo de entidad concreto al derivarlo de *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="0540a-144">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="0540a-145">En EF Core 2.0 se vuelve a incluir este patrón:</span><span class="sxs-lookup"><span data-stu-id="0540a-145">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="0540a-146">Alto rendimiento</span><span class="sxs-lookup"><span data-stu-id="0540a-146">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="0540a-147">Agrupación de DbContext</span><span class="sxs-lookup"><span data-stu-id="0540a-147">DbContext pooling</span></span>

<span data-ttu-id="0540a-148">El patrón básico para usar EF Core en una aplicación de ASP.NET Core normalmente implica el registro de un tipo de DbContext personalizado en el sistema de inserción de dependencias y la posterior obtención de instancias de ese tipo a través de los parámetros del constructor de los controladores.</span><span class="sxs-lookup"><span data-stu-id="0540a-148">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="0540a-149">Esto significa que se crea una nueva instancia de DbContext para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0540a-149">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="0540a-150">En la versión 2.0 se incorpora una nueva manera de registrar tipos de DbContext personalizados en la inserción de dependencias que presenta un grupo de instancias de DbContext reutilizables de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="0540a-150">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="0540a-151">Para usar la agrupación de DbContext, use ```AddDbContextPool``` en lugar de ```AddDbContext``` durante el registro del servicio:</span><span class="sxs-lookup"><span data-stu-id="0540a-151">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="0540a-152">Si se usa este método, en el momento en que un controlador solicita una instancia de DbContext, primero se comprueba si hay una disponible en el grupo.</span><span class="sxs-lookup"><span data-stu-id="0540a-152">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="0540a-153">Una vez que termina el procesamiento de la solicitud, se restablece cualquier estado en la instancia y la propia instancia se devuelve al grupo.</span><span class="sxs-lookup"><span data-stu-id="0540a-153">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="0540a-154">Esto es conceptualmente similar a la forma en que funciona la agrupación de conexiones en los proveedores de ADO.NET y tiene la ventaja de ahorrar algunos de los costos de inicialización de la instancia de DbContext.</span><span class="sxs-lookup"><span data-stu-id="0540a-154">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="0540a-155">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="0540a-155">Limitations</span></span>

<span data-ttu-id="0540a-156">El nuevo método presenta algunas limitaciones con respecto a lo que se puede hacer en el método ```OnConfiguring()``` de DbContext.</span><span class="sxs-lookup"><span data-stu-id="0540a-156">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="0540a-157">Evite el uso de la agrupación de DbContext si mantiene su propio estado (por ejemplo, campos privados) en la clase derivada DbContext que no debe compartirse con otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0540a-157">Avoid using DbContext Pooling if you maintain your own state (e.g. private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="0540a-158">EF Core solo restablece el estado del que es consciente antes de agregar una instancia de DbContext al grupo.</span><span class="sxs-lookup"><span data-stu-id="0540a-158">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="0540a-159">Consultas compiladas de manera explícita</span><span class="sxs-lookup"><span data-stu-id="0540a-159">Explicitly compiled queries</span></span>

<span data-ttu-id="0540a-160">Esta es la segunda característica opcional diseñada para ofrecer ventajas en escenarios de gran escala.</span><span class="sxs-lookup"><span data-stu-id="0540a-160">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="0540a-161">Las API de consulta compiladas de forma manual o explícita han estado disponibles en versiones anteriores de EF y también en LINQ to SQL para permitir que las aplicaciones almacenen en caché la traducción de consultas de modo que se puedan calcular una sola vez y ejecutarse muchas veces.</span><span class="sxs-lookup"><span data-stu-id="0540a-161">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="0540a-162">Aunque en general EF Core puede compilar y almacenar en caché automáticamente las consultas en función de una representación con hash de las expresiones de consulta, este mecanismo puede usarse para obtener una pequeña mejora de rendimiento al omitir el cálculo del hash y la búsqueda en caché, lo que permite que la aplicación use una consulta ya compilada mediante la invocación de un delegado.</span><span class="sxs-lookup"><span data-stu-id="0540a-162">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="0540a-163">Seguimiento de cambios</span><span class="sxs-lookup"><span data-stu-id="0540a-163">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="0540a-164">La asociación permite realizar un seguimiento de un gráfico de entidades nuevas y existentes.</span><span class="sxs-lookup"><span data-stu-id="0540a-164">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="0540a-165">EF Core admite la generación automática de valores de clave a través de una serie de mecanismos.</span><span class="sxs-lookup"><span data-stu-id="0540a-165">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="0540a-166">Al usar esta característica, se genera un valor si la propiedad de clave es el valor predeterminado de CLR, normalmente cero o null.</span><span class="sxs-lookup"><span data-stu-id="0540a-166">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="0540a-167">Esto significa que se puede pasar un gráfico de entidades a `DbContext.Attach` o `DbSet.Attach` y que EF Core marca aquellas entidades que tienen una clave ya establecida como `Unchanged`, mientras que las que no tienen establecida una clave se marcan como `Added`.</span><span class="sxs-lookup"><span data-stu-id="0540a-167">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="0540a-168">Esto facilita la tarea de asociar un gráfico de entidades mixtas nuevas y existentes al usar claves generadas.</span><span class="sxs-lookup"><span data-stu-id="0540a-168">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="0540a-169">`DbContext.Update` y `DbSet.Update` funcionan de la misma manera, salvo que las entidades con una clave establecida se marcan como `Modified` en lugar de `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="0540a-169">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="0540a-170">Consulta</span><span class="sxs-lookup"><span data-stu-id="0540a-170">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="0540a-171">Traducción de LINQ mejorada</span><span class="sxs-lookup"><span data-stu-id="0540a-171">Improved LINQ Translation</span></span>

<span data-ttu-id="0540a-172">Permite que más consultas se ejecuten correctamente, con más lógica evaluada en la base de datos (en lugar de en memoria) y menos datos innecesariamente recuperados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0540a-172">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="0540a-173">Mejoras de GroupJoin</span><span class="sxs-lookup"><span data-stu-id="0540a-173">GroupJoin improvements</span></span>

<span data-ttu-id="0540a-174">Este trabajo mejora el SQL que se genera para las combinaciones agrupadas.</span><span class="sxs-lookup"><span data-stu-id="0540a-174">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="0540a-175">Las combinaciones agrupadas suelen ser un resultado de subconsultas en propiedades de navegación opcionales.</span><span class="sxs-lookup"><span data-stu-id="0540a-175">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="0540a-176">Interpolación de cadenas en FromSql y ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="0540a-176">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="0540a-177">C# 6 presentó la interpolación de cadenas, una característica que permite insertar expresiones de C# directamente en literales de cadena, lo que proporciona una forma útil de compilar cadenas en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0540a-177">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="0540a-178">En EF Core 2.0 se ha agregado compatibilidad especial con las cadenas interpoladas a las dos API principales que aceptan cadenas SQL sin formato: ```FromSql``` y ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="0540a-178">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="0540a-179">Esta nueva compatibilidad permite que la interpolación de cadenas de C# se use de forma "segura".</span><span class="sxs-lookup"><span data-stu-id="0540a-179">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="0540a-180">Es decir, de una forma que protege frente a errores de inserción de SQL comunes que pueden producirse al crear SQL de forma dinámica en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0540a-180">I.e. in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="0540a-181">A continuación se muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0540a-181">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="0540a-182">En este ejemplo hay dos variables insertadas en la cadena de formato SQL.</span><span class="sxs-lookup"><span data-stu-id="0540a-182">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="0540a-183">EF Core genera el SQL siguiente:</span><span class="sxs-lookup"><span data-stu-id="0540a-183">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="0540a-184">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="0540a-184">EF.Functions.Like()</span></span>

<span data-ttu-id="0540a-185">Se ha agregado la propiedad EF.Functions, que EF Core o los proveedores pueden usar para definir métodos que se asignen a los operadores o a las funciones de base de datos de forma que se puedan invocar en consultas LINQ.</span><span class="sxs-lookup"><span data-stu-id="0540a-185">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="0540a-186">El primer ejemplo de este método es Like():</span><span class="sxs-lookup"><span data-stu-id="0540a-186">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

<span data-ttu-id="0540a-187">Observe que Like() incluye una implementación en memoria, lo que puede resultar útil al trabajar en una base de datos en memoria o cuando es necesario evaluar el predicado en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="0540a-187">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur con the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="0540a-188">Administración de bases de datos</span><span class="sxs-lookup"><span data-stu-id="0540a-188">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="0540a-189">Enlace de pluralización para scaffolding de DbContext</span><span class="sxs-lookup"><span data-stu-id="0540a-189">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="0540a-190">EF Core 2.0 presenta un nuevo servicio *IPluralizer* que se usa para singularizar nombres de tipo de entidad y pluralizar nombres DbSet.</span><span class="sxs-lookup"><span data-stu-id="0540a-190">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="0540a-191">La implementación predeterminada no está operativa, por lo que simplemente se trata de un enlace en el que los usuarios pueden conectar fácilmente su propio pluralizador.</span><span class="sxs-lookup"><span data-stu-id="0540a-191">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="0540a-192">Este es el aspecto del enlace de un desarrollador de su propio pluralizador:</span><span class="sxs-lookup"><span data-stu-id="0540a-192">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="0540a-193">Otros</span><span class="sxs-lookup"><span data-stu-id="0540a-193">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="0540a-194">Traslado del proveedor de SQLite de ADO.NET a SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="0540a-194">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="0540a-195">Esto proporciona una solución más robusta en Microsoft.Data.Sqlite para distribuir archivos binarios nativos de SQLite en distintas plataformas.</span><span class="sxs-lookup"><span data-stu-id="0540a-195">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="0540a-196">Solo un proveedor por modelo</span><span class="sxs-lookup"><span data-stu-id="0540a-196">Only one provider per model</span></span>
<span data-ttu-id="0540a-197">Mejora considerablemente la forma en que los proveedores pueden interactuar con el modelo y simplifica el funcionamiento de las convenciones, las anotaciones y las API fluidas con distintos proveedores.</span><span class="sxs-lookup"><span data-stu-id="0540a-197">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="0540a-198">EF Core 2.0 ahora compila un elemento [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diferente para cada proveedor que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="0540a-198">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="0540a-199">Esto suele ser transparente para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0540a-199">This is usually transparent to the application.</span></span> <span data-ttu-id="0540a-200">Esto ha permitido una simplificación de las API de metadatos de nivel inferior, de modo que cualquier acceso a *conceptos de metadatos relacionales comunes* siempre se realiza mediante una llamada a `.Relational` en lugar de a `.SqlServer`, `.Sqlite`, etc.</span><span class="sxs-lookup"><span data-stu-id="0540a-200">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="0540a-201">Registro y diagnóstico consolidados</span><span class="sxs-lookup"><span data-stu-id="0540a-201">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="0540a-202">Los mecanismos de registro (basados en ILogger) y diagnóstico (basados en DiagnosticSource) ahora comparten más código.</span><span class="sxs-lookup"><span data-stu-id="0540a-202">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="0540a-203">Los identificadores de evento de los mensajes enviados a un elemento ILogger han cambiado en 2.0.</span><span class="sxs-lookup"><span data-stu-id="0540a-203">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="0540a-204">Los identificadores de evento ahora son únicos en el código de EF Core.</span><span class="sxs-lookup"><span data-stu-id="0540a-204">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="0540a-205">Ahora, estos mensajes también siguen el patrón estándar de registro estructurado que usa, por ejemplo, MVC.</span><span class="sxs-lookup"><span data-stu-id="0540a-205">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="0540a-206">Las categorías de registrador también han cambiado.</span><span class="sxs-lookup"><span data-stu-id="0540a-206">Logger categories have also changed.</span></span> <span data-ttu-id="0540a-207">Ahora hay un conjunto conocido de categorías a las que se accede a través de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="0540a-207">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="0540a-208">Los eventos de DiagnosticSource ahora usan los mismos nombres de identificador de evento que los mensajes de `ILogger` correspondientes.</span><span class="sxs-lookup"><span data-stu-id="0540a-208">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
