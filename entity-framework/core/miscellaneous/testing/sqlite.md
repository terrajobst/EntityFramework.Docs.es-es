---
title: Pruebas con SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414636"
---
# <a name="testing-with-sqlite"></a>Pruebas con SQLite

SQLite tiene un modo en memoria que le permite usar SQLite para escribir pruebas en una base de datos relacional, sin la sobrecarga de las operaciones de base de datos reales.

> [!TIP]  
> Puede ver el [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en github

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Considere el siguiente servicio que permite al código de la aplicación realizar algunas operaciones relacionadas con blogs. Utiliza internamente un `DbContext` que se conecta a una base de datos SQL Server. Sería útil intercambiar este contexto para conectarse a una base de datos de SQLite en memoria, de modo que podamos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer mucho trabajo para crear un doble de prueba del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de bases de datos

En las pruebas, va a configurar externamente el contexto para usar el proveedor de inmemory. Si está configurando un proveedor de base de datos invalidando `OnConfiguring` en el contexto, deberá agregar código condicional para asegurarse de que solo se configura el proveedor de base de datos si aún no se ha configurado ninguno.

> [!TIP]  
> Si usa ASP.NET Core, no necesitará este código, ya que el proveedor de la base de datos se configura fuera del contexto (en Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Agregar un constructor para las pruebas

La manera más sencilla de habilitar las pruebas en una base de datos diferente es modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica al contexto toda su configuración, como la base de datos a la que se va a conectar. Este es el mismo objeto que se genera al ejecutar el método de configuración en el contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave para realizar pruebas con este proveedor es la posibilidad de indicar al contexto que use SQLite y controlar el ámbito de la base de datos en memoria. El ámbito de la base de datos se controla mediante la apertura y el cierre de la conexión. La base de datos está en el ámbito de la duración de la conexión abierta. Normalmente, desea una base de datos limpia para cada método de prueba.

>[!TIP]
> Para usar `SqliteConnection()` y el método de extensión `.UseSqlite()`, haga referencia al paquete NuGet [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
