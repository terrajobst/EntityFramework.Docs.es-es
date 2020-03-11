---
title: Pruebas con inmemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414030"
---
# <a name="testing-with-inmemory"></a>Pruebas con InMemory

El proveedor de inmemory es útil cuando se desea probar los componentes mediante algo que se aproxima a la conexión a la base de datos real, sin la sobrecarga de las operaciones de base de datos reales.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) de este artículo en GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>La inmemory no es una base de datos relacional

EF Core proveedores de bases de datos no tienen que ser bases de datos relacionales. La inmemory está diseñada para ser una base de datos de uso general para las pruebas y no está diseñada para imitar una base de datos relacional.

Algunos ejemplos de esto son:

* La inmemory le permitirá guardar datos que infrinjan las restricciones de integridad referencial en una base de datos relacional.
* Si usa DefaultValueSql (String) para una propiedad en el modelo, se trata de una API de base de datos relacional y no tendrá ningún efecto cuando se ejecute en inmemory.
* No se admite la [simultaneidad mediante la versión de marca](xref:core/modeling/concurrency#timestamprowversion) de tiempo o fila (`[Timestamp]` o `IsRowVersion`). No se producirá [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) si se realiza una actualización mediante un token de simultaneidad antiguo.

> [!TIP]  
> Para muchos propósitos de prueba, estas diferencias no serán importantes. Sin embargo, si desea probar algo que se comporta más como una verdadera base de datos relacional, considere el uso [del modo en memoria de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Escenario de prueba de ejemplo

Considere el siguiente servicio que permite al código de la aplicación realizar algunas operaciones relacionadas con blogs. Utiliza internamente un `DbContext` que se conecta a una base de datos SQL Server. Sería útil intercambiar este contexto para conectarse a una base de datos inmemory de modo que podamos escribir pruebas eficaces para este servicio sin tener que modificar el código, o hacer mucho trabajo para crear un doble de prueba del contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparar el contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dos proveedores de bases de datos

En las pruebas, va a configurar externamente el contexto para usar el proveedor de inmemory. Si está configurando un proveedor de base de datos invalidando `OnConfiguring` en el contexto, deberá agregar código condicional para asegurarse de que solo se configura el proveedor de base de datos si aún no se ha configurado ninguno.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Si usa ASP.NET Core, no necesitará este código ya que el proveedor de base de datos ya está configurado fuera del contexto (en Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Agregar un constructor para las pruebas

La manera más sencilla de habilitar las pruebas en una base de datos diferente es modificar el contexto para exponer un constructor que acepta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica al contexto toda su configuración, como la base de datos a la que se va a conectar. Este es el mismo objeto que se genera al ejecutar el método de configuración en el contexto.

## <a name="writing-tests"></a>Escribir pruebas

La clave para probar con este proveedor es la capacidad de indicar al contexto que use el proveedor de inmemory y controlar el ámbito de la base de datos en memoria. Normalmente, desea una base de datos limpia para cada método de prueba.

Este es un ejemplo de una clase de prueba que usa la base de datos inmemory. Cada método de prueba especifica un nombre de base de datos único, lo que significa que cada método tiene su propia base de datos inmemory.

>[!TIP]
> Para usar el método de extensión `.UseInMemoryDatabase()`, haga referencia al paquete NuGet [Microsoft. EntityFrameworkCore. inmemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
