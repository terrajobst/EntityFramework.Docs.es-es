---
title: Esquema predeterminado - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995371"
---
# <a name="default-schema"></a>Esquema predeterminado

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

El esquema predeterminado es el esquema de base de datos que los objetos se crearán en si un esquema no está configurado explícitamente para ese objeto.

## <a name="conventions"></a>Convenciones

Por convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado. Por ejemplo, Microsoft SQL Server usará el `dbo` esquema y SQLite no usará un esquema (ya que no se admiten esquemas en SQLite).

## <a name="data-annotations"></a>Anotaciones de datos

No se puede establecer el esquema predeterminado usando anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para especificar un esquema predeterminado.

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
