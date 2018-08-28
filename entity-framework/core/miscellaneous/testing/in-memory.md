---
title: Pruebas con InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997897"
---
# <a name="testing-with-inmemory"></a>Pruebas con InMemory

El proveedor InMemory es útil cuando desea probar componentes con algo que se aproxima al conectarse a la base de datos real, sin la sobrecarga de operaciones de base de datos real.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory no es una base de datos relacional

Proveedores de base de datos EF Core no tiene que ser bases de datos relacionales. InMemory está diseñado para ser una base de datos de uso general para las pruebas y no está diseñado para imitar una base de datos relacional.

Algunos ejemplos de esto son:

* InMemory, podrá guardar los datos que infringen las restricciones de integridad referencial en una base de datos relacional.
* Si usa DefaultValueSql(string) para una propiedad en el modelo, esto es una API de base de datos relacional y no tendrá efecto cuando se ejecuta en InMemory.
* [Simultaneidad a través de la versión de fila o marca de tiempo](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` o `IsRowVersion`) no se admite. No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) se producirá si una actualización se realiza mediante un token de simultaneidad antiguo.

> [!TIP]  
> Para muchos fines de prueba no se importan estas diferencias. Sin embargo, si desea probarlo con algo que se comporta más como una base de datos relacional es true, considere la posibilidad de usar [modo en memoria de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Considere el siguiente servicio que permite que el código de aplicación realizar algunas operaciones relacionadas con blogs. Lo utiliza internamente un `DbContext` que se conecta a una base de datos de SQL Server. Sería útil cambiar este contexto para conectarse a una base de datos InMemory, por lo que podemos escribir pruebas eficaces para este servicio sin tener que modificar el código o hacer una gran cantidad de trabajo para crear una prueba doble del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de base de datos

En las pruebas va a configurar externamente el contexto para usar el proveedor InMemory. Si va a configurar un proveedor de base de datos invalidando `OnConfiguring` en su contexto, a continuación, deberá agregar algún código condicional para asegurarse de que solo se configure el proveedor de base de datos si no se ha configurado alguna ya.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si usa ASP.NET Core, a continuación, no deberían necesitar este código puesto que ya esté configurado el proveedor de base de datos fuera del contexto (en Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Agregue un constructor para las pruebas

Es la manera más sencilla de habilitar las pruebas con otra base de datos modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica el contexto de todos sus valores, por ejemplo, para conectarse a la base de datos. Este es el mismo objeto que se compila mediante la ejecución del método OnConfiguring en su contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave para las pruebas con este proveedor es la capacidad de decirle el contexto para usar el proveedor InMemory y controlar el ámbito de la base de datos en memoria. Normalmente desea limpiar una base de datos de cada método de prueba.

Este es un ejemplo de una clase de prueba que usa la base de datos InMemory. Cada método de prueba especifica un nombre de base de datos único, lo que significa que cada método tiene su propia base de datos InMemory.

>[!TIP]
> Para usar el `.UseInMemoryDatabase()` método de extensión, el paquete NuGet de referencia `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
