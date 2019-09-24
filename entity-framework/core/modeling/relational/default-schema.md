---
title: EF Core de esquema predeterminados
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197130"
---
# <a name="default-schema"></a>Esquema predeterminado

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

El esquema predeterminado es el esquema de la base de datos en el que se crearán los objetos si no se configura explícitamente un esquema para ese objeto.

## <a name="conventions"></a>Convenciones

Por Convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado. Por ejemplo, Microsoft SQL Server usará el esquema `dbo` y SQLite no usará un esquema (dado que los esquemas no se admiten en SQLite).

## <a name="data-annotations"></a>Anotaciones de datos

No se puede establecer el esquema predeterminado con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar un esquema predeterminado.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
