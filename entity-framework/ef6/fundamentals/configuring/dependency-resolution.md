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
# <a name="dependency-resolution"></a>Resolución de dependencias
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

A partir de EF6, Entity Framework contiene un mecanismo general para la obtención de las implementaciones de servicios que necesita. Es decir, cuando EF usa una instancia de algunas interfaces o clases base se le pedirá para una implementación concreta de la interfaz o clase base a utilizar. Esto se logra mediante el uso de la interfaz de IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

El método GetService llama normalmente a EF y se controla mediante una implementación de IDbDependencyResolver proporcionada por EF o la aplicación. Cuando se llama, el argumento de tipo es el tipo de clase base o de interfaz del servicio que se solicita, y el objeto de clave es null o un objeto que proporciona información contextual sobre el servicio solicitado.  

A menos que se indique lo contrario cualquier objeto devuelto debe ser seguro para subprocesos ya que se puede usar como un singleton. En muchos casos, que el objeto devuelto es una fábrica en cuyo caso la fábrica misma debe ser seguro para subprocesos, pero el objeto devuelto desde el generador no necesita ser seguro para subprocesos ya que se solicita una nueva instancia de la factoría para cada usuario.

En este artículo no contiene información detallada sobre cómo implementar IDbDependencyResolver, pero en su lugar, actúa como una referencia para los tipos de servicio (es decir, los tipos de clase base y la interfaz) para que EF llama a GetService y la semántica del objeto clave para cada una de ellas llamadas.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un inicializador de base de datos para el tipo de contexto determinado  

**Clave**: no usar; será null  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Versión introducida**: EF6.0.0

**Objeto devuelto**: un generador para crear un generador de SQL que se puede usar para las migraciones y otras acciones que provocan una base de datos que se creará, como la creación de la base de datos con inicializadores de base de datos.  

**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se generarán SQL. Por ejemplo, se devuelve el generador SQL de SQL Server para la clave "System.Data.SqlClient".  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: proveedor de EF de la que se usará para un nombre invariable del proveedor especificado  

**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET especificando el tipo de base de datos para el que se necesita un proveedor. Por ejemplo, se devuelve el proveedor de SQL Server para la clave "System.Data.SqlClient".  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: el generador de conexiones que se usará cuando EF crea una conexión de base de datos por convención. Es decir, cuando no hay conexión o la cadena de conexión se concede a EF y ninguna cadena de conexión puede encontrarse en el archivo app.config o web.config, este servicio se usa para crear una conexión por convención. Cambiar el generador de conexiones puede permitir que EF que use un tipo diferente de la base de datos (por ejemplo, SQL Server Compact Edition) de forma predeterminada.  

**Clave**: no usar; será null  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un servicio que puede generar un token del manifiesto del proveedor de una conexión. Este servicio se usa normalmente de dos maneras. En primer lugar, puede usarse para evitar la conexión a la base de datos al generar un modelo Code First. En segundo lugar, puede usarse para forzar que Code First para crear un modelo para una versión específica de la base de datos: por ejemplo, para forzar un modelo para SQL Server 2005, aunque a veces se usa SQL Server 2008.  

**Duración del objeto**: Singleton--el mismo objeto puede ser usa varias veces y al mismo tiempo que diferentes subprocesos  

**Clave**: no usar; será null  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un servicio que puede obtener un generador del proveedor de una conexión determinada. En .NET 4.5, el proveedor es públicamente accesible desde la conexión. En .NET 4 la implementación predeterminada de este servicio utiliza la parte de la heurística para buscar el proveedor de búsqueda de coincidencias. Si estos fallan, a continuación, una nueva implementación de este servicio se puede registrar para proporcionar una resolución adecuada.  

**Clave**: no usar; será null  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un generador que se generará una clave de caché del modelo para un contexto determinado. De forma predeterminada, EF almacena en caché un modelo por tipo de DbContext y proveedor. Una implementación diferente de este servicio puede utilizarse para agregar otra información, como el nombre del esquema, a la clave de caché.  

**Clave**: no usar; será null  

## <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: proveedor espacial un EF que agrega compatibilidad para el proveedor EF básico para tipos espaciales geography y geometry.  

**Clave**: DbSptialServices se le pide de dos maneras. En primer lugar, específicos del proveedor de servicios espaciales se solicitan con un objeto DbProviderInfo (que contiene todos los idiomas token de manifiesto y nombre) como clave. En segundo lugar, se pueden solicitar DbSpatialServices sin la clave. Esto se utiliza para resolver el "global espacial proveedor" que se usa al crear tipos de DbGeography o DbGeometry independientes.  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un generador para crear un servicio que permite a un proveedor implementar reintentos u otro comportamiento cuando se ejecutan las consultas y comandos en la base de datos. Si no se proporciona ninguna implementación, a continuación, EF simplemente ejecutará los comandos y propagar cualquier excepción producida. Este servicio sirve para proporcionar una directiva de reintento que resulta especialmente útil cuando se ejecuta en servidores de base de datos en la nube como SQL Azure para SQL Server.  

**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará la estrategia de ejecución.  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, cadena, System.Data.Entity.Migrations.History.HistoryContext\>  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un generador que permite a un proveedor configurar la asignación de HistoryContext para el `__MigrationHistory` tabla usada por migraciones de EF. HistoryContext es un primer DbContext de código y se puede configurar mediante la API fluida normal para cambiar cosas como el nombre de la tabla y las especificaciones de asignación de columna.  

**Clave**: no usar; será null  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: proveedor de ADO.NET de la que se usará para un nombre invariable del proveedor especificado.  

**Clave**: una cadena que contiene el nombre invariable del proveedor ADO.NET  

>[!NOTE]
> Este servicio no es suele cambiar directamente ya que la implementación predeterminada usa el registro del proveedor ADO.NET normal. Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un servicio que se usa para determinar un nombre invariable del proveedor para un tipo determinado de DbProviderFactory. La implementación predeterminada de este servicio utiliza el registro del proveedor ADO.NET. Esto significa que si no está registrado el proveedor de ADO.NET de la manera normal porque se está resolviendo DbProviderFactory con EF, a continuación, también será necesario resolver este servicio.  

**Clave**: instancia The DbProviderFactory para el que se requiere un nombre invariable.  

>[!NOTE]
> Para obtener más detalles sobre los servicios relacionados con el proveedor en EF6, consulte el [modelo de proveedor de EF6](~/ef6/fundamentals/providers/provider-model.md) sección.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: una memoria caché de los ensamblados que contienen las vistas generadas previamente. Un reemplazo se utiliza normalmente para que EF sepa qué ensamblados contienen las vistas generadas previamente sin tener que realizar ninguna detección.  

**Clave**: no usar; será null  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un servicio utilizado por EF Pluralizar y en singular los nombres. De forma predeterminada, se usa un servicio de pluralización de inglés.  

**Clave**: no usar; será null  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Versión introducida**: EF6.0.0

**Los objetos devueltos**: cualquier interceptores que se deben registrar cuando se inicia la aplicación. Tenga en cuenta que estos objetos se solicitan mediante la llamada GetServices y todos los interceptores devueltos por cualquier resolución de dependencia se registra.

**Clave**: no usar; será null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, acción < cadena\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Versión introducida**: EF6.0.0  

**Objeto devuelto**: un generador que se usará para crear el formateador de registro de base de datos que se usa cuando el contexto. Propiedad Database.Log se establece en el contexto especificado.  

**Clave**: no usar; será null.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**Versión introducida**: EF6.1.0  

**Objeto devuelto**: un generador que se usará para crear instancias de contexto para las migraciones cuando el contexto no tiene un constructor sin parámetros accesible.  

**Clave**: objeto el tipo para el tipo de DbContext derivada para el que se necesita un generador.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Versión introducida**: EF6.1.0  

**Objeto devuelto**: un generador que se usará para crear los serializadores para la serialización de anotaciones personalizadas fuertemente tipada de modo que se pueden serializar y desterilized en XML para su uso en migraciones de Code First.  

**Clave**: el nombre de la anotación que se va a serializar o deserializar.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Versión introducida**: EF6.1.0  

**Objeto devuelto**: un generador que se usará para crear controladores para las transacciones para que se puede aplicar un tratamiento especial para situaciones como el control de errores de confirmación.  

**Clave**: ExecutionStrategyKey un objeto que contiene el nombre invariable del proveedor y, opcionalmente, un nombre de servidor para el que se usará el controlador de la transacción.  
