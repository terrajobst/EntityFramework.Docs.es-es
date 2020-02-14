---
title: Novedades en EF Core 5.0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052021"
---
# <a name="whats-new-in-ef-core-50"></a>Novedades en EF Core 5.0

EF Core 5.0 está actualmente en desarrollo.
Esta página contendrá información general sobre los cambios interesantes introducidos en cada versión preliminar.
La primera versión preliminar de EF Core 5.0 se espera provisionalmente para el primer trimestre de 2020.

Esta página no duplica el [plan para EF Core 5.0](plan.md).
En el plan se describen los temas generales relativos a EF Core 5.0, incluido todo lo que estamos planeando incluir antes de publicar la versión final.

A medida que se publique el contenido, se agregarán vínculos que redirigirán de esta página a la documentación oficial.

## <a name="preview-1-not-yet-shipped"></a>Versión preliminar 1 (no publicada todavía)

### <a name="simple-logging"></a>Registro sencillo

Esta característica agrega funcionalidad similar a `Database.Log` en EF6.
Es decir, proporciona una manera sencilla de obtener registros de EF Core sin necesidad de configurar ningún tipo de plataforma de registro externa.

La documentación preliminar se incluye en el [estado semanal de EF del 5 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

En el problema [n.º 2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085) se realiza el seguimiento de la documentación adicional.

### <a name="simple-way-to-get-generated-sql"></a>Forma sencilla de generar contenido SQL

EF Core 5.0 presenta el método de extensión `ToQueryString` que devolverá el contenido SQL que EF Core generará al ejecutar una consulta LINQ.

La documentación preliminar se incluye en el [estado semanal de EF del 9 de enero de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

En el problema [n.º 1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331) se realiza el seguimiento de la documentación adicional.

### <a name="enhanced-debug-views"></a>Vistas de depuración mejoradas

Las vistas de depuración son una forma fácil de consultar los aspectos internos de EF Core al depurar problemas.
Hace algún tiempo ya se implementó una vista de depuración para el modelo.
En el caso de EF Core 5.0, hemos facilitado la lectura de la vista de modelo y hemos agregado una nueva vista de depuración para las entidades de las que se ha realizado un seguimiento en el administrador de estado.

La documentación preliminar se incluye en el [estado semanal de EF del 12 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

En el problema [n.º 2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086) se realiza el seguimiento de la documentación adicional.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Posibilidad de cambiar la conexión o la cadena de conexión en una instancia inicializada de DbContext

Ahora es más fácil crear una instancia de DbContext sin ninguna conexión o cadena de conexión.
Además, la conexión o la cadena de conexión ahora también se pueden mutar en la instancia de contexto.
Esto permite que la misma instancia de contexto se conecte dinámicamente a diferentes bases de datos.

En el problema [n.º 2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075) se realiza el seguimiento de la documentación.

### <a name="change-tracking-proxies"></a>Proxies de seguimiento de cambios

Ahora EF Core puede generar proxies del entorno de ejecución que implementen automáticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) y [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
A continuación, los cambios de valor en las propiedades de las entidades se notifican directamente a EF Core, lo cual evita la necesidad de buscar los cambios.
Sin embargo, los proxies vienen con su propio conjunto de limitaciones, por lo que no son para todo el mundo.

En el problema [n.º 2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076) se realiza el seguimiento de la documentación.

### <a name="improved-handling-of-database-null-semantics"></a>Control mejorado de la semántica de valores NULL de base de datos

Normalmente, las bases de datos relacionales tratan NULL como valores desconocidos y, por lo tanto, no son iguales a otros valores NULL.
C#, por otro lado, trata los valores NULL como valores definidos y los compara igual que con cualquier otro valor NULL.
De forma predeterminada, EF Core traduce las consultas para que usen la semántica de valores NULL de C#.
EF Core 5.0 mejora en gran medida la eficacia de dichas traducciones.

En el problema [n.º 1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612) se realiza el seguimiento de la documentación.

### <a name="indexer-properties"></a>Propiedades del indizador

EF Core 5.0 admite la asignación de propiedades de indizador de C#.
Esto permite a las entidades actuar como bolsas de propiedades en las que las columnas se asignan a las propiedades con nombre en la bolsa.

En el problema [n.º 2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018) se realiza el seguimiento de la documentación.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generación de restricciones CHECK para las asignaciones de enumeración

Las migraciones de EF Core 5.0 ahora pueden generar restricciones CHECK para las asignaciones de propiedades de enumeración.
Por ejemplo:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

En el problema [n.º 2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082) se realiza el seguimiento de la documentación.

### <a name="query-translations-for-more-datetime-constructs"></a>Traducciones de consultas para más construcciones DateTime

Ahora las consultas que contienen la nueva construcción DataTime se traducen.
Además, la función de SQL Server DateDiffWeek ahora se puede asignar.

En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.

### <a name="query-translations-for-more-byte-array-constructs"></a>Traducciones de consultas para más construcciones de matriz de bytes

Las consultas que usan Contains, Length, SequenceEqual, etc. en las propiedades byte[] ahora se traducen a SQL.

La documentación preliminar se incluye en el [estado semanal de EF del 5 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) se realiza el seguimiento de la documentación adicional.

### <a name="query-translation-for-reverse"></a>Traducción de consultas para Reverse

Las consultas que usan `Reverse` ahora se traducen.
Por ejemplo:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.

### <a name="query-translation-for-bitwise-operators"></a>Traducción de consultas para operadores bit a bit

Las consultas que usan operadores bit a bit ahora se traducen en más casos, por ejemplo:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.

### <a name="query-translation-for-strings-on-cosmos"></a>Traducción de consultas para cadenas en Cosmos

Al emplear el proveedor Azure Cosmos DB, las consultas que usan los métodos de cadena Contains, StartsWith y EndsWith ahora se traducen.

En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.
