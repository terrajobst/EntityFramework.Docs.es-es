---
title: Conversiones de valores-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654791"
---
# <a name="value-conversions"></a>Conversiones de valores

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

Los convertidores de valores permiten convertir los valores de propiedad al leer o escribir en la base de datos. Esta conversión puede ser de un valor a otro del mismo tipo (por ejemplo, cifrar cadenas) o de un valor de un tipo a un valor de otro tipo (por ejemplo, convertir valores de enumeración en cadenas en la base de datos y desde ellas).

## <a name="fundamentals"></a>Aspectos básicos

Los convertidores de valores se especifican en términos de un `ModelClrType` y un `ProviderClrType`. El tipo de modelo es el tipo .NET de la propiedad en el tipo de entidad. El tipo de proveedor es el tipo .NET que entiende el proveedor de base de datos. Por ejemplo, para guardar las enumeraciones como cadenas en la base de datos, el tipo de modelo es el tipo de la enumeración y el tipo de proveedor es `String`. Estos dos tipos pueden ser iguales.

Las conversiones se definen utilizando dos `Func` árboles de expresión: uno de `ModelClrType` a `ProviderClrType` y otro de `ProviderClrType` a `ModelClrType`. Los árboles de expresión se usan para que se puedan compilar en el código de acceso a la base de datos para conversiones eficientes. En las conversiones complejas, el árbol de expresión puede ser una llamada simple a un método que realiza la conversión.

## <a name="configuring-a-value-converter"></a>Configurar un convertidor de valores

Las conversiones de valores se definen en las propiedades del `OnModelCreating` de la `DbContext`. Por ejemplo, considere una enumeración y un tipo de entidad definidos como:

``` csharp
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

A continuación, se pueden definir conversiones en `OnModelCreating` para almacenar los valores de enumeración como cadenas (por ejemplo, "Donkey", "Mule",...) en la base de datos:

``` csharp
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
> Un valor `null` nunca se pasará a un convertidor de valores. Esto hace que la implementación de conversiones sea más sencilla y permite que se compartan entre propiedades que aceptan valores NULL y que no aceptan valores NULL.

## <a name="the-valueconverter-class"></a>La clase ValueConverter

Al llamar a `HasConversion` como se muestra anteriormente, se creará una instancia de `ValueConverter` y se establecerá en la propiedad. En su lugar, se puede crear el `ValueConverter` explícitamente. Por ejemplo:

``` csharp
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
> Actualmente no hay ninguna manera de especificar en un lugar que cada propiedad de un tipo determinado debe usar el mismo convertidor de valores. Esta característica se considerará para futuras versiones.

## <a name="built-in-converters"></a>Convertidores integrados

EF Core incluye un conjunto de clases de `ValueConverter` predefinidas, que se encuentran en el espacio de nombres `Microsoft.EntityFrameworkCore.Storage.ValueConversion`. Estos son:

* `BoolToZeroOneConverter`-bool a cero y uno
* `BoolToStringConverter`-bool a cadenas como "Y" y "N"
* `BoolToTwoValuesConverter`-bool a dos valores cualesquiera
* matriz de bytes de `BytesToStringConverter` a una cadena codificada en Base64
* conversiones de `CastingConverter` que requieren solo una conversión de tipos
* `CharToStringConverter`-char a una cadena de un solo carácter
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset a un valor codificado en binario 64 bits
* `DateTimeOffsetToBytesConverter`-DateTimeOffset a la matriz de bytes
* `DateTimeOffsetToStringConverter`-DateTimeOffset a cadena
* `DateTimeToBinaryConverter`: DateTime al valor de 64 bits, incluido DateTimeKind
* `DateTimeToStringConverter`: fecha y hora en cadena
* `DateTimeToTicksConverter`: fecha y hora en pasos
* `EnumToNumberConverter`-enum al número subyacente
* `EnumToStringConverter`-enum a cadena
* `GuidToBytesConverter`-GUID a una matriz de bytes
* `GuidToStringConverter`-GUID a cadena
* `NumberToBytesConverter`: cualquier valor numérico en una matriz de bytes
* `NumberToStringConverter`: cualquier valor numérico a cadena
* `StringToBytesConverter`: cadena en bytes UTF8
* `TimeSpanToStringConverter`-TimeSpan a String
* `TimeSpanToTicksConverter`-TimeSpan a ticks

Observe que `EnumToStringConverter` se incluye en esta lista. Esto significa que no es necesario especificar la conversión explícitamente, como se muestra anteriormente. En su lugar, use simplemente el convertidor integrado:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Tenga en cuenta que todos los convertidores integrados no tienen estado y, por tanto, una sola instancia puede compartirse de forma segura con varias propiedades.

## <a name="pre-defined-conversions"></a>Conversiones predefinidas

En el caso de las conversiones comunes para las que existe un convertidor integrado, no es necesario especificar el convertidor explícitamente. En su lugar, solo tiene que configurar el tipo de proveedor que se debe usar y EF usará automáticamente el convertidor integrado adecuado. La enumeración de las conversiones de cadenas se usa como ejemplo anterior, pero EF lo hará automáticamente si se configura el tipo de proveedor:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

Lo mismo se puede lograr especificando explícitamente el tipo de columna. Por ejemplo, si el tipo de entidad se define de la manera siguiente:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

A continuación, los valores de enumeración se guardarán como cadenas en la base de datos sin ninguna otra configuración en `OnModelCreating`.

## <a name="limitations"></a>Limitaciones

Hay algunas limitaciones actuales conocidas del sistema de conversión de valores:

* Como se indicó anteriormente, no se puede convertir `null`.
* Actualmente no hay ninguna manera de distribuir una conversión de una propiedad a varias columnas o viceversa.
* El uso de conversiones de valores puede afectar a la capacidad de EF Core para traducir expresiones a SQL. Se registrará una advertencia para dichos casos.
La eliminación de estas limitaciones se está considerando en una versión futura.
