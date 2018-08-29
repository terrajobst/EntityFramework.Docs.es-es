---
title: Escribir a un proveedor de base de datos - EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993671"
---
# <a name="writing-a-database-provider"></a>Escribir un proveedor de base de datos

Para obtener información sobre cómo escribir un proveedor de base de datos de Entity Framework Core, vea [por lo que desee escribir un proveedor de EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) por [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Estas entradas no se han actualizado a partir de EF Core 1.1 y ha habido cambios significativos desde ese momento [problema 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) está realizando un seguimiento de las actualizaciones a esta documentación.

La base de código de EF Core es código abierto y contiene varios proveedores de base de datos que pueden usarse como referencia. Puede encontrar el código fuente en https://github.com/aspnet/EntityFrameworkCore. También puede ser útil examinar el código para proveedores de terceros usados, como [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [MySQL de Pomelo](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), y [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). En concreto, estos proyectos están configuradas para ampliar desde y ejecutar pruebas funcionales que se publican en NuGet. Se recomienda este tipo de instalación.

## <a name="keeping-up-to-date-with-provider-changes"></a>Mantener actualizado con los cambios de proveedor

Comenzando con el trabajo después de la versión 2.1, hemos creado un [registro de cambios](provider-log.md) que necesite los cambios correspondientes en el código del proveedor. Esto está pensado para ayudar a cuando se actualiza un proveedor existente para trabajar con una nueva versión de EF Core.

Antes de 2.1, hemos usado el [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) y [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etiquetas en nuestros problemas de GitHub y solicitudes de incorporación de cambios para un propósito similar. Vamos a continiue para utilizar estas etiquetas problemas para proporcionar una indicación de qué elementos de trabajo en una versión determinada también pueden requerir que se realice en los proveedores de trabajo. Un `providers-beware` etiqueta normalmente significa que la implementación de un elemento de trabajo puede interrumpir los proveedores, mientras que un `providers-fyi` etiqueta normalmente significa que no se interrumpirá proveedores, pero código que necesite cambiar en cualquier caso, por ejemplo, para habilitar la funcionalidad nueva.

## <a name="suggested-naming-of-third-party-providers"></a>Sugiere la denominación de los proveedores de terceros

Se recomienda usar la siguiente nomenclatura para los paquetes de NuGet. Esto es coherente con los nombres de paquetes entregados por el equipo de EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Por ejemplo:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
