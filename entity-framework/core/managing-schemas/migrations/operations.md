---
title: 'Operaciones de migración personalizadas: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812046"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="b4bf8-102">Operaciones de migración personalizadas</span><span class="sxs-lookup"><span data-stu-id="b4bf8-102">Custom Migrations Operations</span></span>

<span data-ttu-id="b4bf8-103">La API de MigrationBuilder permite realizar muchos tipos diferentes de operaciones durante una migración, pero está lejos de ser exhaustiva.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="b4bf8-104">Sin embargo, la API también es extensible, lo que le permite definir sus propias operaciones.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="b4bf8-105">Hay dos maneras de extender la API: mediante el método `Sql()` o mediante la definición de objetos de `MigrationOperation` personalizados.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="b4bf8-106">Para ilustrar, echemos un vistazo a la implementación de una operación que crea un usuario de base de datos mediante cada enfoque.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="b4bf8-107">En nuestras migraciones, queremos habilitar la escritura del código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b4bf8-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="b4bf8-108">Usar MigrationBuilder. SQL ()</span><span class="sxs-lookup"><span data-stu-id="b4bf8-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="b4bf8-109">La forma más fácil de implementar una operación personalizada es definir un método de extensión que llame a `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="b4bf8-110">Este es un ejemplo que genera el correspondiente Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="b4bf8-111">Si las migraciones necesitan admitir varios proveedores de bases de datos, puede usar la propiedad `MigrationBuilder.ActiveProvider`.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="b4bf8-112">Este es un ejemplo que admite tanto Microsoft SQL Server como PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

<span data-ttu-id="b4bf8-113">Este enfoque solo funciona si conoce todos los proveedores en los que se va a aplicar la operación personalizada.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="b4bf8-114">Uso de un MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="b4bf8-114">Using a MigrationOperation</span></span>

<span data-ttu-id="b4bf8-115">Para desacoplar la operación personalizada de SQL, puede definir su propia `MigrationOperation` para representarla.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="b4bf8-116">A continuación, la operación se pasa al proveedor para que pueda determinar el SQL adecuado que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="b4bf8-117">Con este enfoque, el método de extensión solo tiene que agregar una de estas operaciones a `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="b4bf8-118">Este enfoque requiere que cada proveedor sepa cómo generar SQL para esta operación en su servicio `IMigrationsSqlGenerator`.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="b4bf8-119">Este es un ejemplo invalidando el generador del SQL Server para administrar la nueva operación.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="b4bf8-120">Reemplace el servicio de generador de SQL de migraciones predeterminado por el actualizado.</span><span class="sxs-lookup"><span data-stu-id="b4bf8-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
