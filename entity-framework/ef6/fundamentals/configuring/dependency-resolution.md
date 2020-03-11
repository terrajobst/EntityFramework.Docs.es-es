---
title: 'Resolución de dependencias: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414798"
---
# <a name="dependency-resolution"></a><span data-ttu-id="d6713-102">Resolución de dependencias</span><span class="sxs-lookup"><span data-stu-id="d6713-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="d6713-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d6713-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d6713-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="d6713-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="d6713-105">A partir de EF6, Entity Framework contiene un mecanismo de uso general para obtener implementaciones de servicios que requiere.</span><span class="sxs-lookup"><span data-stu-id="d6713-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="d6713-106">Es decir, cuando EF usa una instancia de algunas interfaces o clases base, solicitará una implementación concreta de la interfaz o la clase base que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="d6713-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="d6713-107">Esto se logra mediante el uso de la interfaz IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="d6713-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="d6713-108">EF normalmente llama al método GetService y se controla mediante una implementación de IDbDependencyResolver proporcionada por EF o por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d6713-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="d6713-109">Cuando se llama, el argumento de tipo es la interfaz o el tipo de clase base del servicio que se solicita, y el objeto de clave es null o un objeto que proporciona información contextual sobre el servicio solicitado.</span><span class="sxs-lookup"><span data-stu-id="d6713-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="d6713-110">A menos que se indique lo contrario, cualquier objeto devuelto debe ser seguro para subprocesos, ya que se puede utilizar como singleton.</span><span class="sxs-lookup"><span data-stu-id="d6713-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="d6713-111">En muchos casos, el objeto devuelto es un generador, en cuyo caso el propio generador debe ser seguro para subprocesos, pero no es necesario que el objeto devuelto por el generador sea seguro para subprocesos, ya que se solicita una nueva instancia desde el generador para cada uso.</span><span class="sxs-lookup"><span data-stu-id="d6713-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="d6713-112">En este artículo no se incluyen detalles completos sobre cómo implementar IDbDependencyResolver, sino que actúa como referencia para los tipos de servicio (es decir, la interfaz y los tipos de clase base) para los que EF llama a GetService y la semántica del objeto de clave para cada uno de estos llama.</span><span class="sxs-lookup"><span data-stu-id="d6713-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="d6713-113">System. Data. Entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="d6713-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="d6713-114">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-115">**Objeto devuelto**: inicializador de base de datos para el tipo de contexto especificado</span><span class="sxs-lookup"><span data-stu-id="d6713-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="d6713-116">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="d6713-117">FUNC < System. Data. Entity. Migrations. SQL. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="d6713-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="d6713-118">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="d6713-119">**Objeto devuelto**: generador para crear un generador de SQL que se puede usar para las migraciones y otras acciones que hacen que se cree una base de datos, como la creación de bases de datos con inicializadores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d6713-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="d6713-120">**Key**: una cadena que contiene el nombre invariable del proveedor ADO.net que especifica el tipo de base de datos para el que se generará SQL.</span><span class="sxs-lookup"><span data-stu-id="d6713-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="d6713-121">Por ejemplo, se devuelve el generador de SQL Server SQL para la clave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="d6713-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-122">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="d6713-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="d6713-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="d6713-124">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-125">**Objeto devuelto**: el proveedor EF que se va a usar para un nombre invariable de proveedor determinado</span><span class="sxs-lookup"><span data-stu-id="d6713-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="d6713-126">**Key**: una cadena que contiene el nombre invariable del proveedor ADO.net que especifica el tipo de base de datos para el que se necesita un proveedor.</span><span class="sxs-lookup"><span data-stu-id="d6713-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="d6713-127">Por ejemplo, se devuelve el proveedor de SQL Server para la clave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="d6713-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-128">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="d6713-129">System. Data. Entity. Infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="d6713-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="d6713-130">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-131">**Objeto devuelto**: el generador de conexión que se usará cuando EF cree una conexión de base de datos por Convención.</span><span class="sxs-lookup"><span data-stu-id="d6713-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="d6713-132">Es decir, cuando no se proporciona ninguna conexión o cadena de conexión a EF, y no se encuentra ninguna cadena de conexión en App. config o Web. config, este servicio se usa para crear una conexión por Convención.</span><span class="sxs-lookup"><span data-stu-id="d6713-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="d6713-133">Cambiar el generador de conexiones puede permitir que EF use otro tipo de base de datos (por ejemplo, SQL Server Compact Edition) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d6713-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="d6713-134">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-135">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="d6713-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="d6713-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="d6713-137">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-138">**Objeto devuelto**: un servicio que puede generar un token del manifiesto del proveedor a partir de una conexión.</span><span class="sxs-lookup"><span data-stu-id="d6713-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="d6713-139">Este servicio se usa normalmente de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="d6713-139">This service is typically used in two ways.</span></span> <span data-ttu-id="d6713-140">En primer lugar, se puede usar para evitar Code First conectarse a la base de datos al compilar un modelo.</span><span class="sxs-lookup"><span data-stu-id="d6713-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="d6713-141">En segundo lugar, se puede usar para forzar Code First para generar un modelo para una versión de base de datos específica, por ejemplo, para forzar un modelo para SQL Server 2005 incluso si se usa a veces SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="d6713-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="d6713-142">**Duración del objeto**: singleton: el mismo objeto se puede usar varias veces y simultáneamente en diferentes subprocesos</span><span class="sxs-lookup"><span data-stu-id="d6713-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="d6713-143">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="d6713-144">System. Data. Entity. Infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="d6713-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="d6713-145">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-146">**Objeto devuelto**: un servicio que puede obtener un generador de proveedores de una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="d6713-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="d6713-147">En .NET 4,5, el proveedor es accesible públicamente desde la conexión.</span><span class="sxs-lookup"><span data-stu-id="d6713-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="d6713-148">En .NET 4, la implementación predeterminada de este servicio usa algunas heurísticas para encontrar el proveedor coincidente.</span><span class="sxs-lookup"><span data-stu-id="d6713-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="d6713-149">Si se produce un error, se puede registrar una nueva implementación de este servicio para proporcionar una solución adecuada.</span><span class="sxs-lookup"><span data-stu-id="d6713-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="d6713-150">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="d6713-151">FUNC < DbContext, System. Data. Entity. Infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="d6713-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="d6713-152">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-153">**Objeto devuelto**: un generador que generará una clave de caché del modelo para un contexto determinado.</span><span class="sxs-lookup"><span data-stu-id="d6713-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="d6713-154">De forma predeterminada, EF almacena en caché un modelo por tipo DbContext por proveedor.</span><span class="sxs-lookup"><span data-stu-id="d6713-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="d6713-155">Se puede usar una implementación diferente de este servicio para agregar otra información, como el nombre de esquema, a la clave de caché.</span><span class="sxs-lookup"><span data-stu-id="d6713-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="d6713-156">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="d6713-157">System. Data. Entity. Spatial. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="d6713-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="d6713-158">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-159">**Objeto devuelto**: un proveedor espacial de EF que agrega compatibilidad con el proveedor de EF básico para los tipos espaciales Geography y Geometry.</span><span class="sxs-lookup"><span data-stu-id="d6713-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="d6713-160">**Clave**: DbSptialServices se solicita de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="d6713-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="d6713-161">En primer lugar, se solicitan servicios espaciales específicos del proveedor mediante un objeto DbProviderInfo (que contiene el nombre invariable y el token del manifiesto) como clave.</span><span class="sxs-lookup"><span data-stu-id="d6713-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="d6713-162">En segundo lugar, se puede solicitar a DbSpatialServices sin clave.</span><span class="sxs-lookup"><span data-stu-id="d6713-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="d6713-163">Se usa para resolver el "proveedor espacial global", que se usa al crear tipos independientes de DbGeography o DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="d6713-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-164">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="d6713-165">FUNC < System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="d6713-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="d6713-166">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-167">**Objeto devuelto**: generador para crear un servicio que permite a un proveedor implementar reintentos u otro comportamiento cuando las consultas y los comandos se ejecutan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d6713-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="d6713-168">Si no se proporciona ninguna implementación, EF simplemente ejecutará los comandos y propagará las excepciones que se produzcan.</span><span class="sxs-lookup"><span data-stu-id="d6713-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="d6713-169">Por SQL Server este servicio se utiliza para proporcionar una directiva de reintentos que es especialmente útil cuando se ejecuta en servidores de bases de datos basados en la nube, como SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="d6713-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="d6713-170">**Key**: un objeto ExecutionStrategyKey que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará la estrategia de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d6713-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-171">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="d6713-172">FUNC < DbConnection, String, System. Data. Entity. Migrations. History. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="d6713-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="d6713-173">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-174">**Objeto devuelto**: un generador que permite a un proveedor configurar la asignación de HistoryContext a la tabla `__MigrationHistory` utilizada por las migraciones de EF.</span><span class="sxs-lookup"><span data-stu-id="d6713-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="d6713-175">HistoryContext es un DbContext de Code First y se puede configurar mediante la API fluida normal para cambiar aspectos como el nombre de la tabla y las especificaciones de asignación de columnas.</span><span class="sxs-lookup"><span data-stu-id="d6713-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="d6713-176">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-177">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="d6713-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="d6713-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="d6713-179">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-180">**Objeto devuelto**: el proveedor ADO.net que se va a usar para un nombre invariable de proveedor determinado.</span><span class="sxs-lookup"><span data-stu-id="d6713-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="d6713-181">**Key**: una cadena que contiene el nombre invariable del proveedor ADO.net</span><span class="sxs-lookup"><span data-stu-id="d6713-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-182">Normalmente, este servicio no se cambia directamente, ya que la implementación predeterminada usa el registro del proveedor ADO.NET normal.</span><span class="sxs-lookup"><span data-stu-id="d6713-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="d6713-183">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="d6713-184">System. Data. Entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="d6713-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="d6713-185">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-186">**Objeto devuelto**: un servicio que se utiliza para determinar un nombre invariable de proveedor para un tipo determinado de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="d6713-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="d6713-187">La implementación predeterminada de este servicio utiliza el registro del proveedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="d6713-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="d6713-188">Esto significa que si el proveedor ADO.NET no está registrado de la manera normal porque EF está resolviendo DbProviderFactory, también será necesario para resolver este servicio.</span><span class="sxs-lookup"><span data-stu-id="d6713-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="d6713-189">**Key**: la instancia de DbProviderFactory para la que se requiere un nombre invariable.</span><span class="sxs-lookup"><span data-stu-id="d6713-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="d6713-190">Para obtener más información sobre los servicios relacionados con el proveedor en EF6, consulte la sección [modelo de proveedor EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="d6713-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="d6713-191">System. Data. Entity. Core. Mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="d6713-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="d6713-192">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-193">**Objeto devuelto**: memoria caché de los ensamblados que contienen las vistas generadas previamente.</span><span class="sxs-lookup"><span data-stu-id="d6713-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="d6713-194">Un reemplazo se usa normalmente para permitir que EF sepa qué ensamblados contienen vistas generadas previamente sin realizar ninguna detección.</span><span class="sxs-lookup"><span data-stu-id="d6713-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="d6713-195">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="d6713-196">System. Data. Entity. Infrastructure. pluralización. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="d6713-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="d6713-197">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-198">**Objeto devuelto**: un servicio utilizado por EF para pluralar y singularar los nombres.</span><span class="sxs-lookup"><span data-stu-id="d6713-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="d6713-199">De forma predeterminada, se usa un servicio de pluralización en inglés.</span><span class="sxs-lookup"><span data-stu-id="d6713-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="d6713-200">**Clave**: no se utiliza; será null</span><span class="sxs-lookup"><span data-stu-id="d6713-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="d6713-201">System. Data. Entity. Infrastructure. intercepción. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="d6713-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="d6713-202">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="d6713-203">**Objetos devueltos**: todos los interceptores que se deben registrar cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d6713-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="d6713-204">Tenga en cuenta que estos objetos se solicitan mediante la llamada a GetServices y todos los interceptores devueltos por cualquier solucionador de dependencias se registrarán.</span><span class="sxs-lookup"><span data-stu-id="d6713-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="d6713-205">**Clave**: no se utiliza; será null.</span><span class="sxs-lookup"><span data-stu-id="d6713-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="d6713-206">FUNC < System. Data. Entity. DbContext, Action < cadena\>, System. Data. Entity. Infrastructure. intercepción. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="d6713-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="d6713-207">**Versión introducida**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="d6713-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="d6713-208">**Objeto devuelto**: un generador que se usará para crear el formateador del registro de base de datos que se utilizará cuando el contexto. La propiedad Database. log está establecida en el contexto especificado.</span><span class="sxs-lookup"><span data-stu-id="d6713-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="d6713-209">**Clave**: no se utiliza; será null.</span><span class="sxs-lookup"><span data-stu-id="d6713-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="d6713-210">FUNC < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="d6713-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="d6713-211">**Versión introducida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="d6713-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="d6713-212">**Objeto devuelto**: un generador que se usará para crear instancias de contexto para las migraciones cuando el contexto no tenga un constructor sin parámetros accesible.</span><span class="sxs-lookup"><span data-stu-id="d6713-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="d6713-213">**Key**: el objeto de tipo para el tipo de DbContext derivado para el que se necesita un generador.</span><span class="sxs-lookup"><span data-stu-id="d6713-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="d6713-214">FUNC < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="d6713-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="d6713-215">**Versión introducida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="d6713-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="d6713-216">**Objeto devuelto**: un generador que se utilizará para crear serializadores para la serialización de anotaciones personalizadas fuertemente tipadas de modo que se puedan serializar y desesterilizar en XML para su uso en migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="d6713-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="d6713-217">**Key**: nombre de la anotación que se va a serializar o deserializar.</span><span class="sxs-lookup"><span data-stu-id="d6713-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="d6713-218">FUNC < System. Data. Entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="d6713-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="d6713-219">**Versión introducida**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="d6713-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="d6713-220">**Objeto devuelto**: un generador que se usará para crear controladores para las transacciones, de modo que se pueda aplicar un control especial para situaciones como el control de errores de confirmación.</span><span class="sxs-lookup"><span data-stu-id="d6713-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="d6713-221">**Key**: un objeto ExecutionStrategyKey que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará el controlador de transacciones.</span><span class="sxs-lookup"><span data-stu-id="d6713-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
