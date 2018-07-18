---
title: Las cadenas de conexión y los modelos - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122756"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="1d94c-102">Los modelos y las cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="1d94c-102">Connection strings and models</span></span>
<span data-ttu-id="1d94c-103">En este tema se trata cómo Entity Framework detecta qué conexión de base de datos para usar y cómo se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="1d94c-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="1d94c-104">Los modelos creados con el Diseñador de EF Code First y se tratan en este tema.</span><span class="sxs-lookup"><span data-stu-id="1d94c-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="1d94c-105">Normalmente, una aplicación de Entity Framework usa una clase derivada de DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="1d94c-106">Esta clase derivada llamará uno de los constructores de la clase base DbContext para controlar:</span><span class="sxs-lookup"><span data-stu-id="1d94c-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="1d94c-107">¿Cómo se conectará el contexto de una base de datos, es decir, cómo una cadena de conexión se encuentra o usan</span><span class="sxs-lookup"><span data-stu-id="1d94c-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="1d94c-108">Si va a usar el contexto de calcular un modelo con Code First o cargar un modelo creado con EF Designer</span><span class="sxs-lookup"><span data-stu-id="1d94c-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="1d94c-109">Opciones avanzadas adicionales</span><span class="sxs-lookup"><span data-stu-id="1d94c-109">Additional advanced options</span></span>  

<span data-ttu-id="1d94c-110">Los fragmentos siguientes muestran que algunas de las formas de los constructores de DbContext se pueden usar.</span><span class="sxs-lookup"><span data-stu-id="1d94c-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="1d94c-111">Usar Code First con conexión por convención</span><span class="sxs-lookup"><span data-stu-id="1d94c-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="1d94c-112">Si no ha hecho cualquier otra configuración en la aplicación, a continuación, llamar al constructor sin parámetros en DbContext hará que DbContext se ejecuten en modo Code First con una conexión de base de datos creada por convención.</span><span class="sxs-lookup"><span data-stu-id="1d94c-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="1d94c-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-113">For example:</span></span>  

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

<span data-ttu-id="1d94c-114">En este ejemplo DbContext utiliza el nombre completo del espacio de nombres de su nombre de la base de datos de contexto derivado class—Demo.EF.BloggingContext—as y crea una cadena de conexión para esta base de datos con SQL Express o LocalDB.</span><span class="sxs-lookup"><span data-stu-id="1d94c-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="1d94c-115">Si ambos están instalados, se usará SQL Express.</span><span class="sxs-lookup"><span data-stu-id="1d94c-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="1d94c-116">Visual Studio 2010 incluye SQL Express de forma predeterminada y Visual Studio 2012 y versiones posteriores incluyen LocalDB.</span><span class="sxs-lookup"><span data-stu-id="1d94c-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="1d94c-117">Durante la instalación, el paquete EntityFramework NuGet comprueba qué servidor de base de datos está disponible.</span><span class="sxs-lookup"><span data-stu-id="1d94c-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="1d94c-118">El paquete de NuGet, a continuación, actualizará el archivo de configuración estableciendo el servidor de base de datos predeterminado que usa Code First al crear una conexión por convención.</span><span class="sxs-lookup"><span data-stu-id="1d94c-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="1d94c-119">Si se está ejecutando SQL Express, se usará.</span><span class="sxs-lookup"><span data-stu-id="1d94c-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="1d94c-120">Si SQL Express no está disponible, a continuación, LocalDB se registrará como el valor predeterminado en su lugar.</span><span class="sxs-lookup"><span data-stu-id="1d94c-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="1d94c-121">No se realizan cambios en el archivo de configuración si ya contiene un valor para el generador de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1d94c-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="1d94c-122">Usar Code First con conexión por convención y el nombre de la base de datos especificada</span><span class="sxs-lookup"><span data-stu-id="1d94c-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="1d94c-123">Si no ha hecho cualquier otra configuración en la aplicación, a continuación, llamar al constructor de cadena en DbContext con el nombre de la base de datos que desea usar hará que DbContext se ejecuten en modo Code First con una conexión de base de datos creada por convención a la base de datos ese nombre.</span><span class="sxs-lookup"><span data-stu-id="1d94c-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="1d94c-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="1d94c-125">En este ejemplo DbContext usa "BloggingDatabase" como el nombre de la base de datos y crea una cadena de conexión para esta base de datos con SQL Express (instalado con Visual Studio 2010) o LocalDB (instalado con Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="1d94c-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="1d94c-126">Si ambos están instalados, se usará SQL Express.</span><span class="sxs-lookup"><span data-stu-id="1d94c-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="1d94c-127">Usar Code First con la cadena de conexión en el archivo app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="1d94c-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="1d94c-128">Puede colocar una cadena de conexión en el archivo app.config o web.config.</span><span class="sxs-lookup"><span data-stu-id="1d94c-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="1d94c-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="1d94c-130">Esta es una manera fácil para indicar a DbContext para usar un servidor de base de datos que no sea SQL Express o LocalDB, el ejemplo anterior especifica una base de datos de SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="1d94c-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="1d94c-131">Si el nombre de la cadena de conexión coincide con el nombre de su contexto (con o sin la calificación de espacio de nombres), a continuación, se encontrará en DbContext cuando se usa el constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="1d94c-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="1d94c-132">Si el nombre de la cadena de conexión es diferente del nombre de su contexto puede indicar a DbContext para usar esta conexión en modo Code First, pasando el nombre de la cadena de conexión para el constructor DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="1d94c-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="1d94c-134">Como alternativa, puede usar el formato "nombre =\<nombre de la cadena de conexión\>" para la cadena pasada al constructor DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="1d94c-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="1d94c-136">Este formulario de forma explícita que espera que la cadena de conexión que se encuentren en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="1d94c-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="1d94c-137">Se producirá una excepción si no se encuentra una cadena de conexión con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="1d94c-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="1d94c-138">Base de datos/Model First con la cadena de conexión en el archivo app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="1d94c-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="1d94c-139">Los modelos creados con EF Designer son diferentes de Code First en que el modelo ya existe y no se genera desde el código cuando se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d94c-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="1d94c-140">Normalmente, existe el modelo como un archivo EDMX en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1d94c-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="1d94c-141">El diseñador agregará una cadena de conexión de EF a su archivo app.config o web.config.</span><span class="sxs-lookup"><span data-stu-id="1d94c-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="1d94c-142">Esta cadena de conexión es especial porque contiene información sobre cómo encontrar la información en el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="1d94c-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="1d94c-143">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-143">For example:</span></span>  

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

<span data-ttu-id="1d94c-144">EF Designer también generará el código que se indica a DbContext para usar esta conexión, pasando el nombre de la cadena de conexión al constructor DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="1d94c-145">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d94c-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="1d94c-146">DbContext sabe que tiene que para cargar el modelo existente (en lugar de usar Code First para calcular desde el código) porque la cadena de conexión es una cadena de conexión de EF que contiene los detalles del modelo que se utilice.</span><span class="sxs-lookup"><span data-stu-id="1d94c-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="1d94c-147">Otras opciones de constructor DbContext</span><span class="sxs-lookup"><span data-stu-id="1d94c-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="1d94c-148">La clase DbContext contiene otros constructores y los patrones de uso que permiten ciertos escenarios más avanzados.</span><span class="sxs-lookup"><span data-stu-id="1d94c-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="1d94c-149">Algunas de ellas son:</span><span class="sxs-lookup"><span data-stu-id="1d94c-149">Some of these are:</span></span>  

- <span data-ttu-id="1d94c-150">Puede usar la clase DbModelBuilder para generar un modelo Code First sin crear instancias de una instancia de DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="1d94c-151">El resultado de esto es un objeto DbModel.</span><span class="sxs-lookup"><span data-stu-id="1d94c-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="1d94c-152">A continuación, puede pasar este objeto DbModel a uno de los constructores de DbContext cuando esté listo para crear la instancia de DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="1d94c-153">Puede pasar una cadena de conexión completa a DbContext en lugar de simplemente el nombre de cadena de conexión o la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d94c-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="1d94c-154">De forma predeterminada se usa esta cadena de conexión con el proveedor System.Data.SqlClient; Esto se puede cambiar estableciendo una implementación diferente de IConnectionFactory en contexto. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="1d94c-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="1d94c-155">Puede usar un objeto DbConnection existente pasando a un constructor de DbContext.</span><span class="sxs-lookup"><span data-stu-id="1d94c-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="1d94c-156">Si el objeto de conexión es una instancia de EntityConnection, será el modelo especificado en la conexión usa en lugar de calcular un modelo con Code First.</span><span class="sxs-lookup"><span data-stu-id="1d94c-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="1d94c-157">Si el objeto es una instancia de algún otro tipo, por ejemplo, SqlConnection, a continuación, el contexto usará para el modo de Code First.</span><span class="sxs-lookup"><span data-stu-id="1d94c-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="1d94c-158">Puede pasar un ObjectContext existente a un constructor de DbContext para crear una clase DbContext ajuste del contexto existente.</span><span class="sxs-lookup"><span data-stu-id="1d94c-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="1d94c-159">Esto puede usarse para las aplicaciones existentes que utilizan la clase ObjectContext pero que quieren aprovechar las ventajas de DbContext en algunas partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d94c-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
