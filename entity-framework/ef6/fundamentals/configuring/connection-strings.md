---
title: Las cadenas de conexión y los modelos - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490755"
---
# <a name="connection-strings-and-models"></a>Los modelos y las cadenas de conexión
En este tema se trata cómo Entity Framework detecta qué conexión de base de datos para usar y cómo se puede cambiar. Los modelos creados con el Diseñador de EF Code First y se tratan en este tema.  

Normalmente, una aplicación de Entity Framework usa una clase derivada de DbContext. Esta clase derivada llamará uno de los constructores de la clase base DbContext para controlar:  

- ¿Cómo se conectará el contexto de una base de datos, es decir, cómo una cadena de conexión se encuentra o usan  
- Si va a usar el contexto de calcular un modelo con Code First o cargar un modelo creado con EF Designer  
- Opciones avanzadas adicionales  

Los fragmentos siguientes muestran que algunas de las formas de los constructores de DbContext se pueden usar.  

## <a name="use-code-first-with-connection-by-convention"></a>Usar Code First con conexión por convención  

Si no ha hecho cualquier otra configuración en la aplicación, a continuación, llamar al constructor sin parámetros en DbContext hará que DbContext se ejecuten en modo Code First con una conexión de base de datos creada por convención. Por ejemplo:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

En este ejemplo DbContext utiliza el nombre completo del espacio de nombres de su nombre de la base de datos de contexto derivado class—Demo.EF.BloggingContext—as y crea una cadena de conexión para esta base de datos con SQL Express o LocalDB. Si ambos están instalados, se usará SQL Express.  

Visual Studio 2010 incluye SQL Express de forma predeterminada y Visual Studio 2012 y versiones posteriores incluyen LocalDB. Durante la instalación, el paquete EntityFramework NuGet comprueba qué servidor de base de datos está disponible. El paquete de NuGet, a continuación, actualizará el archivo de configuración estableciendo el servidor de base de datos predeterminado que usa Code First al crear una conexión por convención. Si se está ejecutando SQL Express, se usará. Si SQL Express no está disponible, a continuación, LocalDB se registrará como el valor predeterminado en su lugar. No se realizan cambios en el archivo de configuración si ya contiene un valor para el generador de conexión predeterminado.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Usar Code First con conexión por convención y el nombre de la base de datos especificada  

Si no ha hecho cualquier otra configuración en la aplicación, a continuación, llamar al constructor de cadena en DbContext con el nombre de la base de datos que desea usar hará que DbContext se ejecuten en modo Code First con una conexión de base de datos creada por convención a la base de datos ese nombre. Por ejemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

En este ejemplo DbContext usa "BloggingDatabase" como el nombre de la base de datos y crea una cadena de conexión para esta base de datos con SQL Express (instalado con Visual Studio 2010) o LocalDB (instalado con Visual Studio 2012). Si ambos están instalados, se usará SQL Express.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Usar Code First con la cadena de conexión en el archivo app.config/web.config  

Puede colocar una cadena de conexión en el archivo app.config o web.config. Por ejemplo:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Esta es una manera fácil para indicar a DbContext para usar un servidor de base de datos que no sea SQL Express o LocalDB, el ejemplo anterior especifica una base de datos de SQL Server Compact Edition.  

Si el nombre de la cadena de conexión coincide con el nombre de su contexto (con o sin la calificación de espacio de nombres), a continuación, se encontrará en DbContext cuando se usa el constructor sin parámetros. Si el nombre de la cadena de conexión es diferente del nombre de su contexto puede indicar a DbContext para usar esta conexión en modo Code First, pasando el nombre de la cadena de conexión para el constructor DbContext. Por ejemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Como alternativa, puede usar el formato "nombre =\<nombre de la cadena de conexión\>" para la cadena pasada al constructor DbContext. Por ejemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Este formulario de forma explícita que espera que la cadena de conexión que se encuentren en el archivo de configuración. Se producirá una excepción si no se encuentra una cadena de conexión con el nombre especificado.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Base de datos/Model First con la cadena de conexión en el archivo app.config/web.config  

Los modelos creados con EF Designer son diferentes de Code First en que el modelo ya existe y no se genera desde el código cuando se ejecuta la aplicación. Normalmente, existe el modelo como un archivo EDMX en el proyecto.  

El diseñador agregará una cadena de conexión de EF a su archivo app.config o web.config. Esta cadena de conexión es especial porque contiene información sobre cómo encontrar la información en el archivo EDMX. Por ejemplo:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

EF Designer también generará el código que se indica a DbContext para usar esta conexión, pasando el nombre de la cadena de conexión al constructor DbContext. Por ejemplo:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext sabe que tiene que para cargar el modelo existente (en lugar de usar Code First para calcular desde el código) porque la cadena de conexión es una cadena de conexión de EF que contiene los detalles del modelo que se utilice.  

## <a name="other-dbcontext-constructor-options"></a>Otras opciones de constructor DbContext  

La clase DbContext contiene otros constructores y los patrones de uso que permiten ciertos escenarios más avanzados. Algunas de ellas son:  

- Puede usar la clase DbModelBuilder para generar un modelo Code First sin crear instancias de una instancia de DbContext. El resultado de esto es un objeto DbModel. A continuación, puede pasar este objeto DbModel a uno de los constructores de DbContext cuando esté listo para crear la instancia de DbContext.  
- Puede pasar una cadena de conexión completa a DbContext en lugar de simplemente el nombre de cadena de conexión o la base de datos. De forma predeterminada se usa esta cadena de conexión con el proveedor System.Data.SqlClient; Esto se puede cambiar estableciendo una implementación diferente de IConnectionFactory en contexto. Database.DefaultConnectionFactory.  
- Puede usar un objeto DbConnection existente pasando a un constructor de DbContext. Si el objeto de conexión es una instancia de EntityConnection, será el modelo especificado en la conexión usa en lugar de calcular un modelo con Code First. Si el objeto es una instancia de algún otro tipo, por ejemplo, SqlConnection, a continuación, el contexto usará para el modo de Code First.  
- Puede pasar un ObjectContext existente a un constructor de DbContext para crear una clase DbContext ajuste del contexto existente. Esto puede usarse para las aplicaciones existentes que utilizan la clase ObjectContext pero que quieren aprovechar las ventajas de DbContext en algunas partes de la aplicación.  
