---
title: Archivo de configuración - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
caps.latest.revision: 3
ms.openlocfilehash: 24faed6bdae6cc4b31808dc5bbebddb94128d0d3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121936"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="86bd2-102">Archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="86bd2-102">Configuration File Settings</span></span>
<span data-ttu-id="86bd2-103">Entity Framework permite a una serie de valores que se especifique el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="86bd2-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="86bd2-104">En general EF sigue un principio 'convención sobre configuración': todas las opciones descritas en esta publicación tienen un comportamiento predeterminado, sólo deberá preocuparse acerca de cómo cambiar la configuración cuando el valor predeterminado ya no satisface sus requisitos.</span><span class="sxs-lookup"><span data-stu-id="86bd2-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="86bd2-105">Una alternativa basada en código</span><span class="sxs-lookup"><span data-stu-id="86bd2-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="86bd2-106">Todas estas configuraciones pueden aplicarse también mediante código.</span><span class="sxs-lookup"><span data-stu-id="86bd2-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="86bd2-107">A partir de EF6 introdujimos [configuración basada en código](code-based.md), que proporciona un modo centralizado de la aplicación configuración desde el código.</span><span class="sxs-lookup"><span data-stu-id="86bd2-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="86bd2-108">Antes de EF6, todavía se puede aplicar la configuración desde el código, pero deberá usar varias API para configurar diferentes áreas.</span><span class="sxs-lookup"><span data-stu-id="86bd2-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="86bd2-109">La opción de archivo de configuración permite que esta configuración se puede cambiar fácilmente durante la implementación sin actualizar el código.</span><span class="sxs-lookup"><span data-stu-id="86bd2-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="86bd2-110">La sección de configuración de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="86bd2-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="86bd2-111">A partir con EF4.1, puede establecer el inicializador de base de datos para un contexto con el **appSettings** sección del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="86bd2-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="86bd2-112">En EF 4.3 introdujimos personalizado **entityFramework** sección para controlar la nueva configuración.</span><span class="sxs-lookup"><span data-stu-id="86bd2-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="86bd2-113">Entity Framework reconocerá sigue establecidos con el formato antiguo de inicializadores de base de datos, pero se recomienda mover al nuevo formato siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="86bd2-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="86bd2-114">El **entityFramework** sección se agregó automáticamente al archivo de configuración del proyecto cuando se ha instalado el paquete EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="86bd2-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="86bd2-115">Cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="86bd2-115">Connection Strings</span></span>  

<span data-ttu-id="86bd2-116">[Esta página](~/ef6/fundamentals/configuring/connection-strings.md) proporciona más detalles sobre cómo Entity Framework determina la base de datos que se usará, incluidas las cadenas de conexión en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="86bd2-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="86bd2-117">Van de cadenas de conexión en el estándar **connectionStrings** elemento y no requieren la **entityFramework** sección.</span><span class="sxs-lookup"><span data-stu-id="86bd2-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="86bd2-118">Los modelos de código que se basa en primer lugar utilizan cadenas de conexión de ADO.NET normales.</span><span class="sxs-lookup"><span data-stu-id="86bd2-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="86bd2-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="86bd2-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="86bd2-120">EF Designer en función de las cadenas de conexión de modelos uso especiales EF.</span><span class="sxs-lookup"><span data-stu-id="86bd2-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="86bd2-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="86bd2-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="86bd2-122">Tipo de configuración basado en código (EF6 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="86bd2-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="86bd2-123">A partir de EF6, puede especificar el DbConfiguration para EF que use para [configuración basada en código](code-based.md) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86bd2-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="86bd2-124">En la mayoría de los casos no es necesario especificar esta opción como EF detectará automáticamente su DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="86bd2-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="86bd2-125">Para obtener información detallada de cuándo es posible que necesite especificar DbConfiguration en el archivo de configuración, consulte el **mover DbConfiguration** sección de [configuración basada en código](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="86bd2-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="86bd2-126">Para establecer un tipo de DbConfiguration, especifique el nombre de tipo calificado de ensamblado en el **codeConfigurationType** elemento.</span><span class="sxs-lookup"><span data-stu-id="86bd2-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="86bd2-127">Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo.</span><span class="sxs-lookup"><span data-stu-id="86bd2-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="86bd2-128">Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="86bd2-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="86bd2-129">Proveedores de base de datos EF (EF6 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="86bd2-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="86bd2-130">Antes de EF6, partes específicas de Entity Framework de un proveedor de base de datos se tenían que se incluye como parte del proveedor ADO.NET core.</span><span class="sxs-lookup"><span data-stu-id="86bd2-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="86bd2-131">A partir de EF6, las partes concretas de EF están administradas y registrar por separado.</span><span class="sxs-lookup"><span data-stu-id="86bd2-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="86bd2-132">Normalmente no tendrá que registrar proveedores usted mismo.</span><span class="sxs-lookup"><span data-stu-id="86bd2-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="86bd2-133">Esto normalmente se realiza por el proveedor que se instala.</span><span class="sxs-lookup"><span data-stu-id="86bd2-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="86bd2-134">Los proveedores se registran mediante la inclusión de un **proveedor** elemento bajo el **proveedores** sección secundarios de la **entityFramework** sección.</span><span class="sxs-lookup"><span data-stu-id="86bd2-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="86bd2-135">Hay dos atributos necesarios para una entrada del proveedor:</span><span class="sxs-lookup"><span data-stu-id="86bd2-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="86bd2-136">**invariantName** identifica el proveedor de ADO.NET básico que los destinos de proveedor EF</span><span class="sxs-lookup"><span data-stu-id="86bd2-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="86bd2-137">**tipo** es el nombre de tipo calificado de ensamblado de la implementación del proveedor EF</span><span class="sxs-lookup"><span data-stu-id="86bd2-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="86bd2-138">Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo.</span><span class="sxs-lookup"><span data-stu-id="86bd2-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="86bd2-139">Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="86bd2-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="86bd2-140">Como ejemplo aquí es la entrada creada para registrar el proveedor de SQL Server de forma predeterminada al instalar Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86bd2-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="86bd2-141">Interceptores (EF6.1 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="86bd2-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="86bd2-142">Comenzando con EF6.1 puede registrar los interceptores en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="86bd2-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="86bd2-143">Los interceptores permiten ejecutar una lógica adicional cuando EF realiza ciertas operaciones, tales como ejecutar consultas de base de datos, abrir las conexiones, etc.</span><span class="sxs-lookup"><span data-stu-id="86bd2-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="86bd2-144">Los interceptores se registran mediante la inclusión de un **interceptor** elemento bajo el **interceptores** sección secundarios de la **entityFramework** sección.</span><span class="sxs-lookup"><span data-stu-id="86bd2-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="86bd2-145">Por ejemplo, la siguiente configuración registra integrado **DatabaseLogger** interceptor que se registrará todas las operaciones de base de datos en la consola.</span><span class="sxs-lookup"><span data-stu-id="86bd2-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="86bd2-146">Operaciones de base de datos de registro en un archivo (EF6.1 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="86bd2-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="86bd2-147">Registrar los interceptores mediante el archivo de configuración es especialmente útil cuando desea agregar el registro a una aplicación existente para ayudar a depurar un problema.</span><span class="sxs-lookup"><span data-stu-id="86bd2-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="86bd2-148">**DatabaseLogger** admite el registro en un archivo, proporcionando el nombre de archivo como un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="86bd2-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="86bd2-149">De forma predeterminada, esto hará que el archivo de registro se sobrescriba con un archivo nuevo cada vez que se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86bd2-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="86bd2-150">Para anexar en su lugar en el registro de archivo si ya existe usar algo como:</span><span class="sxs-lookup"><span data-stu-id="86bd2-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="86bd2-151">Para obtener más información sobre **DatabaseLogger** y registrar los interceptores, consulte el blog [EF 6.1: activar el registro sin volver a compilar](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="86bd2-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="86bd2-152">Generador de conexión predeterminado primer código</span><span class="sxs-lookup"><span data-stu-id="86bd2-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="86bd2-153">La sección de configuración permite especificar un generador de conexión predeterminada que debe usar Code First para buscar una base de datos que se usará para un contexto.</span><span class="sxs-lookup"><span data-stu-id="86bd2-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="86bd2-154">El generador de conexión predeterminada solo se usa cuando no se ha agregado ninguna cadena de conexión para el archivo de configuración para un contexto.</span><span class="sxs-lookup"><span data-stu-id="86bd2-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="86bd2-155">Cuando instala el paquete NuGet de EF se registró un generador de conexión predeterminada que apunta a SQL Express o LocalDB, dependiendo de cuál ha instalado.</span><span class="sxs-lookup"><span data-stu-id="86bd2-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="86bd2-156">Para establecer un generador de conexión, especifique el nombre de tipo calificado de ensamblado en el **deafultConnectionFactory** elemento.</span><span class="sxs-lookup"><span data-stu-id="86bd2-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="86bd2-157">Nombre completo de ensamblado es el nombre completo del espacio de nombres, seguido por una coma, a continuación, el ensamblado que se encuentra en el tipo.</span><span class="sxs-lookup"><span data-stu-id="86bd2-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="86bd2-158">Opcionalmente puede especificar también la versión del ensamblado, la referencia cultural y el token de clave pública.</span><span class="sxs-lookup"><span data-stu-id="86bd2-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="86bd2-159">Este es un ejemplo de cómo configurar su propio generador de conexión predeterminada:</span><span class="sxs-lookup"><span data-stu-id="86bd2-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="86bd2-160">El ejemplo anterior requiere que la fábrica personalizada para tener un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="86bd2-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="86bd2-161">Si es necesario, puede especificar los parámetros del constructor mediante el **parámetros** elemento.</span><span class="sxs-lookup"><span data-stu-id="86bd2-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="86bd2-162">Por ejemplo, el SqlCeConnectionFactory, que se incluye en Entity Framework, requiere que proporcione un nombre invariable del proveedor al constructor.</span><span class="sxs-lookup"><span data-stu-id="86bd2-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="86bd2-163">El nombre invariable del proveedor identifica la versión de SQL Compact que desea usar.</span><span class="sxs-lookup"><span data-stu-id="86bd2-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="86bd2-164">La siguiente configuración hará que los contextos para utilizar SQL Compact versión 4.0 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="86bd2-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="86bd2-165">Si no establece un generador de conexión de forma predeterminada, Code First utiliza el SqlConnectionFactory, que apunta a `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="86bd2-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="86bd2-166">SqlConnectionFactory también tiene un constructor que le permite invalidar las partes de la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="86bd2-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="86bd2-167">Si desea utilizar una instancia de SQL Server distinto `.\SQLEXPRESS` puede utilizar este constructor para establecer el servidor.</span><span class="sxs-lookup"><span data-stu-id="86bd2-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="86bd2-168">La siguiente configuración hará que Code First usar **miServidorDeBasesDeDatos** para contextos que no tienen una cadena de conexión explícito establecido.</span><span class="sxs-lookup"><span data-stu-id="86bd2-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="86bd2-169">De forma predeterminada, se supone que son argumentos de constructor de tipo cadena.</span><span class="sxs-lookup"><span data-stu-id="86bd2-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="86bd2-170">Puede usar el atributo de tipo para cambiar esto.</span><span class="sxs-lookup"><span data-stu-id="86bd2-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="86bd2-171">Inicializadores de base de datos</span><span class="sxs-lookup"><span data-stu-id="86bd2-171">Database Initializers</span></span>  

<span data-ttu-id="86bd2-172">Inicializadores de base de datos se configuran por contexto.</span><span class="sxs-lookup"><span data-stu-id="86bd2-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="86bd2-173">Se pueden establecer en el archivo de configuración mediante el **contexto** elemento.</span><span class="sxs-lookup"><span data-stu-id="86bd2-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="86bd2-174">Este elemento usa el nombre completo del ensamblado para identificar el contexto que se está configurando.</span><span class="sxs-lookup"><span data-stu-id="86bd2-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="86bd2-175">De forma predeterminada, los contextos de Code First se configuran para usar al inicializador de CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="86bd2-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="86bd2-176">Hay un **disableDatabaseInitialization** atributo el **contexto** elemento que se puede usar para deshabilitar la inicialización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="86bd2-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="86bd2-177">Por ejemplo, la siguiente configuración deshabilita la inicialización de la base de datos para el contexto de Blogging.BlogContext definido en MyAssembly.dll.</span><span class="sxs-lookup"><span data-stu-id="86bd2-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="86bd2-178">Puede usar el **databaseInitializer** elemento va a establecer un inicializador personalizado.</span><span class="sxs-lookup"><span data-stu-id="86bd2-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="86bd2-179">Los parámetros del constructor usar la misma sintaxis como generadores de conexiones de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="86bd2-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="86bd2-180">Puede configurar uno de los inicializadores de base de datos genérico que se incluyen en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86bd2-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="86bd2-181">El **tipo** atributo usa el formato de .NET Framework para tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="86bd2-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="86bd2-182">Por ejemplo, si usa migraciones de Code First, puede configurar la base de datos para migrar automáticamente con el `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializador.</span><span class="sxs-lookup"><span data-stu-id="86bd2-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
