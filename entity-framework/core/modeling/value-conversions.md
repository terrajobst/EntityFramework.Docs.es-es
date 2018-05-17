---
title: Conversiones de valor - Core EF
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a>Conversiones de valor

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

Convertidores de valores permiten valores de propiedades se convierta al leer o escribir en la base de datos. Esta conversión puede ser de un valor a otro del mismo tipo (por ejemplo, cadenas de cifrado) o un valor de un tipo a un valor de otro tipo (por ejemplo, convertir valores enum hacia y desde las cadenas en la base de datos.)

## <a name="fundamentals"></a>Aspectos básicos

Convertidores de valores se especifican en términos de un `ModelClrType` y un `ProviderClrType`. El tipo de modelo es el tipo de .NET de la propiedad en el tipo de entidad. El tipo de proveedor es el tipo de .NET entendido por el proveedor de la base de datos. Por ejemplo, para guardar las enumeraciones como cadenas en la base de datos, el tipo de modelo es el tipo de la enumeración y el tipo de proveedor es `String`. Estos dos tipos pueden ser la misma.

Las conversiones se definen mediante dos `Func` árboles de expresión: uno de `ModelClrType` a `ProviderClrType` y el otro de `ProviderClrType` a `ModelClrType`. Árboles de expresión se utilizan de forma que puede compilar en el código de acceso de la base de datos para conversiones eficaces. Para conversiones complejas, el árbol de expresión puede ser una simple llamada a un método que realiza la conversión.

## <a name="configuring-a-value-converter"></a>Configuración de un convertidor de valores

Conversiones de valor se definen en las propiedades de la OnModelCreating de su DbContext. Por ejemplo, considere un tipo enum y entidad definido como:
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
A continuación, se pueden definir conversiones en OnModelCreating para almacenar los valores de enumeración como cadenas (p. ej. "Burro", "Mula",...) en la base de datos:
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
> A `null` valor nunca se pasará a un convertidor de valores. Esto facilita la implementación de las conversiones y les permite compartir entre las propiedades que aceptan valores NULL y que no aceptan valores NULL.

## <a name="the-valueconverter-class"></a>La clase ValueConverter

Llamar a `HasConversion` como se indicó anteriormente, se creará un `ValueConverter` de instancia y establecerlo en la propiedad. El `ValueConverter` en su lugar se pueden crear explícitamente. Por ejemplo:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Esto puede ser útil cuando varias propiedades, utilice la misma conversión.

> [!NOTE]  
> Actualmente no hay ninguna manera de especificar en un lugar que todas las propiedades de un tipo determinado deben usar el mismo convertidor de valores. Esta característica se considerará para una futura versión.

## <a name="built-in-converters"></a>Convertidores integrados

Predefinen de EF naves de núcleo con un conjunto de `ValueConverter` las clases, que se encuentra en la `Microsoft.EntityFrameworkCore.Storage.ValueConversion` espacio de nombres. Estos son:
* `BoolToZeroOneConverter` -Bool a cero y uno
* `BoolToStringConverter` -Bool a cadenas como "Y" y "N"
* `BoolToTwoValuesConverter` -Bool a dos valores
* `BytesToStringConverter` -Matriz de bytes en cadena codificada en Base64
* `CastingConverter` -Las conversiones que requieren sólo una conversión de Csharp
* `CharToStringConverter` -Carácter a carácter
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset para codificación binaria de valor de 64 bits
* `DateTimeOffsetToBytesConverter` -DateTimeOffset a la matriz de bytes
* `DateTimeOffsetToStringConverter` -DateTimeOffset a cadena
* `DateTimeToBinaryConverter` -Fecha y hora en valor de 64 bits incluido DateTimeKind
* `DateTimeToStringConverter` -Fecha y hora a cadena
* `DateTimeToTicksConverter` -Fecha y hora en pasos
* `EnumToNumberConverter` -Enum número subyacente
* `EnumToStringConverter` -Enum a cadena
* `GuidToBytesConverter` -Guid a la matriz de bytes
* `GuidToStringConverter` -Guid en cadena
* `NumberToBytesConverter` -Cualquier valor numérico a la matriz de bytes
* `NumberToStringConverter` -Cualquier valor numérico a la cadena
* `StringToBytesConverter` -Cadena UTF8 bytes
* `TimeSpanToStringConverter` -TimeSpan en cadena
* `TimeSpanToTicksConverter` -Intervalo de tiempo en pasos

Observe que `EnumToStringConverter` se incluye en esta lista. Esto significa que no hay ninguna necesidad de especificar la conversión explícitamente, como se muestra arriba. En su lugar, simplemente utilice el convertidor integrado:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Tenga en cuenta que todos los convertidores integrados son independientes y por lo que una única instancia puede compartirse con seguridad varias propiedades.

## <a name="pre-defined-conversions"></a>Conversiones predefinidas

Para conversiones normales para las que existe un convertidor integrado no hay especificar explícitamente el convertidor. En su lugar, simplemente configurar qué tipo de proveedor que debe utilizarse y EF utilizará automáticamente el convertidor de generación correspondiente. Enumeración para las conversiones de cadenas se utilizan como un ejemplo anterior, pero EF realmente lo harán automáticamente si se configura el tipo de proveedor:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Lo mismo se consigue especificando explícitamente el tipo de columna. Por ejemplo, si se define el tipo de entidad como para:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
A continuación, se guardarán los valores de enumeración como cadenas en la base de datos sin ninguna configuración adicional en OnModelCreating.

## <a name="limitations"></a>Limitaciones

Existen algunas limitaciones actuales conocidas del sistema de conversión de valor:
* Como se mencionó anteriormente, `null` no se puede convertir.
* Actualmente no hay ninguna manera para distribuir una conversión de una propiedad a varias columnas o viceversa.
* Uso de conversiones de valor puede afectar a la capacidad del núcleo EF para traducir expresiones de SQL. En tales casos, se registrará una advertencia.
Eliminación de estas limitaciones se está considerando para una futura versión.
