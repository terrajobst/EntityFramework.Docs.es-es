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
<a name="custom-migrations-operations"></a>Operaciones de migración personalizadas
============================
La API MigrationBuilder le permite realizar muchos tipos diferentes de operaciones durante una migración, pero es lejos de ser exhaustiva. Sin embargo, la API también es extensible que le permite definir sus propias operaciones. Hay dos maneras de extender la API: con la `Sql()` método, o mediante la definición personalizada `MigrationOperation` objetos.

Para ilustrar, echemos un vistazo a la implementación de una operación que crea un usuario de base de datos mediante cada enfoque. En nuestro migraciones, queremos poder escribir el código siguiente:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Uso de MigrationBuilder.Sql()
----------------------------
La manera más fácil de implementar una operación personalizada es definir un método de extensión llama `MigrationBuilder.Sql()`.
Este es un ejemplo que genera la instrucción Transact-SQL adecuado.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Si necesitan las migraciones ofrecer compatibilidad con varios proveedores de base de datos, puede usar el `MigrationBuilder.ActiveProvider` propiedad. Este es un ejemplo que admite Microsoft SQL Server y PostgreSQL.

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

Este enfoque solo funciona si sabe que todos los proveedores que se aplicará la operación personalizada.

<a name="using-a-migrationoperation"></a>Uso de un MigrationOperation
---------------------------
Para desacoplar la operación personalizada desde el código SQL, puede definir sus propios `MigrationOperation` para representarlo. La operación, a continuación, se pasa al proveedor para que pueda determinar el lenguaje SQL adecuado para generar.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Con este enfoque, el método de extensión basta con que agregue una de estas operaciones se `MigrationBuilder.Operations`.

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

Este enfoque requiere que cada proveedor para saber cómo generar código SQL para esta operación en sus `IMigrationsSqlGenerator` service. Este es un ejemplo Reemplazar generador de SQL Server para controlar la operación de nuevo.

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

Reemplace el servicio de generador predeterminado migraciones sql con el actualizado.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
