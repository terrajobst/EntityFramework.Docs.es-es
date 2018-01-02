---
title: Migraciones - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3e75947e65b8e7707292fec8d74b170aff191395
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
---
<a name="migrations"></a>Migraciones
==========
Las migraciones proporcionan una manera de aplicar cambios de esquema de forma incremental a la base de datos para mantenerla sincronizada con el modelo de EF Core al tiempo que se conservan los datos existentes en la base de datos.

<a name="creating-the-database"></a>Creación de la base de datos
---------------------
Una vez [definido el modelo inicial][1], es el momento de crear la base de datos. Para ello, agregue una migración inicial.
Instale [las herramientas de EF Core][2] y ejecute el comando adecuado.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Se agregan tres archivos al proyecto en el directorio **Migraciones**:

* **00000000000000_InitialCreate.cs**: archivo principal de migraciones. Contiene las operaciones necesarias para aplicar la migración (en `Up()`) y para revertirla (en `Down()`).
* **00000000000000_InitialCreate.Designer.cs**: archivo de metadatos de migraciones. Contiene información que usa EF.
* **MyContextModelSnapshot.cs**: instantánea del modelo actual. Se usa para determinar qué ha cambiado al agregar la siguiente migración.

La marca de tiempo del nombre del archivo ayuda a mantenerlos ordenados cronológicamente para que se pueda ver la progresión de cambios.

> [!TIP]
> Puede mover los archivos de Migraciones y cambiar su espacio de nombres. Se crean nuevas migraciones como elementos del mismo nivel de la última migración.

Luego aplique la migración a la base de datos para crear el esquema.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Adición de otra migración
------------------------
Después de realizar cambios en el modelo de EF Core, el esquema de la base de datos no está sincronizado. Para ponerlo al día, agregue otra migración. El nombre de la migración se puede usar como mensaje de confirmación en un sistema de control de versiones. Por ejemplo, si se han realizado cambios para guardar las opiniones de clientes sobre productos, podría elegir algo parecido a *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Una vez que se ha aplicado scaffolding a la migración, debe revisar su precisión y agregar cualquier operación adicional necesaria para aplicarla correctamente. Por ejemplo, la migración podría contener las siguientes operaciones:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Aunque estas operaciones hacen que el esquema de la base de datos sea compatible, no conservan los nombres de cliente existentes. Para que sea mejor, vuelva a escribirla como se indica a continuación.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Al agregar una nueva migración, se advierte si se ha aplicado scaffolding a una operación que pueda dar lugar a pérdida de datos (por ejemplo, borrado de una columna). Asegúrese de revisar cuidadosamente la precisión de estas migraciones.

Aplique la migración a la base de datos con el comando apropiado.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Eliminación de una migración
--------------------
A veces uno agrega una migración y se da cuenta de que debe realizar cambios adicionales en el modelo de EF Core antes de aplicarla.
Para quitar la última migración, use este comando.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Después de quitarla, puede realizar los cambios de modelo adicionales y volver a agregarla.

<a name="reverting-a-migration"></a>Reversión de una migración
---------------------
Si ya ha aplicado una migración (o varias migraciones) a la base de datos pero necesita revertirla, puede usar el mismo comando que para aplicar migraciones, aunque debe especificar el nombre de la migración que quiere revertir.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Migraciones vacías
----------------
A veces resulta útil agregar una migración sin realizar ningún cambio de modelo. En este caso, al agregar una nueva migración se crea una vacía. Puede personalizar esta migración para llevar a cabo operaciones que no estén directamente relacionadas con el modelo de EF Core.
Algunos aspectos que podría querer administrar de esta manera son:

* Búsqueda de texto completo
* Funciones
* Procedimientos almacenados
* Desencadenadores
* Vistas
* y otros.

<a name="generating-a-sql-script"></a>Generación de un script SQL
-----------------------
Al depurar las migraciones o implementarlas en una base de datos de producción, es útil generar un script SQL. El script luego se puede revisar y ajustar a las necesidades de una base de datos de producción. El script también puede usarse junto con una tecnología de implementación. El comando básico es el siguiente.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Hay varias opciones para este comando.

La migración **from** debe ser la última migración aplicada a la base de datos antes de ejecutar el script. Si no se han aplicado migraciones, especifique `0` (es el valor predeterminado).

La migración **to** debe ser la última migración que se va a aplicar a la base de datos después de ejecutar el script. El valor predeterminado es la última migración del proyecto.

Se puede generar un script **idempotent** de forma opcional. Este script solo aplica migraciones si aún no se han aplicado a la base de datos. Es útil si no sabe exactamente cuál ha sido la última migración aplicada a la base de datos o si va a implementar en varias bases de datos que pueden encontrarse en migraciones diferentes.

<a name="applying-migrations-at-runtime"></a>Aplicación de migraciones en tiempo de ejecución
------------------------------
Algunas aplicaciones pueden querer aplicar migraciones en tiempo de ejecución durante el inicio o la primera ejecución. Para ello, se usa el método `Migrate()`.

Precaución, ya que este método no es para todos. Aunque es excelente para las aplicaciones con una base de datos local, la mayoría de las aplicaciones necesitan estrategias de implementación más sólidas, como la generación de scripts SQL.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> No llame a `EnsureCreated()` antes de `Migrate()`. `EnsureCreated()` omite las migraciones para crear el esquema y provoca un error de `Migrate()`.

> [!NOTE]
> Este método se basa en el servicio `IMigrator`, que se puede usar para escenarios más avanzados. Use `DbContext.GetService<IMigrator>()` para acceder a él.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
