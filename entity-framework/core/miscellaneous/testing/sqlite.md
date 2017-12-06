---
title: Probar con SQLite - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>Pruebas de SQLite

SQLite tiene un modo en memoria que permite utilizar código para escribir pruebas con una base de datos relacional, sin la sobrecarga de operaciones de base de datos real.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) en GitHub

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Tenga en cuenta el siguiente servicio que permite que el código de aplicación para realizar algunas operaciones relacionadas con los blogs. Usa internamente un `DbContext` que se conecta a una base de datos de SQL Server. Podría ser útil intercambiar este contexto para conectarse a una base de datos de SQLite en memoria, por lo que podemos escribir pruebas eficaz para este servicio sin tener que modificar el código, o hacer una gran cantidad de trabajo que se va a crear una prueba de doble del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de base de datos

En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory. Si va a configurar un proveedor de base de datos mediante la invalidación `OnConfiguring` en el contexto, a continuación, debe agregar código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.

> [!TIP]  
> Si utiliza ASP.NET Core, a continuación, no deberían necesitar este código desde el proveedor de base de datos se configuró fuera del contexto (en Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Agregue un constructor para pruebas

La manera más sencilla de habilitar las pruebas en otra base de datos consiste en modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`indica el contexto de todos sus valores, como qué base de datos al que conectarse. Este es el mismo objeto que se crea mediante la ejecución del método OnConfiguring en el contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave que se va a realizar las pruebas con este proveedor es la capacidad para indicar el contexto para utilizar código y controlar el ámbito de la base de datos en memoria. El ámbito de la base de datos se controla mediante la apertura y cierre la conexión. La base de datos se limita a la duración de la conexión está abierta. Normalmente, desea una base de datos limpia para cada método de prueba.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
