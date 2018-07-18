---
title: Resolución de dependencias - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122786"
---
# <a name="dependency-resolution"></a><span data-ttu-id="c87db-102">Resolución de dependencias</span><span class="sxs-lookup"><span data-stu-id="c87db-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="c87db-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c87db-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c87db-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="c87db-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c87db-105">A partir de EF6, Entity Framework contiene un mecanismo general para la obtención de las implementaciones de servicios que necesita.</span><span class="sxs-lookup"><span data-stu-id="c87db-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="c87db-106">Es decir, cuando EF usa una instancia de algunas interfaces o clases base se le pedirá para una implementación concreta de la interfaz o clase base a utilizar.</span><span class="sxs-lookup"><span data-stu-id="c87db-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="c87db-107">Esto se logra mediante el uso de la interfaz de IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="c87db-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="c87db-108">El método GetService llama normalmente a EF y se controla mediante una implementación de IDbDependencyResolver proporcionada por EF o la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c87db-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="c87db-109">Cuando se llama, el argumento de tipo es el tipo de clase base o de interfaz del servicio que se solicita, y el objeto de clave es null o un objeto que proporciona información contextual sobre el servicio solicitado.</span><span class="sxs-lookup"><span data-stu-id="c87db-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="c87db-110">En este artículo no contiene información detallada sobre cómo implementar IDbDependencyResolver, pero en su lugar, actúa como una referencia para los tipos de servicio (es decir, los tipos de clase base y la interfaz) para que EF llama a GetService y la semántica del objeto clave para cada una de ellas llamadas.</span><span class="sxs-lookup"><span data-stu-id="c87db-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="c87db-111">Este documento se mantienen actualizado cuando se agregan servicios adicionales.</span><span class="sxs-lookup"><span data-stu-id="c87db-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="c87db-112">Servicios que se puede resolver</span><span class="sxs-lookup"><span data-stu-id="c87db-112">Services resolved</span></span>  

<span data-ttu-id="c87db-113">A menos que se indique lo contrario cualquier objeto devuelto debe ser seguro para subprocesos ya que se puede usar como un singleton.</span><span class="sxs-lookup"><span data-stu-id="c87db-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="c87db-114">En muchos casos, que el objeto devuelto es una fábrica en cuyo caso la fábrica misma debe ser seguro para subprocesos, pero el objeto devuelto desde el generador no necesita ser seguro para subprocesos ya que se solicita una nueva instancia de la factoría para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="c87db-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="c87db-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="c87db-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="c87db-116">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-117">**Objeto devuelto**: un inicializador de base de datos para el tipo de contexto determinado</span><span class="sxs-lookup"><span data-stu-id="c87db-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="c87db-118">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="c87db-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="c87db-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="c87db-120">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="c87db-121">**Objeto devuelto**: un generador para crear un generador de SQL que se puede usar para las migraciones y otras acciones que provocan una base de datos que se creará, como la creación de la base de datos con inicializadores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c87db-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="c87db-122">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se generarán SQL.</span><span class="sxs-lookup"><span data-stu-id="c87db-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="c87db-123">Por ejemplo, se devuelve el generador SQL de SQL Server para la clave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="c87db-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-124">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="c87db-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="c87db-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="c87db-126">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-127">**Objeto devuelto**: proveedor de EF de la que se usará para un nombre invariable del proveedor especificado</span><span class="sxs-lookup"><span data-stu-id="c87db-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="c87db-128">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se necesita un proveedor.</span><span class="sxs-lookup"><span data-stu-id="c87db-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="c87db-129">Por ejemplo, se devuelve el proveedor de SQL Server para la clave "System.Data.SqlClient".</span><span class="sxs-lookup"><span data-stu-id="c87db-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-130">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="c87db-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="c87db-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="c87db-132">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-133">**Objeto devuelto**: el generador de conexiones que se usará cuando EF crea una conexión de base de datos por convención.</span><span class="sxs-lookup"><span data-stu-id="c87db-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="c87db-134">Es decir, cuando no hay conexión o la cadena de conexión se concede a EF y ninguna cadena de conexión puede encontrarse en el archivo app.config o web.config, este servicio se usa para crear una conexión por convención.</span><span class="sxs-lookup"><span data-stu-id="c87db-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="c87db-135">Cambiar el generador de conexiones puede permitir que EF que use un tipo diferente de la base de datos (por ejemplo, SQL Server Compact Edition) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c87db-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="c87db-136">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-137">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="c87db-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="c87db-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="c87db-139">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-140">**Objeto devuelto**: un servicio que puede generar un token del manifiesto del proveedor de una conexión.</span><span class="sxs-lookup"><span data-stu-id="c87db-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="c87db-141">Este servicio se usa normalmente de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="c87db-141">This service is typically used in two ways.</span></span> <span data-ttu-id="c87db-142">En primer lugar, puede usarse para evitar la conexión a la base de datos al generar un modelo Code First.</span><span class="sxs-lookup"><span data-stu-id="c87db-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="c87db-143">En segundo lugar, puede usarse para forzar que Code First para crear un modelo para una versión específica de la base de datos: por ejemplo, para forzar un modelo para SQL Server 2005, aunque a veces se usa SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="c87db-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="c87db-144">**Duración del objeto**: Singleton--el mismo objeto puede ser usa varias veces y al mismo tiempo que diferentes subprocesos</span><span class="sxs-lookup"><span data-stu-id="c87db-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="c87db-145">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="c87db-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="c87db-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="c87db-147">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-148">**Objeto devuelto**: un servicio que puede obtener un generador del proveedor de una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="c87db-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="c87db-149">En .NET 4.5, el proveedor es públicamente accesible desde la conexión.</span><span class="sxs-lookup"><span data-stu-id="c87db-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="c87db-150">En .NET 4 la implementación predeterminada de este servicio utiliza la parte de la heurística para buscar el proveedor de búsqueda de coincidencias.</span><span class="sxs-lookup"><span data-stu-id="c87db-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="c87db-151">Si estos fallan, a continuación, una nueva implementación de este servicio se puede registrar para proporcionar una resolución adecuada.</span><span class="sxs-lookup"><span data-stu-id="c87db-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="c87db-152">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="c87db-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="c87db-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="c87db-154">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-155">**Objeto devuelto**: un generador que se generará una clave de caché del modelo para un contexto determinado.</span><span class="sxs-lookup"><span data-stu-id="c87db-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="c87db-156">De forma predeterminada, EF almacena en caché un modelo por tipo de DbContext y proveedor.</span><span class="sxs-lookup"><span data-stu-id="c87db-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="c87db-157">Una implementación diferente de este servicio puede utilizarse para agregar otra información, como el nombre del esquema, a la clave de caché.</span><span class="sxs-lookup"><span data-stu-id="c87db-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="c87db-158">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="c87db-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="c87db-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="c87db-160">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-161">**Objeto devuelto**: proveedor espacial un EF que agrega compatibilidad para el proveedor EF básico para tipos espaciales geography y geometry.</span><span class="sxs-lookup"><span data-stu-id="c87db-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="c87db-162">**Clave**: DbSptialServices se le pide de dos maneras.</span><span class="sxs-lookup"><span data-stu-id="c87db-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="c87db-163">En primer lugar, específicos del proveedor de servicios espaciales se solicitan con un objeto DbProviderInfo (que contiene todos los idiomas token de manifiesto y nombre) como clave.</span><span class="sxs-lookup"><span data-stu-id="c87db-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="c87db-164">En segundo lugar, se pueden solicitar DbSpatialServices sin la clave.</span><span class="sxs-lookup"><span data-stu-id="c87db-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="c87db-165">Esto se utiliza para resolver el "global espacial proveedor" que se usa al crear tipos de DbGeography o DbGeometry independientes.</span><span class="sxs-lookup"><span data-stu-id="c87db-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-166">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="c87db-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="c87db-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="c87db-168">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-169">**Objeto devuelto**: un generador para crear un servicio que permite a un proveedor implementar reintentos u otro comportamiento cuando se ejecutan las consultas y comandos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c87db-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="c87db-170">Si no se proporciona ninguna implementación, a continuación, EF simplemente ejecutará los comandos y propagar cualquier excepción producida.</span><span class="sxs-lookup"><span data-stu-id="c87db-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="c87db-171">Este servicio sirve para proporcionar una directiva de reintento que resulta especialmente útil cuando se ejecuta en servidores de base de datos en la nube como SQL Azure para SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c87db-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="c87db-172">**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará la estrategia de ejecución.</span><span class="sxs-lookup"><span data-stu-id="c87db-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-173">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="c87db-174">Func < DbConnection, cadena, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="c87db-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="c87db-175">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-176">**Objeto devuelto**: un generador que permite a un proveedor configurar la asignación de HistoryContext para el `__MigrationHistory` tabla usada por migraciones de EF.</span><span class="sxs-lookup"><span data-stu-id="c87db-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="c87db-177">HistoryContext es un primer DbContext de código y se puede configurar mediante la API fluida normal para cambiar cosas como el nombre de la tabla y las especificaciones de asignación de columna.</span><span class="sxs-lookup"><span data-stu-id="c87db-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="c87db-178">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-179">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="c87db-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="c87db-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="c87db-181">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-182">**Objeto devuelto**: proveedor de ADO.NET de la que se usará para un nombre invariable del proveedor especificado.</span><span class="sxs-lookup"><span data-stu-id="c87db-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="c87db-183">**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET</span><span class="sxs-lookup"><span data-stu-id="c87db-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-184">Este servicio no es suele cambiar directamente ya que la implementación predeterminada usa el registro del proveedor ADO.NET normal.</span><span class="sxs-lookup"><span data-stu-id="c87db-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="c87db-185">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="c87db-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="c87db-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="c87db-187">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-188">**Objeto devuelto**: un servicio que se usa para determinar un nombre invariable del proveedor para un tipo determinado de DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="c87db-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="c87db-189">La implementación predeterminada de este servicio utiliza el registro del proveedor ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="c87db-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="c87db-190">Esto significa que si no está registrado el proveedor de ADO.NET de la manera normal porque se está resolviendo DbProviderFactory con EF, a continuación, también será necesario resolver este servicio.</span><span class="sxs-lookup"><span data-stu-id="c87db-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="c87db-191">**Clave**: instancia The DbProviderFactory para el que se requiere un nombre invariable.</span><span class="sxs-lookup"><span data-stu-id="c87db-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="c87db-192">Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.</span><span class="sxs-lookup"><span data-stu-id="c87db-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="c87db-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="c87db-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="c87db-194">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-195">**Objeto devuelto**: una memoria caché de los ensamblados que contienen las vistas generadas previamente.</span><span class="sxs-lookup"><span data-stu-id="c87db-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="c87db-196">Un reemplazo se utiliza normalmente para que EF sepa qué ensamblados contienen las vistas generadas previamente sin tener que realizar ninguna detección.</span><span class="sxs-lookup"><span data-stu-id="c87db-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="c87db-197">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="c87db-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="c87db-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="c87db-199">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-200">**Objeto devuelto**: un servicio utilizado por EF Pluralizar y en singular los nombres.</span><span class="sxs-lookup"><span data-stu-id="c87db-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="c87db-201">De forma predeterminada, se usa un servicio de pluralización de inglés.</span><span class="sxs-lookup"><span data-stu-id="c87db-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="c87db-202">**Clave**: no usar; será null</span><span class="sxs-lookup"><span data-stu-id="c87db-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="c87db-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="c87db-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="c87db-204">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="c87db-205">**Los objetos devueltos**: cualquier interceptores que se deben registrar cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c87db-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="c87db-206">Tenga en cuenta que estos objetos se solicitan mediante la llamada GetServices y todos los interceptores devueltos por cualquier resolución de dependencia se registra.</span><span class="sxs-lookup"><span data-stu-id="c87db-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="c87db-207">**Clave**: no usar; será null.</span><span class="sxs-lookup"><span data-stu-id="c87db-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="c87db-208">Func < System.Data.Entity.DbContext, acción < cadena\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="c87db-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="c87db-209">**Versión introducida**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="c87db-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="c87db-210">**Objeto devuelto**: un generador que se usará para crear el formateador de registro de base de datos que se usa cuando el contexto. Propiedad Database.Log se establece en el contexto especificado.</span><span class="sxs-lookup"><span data-stu-id="c87db-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="c87db-211">**Clave**: no usar; será null.</span><span class="sxs-lookup"><span data-stu-id="c87db-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="c87db-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="c87db-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="c87db-213">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="c87db-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="c87db-214">**Objeto devuelto**: un generador que se usará para crear instancias de contexto para las migraciones cuando el contexto no tiene un constructor sin parámetros accesible.</span><span class="sxs-lookup"><span data-stu-id="c87db-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="c87db-215">**Clave**: objeto el tipo para el tipo de DbContext derivada para el que se necesita un generador.</span><span class="sxs-lookup"><span data-stu-id="c87db-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="c87db-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="c87db-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="c87db-217">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="c87db-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="c87db-218">**Objeto devuelto**: un generador que se usará para crear los serializadores para la serialización de anotaciones personalizadas fuertemente tipada de modo que se pueden serializar y desterilized en XML para su uso en migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="c87db-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="c87db-219">**Clave**: el nombre de la anotación que se va a serializar o deserializar.</span><span class="sxs-lookup"><span data-stu-id="c87db-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="c87db-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="c87db-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="c87db-221">**Versión introducida**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="c87db-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="c87db-222">**Objeto devuelto**: un generador que se usará para crear controladores para las transacciones para que se puede aplicar un tratamiento especial para situaciones como el control de errores de confirmación.</span><span class="sxs-lookup"><span data-stu-id="c87db-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="c87db-223">**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará el controlador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="c87db-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
