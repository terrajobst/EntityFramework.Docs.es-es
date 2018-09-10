---
title: Resolución de dependencias - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: c6c56c3048e17a5c888ffe564e7606abf8b0c4ed
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251250"
---
# <a name="dependency-resolution"></a><span data-ttu-id="821b6-102">Resolución de dependencias</span><span class="sxs-lookup"><span data-stu-id="821b6-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="821b6-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="821b6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="821b6-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="821b6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="821b6-105">A partir de EF6, Entity Framework contiene un mecanismo general para la obtención de las implementaciones de servicios que necesita.</span><span class="sxs-lookup"><span data-stu-id="821b6-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="821b6-106">Es decir, cuando EF usa una instancia de algunas interfaces o clases base se le pedirá para una implementación concreta de la interfaz o clase base a utilizar.</span><span class="sxs-lookup"><span data-stu-id="821b6-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="821b6-107">Esto se logra mediante el uso de la interfaz de IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="821b6-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="821b6-108">El método GetService llama normalmente a EF y se controla mediante una implementación de IDbDependencyResolver proporcionada por EF o la aplicación.</span><span class="sxs-lookup"><span data-stu-id="821b6-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="821b6-109">Cuando se llama, el argumento de tipo es el tipo de clase base o de interfaz del servicio que se solicita, y el objeto de clave es null o un objeto que proporciona información contextual sobre el servicio solicitado.</span><span class="sxs-lookup"><span data-stu-id="821b6-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="821b6-110">A menos que se indique lo contrario cualquier objeto devuelto debe ser seguro para subprocesos ya que se puede usar como un singleton.</span><span class="sxs-lookup"><span data-stu-id="821b6-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="821b6-111">En muchos casos, que el objeto devuelto es una fábrica en cuyo caso la fábrica misma debe ser seguro para subprocesos, pero el objeto devuelto desde el generador no necesita ser seguro para subprocesos ya que se solicita una nueva instancia de la factoría para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="821b6-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="821b6-112">En este artículo no contiene información detallada sobre cómo implementar IDbDependencyResolver, pero en su lugar, actúa como una referencia para los tipos de servicio (es decir, los tipos de clase base y la interfaz) para que EF llama a GetService y la semántica del objeto clave para cada una de ellas llamadas.</span><span class="sxs-lookup"><span data-stu-id="821b6-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="821b6-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="821b6-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="821b6-114">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-115">**Objeto devuelto**: un inicializador de base de datos para el tipo de contexto determinado</span><span class="sxs-lookup"><span data-stu-id="821b6-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="821b6-116">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="821b6-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="821b6-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="821b6-118">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="821b6-119">**Objeto devuelto**: un generador para crear un generador de SQL que se puede usar para las migraciones y otras acciones que provocan una base de datos que se creará, como la creación de la base de datos con inicializadores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="821b6-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="821b6-120">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se generarán SQL.</span><span class="sxs-lookup"><span data-stu-id="821b6-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="821b6-121">Por ejemplo, se devuelve el generador SQL de SQL Server para la clave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="821b6-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-122">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="821b6-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="821b6-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="821b6-124">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-125">**Objeto devuelto**: proveedor de EF de la que se usará para un nombre invariable del proveedor especificado</span><span class="sxs-lookup"><span data-stu-id="821b6-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="821b6-126">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se necesita un proveedor.</span><span class="sxs-lookup"><span data-stu-id="821b6-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="821b6-127">Por ejemplo, se devuelve el proveedor de SQL Server para la clave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="821b6-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-128">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="821b6-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="821b6-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="821b6-130">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-131">**Objeto devuelto**: el generador de conexiones que se usará cuando EF crea una conexión de base de datos por convención.</span><span class="sxs-lookup"><span data-stu-id="821b6-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="821b6-132">Es decir, cuando no hay conexión o la cadena de conexión se concede a EF y ninguna cadena de conexión puede encontrarse en el archivo app.config o web.config, este servicio se usa para crear una conexión por convención.</span><span class="sxs-lookup"><span data-stu-id="821b6-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="821b6-133">Cambiar el generador de conexiones puede permitir que EF que use un tipo diferente de la base de datos (por ejemplo, SQL Server Compact Edition) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="821b6-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="821b6-134">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-135">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="821b6-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="821b6-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="821b6-137">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-138">**Objeto devuelto**: un servicio que puede generar un token del manifiesto del proveedor de una conexión.</span><span class="sxs-lookup"><span data-stu-id="821b6-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="821b6-139">Este servicio se usa normalmente de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="821b6-139">This service is typically used in two ways.</span></span> <span data-ttu-id="821b6-140">En primer lugar, puede usarse para evitar la conexión a la base de datos al generar un modelo Code First.</span><span class="sxs-lookup"><span data-stu-id="821b6-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="821b6-141">En segundo lugar, puede usarse para forzar que Code First para crear un modelo para una versión específica de la base de datos: por ejemplo, para forzar un modelo para SQL Server 2005, aunque a veces se usa SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="821b6-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="821b6-142">**Duración del objeto**: Singleton--el mismo objeto puede ser usa varias veces y al mismo tiempo que diferentes subprocesos</span><span class="sxs-lookup"><span data-stu-id="821b6-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="821b6-143">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="821b6-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="821b6-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="821b6-145">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-146">**Objeto devuelto**: un servicio que puede obtener un generador del proveedor de una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="821b6-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="821b6-147">En .NET 4.5, el proveedor es públicamente accesible desde la conexión.</span><span class="sxs-lookup"><span data-stu-id="821b6-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="821b6-148">En .NET 4 la implementación predeterminada de este servicio utiliza la parte de la heurística para buscar el proveedor de búsqueda de coincidencias.</span><span class="sxs-lookup"><span data-stu-id="821b6-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="821b6-149">Si estos fallan, a continuación, una nueva implementación de este servicio se puede registrar para proporcionar una resolución adecuada.</span><span class="sxs-lookup"><span data-stu-id="821b6-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="821b6-150">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="821b6-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="821b6-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="821b6-152">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-153">**Objeto devuelto**: un generador que se generará una clave de caché del modelo para un contexto determinado.</span><span class="sxs-lookup"><span data-stu-id="821b6-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="821b6-154">De forma predeterminada, EF almacena en caché un modelo por tipo de DbContext y proveedor.</span><span class="sxs-lookup"><span data-stu-id="821b6-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="821b6-155">Una implementación diferente de este servicio puede utilizarse para agregar otra información, como el nombre del esquema, a la clave de caché.</span><span class="sxs-lookup"><span data-stu-id="821b6-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="821b6-156">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="821b6-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="821b6-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="821b6-158">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-159">**Objeto devuelto**: proveedor espacial un EF que agrega compatibilidad para el proveedor EF básico para tipos espaciales geography y geometry.</span><span class="sxs-lookup"><span data-stu-id="821b6-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="821b6-160">**Clave**: DbSptialServices se le pide de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="821b6-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="821b6-161">En primer lugar, específicos del proveedor de servicios espaciales se solicitan con un objeto DbProviderInfo (que contiene todos los idiomas token de manifiesto y nombre) como clave.</span><span class="sxs-lookup"><span data-stu-id="821b6-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="821b6-162">En segundo lugar, se pueden solicitar DbSpatialServices sin la clave.</span><span class="sxs-lookup"><span data-stu-id="821b6-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="821b6-163">Esto se utiliza para resolver el "global espacial proveedor" que se usa al crear tipos de DbGeography o DbGeometry independientes.</span><span class="sxs-lookup"><span data-stu-id="821b6-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-164">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="821b6-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="821b6-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="821b6-166">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-167">**Objeto devuelto**: un generador para crear un servicio que permite a un proveedor implementar reintentos u otro comportamiento cuando se ejecutan las consultas y comandos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="821b6-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="821b6-168">Si no se proporciona ninguna implementación, a continuación, EF simplemente ejecutará los comandos y propagar cualquier excepción producida.</span><span class="sxs-lookup"><span data-stu-id="821b6-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="821b6-169">Este servicio sirve para proporcionar una directiva de reintento que resulta especialmente útil cuando se ejecuta en servidores de base de datos en la nube como SQL Azure para SQL Server.</span><span class="sxs-lookup"><span data-stu-id="821b6-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="821b6-170">**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará la estrategia de ejecución.</span><span class="sxs-lookup"><span data-stu-id="821b6-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-171">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="821b6-172">Func < DbConnection, cadena, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="821b6-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="821b6-173">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-174">**Objeto devuelto**: un generador que permite a un proveedor configurar la asignación de HistoryContext para el `__MigrationHistory` tabla usada por migraciones de EF.</span><span class="sxs-lookup"><span data-stu-id="821b6-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="821b6-175">HistoryContext es un primer DbContext de código y se puede configurar mediante la API fluida normal para cambiar cosas como el nombre de la tabla y las especificaciones de asignación de columna.</span><span class="sxs-lookup"><span data-stu-id="821b6-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="821b6-176">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-177">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="821b6-178">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="821b6-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="821b6-179">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-180">**Objeto devuelto**: proveedor de ADO.NET de la que se usará para un nombre invariable del proveedor especificado.</span><span class="sxs-lookup"><span data-stu-id="821b6-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="821b6-181">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET</span><span class="sxs-lookup"><span data-stu-id="821b6-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-182">Este servicio no es suele cambiar directamente ya que la implementación predeterminada usa el registro del proveedor ADO.NET normal.</span><span class="sxs-lookup"><span data-stu-id="821b6-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="821b6-183">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="821b6-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="821b6-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="821b6-185">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-186">**Objeto devuelto**: un servicio que se usa para determinar un nombre invariable del proveedor para un tipo determinado de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="821b6-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="821b6-187">La implementación predeterminada de este servicio utiliza el registro del proveedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="821b6-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="821b6-188">Esto significa que si no está registrado el proveedor de ADO.NET de la manera normal porque se está resolviendo DbProviderFactory con EF, a continuación, también será necesario resolver este servicio.</span><span class="sxs-lookup"><span data-stu-id="821b6-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="821b6-189">**Clave**: instancia The DbProviderFactory para el que se requiere un nombre invariable.</span><span class="sxs-lookup"><span data-stu-id="821b6-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="821b6-190">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="821b6-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="821b6-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="821b6-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="821b6-192">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-193">**Objeto devuelto**: una memoria caché de los ensamblados que contienen las vistas generadas previamente.</span><span class="sxs-lookup"><span data-stu-id="821b6-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="821b6-194">Un reemplazo se utiliza normalmente para que EF sepa qué ensamblados contienen las vistas generadas previamente sin tener que realizar ninguna detección.</span><span class="sxs-lookup"><span data-stu-id="821b6-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="821b6-195">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="821b6-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="821b6-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="821b6-197">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-198">**Objeto devuelto**: un servicio utilizado por EF Pluralizar y en singular los nombres.</span><span class="sxs-lookup"><span data-stu-id="821b6-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="821b6-199">De forma predeterminada, se usa un servicio de pluralización de inglés.</span><span class="sxs-lookup"><span data-stu-id="821b6-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="821b6-200">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="821b6-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="821b6-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="821b6-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="821b6-202">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="821b6-203">**Los objetos devueltos**: cualquier interceptores que se deben registrar cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="821b6-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="821b6-204">Tenga en cuenta que estos objetos se solicitan mediante la llamada GetServices y todos los interceptores devueltos por cualquier resolución de dependencia se registra.</span><span class="sxs-lookup"><span data-stu-id="821b6-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="821b6-205">**Clave**: no usar; será null.</span><span class="sxs-lookup"><span data-stu-id="821b6-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="821b6-206">Func < System.Data.Entity.DbContext, acción < cadena\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="821b6-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="821b6-207">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="821b6-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="821b6-208">**Objeto devuelto**: un generador que se usará para crear el formateador de registro de base de datos que se usa cuando el contexto. Propiedad Database.Log se establece en el contexto especificado.</span><span class="sxs-lookup"><span data-stu-id="821b6-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="821b6-209">**Clave**: no usar; será null.</span><span class="sxs-lookup"><span data-stu-id="821b6-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="821b6-210">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="821b6-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="821b6-211">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="821b6-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="821b6-212">**Objeto devuelto**: un generador que se usará para crear instancias de contexto para las migraciones cuando el contexto no tiene un constructor sin parámetros accesible.</span><span class="sxs-lookup"><span data-stu-id="821b6-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="821b6-213">**Clave**: objeto el tipo para el tipo de DbContext derivada para el que se necesita un generador.</span><span class="sxs-lookup"><span data-stu-id="821b6-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="821b6-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="821b6-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="821b6-215">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="821b6-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="821b6-216">**Objeto devuelto**: un generador que se usará para crear los serializadores para la serialización de anotaciones personalizadas fuertemente tipada de modo que se pueden serializar y desterilized en XML para su uso en migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="821b6-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="821b6-217">**Clave**: el nombre de la anotación que se va a serializar o deserializar.</span><span class="sxs-lookup"><span data-stu-id="821b6-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="821b6-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="821b6-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="821b6-219">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="821b6-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="821b6-220">**Objeto devuelto**: un generador que se usará para crear controladores para las transacciones para que se puede aplicar un tratamiento especial para situaciones como el control de errores de confirmación.</span><span class="sxs-lookup"><span data-stu-id="821b6-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="821b6-221">**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará el controlador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="821b6-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
