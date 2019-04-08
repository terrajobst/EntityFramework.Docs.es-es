---
title: 'Novedades de EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58867962"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nuevas características incluidas en EF Core 3.0 (actualmente en versión preliminar)

> [!IMPORTANT]
> Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.

En la lista siguiente se incluyen las principales características nuevas planeadas para EF Core 3.0.
La mayoría de estas características no están incluidas en la versión preliminar actual, pero estarán disponibles cuando avancemos hacia RTM.

El motivo es que al principio de la versión, nos centraremos en la implementación de los [cambios importantes](xref:core/what-is-new/ef-core-3.0/breaking-changes) planeados.
Muchos de estos cambios son, por sí solos, mejoras de EF Core.
Otros muchos son necesarios para desbloquear más mejoras. 

Para obtener una lista completa de las correcciones de errores y las mejoras en curso, puede ver [esta consulta en nuestro rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Mejoras de LINQ 

[Problema de seguimiento n.º 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual.

LINQ permite escribir consultas a la base de datos sin abandonar el idioma de elección, con lo que se aprovecha la información de tipo enriquecido para obtener la comprobación de IntelliSense y de tipos en tiempo de compilación.
Pero LINQ también le permite escribir un número ilimitado de consultas complicadas, y eso siempre ha sido un gran reto para los proveedores de LINQ.
En las primeras versiones de EF Core, resolvimos esto en parte al calcular qué partes de una consulta se podían traducir a SQL, y luego al permitir que el resto de la consulta se ejecutara en memoria en el cliente.
Esta ejecución de cliente puede ser deseable en algunas situaciones, pero en muchos otros casos puede dar como resultado consultas ineficientes que pueden no ser identificadas hasta que una aplicación se implemente en producción.
En EF Core 3.0, tenemos previsto hacer cambios profundos en cómo funciona nuestra implementación de LINQ, y cómo la probamos.
Los objetivos son hacerla más eficaz (por ejemplo, evitar interrumpir las consultas en las versiones de revisión), poder traducir más expresiones correctamente a SQL, generar consultas eficientes en más casos y evitar que las consultas ineficientes pasen desapercibidas.

## <a name="cosmos-db-support"></a>Compatibilidad con Cosmos DB 

[Problema de seguimiento n.º 8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Esta característica está incluida en la versión preliminar actual, pero todavía no está completa. 

Estamos trabajando en un proveedor de Cosmos DB para EF Core, para que los desarrolladores familiarizados con el modelo de programación de EF puedan utilizar fácilmente Azure Cosmos DB como base de datos de aplicación.
El objetivo es hacer que algunas de las ventajas de Cosmos DB, como la distribución global, la disponibilidad "AlwaysOn", la escalabilidad elástica y la baja latencia, sean aún más accesibles para los desarrolladores de .NET.
El proveedor habilitará la mayoría de las características de EF Core, como el seguimiento automático de cambios, LINQ y conversiones de valores, en comparación con SQL API de Cosmos DB.
Comenzamos este esfuerzo antes de EF Core 2.2 y [hemos puesto a disposición algunas versiones preliminares del proveedor](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
El nuevo plan es continuar desarrollando el proveedor junto con EF Core 3.0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales

[Problema de seguimiento n.º 9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Esta característica se introducirá en EF Core 3.0 (versión preliminar 4).

Considere el modelo siguiente:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

A partir de la versión EF Core 3.0, si `OrderDetails` es propiedad de `Order` o está asignado explícitamente a la misma tabla, será posible agregar `Order` sin `OrderDetails` y todas las propiedades `OrderDetails` excepto la clave principal se asignarán a columnas que aceptan valores NULL.
Al realizar consultas, EF Core establecerá `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.

## <a name="c-80-support"></a>Compatibilidad con C# 8.0

[Problema de seguimiento n.º 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Problema de seguimiento n.º 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual.

Queremos que, mientras usan EF Core, nuestros clientes aprovechen algunas de las [nuevas características que se incluyen en C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como las secuencias de sincronización (incluida `await foreach`) y los tipos de referencia que aceptan valores NULL.

## <a name="reverse-engineering-of-database-views"></a>Ingeniería inversa de vistas de base de datos

[Problema de seguimiento n.º 1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Esta característica no está incluida en la versión preliminar actual.

Los [tipos de consulta](xref:core/modeling/query-types), presentados en EF Core 2.1 y considerados tipos de entidad sin claves en EF Core 3.0, representan datos que se pueden leer desde la base de datos, pero que no se pueden actualizar.
Esta característica los convierte en una elección excelente para las vistas de base de datos en la mayoría de los escenarios, por lo que tenemos previsto automatizar la creación de tipos de entidad sin claves al usar de técnicas de ingeniería inversa en vistas de base de datos.

## <a name="property-bag-entities"></a>Entidades de contenedor de propiedades

[Problema de seguimiento n.º 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) y [n.º 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual. 

Esta característica trata de permitir entidades que almacenan datos en propiedades indexadas en lugar de propiedades regulares, y también de utilizar instancias de la misma clase .NET (potencialmente algo tan simple como un `Dictionary<string, object>`) para representar diferentes tipos de entidades en el mismo modelo de EF Core.
Esta característica es un punto de partida para admitir las relaciones varios a varios sin una entidad de unión ([problema n.° 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), que es una de las mejoras más solicitadas por EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 en .NET Core

[Problema de seguimiento n.º 271 de EF 6](https://github.com/aspnet/EntityFramework6/issues/271)

Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual. 

Entendemos que muchas aplicaciones existentes utilizan versiones anteriores de EF, y que portarlas a EF Core solo para aprovechar las ventajas de .NET Core puede requerir a veces un esfuerzo considerable.
Por ese motivo, adaptaremos la próxima versión de EF 6 para se ejecute en .NET Core 3.0.
Hacemos esto para facilitar la portabilidad de las aplicaciones existentes con cambios mínimos.
Habrá algunas limitaciones. Por ejemplo:
- Se necesitarán nuevos proveedores para trabajar con otras bases de datos, además de la compatibilidad de SQL Server incluida en .NET Core.
- No se habilitará la compatibilidad espacial con SQL Server.

Tenga en cuenta también que, en este momento, no hay características nuevas planeadas para EF 6.
