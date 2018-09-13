---
title: 'Interceptar las operaciones de base de datos: EF6 y registro'
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 3f06e073f3ab6e46883663620219e302d5db1d60
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490095"
---
# <a name="logging-and-intercepting-database-operations"></a>Interceptar las operaciones de base de datos y registro
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

A partir de Entity Framework 6, cada vez que Entity Framework envía un comando a la base de datos, este comando se puede interceptar el código de aplicación. Esto se suele utilizar para el registro de SQL, pero también puede utilizarse para modificar o anular el comando.  

En concreto, EF incluye:  

- Una propiedad de registro para el contexto similar a DataContext.Log en LINQ to SQL  
- Un mecanismo para personalizar el contenido y el formato de la salida que se envían al registro  
- Bloques de creación de bajo nivel para lo que proporciona mayor control y flexibilidad de intercepción  

## <a name="context-log-property"></a>Propiedad del registro de contexto  

La propiedad DbContext.Database.Log puede establecerse en un delegado para cualquier método que toma una cadena. Normalmente se utiliza con cualquier TextWriter si se establece en el método de "Escritura" de esa TextWriter. Todos los SQL generado por el contexto actual se registrará en ese sistema de escritura. Por ejemplo, el código siguiente registrará SQL en la consola:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Tenga en cuenta que el contexto. Database.Log se establece en Console.Write. Esto es todo lo que se necesita para iniciar SQL en la consola.  

Vamos a agregar cierto código simple de la consulta/insert/update para que podamos ver algunos resultados:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Esto generará el siguiente resultado:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Tenga en cuenta que este es el resultado suponiendo que ya se ha realizado cualquier inicialización de la base de datos. Si no hubiera sucedido ya inicialización base de datos, a continuación, habría mucho más resultados que muestra todo el trabajo de migraciones hace en segundo plano para buscar o crear una nueva base de datos.)  

## <a name="what-gets-logged"></a>¿Qué se registra?  

Se registrará cuando se establece la propiedad del registro todos los elementos siguientes:  

- SQL para todos los distintos tipos de comandos. Por ejemplo:  
    - Consultas, incluidas las consultas LINQ normales, consultas eSQL y consultas sin procesar de métodos como SqlQuery  
    - Inserciones, actualizaciones y eliminaciones que se generan como parte de SaveChanges  
    - Relación de carga de consultas como las generadas por la carga diferida  
- Parámetros  
- Si se ejecuta el comando asincrónicamente  
- Una marca de tiempo que indica cuándo inició ejecutando el comando  
- Si el comando se completó correctamente, no se pudo iniciando una excepción, o bien, para async, se ha cancelado  
- Indicaciones sobre el valor del resultado  
- La cantidad aproximada de tiempo que tardó en ejecutar el comando. Tenga en cuenta que esto es la hora de enviar el comando para obtener el objeto de resultado. No incluye el tiempo para leer los resultados.  

Examinar la salida del ejemplo anterior, cada uno de los cuatro comandos registrados son:  

- La consulta resultante de la llamada al contexto. Blogs.First  
    - Tenga en cuenta que el método ToString de obtención de la instrucción SQL no hubieran funcionado para esta consulta desde "First" no proporciona un tipo IQueryable en el que se podría llamar ToString  
- La consulta resultante de la carga diferida del blog. Publicaciones  
    - Tenga en cuenta que ocurre con los detalles del parámetro para el valor de clave para que la carga diferida  
    - Solo las propiedades del parámetro que se establecen en valores no predeterminados se registran. Por ejemplo, la propiedad de tamaño se muestra solo si es distinto de cero.  
- Dos comandos resultantes de SaveChangesAsync; uno para la actualización cambiar un título de post, el otro para una instrucción insert agregar una nueva publicación  
    - Tenga en cuenta los detalles del parámetro para las propiedades de clave externa y título  
    - Tenga en cuenta que se ejecutan estos comandos de forma asincrónica  

## <a name="logging-to-different-places"></a>Registro en distintos lugares  

Como se mostró anteriormente el registro en la consola es muy fácil. También es fácil de iniciar sesión en memoria, archivo, etc. mediante diferentes tipos de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Si está familiarizado con LINQ to SQL, es posible que observe que en LINQ to SQL se establece la propiedad del registro en el TextWriter objeto real (por ejemplo, Console.Out) mientras en EF se establece la propiedad del registro a un método que acepta una cadena (por ejemplo Console.Write o Console.Out.Write). La razón de esto es desacoplar EF de TextWriter aceptando cualquier delegado que puede actuar como un receptor para las cadenas. Por ejemplo, imagine que ya tiene algún marco de trabajo de registro y define un método de registro de este modo:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Esto podría vincularse a la propiedad del registro de EF similar al siguiente:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Registro del resultado  

El registrador predeterminado registra el texto de comando (SQL), parámetros y la línea "Ejecutándose" con una marca de tiempo antes de que el comando se envía a la base de datos. Una línea "completada" que contiene el tiempo transcurrido es registrada siguiendo la ejecución del comando.  

Tenga en cuenta que para los comandos asincrónicos la línea "completada" no se registra hasta que la tarea asincrónica realmente se completa, se produce un error o se cancela.  

La línea "completada" contiene información diferente según el tipo de comando y si la ejecución fue correcta.  

### <a name="successful-execution"></a>Ejecución correcta  

Para los comandos que se completa correctamente el resultado es "completado en x ms con resultado:" seguida de alguna indicación de cuál fue el resultado. Para los comandos que devuelven el resultado de un lector de datos indicación es el tipo de [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) devuelto. Para los comandos que devuelven un valor entero como la actualización de comando mostrado anteriormente el resultado que se muestra es ese entero.  

### <a name="failed-execution"></a>Ejecución con errores  

Para los comandos que se producirá un error iniciando una excepción, el resultado contiene el mensaje de la excepción. Por ejemplo, mediante SqlQuery a consulta en una tabla que existe dará resultado en el registro como resultado algo parecido a esto:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Ejecución cancelada  

Para los comandos asincrónicos que se cancela la tarea el resultado podría ser un error con una excepción, ya que esto es lo que hace el proveedor ADO.NET subyacente a menudo cuando se intenta cancelar. Si esto no sucede y se cancela la tarea sin problemas, a continuación, el resultado tendrá un aspecto similar al siguiente:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Cambiar el contenido del registro y el formato  

En segundo plano el Database.Log propiedad hace que el uso de un objeto DatabaseLogFormatter. Este objeto enlaza eficazmente una implementación IDbCommandInterceptor (ver abajo) a un delegado que acepta cadenas y una clase DbContext. Esto significa que se llaman antes y después de la ejecución de comandos de EF de métodos en DatabaseLogFormatter. Estos métodos DatabaseLogFormatter recopilan y aplicar formato al resultado de registro y envían al delegado.  

### <a name="customizing-databaselogformatter"></a>Personalizar DatabaseLogFormatter  

Cambiar lo que se registra y cómo se da formato se puede lograr mediante la creación de una nueva clase que deriva de DatabaseLogFormatter e invalida los métodos según corresponda. Para invalidar métodos más comunes son:  

- LogCommand: invalidar este método para cambiar cómo se registran los comandos antes de ejecutarlos. De manera predeterminada LogCommand llama LogParameter para cada parámetro; puede hacer lo mismo en la invalidación o controlan los parámetros de forma diferente en su lugar.  
- LogResult: invalidar este método para cambiar cómo se registra el resultado de ejecutar un comando.  
- LogParameter: invalidar este método para cambiar el formato y contenido del registro de parámetro.  

Por ejemplo, suponga que deseamos iniciar sesión sólo una línea antes de cada comando se envía a la base de datos. Esto puede hacerse con dos invalidaciones:  

- Invalidar LogCommand para dar formato y escribir la línea de SQL  
- Invalidar LogResult para no hacer nada.  

El código sería algo parecido a esto:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Para iniciar una sesión de salida simplemente llamada el método de escritura que se enviará la salida al delegado escritura configurado.  

(Tenga en cuenta que este código hace eliminación simplista de saltos de línea solo como ejemplo. Probablemente no funcionará bien para ver SQL compleja.)  

### <a name="setting-the-databaselogformatter"></a>Establecer el DatabaseLogFormatter  

Una vez que se ha creado una nueva clase DatabaseLogFormatter debe registrarse con EF. Esto se realiza mediante la configuración basada en código. En pocas palabras, esto significa crear una nueva clase que derive de DbConfiguration en el mismo ensamblado que la clase DbContext y, a continuación, llamar a SetDatabaseLogFormatter en el constructor de esta nueva clase. Por ejemplo:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Uso de la nueva DatabaseLogFormatter  

Este nuevo DatabaseLogFormatter ahora se usará cada vez que se establece Database.Log. Por lo tanto, ejecutar el código de la parte 1 ahora generará el siguiente resultado:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Bloques de creación de intercepción  

Hasta ahora hemos examinado cómo usar DbContext.Database.Log para iniciar el SQL generado por EF. Pero este código es realmente una fachada relativamente delgada sobre algunos bloques de creación de bajo nivel para la intercepción más general.  

### <a name="interception-interfaces"></a>Interfaces de intercepción  

El código de intercepción se compila en torno al concepto de interfaces de intercepción. Estas interfaces se heredan de IDbInterceptor y definen los métodos que se llaman cuando EF realiza alguna acción. El objetivo es tener una interfaz por tipo de objeto que se intercepten. Por ejemplo, la interfaz IDbCommandInterceptor define métodos que se llaman antes de que EF realiza una llamada a ExecuteNonQuery, ExecuteReader, ExecuteScalar y métodos relacionados. Del mismo modo, la interfaz define métodos que se llama cuando se completa cada una de estas operaciones. La clase DatabaseLogFormatter que analizamos anteriormente implementa esta interfaz para registrar los comandos.  

### <a name="the-interception-context"></a>El contexto de intercepción  

Busca los métodos definidos en cualquiera de las interfaces del interceptor es evidente que todas las llamadas es un objeto de tipo DbInterceptionContext o algún tipo proporcionado derivadas de este como DbCommandInterceptionContext\<\>. Este objeto contiene información contextual sobre la acción que EF está teniendo. Por ejemplo, si la acción se ha puesto en nombre de un elemento DbContext, DbContext se incluye en el DbInterceptionContext. De forma similar, para los comandos que se ejecutan de forma asincrónica, se establece la marca IsAsync en DbCommandInterceptionContext.  

### <a name="result-handling"></a>Control de resultados  

El DbCommandInterceptionContext\< \> clase contiene propiedades que se llama a la excepción original, OriginalResult, excepción y resultado. Estas propiedades se establecen en null o cero para las llamadas a los métodos de intercepción que se llaman antes de ejecuta la operación, es decir, para el... Ejecutar métodos. Si la operación se ejecuta y se realiza correctamente, se establecen en el resultado de la operación de resultado y OriginalResult. Estos valores, a continuación, se pueden observar en los métodos de intercepción que se invocan después de que se ha ejecutado la operación, es decir, en los... Métodos ejecutados. Del mismo modo, si inicia la operación, se establecerá las propiedades de excepción y la excepción original.  

#### <a name="suppressing-execution"></a>Supresión de ejecución  

Si un interceptor establece la propiedad Result antes de que ha ejecutado el comando (en uno de los... Ejecutar métodos), a continuación, EF no intentará ejecutar el comando, pero usará únicamente el conjunto de resultados. En otras palabras, el interceptor puede suprimir la ejecución del comando, pero tienen EF continuar como si hubiera ejecutado el comando.  

Un ejemplo de cómo se puede utilizar es el comando de procesamiento por lotes que se ha realizado tradicionalmente con un proveedor de ajuste. El interceptor almacenaría el comando para ejecutarlas posteriormente como un lote, pero sería "fingir" a EF que el comando ejecutado como normal. Tenga en cuenta que necesita más para implementar el procesamiento por lotes, pero esto es un ejemplo de cómo se podría utilizar cambiar el resultado de intercepción.  

También se puede suprimir la ejecución estableciendo la propiedad de excepción en uno de los... Ejecutar métodos. Esto hace que EF continuar como si se hubiese producido un error de ejecución de la operación generando la excepción especificada. Esto es posible que, por supuesto, producir la aplicación se bloquee, pero también puede ser una excepción transitoria u otra excepción que se controla mediante EF. Por ejemplo, esto podría usarse en entornos de prueba para probar el comportamiento de una aplicación cuando se produce un error en la ejecución del comando.  

#### <a name="changing-the-result-after-execution"></a>Cambiar el resultado después de la ejecución  

Si un interceptor establece la propiedad Result después de que ha ejecutado el comando (en uno de los... Ejecutar métodos), a continuación, EF usará el resultado cambiado en lugar del resultado que devolvió realmente de la operación. De forma similar, si un interceptor establece la propiedad de excepción después de que ha ejecutado el comando, a continuación, EF producirá la excepción de conjunto como si la operación tenía produce la excepción.  

Un interceptor también puede establecer la propiedad Exception en null que indica que no se produce ninguna excepción. Esto puede ser útil si no se pudo ejecutar la operación, pero el interceptor de deseos de EF para continuar como si la operación se haya completado. Este proceso también implica establecer el resultado para que EF tiene algún valor de resultado para trabajar con a medida que continúa.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult y la excepción original  

Después de EF ha ejecutado una operación de establecer las propiedades de resultados y OriginalResult si la ejecución no se produjeron errores o las propiedades de excepción y la excepción original si el error de ejecución con una excepción.  

Las propiedades OriginalResult y la excepción original son de solo lectura y solo se establecen por EF después de ejecutar realmente una operación. No se puede establecer estas propiedades por los interceptores. Esto significa que cualquier interceptor puede distinguir entre una excepción o el resultado que se ha establecido algún otro interceptor en lugar de la excepción real o el resultado que se produjeron cuando la operación se ha ejecutado.  

### <a name="registering-interceptors"></a>Registro de interceptores  

Una vez que se ha creado una clase que implementa una o varias de las interfaces de intercepción se puede registrar con EF mediante la clase DbInterception. Por ejemplo:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Los interceptores también se pueden registrar en el nivel de dominio de aplicación mediante el mecanismo de configuración basado en código de DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Ejemplo: Inicio de sesión NLog  

Vamos a poner todo esto juntos en un ejemplo que el uso IDbCommandInterceptor y [NLog](http://nlog-project.org/) para:  

- Registrar una advertencia para cualquier comando que se ejecuta de forma que no sea asincrónica  
- Registrar un error para cualquier comando que se produce cuando se ejecuta  

Esta es la clase que realiza el registro, que se debe registrar como se indicó anteriormente:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Observe cómo este código usa el contexto de intercepción para detectar cuándo se ejecuta un comando que no es asincrónica y para detectar cuando se produjo un error al ejecutar un comando.  
