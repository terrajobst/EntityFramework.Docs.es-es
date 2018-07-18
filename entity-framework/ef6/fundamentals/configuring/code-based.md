---
title: Configuración basada en código - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
caps.latest.revision: 3
ms.openlocfilehash: d6a33434e582fcd7ce756b447d7f2cbab4ca43ec
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122745"
---
# <a name="code-based-configuration"></a><span data-ttu-id="e01fd-102">Configuración basada en código</span><span class="sxs-lookup"><span data-stu-id="e01fd-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="e01fd-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e01fd-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="e01fd-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="e01fd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="e01fd-105">Configuración de una aplicación de Entity Framework puede especificarse en un archivo de configuración (app.config/web.config) o a través del código.</span><span class="sxs-lookup"><span data-stu-id="e01fd-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="e01fd-106">Esto último se conoce como configuración basada en código.</span><span class="sxs-lookup"><span data-stu-id="e01fd-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="e01fd-107">Configuración en un archivo de configuración se describe en un [otro artículo](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="e01fd-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="e01fd-108">El archivo de configuración tiene prioridad sobre la configuración basada en código.</span><span class="sxs-lookup"><span data-stu-id="e01fd-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="e01fd-109">En otras palabras, si se establece una opción de configuración en código y en el archivo de configuración, se usa la configuración en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="e01fd-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="e01fd-110">Uso de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="e01fd-110">Using DbConfiguration</span></span>  

<span data-ttu-id="e01fd-111">Configuración de código basado en EF6 y versiones posteriores se logra mediante la creación de una subclase de System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e01fd-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="e01fd-112">Las siguientes directrices se deben seguir al crear subclases de DbConfiguration:</span><span class="sxs-lookup"><span data-stu-id="e01fd-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="e01fd-113">Crear solo una clase DbConfiguration para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="e01fd-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="e01fd-114">Esta clase especifica la configuración de todo el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="e01fd-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="e01fd-115">La clase DbConfiguration se coloque en el mismo ensamblado que la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="e01fd-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="e01fd-116">(Consulte la *DbConfiguration mover* sección si desea cambiar esto.)</span><span class="sxs-lookup"><span data-stu-id="e01fd-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="e01fd-117">Asigne a la clase DbConfiguration un constructor sin parámetros público.</span><span class="sxs-lookup"><span data-stu-id="e01fd-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="e01fd-118">Establecer opciones de configuración mediante una llamada a métodos protegidos de DbConfiguration desde dentro de este constructor.</span><span class="sxs-lookup"><span data-stu-id="e01fd-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="e01fd-119">Siga estas instrucciones permite a EF detectar y usar la configuración automáticamente ambas herramientas que necesita tener acceso a su modelo y cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e01fd-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="e01fd-120">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="e01fd-120">Example</span></span>  

<span data-ttu-id="e01fd-121">Una clase derivada de DbConfiguration podría parecerse a esto:</span><span class="sxs-lookup"><span data-stu-id="e01fd-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="e01fd-122">Esta clase configura EF para usar la estrategia de ejecución de SQL Azure - para reintentar automáticamente operaciones de base de datos - y que se usará la base de datos Local para bases de datos que se crean mediante la convención de Code First.</span><span class="sxs-lookup"><span data-stu-id="e01fd-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="e01fd-123">Mover DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="e01fd-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="e01fd-124">Hay casos donde no es posible poner su clase DbConfiguration en el mismo ensamblado que la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="e01fd-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="e01fd-125">Por ejemplo, puede tener dos clases DbContext en ensamblados diferentes.</span><span class="sxs-lookup"><span data-stu-id="e01fd-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="e01fd-126">Hay dos opciones para controlar esto.</span><span class="sxs-lookup"><span data-stu-id="e01fd-126">There are two options for handling this.</span></span>  

<span data-ttu-id="e01fd-127">La primera opción es utilizar el archivo de configuración para especificar la instancia de DbConfiguration a utilizar.</span><span class="sxs-lookup"><span data-stu-id="e01fd-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="e01fd-128">Para ello, establezca el atributo codeConfigurationType de la sección entityFramework.</span><span class="sxs-lookup"><span data-stu-id="e01fd-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="e01fd-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e01fd-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="e01fd-130">El valor de codeConfigurationType debe ser el ensamblado y el nombre completo del espacio de nombres de la clase DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e01fd-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="e01fd-131">La segunda opción es colocar DbConfigurationTypeAttribute en la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="e01fd-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="e01fd-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e01fd-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="e01fd-133">El valor pasado al atributo puede ser el tipo de DbConfiguration: como se muestra arriba - o el ensamblado y el espacio de nombres califican el nombre de tipo cadena.</span><span class="sxs-lookup"><span data-stu-id="e01fd-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="e01fd-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e01fd-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="e01fd-135">Establecer explícitamente DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="e01fd-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="e01fd-136">Existen algunas situaciones donde se necesite configuración antes de cualquier tipo de DbContext se ha usado.</span><span class="sxs-lookup"><span data-stu-id="e01fd-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="e01fd-137">Ejemplos de esto son:</span><span class="sxs-lookup"><span data-stu-id="e01fd-137">Examples of this include:</span></span>  

- <span data-ttu-id="e01fd-138">Uso de DbModelBuilder para crear un modelo sin un contexto</span><span class="sxs-lookup"><span data-stu-id="e01fd-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="e01fd-139">Uso de otro código de framework o utilidad que usa una clase DbContext que se usa ese contexto antes de que se usa el contexto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e01fd-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="e01fd-140">En tales situaciones EF no puede detectar automáticamente la configuración y en su lugar, debe realizar una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="e01fd-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="e01fd-141">Establezca el tipo de DbConfiguration en el archivo de configuración, como se describe en el *DbConfiguration mover* sección anterior</span><span class="sxs-lookup"><span data-stu-id="e01fd-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="e01fd-142">Llame al método estático de DbConfiguration.SetConfiguration durante el inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e01fd-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="e01fd-143">Invalidación de DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="e01fd-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="e01fd-144">Existen algunas situaciones donde es necesario para invalidar la configuración establecida en el DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e01fd-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="e01fd-145">Esto normalmente no se realiza por los desarrolladores de aplicaciones sino por proveedores de terceros y complementos que no se pueden usar una clase derivada de DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e01fd-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="e01fd-146">Para ello, Entity Framework permite registrar un controlador de eventos que se puede modificar la configuración existente antes de que está bloqueado.</span><span class="sxs-lookup"><span data-stu-id="e01fd-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="e01fd-147">También proporciona un método de azúcar específicamente para reemplazar cualquier servicio devuelto por el localizador de servicios EF.</span><span class="sxs-lookup"><span data-stu-id="e01fd-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="e01fd-148">Se trata cómo está pensado para usarse:</span><span class="sxs-lookup"><span data-stu-id="e01fd-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="e01fd-149">Al iniciar la aplicación (antes de que se usa EF) el complemento o el proveedor debe registrar el método de controlador de eventos para este evento.</span><span class="sxs-lookup"><span data-stu-id="e01fd-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="e01fd-150">(Tenga en cuenta que esto debe ocurrir antes de la aplicación usa EF.)</span><span class="sxs-lookup"><span data-stu-id="e01fd-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="e01fd-151">El controlador de eventos realiza una llamada a ReplaceService para todos los servicios que deben reemplazarse.</span><span class="sxs-lookup"><span data-stu-id="e01fd-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="e01fd-152">Por ejemplo, sustituye IDbConnectionFactory y DbProviderService, registrar un controlador algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="e01fd-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="e01fd-153">En el código anterior MyProviderServices y MyConnectionFactory representan sus implementaciones del servicio.</span><span class="sxs-lookup"><span data-stu-id="e01fd-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="e01fd-154">También puede agregar controladores de dependencia adicionales para obtener el mismo efecto.</span><span class="sxs-lookup"><span data-stu-id="e01fd-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="e01fd-155">Tenga en cuenta que también podría encapsular DbProviderFactory de esta manera, pero al hacerlo así solo afectarán a EF y no los usos de DbProviderFactory fuera de EF.</span><span class="sxs-lookup"><span data-stu-id="e01fd-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="e01fd-156">Por este motivo probablemente deseará ajustar DbProviderFactory si tiene antes de seguir.</span><span class="sxs-lookup"><span data-stu-id="e01fd-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="e01fd-157">También debe tener en cuenta los servicios que se ejecutan externamente a la aplicación: por ejemplo, al ejecutar las migraciones desde la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="e01fd-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="e01fd-158">Al ejecutar migrar desde la consola que intentará encontrar su DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e01fd-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="e01fd-159">Sin embargo, si no obtendrá el servicio ajustado depende de dónde registra el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="e01fd-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="e01fd-160">Si está registrado como parte de la construcción de su DbConfiguration, a continuación, se debe ejecutar el código y debe obtener encapsular el servicio.</span><span class="sxs-lookup"><span data-stu-id="e01fd-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="e01fd-161">Normalmente esto no será el caso y esto significa que las herramientas no obtención el servicio ajustado.</span><span class="sxs-lookup"><span data-stu-id="e01fd-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
