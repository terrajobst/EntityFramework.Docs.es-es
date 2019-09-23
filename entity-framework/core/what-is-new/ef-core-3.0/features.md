---
title: 'Novedades de EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005557"
---
# <a name="new-features-included-in-ef-core-30"></a>Nuevas características incluidas en EF Core 3.0

En la lista siguiente se incluyen las principales características nuevas planeadas para EF Core 3.0.

EF Core 3.0 es una versión principal que también presenta numerosos [cambios importantes](xref:core/what-is-new/ef-core-3.0/breaking-changes). Se trata de mejoras en la API que podrían afectar negativamente a las aplicaciones existentes.  

## <a name="linq-improvements"></a>Mejoras de LINQ 

LINQ permite escribir consultas a la base de datos sin abandonar el idioma de elección, con lo que se aprovecha la información de tipo enriquecido para ofrecer la comprobación de IntelliSense y de tipos en tiempo de compilación.
Pero LINQ también permite escribir un número ilimitado de consultas complicadas que contienen expresiones arbitrarias (llamadas a métodos u operaciones).
Controlar todas esas combinaciones siempre ha sido un desafío importante para los proveedores LINQ.
En EF Core 3.0, hemos vuelto a escribir nuestra implementación de LINQ para habilitar la traducción de más expresiones en SQL, generar consultas eficaces en más casos, evitar que las consultas ineficaces no se detecten y facilitar la introducción gradual de las mejoras de las capacidades y el rendimiento de la nueva consulta, sin interrumpir las aplicaciones y los proveedores de datos existentes.

### <a name="client-evaluation"></a>Evaluación de cliente

El cambio de diseño principal en EF Core 3.0 está relacionado con cómo controla las expresiones LINQ que no se pueden traducir a SQL o parámetros:

En las primeras versiones, EF Core simplemente averiguaba qué partes de una consulta se pueden traducir a SQL y ejecutaba el resto de la consulta en el cliente.
Este tipo de ejecución en el lado cliente puede ser una opción interesante en algunas situaciones, pero en muchos otros casos puede dar lugar a consultas ineficaces.
Por ejemplo, si EF Core 2.2 no podía traducir un predicado en una llamada a `Where()`, ejecutaba una instrucción SQL sin filtro, leía todas las filas de la base de datos y luego las filtraba en memoria.
Esta operación puede ser aceptable si la base de datos contiene pocas filas, pero puede dar lugar a problemas de rendimiento considerables o incluso errores en la aplicación si la base de datos contiene muchas filas.
En EF Core 3.0 hemos restringido la evaluación de cliente para que solo suceda en la proyección de nivel superior (la última llamada a `Select()`).
Cuando EF Core 3.0 detecta expresiones que no se pueden traducir en ningún otro lugar de la consulta, produce una excepción en tiempo de ejecución.

## <a name="cosmos-db-support"></a>Compatibilidad con Cosmos DB 

Con el proveedor de Cosmos DB para EF Core, los desarrolladores que están familiarizados con el modelo de programación de EF puedan usar fácilmente Azure Cosmos DB como base de datos de aplicación.
El objetivo es hacer que algunas de las ventajas de Cosmos DB, como la distribución global, la disponibilidad "AlwaysOn", la escalabilidad elástica y la baja latencia, sean aún más accesibles para los desarrolladores de .NET.
El proveedor habilitará la mayoría de las características de EF Core, como el seguimiento automático de cambios, LINQ y conversiones de valores, en comparación con SQL API de Cosmos DB.

## <a name="c-80-support"></a>Compatibilidad con C# 8.0

EF Core 3.0 aprovecha algunas de las nuevas características de C# 8.0:

### <a name="asynchronous-streams"></a>Secuencias asincrónicas

Los resultados de la consulta asincrónica se exponen ahora mediante la nueva interfaz de `await foreach` estándar y se pueden usar con `IAsyncEnumerable<T>`.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>Tipos de referencia que aceptan valores NULL 

Cuando esta nueva característica está habilitada en el código, EF Core puede tener en cuenta la nulabilidad de las propiedades de los tipos de referencia (ya sea de tipos primitivos, como las propiedades de navegación o de cadena) para decidir la nulabilidad de las columnas y las relaciones de la base de datos.

## <a name="interception"></a>Interception

La nueva API de interceptación en EF Core 3.0 permite observar y modificar mediante programación el resultado de las operaciones detalladas de la base de datos que se producen como parte del funcionamiento normal de EF Core, como abrir conexiones, iniciar transacciones y ejecutar comandos. 

## <a name="reverse-engineering-of-database-views"></a>Ingeniería inversa de vistas de base de datos

Los tipos de entidad sin claves (denominados anteriormente [tipos de consulta](xref:core/modeling/query-types)) representan los datos que se pueden leer de la base de datos, pero que no se pueden actualizar.
Esta característica los convierte en una elección excelente para asignar vistas de base de datos en la mayoría de los escenarios, por lo que hemos automatizado la creación de tipos de entidad sin claves al usar técnicas de ingeniería inversa en vistas de base de datos.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales

A partir de la versión EF Core 3.0, si `OrderDetails` pertenece a `Order` o está asignado a la misma tabla explícitamente, será posible agregar `Order` sin `OrderDetails`; todas las propiedades `OrderDetails`, salvo la clave principal, se asignarán a columnas que aceptan valores NULL.

Al realizar consultas, EF Core establecerá `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor, o bien no tiene las propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 en .NET Core

Entendemos que muchas aplicaciones existentes utilizan versiones anteriores de EF, y que portarlas a EF Core solo para aprovechar las ventajas de .NET Core puede requerir a veces un esfuerzo considerable.
Por ese motivo, hemos habilitado la versión más reciente de EF 6 para que se ejecute en .NET Core 3.0.
Existen algunas limitaciones, como por ejemplo:
- Se necesitan nuevos proveedores para trabajar en .NET Core
- No se habilitará la compatibilidad espacial con SQL Server.

## <a name="postponed-features"></a>Características pospuestas

Algunas características planeadas originalmente para EF Core 3.0 se pospusieron para versiones futuras: 

- Capacidad de omitir partes de un modelo en migraciones, con seguimiento realizado a través del problema [n.° 2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entidades contenedoras de propiedades, de las que se realiza un seguimiento a través de dos problemas independientes: [n.° 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre las entidades de tipo compartido y [n.° 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre la compatibilidad con la asignación de propiedades indizadas.
