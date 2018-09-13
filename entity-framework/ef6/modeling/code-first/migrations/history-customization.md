---
title: Personalización de la tabla de historial de migraciones - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: 6644bf2b0ac703a9f3a779b17b31d79d40cc5b69
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489217"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="b74be-102">Personalización de la tabla de historial de migraciones</span><span class="sxs-lookup"><span data-stu-id="b74be-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="b74be-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b74be-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b74be-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="b74be-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="b74be-105">En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos.</span><span class="sxs-lookup"><span data-stu-id="b74be-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="b74be-106">Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b74be-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="b74be-107">¿Qué es la tabla de historial de migraciones?</span><span class="sxs-lookup"><span data-stu-id="b74be-107">What is Migrations History Table?</span></span>

<span data-ttu-id="b74be-108">Tabla de historial de migraciones es una tabla usada por migraciones de Code First para almacenar los detalles acerca de las migraciones que se aplica a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b74be-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="b74be-109">De forma predeterminada, el nombre de la tabla en la base de datos es \_ \_MigrationHistory y se crea cuando se aplica la primera migración, haga lo la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b74be-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="b74be-110">En Entity Framework 5 esta tabla era una tabla del sistema si la aplicación utiliza la base de datos de Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="b74be-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="b74be-111">Esto ha cambiado pero en Entity Framework 6 y la tabla de historial de migraciones ya no se marca una tabla del sistema.</span><span class="sxs-lookup"><span data-stu-id="b74be-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="b74be-112">¿Por qué personalizar la tabla de historial de migraciones?</span><span class="sxs-lookup"><span data-stu-id="b74be-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="b74be-113">Tabla de historial de migraciones se supone que debe usarse únicamente por migraciones de Code First y cambiarla manualmente puede interrumpir las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b74be-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="b74be-114">Sin embargo a veces la configuración predeterminada no es adecuada y la tabla debe personalizarse, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b74be-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="b74be-115">Deberá cambiar los nombres o las facetas de las columnas para habilitar un 3<sup>rd</sup> proveedor migraciones</span><span class="sxs-lookup"><span data-stu-id="b74be-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="b74be-116">Para cambiar el nombre de la tabla</span><span class="sxs-lookup"><span data-stu-id="b74be-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="b74be-117">Debe usar un esquema no predeterminado para el \_ \_tabla MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="b74be-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="b74be-118">Necesita almacenar datos adicionales para una versión concreta del contexto y, por tanto, deberá agregar una columna adicional a la tabla</span><span class="sxs-lookup"><span data-stu-id="b74be-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="b74be-119">Palabras de la medida de precaución</span><span class="sxs-lookup"><span data-stu-id="b74be-119">Words of precaution</span></span>

<span data-ttu-id="b74be-120">Cambiar la tabla de historial de migración es eficaz, pero debe tener cuidado de no abusar.</span><span class="sxs-lookup"><span data-stu-id="b74be-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="b74be-121">En tiempo de ejecución EF actualmente no comprueba si la tabla de historial de migraciones personalizado es compatible con el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b74be-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="b74be-122">Si la aplicación no es posible interrumpir en tiempo de ejecución o se comportan de manera impredecible.</span><span class="sxs-lookup"><span data-stu-id="b74be-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="b74be-123">Esto es incluso más importante si usa varios contextos por base de datos en cuyo caso varios contextos pueden usar la misma tabla de historial de migración para almacenar información acerca de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b74be-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="b74be-124">¿Cómo personalizar la tabla de historial de migraciones?</span><span class="sxs-lookup"><span data-stu-id="b74be-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="b74be-125">Antes de empezar necesita saber que puede personalizar la tabla de historial de migraciones antes de aplicar la primera migración.</span><span class="sxs-lookup"><span data-stu-id="b74be-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="b74be-126">Ahora, en el código.</span><span class="sxs-lookup"><span data-stu-id="b74be-126">Now, to the code.</span></span>

<span data-ttu-id="b74be-127">En primer lugar, deberá crear una clase derivada de la clase System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="b74be-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="b74be-128">La clase HistoryContext se deriva de la clase DbContext para que la configuración de la tabla de historial de migraciones es muy similar a la configuración de los modelos EF con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="b74be-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="b74be-129">Solo necesita invalidar el método OnModelCreating y usar la API fluida para configurar la tabla.</span><span class="sxs-lookup"><span data-stu-id="b74be-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="b74be-130">Normalmente, al configurar los modelos EF no es necesario llamar a base. OnModelCreating() desde el método OnModelCreating reemplazado, ya que el DbContext.OnModelCreating() tiene un cuerpo vacío.</span><span class="sxs-lookup"><span data-stu-id="b74be-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="b74be-131">No es el caso cuando la configuración de la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="b74be-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="b74be-132">En este caso la primera cosa que hacer en la invalidación OnModelCreating() es realmente llame a la base. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="b74be-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="b74be-133">Esto configurará la tabla de historial de migraciones en el modo predeterminado que, a continuación, ajustar en el método de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="b74be-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="b74be-134">Supongamos que desea cambiar el nombre de la tabla de historial de migraciones y colocarlo en un esquema personalizado denominado "admin".</span><span class="sxs-lookup"><span data-stu-id="b74be-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="b74be-135">Además el DBA desea cambiar el nombre de la columna MigrationId migración\_identificador.</span><span class="sxs-lookup"><span data-stu-id="b74be-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="b74be-136">Se puede conseguir mediante la creación de la siguiente clase que deriva de HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="b74be-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="b74be-137">Cuando esté listo el HistoryContext personalizado deberá asegurarse de que EF lo sepa registrarlo mediante [configuración basada en código](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="b74be-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="b74be-138">Eso es prácticamente todo.</span><span class="sxs-lookup"><span data-stu-id="b74be-138">That’s pretty much it.</span></span> <span data-ttu-id="b74be-139">Ahora puede ir a la consola de administrador de paquetes, Enable-Migrations, Add-Migration y, finalmente, Update-Database.</span><span class="sxs-lookup"><span data-stu-id="b74be-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="b74be-140">Esto debería producir agregar a la base de datos de una tabla de historial de migraciones configurada según los detalles especificados en la clase HistoryContext derivada.</span><span class="sxs-lookup"><span data-stu-id="b74be-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Base de datos](~/ef6/media/database.png)
