---
title: Probar con InMemory - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995592"
---
# <a name="testing-with-inmemory"></a>Probar con InMemory

El proveedor de InMemory es útil cuando desea probar los componentes con algo que se aproxima al conectarse a la base de datos real, sin la sobrecarga de operaciones de base de datos real.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory no es una base de datos relacional

Proveedores de base de datos principales EF no tiene que ser bases de datos relacionales. InMemory está diseñado para ser una base de datos de uso general para realizar pruebas y no está diseñado para imitar una base de datos relacional.

Algunos ejemplos de este comportamiento son:
* InMemory le permitirá guardar los datos que infringen las restricciones de integridad referencial en una base de datos relacional.

* Si usa DefaultValueSql(string) para una propiedad en el modelo, esto es una API de base de datos relacional y no tiene ningún efecto cuando se ejecuta en InMemory.

> [!TIP]  
> Para fines de prueba no se importan estas diferencias. Sin embargo, si desea probar con algo que se comporta más como una base de datos relacional es true, a continuación, considere el uso de [modo in-memory de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Tenga en cuenta el siguiente servicio que permite que el código de aplicación para realizar algunas operaciones relacionadas con los blogs. Usa internamente un `DbContext` que se conecta a una base de datos de SQL Server. Podría ser útil intercambiar este contexto para conectarse a una base de datos InMemory, por lo que podemos escribir pruebas eficaz para este servicio sin tener que modificar el código, o hacer una gran cantidad de trabajo que se va a crear una prueba de doble del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de base de datos

En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory. Si va a configurar un proveedor de base de datos mediante la invalidación `OnConfiguring` en el contexto, a continuación, debe agregar código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si utiliza ASP.NET Core, a continuación, no deberían necesitar este código desde el proveedor de base de datos ya se configuró fuera del contexto (en Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Agregue un constructor para pruebas

La manera más sencilla de habilitar las pruebas en otra base de datos consiste en modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`indica el contexto de todos sus valores, como qué base de datos al que conectarse. Este es el mismo objeto que se crea mediante la ejecución del método OnConfiguring en el contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave que se va a realizar las pruebas con este proveedor es la capacidad para indicar el contexto para usar el proveedor InMemory y controlar el ámbito de la base de datos en memoria. Normalmente, desea una base de datos limpia para cada método de prueba.

Este es un ejemplo de una clase de prueba que usa la base de datos InMemory. Cada método de prueba especifica un nombre único de la base de datos, lo que significa que cada método tiene su propia base de datos InMemory.

>[!TIP]
> Para usar el `.UseInMemoryDatabase()` método de extensión, el paquete NuGet de referencia `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
