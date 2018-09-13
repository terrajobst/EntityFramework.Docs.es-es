---
title: El modelo de proveedor de Entity Framework 6 - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 13276feb0b22ea8068d7e1f645d48a3d41d77cdf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490190"
---
# <a name="the-entity-framework-6-provider-model"></a>El modelo de proveedor de Entity Framework 6

El modelo de proveedor de Entity Framework permite a Entity Framework para su uso con distintos tipos de servidor de base de datos. Por ejemplo, un proveedor puede estar conectado para permitir que EF para usarse con Microsoft SQL Server, aunque se puede conectar a otro proveedor para permitir que EF para usarse con Microsoft SQL Server Compact Edition. Los proveedores de EF6 que somos conscientes de pueden encontrarse en el [proveedores de Entity Framework](~/ef6/fundamentals/providers/index.md) página.

Ciertos cambios eran necesarios a la forma en que EF interactúa con proveedores para permitir que EF liberarse bajo una licencia de código abierto. Estos cambios requieren la regeneración de los proveedores EF con los ensamblados de EF6 junto con los nuevos mecanismos de registro del proveedor.

## <a name="rebuilding"></a>Volver a generar

Con EF6 ahora se incluye el código principal que anteriormente formaba parte de .NET Framework como ensamblados de fuera de banda (OOB). Encontrará más información sobre cómo crear aplicaciones para EF6 en el [actualizar las aplicaciones para EF6](~/ef6/what-is-new/upgrading-to-ef6.md) página. Los proveedores también necesitará volver a generar usando estas instrucciones.

## <a name="provider-types-overview"></a>Información general sobre tipos de proveedor

Un proveedor de EF es realmente una colección de servicios específicos del proveedor definidos por los tipos CLR que estos servicios se extienden desde (para una clase base) o se implementan (para una interfaz). Dos de estos servicios son fundamentales y es necesario para EF funcione en absoluto. Otros son opcionales y solo es necesario implementar si se requiere una funcionalidad específica o las implementaciones predeterminadas de estos servicios no funciona para el servidor de base de datos específica usando como objetivo.

## <a name="fundamental-provider-types"></a>Tipos fundamentales de proveedor

### <a name="dbproviderfactory"></a>DbProviderFactory

EF depende de si hay un tipo derivado de [System.Data.Common.DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) para llevar a cabo todo el acceso de bajo nivel de base de datos. No es parte de EF DbProviderFactory sino que es una clase en .NET Framework que actúa un punto de entrada para los proveedores ADO.NET que puede utilizarse por EF, otra Micro-o/RM o directamente por una aplicación para obtener instancias de las conexiones, comandos, parámetros y otras abstracciones de ADO.NET en un proveedor de manera independiente. Para obtener más información sobre el objeto DbProviderFactory una se encuentra en la [documentación de MSDN para ADO.NET](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

EF depende de si hay un tipo derivado de DbProviderServices para proporcionar funcionalidad adicional necesaria para EF encima de la funcionalidad ya proporcionada por el proveedor de ADO.NET. En versiones anteriores de EF, la clase DbProviderServices formaba parte de .NET Framework y se ha encontrado en el espacio de nombres System.Data.Common. Comenzando con EF6, esta clase es ahora parte de EntityFramework.dll y en el espacio de nombres System.Data.Entity.Core.Common.

Pueden encontrar más detalles sobre la funcionalidad fundamental de una implementación de DbProviderServices en [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx). Sin embargo, tenga en cuenta que en el momento de escribir esta información no se actualiza para EF6 aunque la mayoría de los conceptos siguen siendo válida. Las implementaciones de SQL Server y SQL Server Compact de DbProviderServices también se incorporan a la [abierto codebase](https://github.com/aspnet/EntityFramework6/) y puede servir como referencia útil para otras implementaciones.

En versiones anteriores de EF se obtuvo la implementación de DbProviderServices para usar directamente desde un proveedor de ADO.NET. Esto se hizo convirtiendo DbProviderFactory a IServiceProvider y llamar al método GetService. Esto estrechamente el proveedor EF a para el objeto DbProviderFactory. Este acoplamiento bloqueado EF se desplacen fuera de .NET Framework y, por tanto, para EF6 se quitó este estrecho acoplamiento y ahora está registrada una implementación de DbProviderServices directamente en el archivo de configuración de la aplicación o en basada en código configuración como se describe con más detalle la _registrar DbProviderServices_ sección más adelante.

## <a name="additional-services"></a>Servicios adicionales

Además de los servicios fundamentales que se ha descrito anteriormente, hay también muchos otros servicios usados por EF que son específicas del proveedor siempre o a veces. Las implementaciones de específico del proveedor predeterminado de estos servicios pueden proporcionar una implementación de DbProviderServices. Las aplicaciones también pueden invalidar las implementaciones de estos servicios, o proporcionar implementaciones cuando un tipo de DbProviderServices no proporciona un valor predeterminado. Esto se describe con más detalle en la _resolver servicios adicionales_ sección más adelante.

Los tipos de servicio adicionales que un proveedor puede ser de interés para un proveedor se enumeran a continuación. Encontrará más detalles sobre cada uno de estos tipos de servicio en la documentación de API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Se trata de un servicio opcional que permite a un proveedor implementar reintentos u otro comportamiento cuando se ejecutan las consultas y comandos en la base de datos. Si no se proporciona ninguna implementación, a continuación, EF simplemente ejecutará los comandos y propagar cualquier excepción producida. Este servicio sirve para proporcionar una directiva de reintento que resulta especialmente útil cuando se ejecuta en servidores de base de datos en la nube como SQL Azure para SQL Server.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Se trata de un servicio opcional que permite a un proveedor crear objetos de DbConnection por convención, cuando se especifica un nombre de la base de datos. Tenga en cuenta que mientras este servicio se puede resolver por una implementación de DbProviderServices que ha estado presente desde EF 4.1 y puede también establecerse explícitamente en el archivo de configuración o en código. El proveedor solo obtendrá una oportunidad de resolver este servicio si ha registrado como proveedor predeterminado (consulte _el proveedor predeterminado_ a continuación) y si un generador de conexión predeterminada no se ha establecido en otro lugar.

### <a name="dbspatialservices"></a>DbSpatialServices

Se trata de un servicio opcional que permite a un proveedor agregar compatibilidad para tipos espaciales geography y geometry. Una implementación de este servicio debe proporcionarse en orden para una aplicación para usar EF con tipos espaciales. Se le pide DbSptialServices de dos maneras. En primer lugar, específicos del proveedor de servicios espaciales se solicitan con un objeto DbProviderInfo (que contiene todos los idiomas token de manifiesto y nombre) como clave. En segundo lugar, se pueden solicitar DbSpatialServices sin la clave. Esto se utiliza para resolver el "global espacial proveedor" que se usa al crear tipos de DbGeography o DbGeometry independientes.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Se trata de un servicio opcional que permite las migraciones de EF que va a usar para la generación de SQL que se usa en la creación y modificación de esquemas de base de datos Code First. Se requiere una implementación con el fin de admitir las migraciones. Si no se proporciona una implementación, a continuación, se también se usará cuando se crean las bases de datos con los inicializadores de base de datos o el método Database.Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, cadena, HistoryContextFactory >

Se trata de un servicio opcional que permite a un proveedor configurar la asignación de HistoryContext para el `__MigrationHistory` tabla usada por migraciones de EF. HistoryContext es un primer DbContext de código y se puede configurar mediante la API fluida normal para cambiar cosas como el nombre de la tabla y las especificaciones de asignación de columna. La implementación predeterminada de este servicio devuelto por EF para todos los proveedores puede funcionar para un servidor de base de datos determinada si todas las tablas y columnas asignaciones predeterminadas son compatibles con ese proveedor. En este caso el proveedor no es necesario proporcionar una implementación de este servicio.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Se trata de un servicio opcional para obtener DbProviderFactory correcto de un determinado objeto DbConnection. La implementación predeterminada de este servicio devuelto por EF para todos los proveedores está diseñada para funcionar en todos los proveedores. Sin embargo, cuando se ejecuta en .NET 4, el objeto DbProviderFactory no es accesible públicamente desde uno si su DbConnections. Por lo tanto, EF usa parte de la heurística para buscar los proveedores registrados para encontrar a una coincidencia. Es posible que algunos proveedores de estas heurísticas se producirá un error y en dichas situaciones, el proveedor debería proporcionar una nueva implementación.

## <a name="registering-dbproviderservices"></a>Registrar DbProviderServices

La implementación de DbProviderServices para usar se puede registrar en el archivo de configuración (app.config o web.config) o mediante configuración basada en código de la aplicación. En ambos casos el registro usa el nombre del proveedor"invariable" como clave. Esto permite que varios proveedores registrar y utilizar en una sola aplicación. El nombre invariable usado para los registros EF es el mismo que el nombre invariable usado para cadenas de conexión y registro del proveedor ADO.NET. Por ejemplo, se usa "System.Data.SqlClient" para el nombre invariable de SQL Server.

### <a name="config-file-registration"></a>Registro en el archivo de configuración

El tipo de DbProviderServices para utilizar se registra como un elemento de proveedor en la lista de proveedores de la sección entityFramework del archivo de configuración de la aplicación. Por ejemplo:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

El _tipo_ cadena debe ser el nombre de tipo calificado con el ensamblado de la implementación de DbProviderServices a utilizar.

### <a name="code-based-registration"></a>Registro basado en código

A partir de los proveedores de EF6 también se puede registrar mediante código. Esto permite que un proveedor EF para usarse sin realizar ningún cambio al archivo de configuración de la aplicación. Para usar la configuración basada en el código de una aplicación debe crear una clase DbConfiguration como se describe en el [documentación sobre la configuración basada en código](http://msdn.com/data/jj680699). El constructor de la clase DbConfiguration, a continuación, debe llamar a SetProviderServices para registrar el proveedor EF. Por ejemplo:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Resolver los servicios adicionales

Como se mencionó anteriormente en el _información general sobre los tipos de proveedor_ sección un DbProviderServices clase puede usarse también para resolver los servicios adicionales. Esto es posible porque DbProviderServices implementa IDbDependencyResolver y cada tipo de DbProviderServices registrado se agrega como una resolución"predeterminada". El mecanismo de IDbDpendencyResolver se describe con más detalle en [resolución de dependencia](~/ef6/fundamentals/configuring/dependency-resolution.md). Sin embargo, no es necesario comprender todos los conceptos en esta especificación para resolver los servicios adicionales en un proveedor.

La manera más común para un proveedor resolver los servicios adicionales es llamar a DbProviderServices.AddDependencyResolver para cada servicio en el constructor de la clase DbProviderServices. Por ejemplo, SqlProviderServices (el proveedor EF para SQL Server) tiene código similar al siguiente para la inicialización:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Este constructor utiliza las clases de auxiliares siguientes:

*   SingletonDependencyResolver: proporciona una manera sencilla para resolver los servicios de Singleton, es decir, los servicios para que la misma instancia se devuelve cada vez que se llama a GetService. Los servicios transitorios a menudo se registran como una fábrica de singleton que se usará para crear instancias transitorias bajo demanda.
*   ExecutionStrategyResolver: una resolución específica para devolver IExecutionStrategy implementaciones.

En lugar de usar DbProviderServices.AddDependencyResolver también es posible invalidar DbProviderServices.GetService y resolver los servicios adicionales directamente. Este método se llamará cuando EF necesita un servicio definido por un tipo determinado y, en algunos casos, para una clave determinada. El método debe devolver el servicio si puede, o devuelve null para participar de devolver el servicio y permita que la otra clase resolverlo. Por ejemplo, para resolver el generador de conexiones de forma predeterminada el código en GetService sería algo parecido a esto:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Orden de registro

Cuando varias implementaciones de DbProviderServices se registran en el archivo de configuración de la aplicación se agregará como solucionadores secundaria en el orden en que aparecen. Puesto que las resoluciones siempre se agregan a la parte superior de la cadena de resolución secundaria que esto significa que el proveedor al final de la lista tendrá la posibilidad para resolver las dependencias antes de los demás. (Esto puede parecer un poco contradictorio al principio, pero tiene sentido si imagine teniendo cada proveedor de la lista y apilamiento encima de los proveedores existentes).

Esta ordenación normalmente no tiene importancia ya que la mayoría de los servicios de proveedor son específicos del proveedor y con clave por el nombre invariable del proveedor. Sin embargo, para los servicios que no están organizados por nombre invariable del proveedor o algunos otro específica del proveedor clave que se resolverá el servicio basado en este orden. Por ejemplo, si no se establece explícitamente forma distinta en otro caso contrario, el generador de conexión predeterminado procederán del proveedor de nivel superior en la cadena.

## <a name="additional-config-file-registrations"></a>Registros de archivo de configuración adicionales

Es posible registrar de forma explícita algunos de los servicios adicionales del proveedor que se ha descrito anteriormente directamente en el archivo de configuración de la aplicación. Cuando esto se realiza el registro en el archivo de configuración se usará en lugar de nada devuelto por el método GetService de la implementación de DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Registrar el generador de conexión predeterminado

Iniciar con EF5 el paquete EntityFramework NuGet automáticamente registra el generador de conexiones SQL Express o el generador de conexión LocalDb en el archivo de configuración.

Por ejemplo:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

El _tipo_ es el nombre de tipo calificado con el ensamblado para el generador de conexión predeterminado, que debe implementar IDbConnectionFactory.

Se recomienda que un paquete de NuGet del proveedor establecida el generador de conexión predeterminado de esta manera, cuando está instalado. Consulte _paquetes de NuGet para proveedores_ a continuación.

## <a name="additional-ef6-provider-changes"></a>Cambios adicionales de proveedor de EF6

### <a name="spatial-provider-changes"></a>Cambios de proveedor espacial

Los proveedores que admiten tipos espaciales ahora deben implementar algunos métodos adicionales en clases derivadas de DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

También hay nuevas versiones asincrónicas de los métodos existentes que se recomiendan para invalidarse según las implementaciones predeterminadas delegan a los métodos sincrónicos y, por tanto, no se ejecutan asincrónicamente:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Compatibilidad nativa para Enumerable.Contains

EF6 presenta a un nuevo tipo de expresión, DbInExpression, que se agregó para solucionar problemas de rendimiento en torno a uso de Enumerable.Contains en consultas LINQ. La clase DbProviderManifest tiene un nuevo método virtual SupportsInExpression, que es invocado por EF para determinar si un proveedor controla el nuevo tipo de expresión. Para ofrecer compatibilidad con las implementaciones existentes del proveedor, el método devuelve false. Para aprovechar esta mejora, un proveedor de EF6 puede agregar código para controlar DbInExpression e invalidar SupportsInExpression devuelva true. Puede crearse una instancia de DbInExpression llamando al método DbExpressionBuilder.In. Una instancia de DbInExpression se compone de un DbExpression, que normalmente representa una columna de tabla y una lista de DbConstantExpression para buscar una coincidencia.

## <a name="nuget-packages-for-providers"></a>Paquetes de NuGet para proveedores

Una manera para que esté disponible un proveedor de EF6 es lo libera como un paquete de NuGet. Uso de un paquete de NuGet tiene las siguientes ventajas:

*   Es fácil de usar NuGet para agregar el registro del proveedor al archivo de configuración de la aplicación
*   Pueden realizarse cambios adicionales al archivo de configuración para establecer el generador de conexiones predeterminado para que las conexiones realizadas por convención, use el proveedor registrado
*   NuGet controla agregar redirecciones de enlace para que el proveedor de EF6 debe continuar funcionando incluso después de que se publica un nuevo paquete EF

Un ejemplo de esto es el paquete de EntityFramework.SqlServerCompact que se incluye en el [abierto codebase](http://github.com/aspnet/entityframework6). Este paquete proporciona una buena plantilla para crear el proveedor EF paquetes de NuGet.

### <a name="powershell-commands"></a>Comandos de PowerShell

Cuando se instala el paquete EntityFramework NuGet registra un módulo de PowerShell que contiene dos comandos que son muy útiles para los paquetes de proveedor:

*   EFProvider agregar agrega una nueva entidad para el proveedor en el archivo de configuración del proyecto de destino y se asegura de que está al final de la lista de los proveedores registrados.
*   EFDefaultConnectionFactory agregar agrega o actualiza el registro defaultConnectionFactory en el archivo de configuración del proyecto de destino.

Estos dos comandos se encargan de agregar una sección entityFramework al archivo de configuración y agregar una colección de proveedores, si es necesario.

Su objetivo es que estos comandos se llama desde la secuencia de comandos de NuGet install.ps1. Por ejemplo, install.ps1 para el proveedor de SQL Compact es similar al siguiente:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Para obtener más información acerca de estos comandos se puede obtener mediante el uso de get-help en la ventana de consola de administrador de paquetes.

## <a name="wrapping-providers"></a>Proveedores de ajuste

Un proveedor de ajuste es un proveedor EF o ADO.NET que contiene un proveedor existente para ampliarlo con otras funcionalidades, como la generación de perfiles o capacidades de seguimiento. Se puede registrar proveedores de ajuste de la manera normal, pero a menudo resulta más conveniente configurar el proveedor de ajuste en tiempo de ejecución mediante la interceptación de la resolución de los servicios relacionados con el proveedor. El evento estático OnLockingConfiguration en la clase DbConfiguration puede usarse para hacer esto.

OnLockingConfiguration se llama después de EF ha determinado que se obtendrán de toda la configuración de EF para el dominio de aplicación, pero antes de que se bloquee para su uso. Al iniciar la aplicación (antes de que se usa EF) la aplicación debe registrar un controlador de eventos para este evento. (Estamos considerando agregar compatibilidad para registrar este controlador en el archivo de configuración, pero esto no se admite todavía). El controlador de eventos, a continuación, debe realizar una llamada a ReplaceService para todos los servicios que deben ajustarse.  

Por ejemplo, para encapsular IDbConnectionFactory y DbProviderService, se debe registrar un controlador algo parecido a esto:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

El servicio que se ha resuelto y debe ajustarse junto con la clave que se usó para resolver el servicio se pasan al controlador. El controlador, a continuación, puede ajustar este servicio y reemplace el servicio devuelto por la versión ajustada.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Resolución de un objeto DbProviderFactory con EF.

DbProviderFactory es uno de los tipos de proveedor fundamentales necesarios para EF, como se describe en el _información general sobre los tipos de proveedor_ sección anterior. Como ya se ha mencionado, no es un tipo de EF y registro no suele ser parte de la configuración de EF, pero en su lugar es el registro del proveedor ADO.NET normal en el archivo machine.config o el archivo de configuración de la aplicación.

A pesar de esta EF sigue usando su mecanismo de resolución de dependencia normal cuando se busca un objeto DbProviderFactory para usar. El solucionador predeterminado usa el registro ADO.NET normal en los archivos de configuración y aquí es suele ser transparente. Pero debido a la resolución de dependencia normal se usa el mecanismo significa que se puede usar un IDbDependencyResolver para resolver un objeto DbProviderFactory incluso cuando no se ha realizado el registro ADO.NET normal.

Resolver DbProviderFactory de este modo tiene varias implicaciones:

*   Una aplicación mediante la configuración basada en el código puede agregar llamadas en su clase DbConfiguration para registrar el objeto DbProviderFactory adecuado. Esto es especialmente útil para las aplicaciones que no desean (o no) hacer uso de cualquier configuración basada en archivo en absoluto.
*   El servicio se puede ajustar o reemplazar utilizando ReplaceService como se describe en el _ajuste proveedores_ sección anterior
*   En teoría, una implementación de DbProviderServices pudo resolver un objeto DbProviderFactory.

El punto importante a tener en cuenta acerca de cómo realizar cualquiera de estas acciones es que sólo afectarán a la búsqueda de DbProviderFactory EF. Otro código de EF no puede seguir esperando que el proveedor de ADO.NET que estar registrado en la forma habitual y puede producir un error si no se encuentra el registro. Por este motivo es normalmente más adecuado para un objeto DbProviderFactory para registrarse en la forma habitual de ADO.NET.

### <a name="related-services"></a>Servicios relacionados

Si EF se usa para resolver un objeto DbProviderFactory, a continuación, también debe resolver los servicios IProviderInvariantName y IDbProviderFactoryResolver.

IProviderInvariantName es un servicio que se usa para determinar un nombre invariable del proveedor para un tipo determinado de DbProviderFactory. La implementación predeterminada de este servicio utiliza el registro del proveedor ADO.NET. Esto significa que si no está registrado el proveedor de ADO.NET de la manera normal porque se está resolviendo DbProviderFactory con EF, a continuación, también será necesario resolver este servicio. Tenga en cuenta que una resolución para este servicio se agrega automáticamente al usar el método DbConfiguration.SetProviderFactory.

Como se describe en el _información general sobre los tipos de proveedor_ sección anterior, se usa el IDbProviderFactoryResolver obtener DbProviderFactory correcto de un determinado objeto DbConnection. La implementación predeterminada de este servicio cuando se ejecuta en .NET 4 usa el registro del proveedor ADO.NET. Esto significa que si no está registrado el proveedor de ADO.NET de la manera normal porque se está resolviendo DbProviderFactory con EF, a continuación, también será necesario resolver este servicio.
