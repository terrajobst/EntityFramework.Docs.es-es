---
title: Operaciones de migración personalizadas - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 510d585534b4809179c905ee5b77cab4209a2b8f
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614290"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="973f0-102">Operaciones de migración personalizadas</span><span class="sxs-lookup"><span data-stu-id="973f0-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="973f0-103">La API MigrationBuilder le permite realizar muchos tipos diferentes de operaciones durante una migración, pero es lejos de ser exhaustiva.</span><span class="sxs-lookup"><span data-stu-id="973f0-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="973f0-104">Sin embargo, la API también es extensible que le permite definir sus propias operaciones.</span><span class="sxs-lookup"><span data-stu-id="973f0-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="973f0-105">Hay dos maneras de extender la API: con la `Sql()` método, o mediante la definición personalizada `MigrationOperation` objetos.</span><span class="sxs-lookup"><span data-stu-id="973f0-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="973f0-106">Para ilustrar, echemos un vistazo a la implementación de una operación que crea un usuario de base de datos mediante cada enfoque.</span><span class="sxs-lookup"><span data-stu-id="973f0-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="973f0-107">En nuestro migraciones, queremos poder escribir el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="973f0-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="973f0-108">Uso de MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="973f0-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="973f0-109">La manera más fácil de implementar una operación personalizada es definir un método de extensión llama `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="973f0-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="973f0-110">Este es un ejemplo que genera la instrucción Transact-SQL adecuado.</span><span class="sxs-lookup"><span data-stu-id="973f0-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="973f0-111">Si necesitan las migraciones ofrecer compatibilidad con varios proveedores de base de datos, puede usar el `MigrationBuilder.ActiveProvider` propiedad.</span><span class="sxs-lookup"><span data-stu-id="973f0-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="973f0-112">Este es un ejemplo que admite Microsoft SQL Server y PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="973f0-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="973f0-113">Este enfoque solo funciona si sabe que todos los proveedores que se aplicará la operación personalizada.</span><span class="sxs-lookup"><span data-stu-id="973f0-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="973f0-114">Uso de un MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="973f0-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="973f0-115">Para desacoplar la operación personalizada desde el código SQL, puede definir sus propios `MigrationOperation` para representarlo.</span><span class="sxs-lookup"><span data-stu-id="973f0-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="973f0-116">La operación, a continuación, se pasa al proveedor para que pueda determinar el lenguaje SQL adecuado para generar.</span><span class="sxs-lookup"><span data-stu-id="973f0-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="973f0-117">Con este enfoque, el método de extensión basta con que agregue una de estas operaciones se `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="973f0-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="973f0-118">Este enfoque requiere que cada proveedor para saber cómo generar código SQL para esta operación en sus `IMigrationsSqlGenerator` service.</span><span class="sxs-lookup"><span data-stu-id="973f0-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="973f0-119">Este es un ejemplo Reemplazar generador de SQL Server para controlar la operación de nuevo.</span><span class="sxs-lookup"><span data-stu-id="973f0-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="973f0-120">Reemplace el servicio de generador predeterminado migraciones sql con el actualizado.</span><span class="sxs-lookup"><span data-stu-id="973f0-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
