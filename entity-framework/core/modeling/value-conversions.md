---
title: Conversiones de valor - Core EF
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 50acba39cdec16caa9300fcaf47ab6242a4f69fb
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="value-conversions"></a><span data-ttu-id="c79d1-102">Conversiones de valor</span><span class="sxs-lookup"><span data-stu-id="c79d1-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="c79d1-103">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c79d1-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c79d1-104">Convertidores de valores permiten valores de propiedades se convierta al leer o escribir en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c79d1-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="c79d1-105">Esta conversión puede ser de un valor a otro del mismo tipo (por ejemplo, cadenas de cifrado) o un valor de un tipo a un valor de otro tipo (por ejemplo, convertir valores enum hacia y desde las cadenas en la base de datos.)</span><span class="sxs-lookup"><span data-stu-id="c79d1-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="c79d1-106">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="c79d1-106">Fundamentals</span></span>

<span data-ttu-id="c79d1-107">Convertidores de valores se especifican en términos de un `ModelClrType` y un `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="c79d1-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="c79d1-108">El tipo de modelo es el tipo de .NET de la propiedad en el tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="c79d1-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="c79d1-109">El tipo de proveedor es el tipo de .NET entendido por el proveedor de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c79d1-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="c79d1-110">Por ejemplo, para guardar las enumeraciones como cadenas en la base de datos, el tipo de modelo es el tipo de la enumeración y el tipo de proveedor es `String`.</span><span class="sxs-lookup"><span data-stu-id="c79d1-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="c79d1-111">Estos dos tipos pueden ser la misma.</span><span class="sxs-lookup"><span data-stu-id="c79d1-111">These two types can be the same.</span></span>

<span data-ttu-id="c79d1-112">Las conversiones se definen mediante dos `Func` árboles de expresión: uno de `ModelClrType` a `ProviderClrType` y el otro de `ProviderClrType` a `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="c79d1-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="c79d1-113">Árboles de expresión se utilizan de forma que puede compilar en el código de acceso de la base de datos para conversiones eficaces.</span><span class="sxs-lookup"><span data-stu-id="c79d1-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="c79d1-114">Para conversiones complejas, el árbol de expresión puede ser una simple llamada a un método que realiza la conversión.</span><span class="sxs-lookup"><span data-stu-id="c79d1-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="c79d1-115">Configuración de un convertidor de valores</span><span class="sxs-lookup"><span data-stu-id="c79d1-115">Configuring a value converter</span></span>

<span data-ttu-id="c79d1-116">Conversiones de valor se definen en las propiedades de la OnModelCreating de su DbContext.</span><span class="sxs-lookup"><span data-stu-id="c79d1-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="c79d1-117">Por ejemplo, considere un tipo enum y entidad definido como:</span><span class="sxs-lookup"><span data-stu-id="c79d1-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="c79d1-118">A continuación, se pueden definir conversiones en OnModelCreating para almacenar los valores de enumeración como cadenas (p. ej. "Burro", "Mula",...) en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="c79d1-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="c79d1-119">A `null` valor nunca se pasará a un convertidor de valores.</span><span class="sxs-lookup"><span data-stu-id="c79d1-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="c79d1-120">Esto facilita la implementación de las conversiones y les permite compartir entre las propiedades que aceptan valores NULL y que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="c79d1-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="c79d1-121">La clase ValueConverter</span><span class="sxs-lookup"><span data-stu-id="c79d1-121">The ValueConverter class</span></span>

<span data-ttu-id="c79d1-122">Llamar a `HasConversion` como se indicó anteriormente, se creará un `ValueConverter` de instancia y establecerlo en la propiedad.</span><span class="sxs-lookup"><span data-stu-id="c79d1-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="c79d1-123">El `ValueConverter` en su lugar se pueden crear explícitamente.</span><span class="sxs-lookup"><span data-stu-id="c79d1-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="c79d1-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c79d1-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="c79d1-125">Esto puede ser útil cuando varias propiedades, utilice la misma conversión.</span><span class="sxs-lookup"><span data-stu-id="c79d1-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="c79d1-126">Actualmente no hay ninguna manera de especificar en un lugar que todas las propiedades de un tipo determinado deben usar el mismo convertidor de valores.</span><span class="sxs-lookup"><span data-stu-id="c79d1-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="c79d1-127">Esta característica se considerará para una futura versión.</span><span class="sxs-lookup"><span data-stu-id="c79d1-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="c79d1-128">Convertidores integrados</span><span class="sxs-lookup"><span data-stu-id="c79d1-128">Built-in converters</span></span>

<span data-ttu-id="c79d1-129">Predefinen de EF naves de núcleo con un conjunto de `ValueConverter` las clases, que se encuentra en la `Microsoft.EntityFrameworkCore.Storage.Converters` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c79d1-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.Converters` namespace.</span></span> <span data-ttu-id="c79d1-130">Estos son:</span><span class="sxs-lookup"><span data-stu-id="c79d1-130">These are:</span></span>
* <span data-ttu-id="c79d1-131">`BoolToZeroOneConverter` -Bool a cero y uno</span><span class="sxs-lookup"><span data-stu-id="c79d1-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="c79d1-132">`BoolToStringConverter` -Bool a cadenas como "Y" y "N"</span><span class="sxs-lookup"><span data-stu-id="c79d1-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="c79d1-133">`BoolToTwoValuesConverter` -Bool a dos valores</span><span class="sxs-lookup"><span data-stu-id="c79d1-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="c79d1-134">`BytesToStringConverter` -Matriz de bytes en cadena codificada en Base64</span><span class="sxs-lookup"><span data-stu-id="c79d1-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="c79d1-135">`CastingConverter` -Las conversiones que requieren sólo una conversión de Csharp</span><span class="sxs-lookup"><span data-stu-id="c79d1-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="c79d1-136">`CharToStringConverter` -Carácter a carácter</span><span class="sxs-lookup"><span data-stu-id="c79d1-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="c79d1-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset para codificación binaria de valor de 64 bits</span><span class="sxs-lookup"><span data-stu-id="c79d1-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="c79d1-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset a la matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="c79d1-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="c79d1-139">`DateTimeOffsetToStringConverter` -DateTimeOffset a cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="c79d1-140">`DateTimeToBinaryConverter` -Fecha y hora en valor de 64 bits incluido DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="c79d1-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="c79d1-141">`DateTimeToStringConverter` -Fecha y hora a cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="c79d1-142">`DateTimeToTicksConverter` -Fecha y hora en pasos</span><span class="sxs-lookup"><span data-stu-id="c79d1-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="c79d1-143">`EnumToNumberConverter` -Enum número subyacente</span><span class="sxs-lookup"><span data-stu-id="c79d1-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="c79d1-144">`EnumToStringConverter` -Enum a cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="c79d1-145">`GuidToBytesConverter` -Guid a la matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="c79d1-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="c79d1-146">`GuidToStringConverter` -Guid en cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="c79d1-147">`NumberToBytesConverter` -Cualquier valor numérico a la matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="c79d1-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="c79d1-148">`NumberToStringConverter` -Cualquier valor numérico a la cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="c79d1-149">`StringToBytesConverter` -Cadena UTF8 bytes</span><span class="sxs-lookup"><span data-stu-id="c79d1-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="c79d1-150">`TimeSpanToStringConverter` -TimeSpan en cadena</span><span class="sxs-lookup"><span data-stu-id="c79d1-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="c79d1-151">`TimeSpanToTicksConverter` -Intervalo de tiempo en pasos</span><span class="sxs-lookup"><span data-stu-id="c79d1-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="c79d1-152">Observe que `EnumToStringConverter` se incluye en esta lista.</span><span class="sxs-lookup"><span data-stu-id="c79d1-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="c79d1-153">Esto significa que no hay ninguna necesidad de especificar la conversión explícitamente, como se muestra arriba.</span><span class="sxs-lookup"><span data-stu-id="c79d1-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="c79d1-154">En su lugar, simplemente utilice el convertidor integrado:</span><span class="sxs-lookup"><span data-stu-id="c79d1-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="c79d1-155">Tenga en cuenta que todos los convertidores integrados son independientes y por lo que una única instancia puede compartirse con seguridad varias propiedades.</span><span class="sxs-lookup"><span data-stu-id="c79d1-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="c79d1-156">Conversiones predefinidas</span><span class="sxs-lookup"><span data-stu-id="c79d1-156">Pre-defined conversions</span></span>

<span data-ttu-id="c79d1-157">Para conversiones normales para las que existe un convertidor integrado no hay especificar explícitamente el convertidor.</span><span class="sxs-lookup"><span data-stu-id="c79d1-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="c79d1-158">En su lugar, simplemente configurar qué tipo de proveedor que debe utilizarse y EF utilizará automáticamente el convertidor de generación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="c79d1-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="c79d1-159">Enumeración para las conversiones de cadenas se utilizan como un ejemplo anterior, pero EF realmente lo harán automáticamente si se configura el tipo de proveedor:</span><span class="sxs-lookup"><span data-stu-id="c79d1-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="c79d1-160">Lo mismo se consigue especificando explícitamente el tipo de columna.</span><span class="sxs-lookup"><span data-stu-id="c79d1-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="c79d1-161">Por ejemplo, si se define el tipo de entidad como para:</span><span class="sxs-lookup"><span data-stu-id="c79d1-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="c79d1-162">A continuación, se guardarán los valores de enumeración como cadenas en la base de datos sin ninguna configuración adicional en OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="c79d1-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="c79d1-163">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="c79d1-163">Limitations</span></span>

<span data-ttu-id="c79d1-164">Existen algunas limitaciones actuales conocidas del sistema de conversión de valor:</span><span class="sxs-lookup"><span data-stu-id="c79d1-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="c79d1-165">Como se mencionó anteriormente, `null` no se puede convertir.</span><span class="sxs-lookup"><span data-stu-id="c79d1-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="c79d1-166">Actualmente no hay ninguna manera de difundir una conversión de una propiedad a multuple columnas o viceversa.</span><span class="sxs-lookup"><span data-stu-id="c79d1-166">There is currently no way to spread a conversion of one property to multuple columns or vice-versa.</span></span>
* <span data-ttu-id="c79d1-167">Uso de conversiones de valor puede afectar a la capacidad del núcleo EF para traducir expresiones de SQL.</span><span class="sxs-lookup"><span data-stu-id="c79d1-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="c79d1-168">En tales casos, se registrará una advertencia.</span><span class="sxs-lookup"><span data-stu-id="c79d1-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="c79d1-169">Eliminación de estas limitaciones se está considerando para una futura versión.</span><span class="sxs-lookup"><span data-stu-id="c79d1-169">Removal of these limitations is being considered for a future release.</span></span>
