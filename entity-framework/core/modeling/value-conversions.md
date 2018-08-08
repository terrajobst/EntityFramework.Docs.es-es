---
title: Conversiones de valores - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: d5189cef6d44fdf3fd6116a2952ce07ff3a389d4
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614404"
---
# <a name="value-conversions"></a>Conversiones de valores

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

Convertidores de valores, permiten valores de propiedad que se puede convertir al leer o escribir en la base de datos. Esta conversión puede ser de un valor a otro del mismo tipo (por ejemplo, cifrado de cadenas) o de un valor de un tipo a un valor de otro tipo (por ejemplo, convertir valores de enumeración hacia y desde cadenas en la base de datos.)

## <a name="fundamentals"></a>Aspectos básicos

Convertidores de valores se especifican en términos de un `ModelClrType` y un `ProviderClrType`. El tipo de modelo es el tipo de .NET de la propiedad del tipo de entidad. El tipo de proveedor es el tipo de .NET lo entendido el proveedor de base de datos. Por ejemplo, para guardar las enumeraciones como cadenas en la base de datos, el tipo de modelo es el tipo de la enumeración y el tipo de proveedor es `String`. Estos dos tipos pueden ser el mismo.

Las conversiones se definen mediante dos `Func` árboles de expresión: uno de `ModelClrType` a `ProviderClrType` y otra de `ProviderClrType` a `ModelClrType`. Árboles de expresión se usan para que se pueden compilar en el código de acceso de la base de datos para las conversiones eficaz. Para las conversiones de complejas, el árbol de expresión puede ser una sola llamada a un método que realiza la conversión.

## <a name="configuring-a-value-converter"></a>Configuración de un convertidor de valores

Conversiones de valores se definen en las propiedades de la `OnModelCreating` de su `DbContext`. Por ejemplo, considere la posibilidad de un tipo enum y entidad definido como:
```Csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
A continuación, se pueden definir conversiones en `OnModelCreating` para almacenar los valores de enumeración como cadenas (por ejemplo, "Burro", "Mula",...) en la base de datos:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> Un `null` valor nunca se pasará a un convertidor de valores. Esto facilita la implementación de las conversiones y les permite compartir entre las propiedades que aceptan valores NULL y que no aceptan valores NULL.

## <a name="the-valueconverter-class"></a>La clase ValueConverter

Una llamada a `HasConversion` como se indicó anteriormente, se creará un `ValueConverter` de instancia y establézcalo en la propiedad. El `ValueConverter` en su lugar, se pueden crear explícitamente. Por ejemplo:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Esto puede ser útil cuando varias propiedades usan la misma conversión.

> [!NOTE]  
> Actualmente no hay ninguna forma de especificar en un mismo lugar que todas las propiedades de un tipo determinado deben usar el mismo convertidor de valores. Esta característica se considerará una versión futura.

## <a name="built-in-converters"></a>Convertidores de tipos integrados

EF Core incluye un conjunto de predefinidos `ValueConverter` clases, que se encuentran en el `Microsoft.EntityFrameworkCore.Storage.ValueConversion` espacio de nombres. Estos son:
* `BoolToZeroOneConverter` -Bool a cero y uno
* `BoolToStringConverter` -Bool a cadenas como "Y" y "N"
* `BoolToTwoValuesConverter` -Bool a los dos valores.
* `BytesToStringConverter` -Matriz de bytes en cadena codificada en Base64
* `CastingConverter` -Las conversiones que requieren solo una conversión de Csharp
* `CharToStringConverter` -Char cadena de carácter único
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset al valor de 64 bits con codificación binaria
* `DateTimeOffsetToBytesConverter` -DateTimeOffset a la matriz de bytes
* `DateTimeOffsetToStringConverter` -DateTimeOffset a cadena
* `DateTimeToBinaryConverter` -Fecha y hora al valor de 64 bits incluido DateTimeKind
* `DateTimeToStringConverter` -DateTime a string
* `DateTimeToTicksConverter` -Fecha y hora en ciclos
* `EnumToNumberConverter` -Enumeración número subyacente
* `EnumToStringConverter` -Enum a cadena
* `GuidToBytesConverter` -Guid en la matriz de bytes
* `GuidToStringConverter` -Guid en cadena
* `NumberToBytesConverter` -Cualquier valor numérico para la matriz de bytes
* `NumberToStringConverter` -Cualquier valor numérico en cadena
* `StringToBytesConverter` -Cadena bytes UTF8
* `TimeSpanToStringConverter` -TimeSpan a cadena
* `TimeSpanToTicksConverter` -Intervalo de tiempo en pasos

Tenga en cuenta que `EnumToStringConverter` se incluye en esta lista. Esto significa que no hay ninguna necesidad de especificar la conversión de forma explícita, como se indicó anteriormente. En su lugar, use el convertidor de tipos integrado:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Tenga en cuenta que todos los convertidores integrados tienen estados y por lo que una sola instancia puede compartirse con seguridad entre varias propiedades.

## <a name="pre-defined-conversions"></a>Conversiones predefinidas

Para las conversiones comunes para el que existe un convertidor de tipos integrado, no hay ninguna necesidad especificar explícitamente el convertidor. En su lugar, se configura simplemente se debe usar el tipo de proveedor y EF utilizará automáticamente el convertidor integrado apropiado. Enumeración para las conversiones de cadenas se usan como un ejemplo anterior, pero EF realmente hacen esto automáticamente si se configura el tipo de proveedor:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Lo mismo se puede lograr si se especifica explícitamente el tipo de columna. Por ejemplo, si el tipo de entidad se define como para:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
A continuación, los valores de enumeración se guardará como cadenas en la base de datos sin ninguna configuración adicional en `OnModelCreating`.

## <a name="limitations"></a>Limitaciones

Hay algunas limitaciones conocidas de actuales del sistema de conversión de valor:
* Como se mencionó anteriormente, `null` no se puede convertir.
* Actualmente no hay ninguna manera de distribuir una conversión de una propiedad a varias columnas o viceversa.
* Uso de conversiones de valores puede afectar a la capacidad de EF Core para traducir expresiones a SQL. Para estos casos, se registrará una advertencia.
Eliminación de estas limitaciones se está considerando para una versión futura.
