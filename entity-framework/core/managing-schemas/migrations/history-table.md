---
title: 'Tabla de historial de migraciones personalizadas: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655716"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="beade-102">Tabla de historial de migraciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="beade-102">Custom Migrations History Table</span></span>

<span data-ttu-id="beade-103">De forma predeterminada, EF Core realiza un seguimiento de las migraciones que se han aplicado a la base de datos mediante su grabación en una tabla denominada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="beade-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="beade-104">Por varias razones, puede que desee personalizar esta tabla para satisfacer mejor sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="beade-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="beade-105">Si personaliza la tabla de historial de migraciones *después* de aplicar las migraciones, es responsable de actualizar la tabla existente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="beade-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="beade-106">Esquema y nombre de tabla</span><span class="sxs-lookup"><span data-stu-id="beade-106">Schema and table name</span></span>

<span data-ttu-id="beade-107">Puede cambiar el esquema y el nombre de la tabla mediante el método `MigrationsHistoryTable()` en `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="beade-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="beade-108">Este es un ejemplo del uso del proveedor de EF Core de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="beade-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="beade-109">Otros cambios</span><span class="sxs-lookup"><span data-stu-id="beade-109">Other changes</span></span>

<span data-ttu-id="beade-110">Para configurar aspectos adicionales de la tabla, invalide y reemplace el servicio de `IHistoryRepository` específico del proveedor.</span><span class="sxs-lookup"><span data-stu-id="beade-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="beade-111">Este es un ejemplo de cómo cambiar el nombre de la columna MigrationId a *ID* en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="beade-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="beade-112">`SqlServerHistoryRepository` está dentro de un espacio de nombres interno y puede cambiar en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="beade-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
