---
title: 'Configuración del archivo de configuración: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886551"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="feed0-102">Configuración del archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="feed0-102">Configuration File Settings</span></span>
<span data-ttu-id="feed0-103">Entity Framework permite especificar una serie de valores desde el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="feed0-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="feed0-104">En general, EF sigue un comportamiento de "Convención sobre configuración": todos los valores de configuración descritos en esta publicación tienen un comportamiento predeterminado, solo tiene que preocuparse por cambiar el valor cuando el valor predeterminado ya no satisface sus requisitos.</span><span class="sxs-lookup"><span data-stu-id="feed0-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="feed0-105">Una alternativa basada en código</span><span class="sxs-lookup"><span data-stu-id="feed0-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="feed0-106">Todos estos valores también se pueden aplicar mediante código.</span><span class="sxs-lookup"><span data-stu-id="feed0-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="feed0-107">A partir de EF6, se presentó la [configuración basada en código](code-based.md), que proporciona una manera centralizada de aplicar la configuración desde el código.</span><span class="sxs-lookup"><span data-stu-id="feed0-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="feed0-108">Antes de EF6, todavía se puede aplicar la configuración desde el código, pero es necesario usar varias API para configurar distintas áreas.</span><span class="sxs-lookup"><span data-stu-id="feed0-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="feed0-109">La opción del archivo de configuración permite cambiar fácilmente esta configuración durante la implementación sin necesidad de actualizar el código.</span><span class="sxs-lookup"><span data-stu-id="feed0-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="feed0-110">La sección de configuración Entity Framework</span><span class="sxs-lookup"><span data-stu-id="feed0-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="feed0-111">A partir de EF 4.1, podría establecer el inicializador de base de datos para un contexto mediante la sección **appSettings** del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="feed0-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="feed0-112">En EF 4,3, se presentó la sección **entityFramework** personalizada para controlar la nueva configuración.</span><span class="sxs-lookup"><span data-stu-id="feed0-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="feed0-113">Entity Framework seguirá reconociendo los inicializadores de base de datos establecidos con el formato antiguo, pero se recomienda pasar al nuevo formato siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="feed0-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="feed0-114">La sección **entityFramework** se agregó automáticamente al archivo de configuración del proyecto al instalar el paquete NuGet entityFramework.</span><span class="sxs-lookup"><span data-stu-id="feed0-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="feed0-115">Cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="feed0-115">Connection Strings</span></span>  

<span data-ttu-id="feed0-116">En [esta página](~/ef6/fundamentals/configuring/connection-strings.md) se proporcionan más detalles sobre cómo Entity Framework determina la base de datos que se va a usar, incluidas las cadenas de conexión en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="feed0-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="feed0-117">Las cadenas de conexión van en el elemento **connectionStrings** estándar y no requieren la sección **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="feed0-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="feed0-118">Los modelos basados en Code First usan cadenas de conexión ADO.NET normales.</span><span class="sxs-lookup"><span data-stu-id="feed0-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="feed0-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="feed0-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="feed0-120">Los modelos basados en el diseñador EF usan cadenas de conexión EF especiales.</span><span class="sxs-lookup"><span data-stu-id="feed0-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="feed0-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="feed0-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="feed0-122">Tipo de configuración basada en código (EF6 en adelante)</span><span class="sxs-lookup"><span data-stu-id="feed0-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="feed0-123">A partir de EF6, puede especificar el DbConfiguration para EF que se va a usar para la [configuración basada en código](code-based.md) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="feed0-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="feed0-124">En la mayoría de los casos, no es necesario especificar esta configuración, ya que EF detectará automáticamente el DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="feed0-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="feed0-125">Para obtener detalles sobre Cuándo es posible que tenga que especificar DbConfiguration en el archivo de configuración, consulte la sección **mover DbConfiguration** de [configuración basada en código](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="feed0-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="feed0-126">Para establecer un tipo DbConfiguration, especifique el nombre del tipo calificado con el ensamblado en el elemento **codeConfigurationType** .</span><span class="sxs-lookup"><span data-stu-id="feed0-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="feed0-127">Un nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido de una coma, el ensamblado en el que reside el tipo.</span><span class="sxs-lookup"><span data-stu-id="feed0-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="feed0-128">También puede especificar la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="feed0-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="feed0-129">Proveedores de bases de datos de EF (EF6 en adelante)</span><span class="sxs-lookup"><span data-stu-id="feed0-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="feed0-130">Antes de EF6, los elementos específicos de Entity Framework de un proveedor de base de datos debían incluirse como parte del proveedor ADO.NET básico.</span><span class="sxs-lookup"><span data-stu-id="feed0-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="feed0-131">A partir de EF6, las partes específicas de EF ahora se administran y registran por separado.</span><span class="sxs-lookup"><span data-stu-id="feed0-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="feed0-132">Normalmente no necesitará registrar los proveedores.</span><span class="sxs-lookup"><span data-stu-id="feed0-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="feed0-133">Esto lo realiza normalmente el proveedor cuando se instala.</span><span class="sxs-lookup"><span data-stu-id="feed0-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="feed0-134">Los proveedores se registran mediante la inclusión de un elemento de **proveedor** en la sección de **proveedores** secundarios de la sección **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="feed0-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="feed0-135">Hay dos atributos necesarios para una entrada de proveedor:</span><span class="sxs-lookup"><span data-stu-id="feed0-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="feed0-136">**invariantName** identifica el proveedor ADO.net principal al que se destina este proveedor EF</span><span class="sxs-lookup"><span data-stu-id="feed0-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="feed0-137">**Type** es el nombre de tipo calificado con el ensamblado de la implementación del proveedor EF</span><span class="sxs-lookup"><span data-stu-id="feed0-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="feed0-138">Un nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido de una coma, el ensamblado en el que reside el tipo.</span><span class="sxs-lookup"><span data-stu-id="feed0-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="feed0-139">También puede especificar la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="feed0-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="feed0-140">Como ejemplo, aquí se muestra la entrada que se crea para registrar el proveedor de SQL Server predeterminado al instalar Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="feed0-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="feed0-141">Interceptores (EF 6.1 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="feed0-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="feed0-142">A partir de EF 6.1, puede registrar los interceptores en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="feed0-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="feed0-143">Los interceptores permiten ejecutar lógica adicional cuando EF realiza ciertas operaciones, como la ejecución de consultas de base de datos, la apertura de conexiones, etc.</span><span class="sxs-lookup"><span data-stu-id="feed0-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="feed0-144">Los interceptores se registran mediante la inclusión de un elemento **interceptor** en la sección de interceptores secundaria de la sección **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="feed0-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="feed0-145">Por ejemplo, la configuración siguiente registra el interceptor de **DatabaseLogger** integrado que registrará todas las operaciones de base de datos en la consola.</span><span class="sxs-lookup"><span data-stu-id="feed0-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="feed0-146">Registro de operaciones de base de datos en un archivo (EF 6.1 en adelante)</span><span class="sxs-lookup"><span data-stu-id="feed0-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="feed0-147">El registro de los interceptores mediante el archivo de configuración es especialmente útil si desea agregar el registro a una aplicación existente para ayudar a depurar un problema.</span><span class="sxs-lookup"><span data-stu-id="feed0-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="feed0-148">**DatabaseLogger** admite el registro en un archivo proporcionando el nombre de archivo como un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="feed0-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="feed0-149">De forma predeterminada, esto hará que el archivo de registro se sobrescriba con un nuevo archivo cada vez que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="feed0-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="feed0-150">En su lugar, anexe al archivo de registro si ya existe, use algo parecido a:</span><span class="sxs-lookup"><span data-stu-id="feed0-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="feed0-151">Para obtener información adicional sobre **DatabaseLogger** y el registro de interceptores, consulte [la entrada de blog EF 6,1: Activar el registro sin volver a](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)compilar.</span><span class="sxs-lookup"><span data-stu-id="feed0-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="feed0-152">Code First factoría de conexión predeterminada</span><span class="sxs-lookup"><span data-stu-id="feed0-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="feed0-153">La sección configuración permite especificar un generador de conexiones predeterminado que Code First debe usar para buscar una base de datos que se utilizará para un contexto.</span><span class="sxs-lookup"><span data-stu-id="feed0-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="feed0-154">El generador de conexiones predeterminado solo se usa cuando no se ha agregado ninguna cadena de conexión al archivo de configuración para un contexto.</span><span class="sxs-lookup"><span data-stu-id="feed0-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="feed0-155">Al instalar el paquete de NuGet de EF, se registró una factoría de conexión predeterminada que señala a SQL Express o a LocalDB, en función de la que haya instalado.</span><span class="sxs-lookup"><span data-stu-id="feed0-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="feed0-156">Para establecer un generador de conexiones, especifique el nombre del tipo calificado con el ensamblado en el elemento **defaultConnectionFactory** .</span><span class="sxs-lookup"><span data-stu-id="feed0-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="feed0-157">Un nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido de una coma, el ensamblado en el que reside el tipo.</span><span class="sxs-lookup"><span data-stu-id="feed0-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="feed0-158">También puede especificar la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="feed0-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="feed0-159">A continuación se muestra un ejemplo de configuración de su propio generador de conexiones predeterminado:</span><span class="sxs-lookup"><span data-stu-id="feed0-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="feed0-160">En el ejemplo anterior se requiere que el generador personalizado tenga un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="feed0-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="feed0-161">Si es necesario, puede especificar parámetros de constructor mediante el elemento Parameters.</span><span class="sxs-lookup"><span data-stu-id="feed0-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="feed0-162">Por ejemplo, SqlCeConnectionFactory, que se incluye en Entity Framework, requiere que proporcione un nombre invariable de proveedor al constructor.</span><span class="sxs-lookup"><span data-stu-id="feed0-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="feed0-163">El nombre invariable del proveedor identifica la versión de SQL Compact que quiere usar.</span><span class="sxs-lookup"><span data-stu-id="feed0-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="feed0-164">La configuración siguiente hará que los contextos usen SQL Compact versión 4,0 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="feed0-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="feed0-165">Si no establece un generador de conexiones predeterminado, Code First usa SqlConnectionFactory, que apunta a `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="feed0-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="feed0-166">SqlConnectionFactory también tiene un constructor que permite invalidar partes de la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="feed0-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="feed0-167">Si desea utilizar una instancia de SQL Server distinta de `.\SQLEXPRESS` , puede usar este constructor para establecer el servidor.</span><span class="sxs-lookup"><span data-stu-id="feed0-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="feed0-168">La configuración siguiente hará que Code First use **MyDatabaseServer** para los contextos que no tienen un conjunto de cadenas de conexión explícitas.</span><span class="sxs-lookup"><span data-stu-id="feed0-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="feed0-169">De forma predeterminada, se supone que los argumentos del constructor son del tipo cadena.</span><span class="sxs-lookup"><span data-stu-id="feed0-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="feed0-170">Puede usar el atributo type para cambiar esto.</span><span class="sxs-lookup"><span data-stu-id="feed0-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="feed0-171">Inicializadores de base de datos</span><span class="sxs-lookup"><span data-stu-id="feed0-171">Database Initializers</span></span>  

<span data-ttu-id="feed0-172">Los inicializadores de base de datos se configuran en función del contexto.</span><span class="sxs-lookup"><span data-stu-id="feed0-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="feed0-173">Se pueden establecer en el archivo de configuración mediante el elemento de **contexto** .</span><span class="sxs-lookup"><span data-stu-id="feed0-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="feed0-174">Este elemento usa el nombre completo del ensamblado para identificar el contexto que se está configurando.</span><span class="sxs-lookup"><span data-stu-id="feed0-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="feed0-175">De forma predeterminada, Code First contextos se configuran para usar el inicializador CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="feed0-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="feed0-176">Hay un atributo **disableDatabaseInitialization** en el elemento de **contexto** que se puede usar para deshabilitar la inicialización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="feed0-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="feed0-177">Por ejemplo, la siguiente configuración deshabilita la inicialización de la base de datos para el contexto blog. BlogContext definido en myAssembly. dll.</span><span class="sxs-lookup"><span data-stu-id="feed0-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="feed0-178">Puede usar el elemento **databaseInitializer** para establecer un inicializador personalizado.</span><span class="sxs-lookup"><span data-stu-id="feed0-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="feed0-179">Los parámetros de constructor utilizan la misma sintaxis que los generadores de conexiones predeterminados.</span><span class="sxs-lookup"><span data-stu-id="feed0-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="feed0-180">Puede configurar uno de los inicializadores de base de datos genéricos que se incluyen en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="feed0-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="feed0-181">El atributo **Type** utiliza el formato de .NET Framework para los tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="feed0-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="feed0-182">Por ejemplo, si usa migraciones de Code First, puede configurar la base de datos que se va a migrar automáticamente mediante `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` el inicializador.</span><span class="sxs-lookup"><span data-stu-id="feed0-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
