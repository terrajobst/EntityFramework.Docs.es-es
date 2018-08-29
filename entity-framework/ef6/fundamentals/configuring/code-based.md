---
title: Configuración basada en código - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 58acb7e74fee66cc70f78ef2c3474524d19264de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993338"
---
# <a name="code-based-configuration"></a>Configuración basada en código
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Configuración de una aplicación de Entity Framework puede especificarse en un archivo de configuración (app.config/web.config) o a través del código. Esto último se conoce como configuración basada en código.  

Configuración en un archivo de configuración se describe en un [otro artículo](config-file.md). El archivo de configuración tiene prioridad sobre la configuración basada en código. En otras palabras, si se establece una opción de configuración en código y en el archivo de configuración, se usa la configuración en el archivo de configuración.  

## <a name="using-dbconfiguration"></a>Uso de DbConfiguration  

Configuración de código basado en EF6 y versiones posteriores se logra mediante la creación de una subclase de System.Data.Entity.Config.DbConfiguration. Las siguientes directrices se deben seguir al crear subclases de DbConfiguration:  

- Crear solo una clase DbConfiguration para su aplicación. Esta clase especifica la configuración de todo el dominio de aplicación.  
- La clase DbConfiguration se coloque en el mismo ensamblado que la clase DbContext. (Consulte la *DbConfiguration mover* sección si desea cambiar esto.)  
- Asigne a la clase DbConfiguration un constructor sin parámetros público.  
- Establecer opciones de configuración mediante una llamada a métodos protegidos de DbConfiguration desde dentro de este constructor.  

Siga estas instrucciones permite a EF detectar y usar la configuración automáticamente ambas herramientas que necesita tener acceso a su modelo y cuando se ejecuta la aplicación.  

## <a name="example"></a>Ejemplo  

Una clase derivada de DbConfiguration podría parecerse a esto:  

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

Esta clase configura EF para usar la estrategia de ejecución de SQL Azure - para reintentar automáticamente operaciones de base de datos - y que se usará la base de datos Local para bases de datos que se crean mediante la convención de Code First.  

## <a name="moving-dbconfiguration"></a>Mover DbConfiguration  

Hay casos donde no es posible poner su clase DbConfiguration en el mismo ensamblado que la clase DbContext. Por ejemplo, puede tener dos clases DbContext en ensamblados diferentes. Hay dos opciones para controlar esto.  

La primera opción es utilizar el archivo de configuración para especificar la instancia de DbConfiguration a utilizar. Para ello, establezca el atributo codeConfigurationType de la sección entityFramework. Por ejemplo:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

El valor de codeConfigurationType debe ser el ensamblado y el nombre completo del espacio de nombres de la clase DbConfiguration.  

La segunda opción es colocar DbConfigurationTypeAttribute en la clase de contexto. Por ejemplo:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

El valor pasado al atributo puede ser el tipo de DbConfiguration: como se muestra arriba - o el ensamblado y el espacio de nombres califican el nombre de tipo cadena. Por ejemplo:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Establecer explícitamente DbConfiguration  

Existen algunas situaciones donde se necesite configuración antes de cualquier tipo de DbContext se ha usado. Ejemplos de esto son:  

- Uso de DbModelBuilder para crear un modelo sin un contexto  
- Uso de otro código de framework o utilidad que usa una clase DbContext que se usa ese contexto antes de que se usa el contexto de la aplicación  

En tales situaciones EF no puede detectar automáticamente la configuración y en su lugar, debe realizar una de las siguientes acciones:  

- Establezca el tipo de DbConfiguration en el archivo de configuración, como se describe en el *DbConfiguration mover* sección anterior
- Llame al método estático de DbConfiguration.SetConfiguration durante el inicio de la aplicación  

## <a name="overriding-dbconfiguration"></a>Invalidación de DbConfiguration  

Existen algunas situaciones donde es necesario para invalidar la configuración establecida en el DbConfiguration. Esto normalmente no se realiza por los desarrolladores de aplicaciones sino por proveedores de terceros y complementos que no se pueden usar una clase derivada de DbConfiguration.  

Para ello, Entity Framework permite registrar un controlador de eventos que se puede modificar la configuración existente antes de que está bloqueado.  También proporciona un método de azúcar específicamente para reemplazar cualquier servicio devuelto por el localizador de servicios EF. Se trata cómo está pensado para usarse:  

- Al iniciar la aplicación (antes de que se usa EF) el complemento o el proveedor debe registrar el método de controlador de eventos para este evento. (Tenga en cuenta que esto debe ocurrir antes de la aplicación usa EF.)  
- El controlador de eventos realiza una llamada a ReplaceService para todos los servicios que deben reemplazarse.  

Por ejemplo, sustituye IDbConnectionFactory y DbProviderService, registrar un controlador algo parecido a esto:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

En el código anterior MyProviderServices y MyConnectionFactory representan sus implementaciones del servicio.  

También puede agregar controladores de dependencia adicionales para obtener el mismo efecto.  

Tenga en cuenta que también podría encapsular DbProviderFactory de esta manera, pero al hacerlo así solo afectarán a EF y no los usos de DbProviderFactory fuera de EF. Por este motivo probablemente deseará ajustar DbProviderFactory si tiene antes de seguir.  

También debe tener en cuenta los servicios que se ejecutan externamente a la aplicación: por ejemplo, al ejecutar las migraciones desde la consola de administrador de paquetes. Al ejecutar migrar desde la consola que intentará encontrar su DbConfiguration. Sin embargo, si no obtendrá el servicio ajustado depende de dónde registra el controlador de eventos. Si está registrado como parte de la construcción de su DbConfiguration, a continuación, se debe ejecutar el código y debe obtener encapsular el servicio. Normalmente esto no será el caso y esto significa que las herramientas no obtención el servicio ajustado.  
