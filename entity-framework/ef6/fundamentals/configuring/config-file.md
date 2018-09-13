---
title: Archivo de configuración - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490881"
---
# <a name="configuration-file-settings"></a>Archivo de configuración
Entity Framework permite a una serie de valores que se especifique el archivo de configuración. En general EF sigue un principio 'convención sobre configuración': todas las opciones descritas en esta publicación tienen un comportamiento predeterminado, sólo deberá preocuparse acerca de cómo cambiar la configuración cuando el valor predeterminado ya no satisface sus requisitos.  

## <a name="a-code-based-alternative"></a>Una alternativa basada en código  

Todas estas configuraciones pueden aplicarse también mediante código. A partir de EF6 introdujimos [configuración basada en código](code-based.md), que proporciona un modo centralizado de la aplicación configuración desde el código. Antes de EF6, todavía se puede aplicar la configuración desde el código, pero deberá usar varias API para configurar diferentes áreas. La opción de archivo de configuración permite que esta configuración se puede cambiar fácilmente durante la implementación sin actualizar el código.

## <a name="the-entity-framework-configuration-section"></a>La sección de configuración de Entity Framework  

A partir con EF4.1, puede establecer el inicializador de base de datos para un contexto con el **appSettings** sección del archivo de configuración. En EF 4.3 introdujimos personalizado **entityFramework** sección para controlar la nueva configuración. Entity Framework reconocerá sigue establecidos con el formato antiguo de inicializadores de base de datos, pero se recomienda mover al nuevo formato siempre que sea posible.

El **entityFramework** sección se agregó automáticamente al archivo de configuración del proyecto cuando se ha instalado el paquete EntityFramework NuGet.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Cadenas de conexión  

[Esta página](~/ef6/fundamentals/configuring/connection-strings.md) proporciona más detalles sobre cómo Entity Framework determina la base de datos que se usará, incluidas las cadenas de conexión en el archivo de configuración.  

Van de cadenas de conexión en el estándar **connectionStrings** elemento y no requieren la **entityFramework** sección.  

Los modelos de código que se basa en primer lugar utilizan cadenas de conexión de ADO.NET normales. Por ejemplo:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF Designer en función de las cadenas de conexión de modelos uso especiales EF. Por ejemplo:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Tipo de configuración basado en código (EF6 y versiones posteriores)  

A partir de EF6, puede especificar el DbConfiguration para EF que use para [configuración basada en código](code-based.md) en la aplicación. En la mayoría de los casos no es necesario especificar esta opción como EF detectará automáticamente su DbConfiguration. Para obtener información detallada de cuándo es posible que necesite especificar DbConfiguration en el archivo de configuración, consulte el **mover DbConfiguration** sección de [configuración basada en código](code-based.md).  

Para establecer un tipo de DbConfiguration, especifique el nombre de tipo calificado de ensamblado en el **codeConfigurationType** elemento.  

> [!NOTE]
> Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo. Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Proveedores de base de datos EF (EF6 y versiones posteriores)  

Antes de EF6, partes específicas de Entity Framework de un proveedor de base de datos se tenían que se incluye como parte del proveedor ADO.NET core. A partir de EF6, las partes concretas de EF están administradas y registrar por separado.  

Normalmente no tendrá que registrar proveedores usted mismo. Esto normalmente se realiza por el proveedor que se instala.  

Los proveedores se registran mediante la inclusión de un **proveedor** elemento bajo el **proveedores** sección secundarios de la **entityFramework** sección. Hay dos atributos necesarios para una entrada del proveedor:  

- **invariantName** identifica el proveedor de ADO.NET básico que los destinos de proveedor EF  
- **tipo** es el nombre de tipo calificado de ensamblado de la implementación del proveedor EF  

> [!NOTE]
> Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo. Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.  

Como ejemplo aquí es la entrada creada para registrar el proveedor de SQL Server de forma predeterminada al instalar Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptores (EF6.1 y versiones posteriores)  

Comenzando con EF6.1 puede registrar los interceptores en el archivo de configuración. Los interceptores permiten ejecutar una lógica adicional cuando EF realiza ciertas operaciones, tales como ejecutar consultas de base de datos, abrir las conexiones, etc.  

Los interceptores se registran mediante la inclusión de un **interceptor** elemento bajo el **interceptores** sección secundarios de la **entityFramework** sección. Por ejemplo, la siguiente configuración registra integrado **DatabaseLogger** interceptor que se registrará todas las operaciones de base de datos en la consola.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Operaciones de base de datos de registro en un archivo (EF6.1 y versiones posteriores)  

Registrar los interceptores mediante el archivo de configuración es especialmente útil cuando desea agregar el registro a una aplicación existente para ayudar a depurar un problema. **DatabaseLogger** admite el registro en un archivo, proporcionando el nombre de archivo como un parámetro de constructor.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

De forma predeterminada, esto hará que el archivo de registro se sobrescriba con un archivo nuevo cada vez que se inicia la aplicación. Para anexar en su lugar en el registro de archivo si ya existe usar algo como:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Para obtener más información sobre **DatabaseLogger** y registrar los interceptores, consulte el blog [EF 6.1: activar el registro sin volver a compilar](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Generador de conexión predeterminado primer código  

La sección de configuración permite especificar un generador de conexión predeterminada que debe usar Code First para buscar una base de datos que se usará para un contexto. El generador de conexión predeterminada solo se usa cuando no se ha agregado ninguna cadena de conexión para el archivo de configuración para un contexto.  

Cuando instala el paquete NuGet de EF se registró un generador de conexión predeterminada que apunta a SQL Express o LocalDB, dependiendo de cuál ha instalado.  

Para establecer un generador de conexión, especifique el nombre de tipo calificado de ensamblado en el **deafultConnectionFactory** elemento.  

> [!NOTE]
> Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo. Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.  

Este es un ejemplo de cómo configurar su propio generador de conexión predeterminada:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

El ejemplo anterior requiere que la fábrica personalizada para tener un constructor sin parámetros. Si es necesario, puede especificar los parámetros del constructor mediante el **parámetros** elemento.  

Por ejemplo, el SqlCeConnectionFactory, que se incluye en Entity Framework, requiere que proporcione un nombre invariable del proveedor al constructor. El nombre invariable del proveedor identifica la versión de SQL Compact que desea usar. La siguiente configuración hará que los contextos para utilizar SQL Compact versión 4.0 de forma predeterminada.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Si no establece un generador de conexión de forma predeterminada, Code First utiliza el SqlConnectionFactory, que apunta a `.\SQLEXPRESS`. SqlConnectionFactory también tiene un constructor que le permite invalidar las partes de la cadena de conexión. Si desea utilizar una instancia de SQL Server distinto `.\SQLEXPRESS` puede utilizar este constructor para establecer el servidor.  

La siguiente configuración hará que Code First usar **miServidorDeBasesDeDatos** para contextos que no tienen una cadena de conexión explícito establecido.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

De forma predeterminada, se supone que son argumentos de constructor de tipo cadena. Puede usar el atributo de tipo para cambiar esto.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicializadores de base de datos  

Inicializadores de base de datos se configuran por contexto. Se pueden establecer en el archivo de configuración mediante el **contexto** elemento. Este elemento usa el nombre completo del ensamblado para identificar el contexto que se está configurando.  

De forma predeterminada, los contextos de Code First se configuran para usar al inicializador de CreateDatabaseIfNotExists. Hay un **disableDatabaseInitialization** atributo el **contexto** elemento que se puede usar para deshabilitar la inicialización de la base de datos.  

Por ejemplo, la siguiente configuración deshabilita la inicialización de la base de datos para el contexto de Blogging.BlogContext definido en MyAssembly.dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Puede usar el **databaseInitializer** elemento va a establecer un inicializador personalizado.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Los parámetros del constructor usar la misma sintaxis como generadores de conexiones de forma predeterminada.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Puede configurar uno de los inicializadores de base de datos genérico que se incluyen en Entity Framework. El **tipo** atributo usa el formato de .NET Framework para tipos genéricos.  

Por ejemplo, si usa migraciones de Code First, puede configurar la base de datos para migrar automáticamente con el `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializador.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
