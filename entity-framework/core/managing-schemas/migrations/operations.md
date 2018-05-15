---
title: 'Operaciones de migraciones personalizado: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a>Operaciones de migraciones personalizado
============================
La API de MigrationBuilder le permite realizar muchos tipos diferentes de operaciones durante una migración, pero es muy alejado del exhaustiva. Sin embargo, la API también es extensible, lo que le permite definir sus propias operaciones. Hay dos maneras de extender la API: mediante la `Sql()` (método), o mediante la definición personalizada `MigrationOperation` objetos.

Para ilustrar, echemos un vistazo a implementar una operación que crea un usuario de base de datos con cada enfoque. En nuestro migraciones, queremos poder escribir el código siguiente:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Usar MigrationBuilder.Sql()
----------------------------
La manera más fácil de implementar una operación personalizada consiste en definir un método de extensión que llama a `MigrationBuilder.Sql()`.
Este es un ejemplo que genera el código Transact-SQL adecuado.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Si necesitan las migraciones admitir varios proveedores de base de datos, puede usar el `MigrationBuilder.ActiveProvider` propiedad. Este es un ejemplo que admite Microsoft SQL Server y PostgreSQL.

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
}
```

Este método solo funciona si sabe que todos los proveedores de dónde se aplicará la operación personalizada.

<a name="using-a-migrationoperation"></a>Uso de un MigrationOperation
---------------------------
Para desacoplar la operación personalizada de la instrucción SQL, puede definir sus propios `MigrationOperation` para representarlo. La operación, a continuación, se pasa al proveedor para que pueda determinar el adecuados de SQL para generar.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Con este enfoque, el método de extensión solo debe agregar una de estas operaciones para `MigrationBuilder.Operations`.

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

Este enfoque requiere que cada proveedor para saber cómo generar código SQL para esta operación en sus `IMigrationsSqlGenerator` servicio. Este es un ejemplo Reemplazar generador de SQL Server para controlar la operación de nuevo.

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

Reemplazar el servicio de generador de sql de migraciones de forma predeterminada por el actualizado.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
