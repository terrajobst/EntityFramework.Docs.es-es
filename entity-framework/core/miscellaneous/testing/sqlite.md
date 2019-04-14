---
title: Pruebas con SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562538"
---
# <a name="testing-with-sqlite"></a>Pruebas con SQLite

SQLite tiene un modo en memoria que le permite usar SQLite para escribir pruebas de una base de datos relacional, sin la sobrecarga de operaciones de base de datos real.

> [!TIP]  
> Puede ver en este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) en GitHub

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Considere el siguiente servicio que permite que el código de aplicación realizar algunas operaciones relacionadas con blogs. Lo utiliza internamente un `DbContext` que se conecta a una base de datos de SQL Server. Sería útil cambiar este contexto para conectarse a una base de datos de SQLite en memoria, por lo que podemos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer una gran cantidad de trabajo para crear una prueba doble del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de base de datos

En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory. Si va a configurar un proveedor de base de datos invalidando `OnConfiguring` en su contexto, a continuación, deberá agregar algún código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.

> [!TIP]  
> Si usa ASP.NET Core, a continuación, no deberían necesitar este código ya que esté configurado el proveedor de base de datos fuera del contexto (en Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Agregue un constructor para las pruebas

Es la manera más sencilla de habilitar las pruebas con otra base de datos modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica el contexto de todos sus valores, por ejemplo, para conectarse a la base de datos. Este es el mismo objeto que se compila mediante la ejecución del método OnConfiguring en su contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave para las pruebas con este proveedor es la capacidad de decirle el contexto para usar SQLite y controlar el ámbito de la base de datos en memoria. El ámbito de la base de datos se controla mediante la apertura y cierre la conexión. La base de datos se limita a la duración de la conexión está abierta. Normalmente desea limpiar una base de datos de cada método de prueba.

>[!TIP]
> Para usar `SqliteConnection()` y `.UseSqlite()` método de extensión, el paquete NuGet de referencia [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
