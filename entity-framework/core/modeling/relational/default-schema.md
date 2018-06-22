---
title: Esquema predeterminado - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052755"
---
# <a name="default-schema"></a>Esquema predeterminado

> [!NOTE]  
> La configuración de esta sección es aplicable a bases de datos relacionales en general. Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).

El esquema predeterminado es el esquema de base de datos que si un esquema no está configurado explícitamente para ese objeto, se creará en objetos.

## <a name="conventions"></a>Convenciones

Por convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado. Por ejemplo, Microsoft SQL Server usará la `dbo` esquema y SQLite no utilizarán un esquema (ya que no se admiten esquemas en SQLite).

## <a name="data-annotations"></a>Anotaciones de datos

No puede establecer el esquema predeterminado mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar un esquema predeterminado.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
