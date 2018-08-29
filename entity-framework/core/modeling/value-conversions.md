---
title: Conversões de valor - EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152504"
---
# <a name="value-conversions"></a><span data-ttu-id="95fa7-102">Conversões de valor</span><span class="sxs-lookup"><span data-stu-id="95fa7-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="95fa7-103">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="95fa7-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="95fa7-104">Conversores de valor permitem que os valores de propriedade a ser convertido durante a leitura ou gravação no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95fa7-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="95fa7-105">Essa conversão pode ser de um valor para outra do mesmo tipo (por exemplo, cadeias de caracteres com criptografia) ou de um valor de um tipo para um valor de outro tipo (por exemplo, converter enum valores em cadeias de caracteres no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="95fa7-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="95fa7-106">Princípios básicos</span><span class="sxs-lookup"><span data-stu-id="95fa7-106">Fundamentals</span></span>

<span data-ttu-id="95fa7-107">Conversores de valor são especificados em termos de um `ModelClrType` e um `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="95fa7-108">O tipo de modelo é o tipo de .NET da propriedade no tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="95fa7-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="95fa7-109">O tipo de provedor é o tipo de .NET entendido pelo provedor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95fa7-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="95fa7-110">Por exemplo, para salvar as enumerações como cadeias de caracteres no banco de dados, o tipo de modelo é o tipo de enumeração, e o tipo de provedor é `String`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="95fa7-111">Esses dois tipos podem ser o mesmo.</span><span class="sxs-lookup"><span data-stu-id="95fa7-111">These two types can be the same.</span></span>

<span data-ttu-id="95fa7-112">As conversões são definidas usando duas `Func` árvores de expressão: um da `ModelClrType` para `ProviderClrType` e o outro da `ProviderClrType` para `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="95fa7-113">Árvores de expressão são usadas para que eles podem ser compilados em código de acesso a banco de dados para conversões eficientes.</span><span class="sxs-lookup"><span data-stu-id="95fa7-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="95fa7-114">Para conversões complexas, a árvore de expressão pode ser uma chamada simples para um método que executa a conversão.</span><span class="sxs-lookup"><span data-stu-id="95fa7-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="95fa7-115">Configurando um conversor de valor</span><span class="sxs-lookup"><span data-stu-id="95fa7-115">Configuring a value converter</span></span>

<span data-ttu-id="95fa7-116">Conversões de valor são definidas nas propriedades na `OnModelCreating` de seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="95fa7-117">Por exemplo, considere um tipo enum e a entidade definido como:</span><span class="sxs-lookup"><span data-stu-id="95fa7-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="95fa7-118">Em seguida, as conversões podem ser definidas em `OnModelCreating` para armazenar os valores de enum como cadeias de caracteres (por exemplo, "Burro", "outras" mulas "",...) no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="95fa7-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="95fa7-119">Um `null` valor nunca será passado para um conversor de valor.</span><span class="sxs-lookup"><span data-stu-id="95fa7-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="95fa7-120">Isso facilita a implementação de conversões e lhes permite ser compartilhado entre as propriedades que permitem valor nulas e não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="95fa7-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="95fa7-121">A classe ValueConverter</span><span class="sxs-lookup"><span data-stu-id="95fa7-121">The ValueConverter class</span></span>

<span data-ttu-id="95fa7-122">Chamando `HasConversion` conforme mostrado acima criará um `ValueConverter` da instância e defini-lo na propriedade.</span><span class="sxs-lookup"><span data-stu-id="95fa7-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="95fa7-123">O `ValueConverter` em vez disso, podem ser criadas explicitamente.</span><span class="sxs-lookup"><span data-stu-id="95fa7-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="95fa7-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="95fa7-124">For example:</span></span>
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="95fa7-125">Isso pode ser útil quando várias propriedades usam a mesma conversão.</span><span class="sxs-lookup"><span data-stu-id="95fa7-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="95fa7-126">Atualmente, não há nenhuma maneira de especificar em um só lugar que todas as propriedades de um determinado tipo devem usar o mesmo conversor de valor.</span><span class="sxs-lookup"><span data-stu-id="95fa7-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="95fa7-127">Esse recurso será considerado para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="95fa7-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="95fa7-128">Conversores internos</span><span class="sxs-lookup"><span data-stu-id="95fa7-128">Built-in converters</span></span>

<span data-ttu-id="95fa7-129">EF Core envia com um conjunto de predefinidos `ValueConverter` classes, encontradas no `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span><span class="sxs-lookup"><span data-stu-id="95fa7-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="95fa7-130">Elas são:</span><span class="sxs-lookup"><span data-stu-id="95fa7-130">These are:</span></span>
* <span data-ttu-id="95fa7-131">`BoolToZeroOneConverter` -Bool para zero e um</span><span class="sxs-lookup"><span data-stu-id="95fa7-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="95fa7-132">`BoolToStringConverter` -Bool em cadeias de caracteres como "Y" e "N"</span><span class="sxs-lookup"><span data-stu-id="95fa7-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="95fa7-133">`BoolToTwoValuesConverter` -Bool em quaisquer dois valores</span><span class="sxs-lookup"><span data-stu-id="95fa7-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="95fa7-134">`BytesToStringConverter` -Matriz de byte a cadeia de caracteres codificada em Base64</span><span class="sxs-lookup"><span data-stu-id="95fa7-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="95fa7-135">`CastingConverter` -Conversões que exigem apenas uma conversão de tipo</span><span class="sxs-lookup"><span data-stu-id="95fa7-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="95fa7-136">`CharToStringConverter` -Char para única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="95fa7-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset para o valor codificado por binário de 64 bits</span><span class="sxs-lookup"><span data-stu-id="95fa7-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="95fa7-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset para matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="95fa7-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="95fa7-139">`DateTimeOffsetToStringConverter` -DateTimeOffset em cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="95fa7-140">`DateTimeToBinaryConverter` -Data e hora para o valor de 64 bits, incluindo DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="95fa7-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="95fa7-141">`DateTimeToStringConverter` -DateTime como cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="95fa7-142">`DateTimeToTicksConverter` -Data e hora para tiques</span><span class="sxs-lookup"><span data-stu-id="95fa7-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="95fa7-143">`EnumToNumberConverter` -Enumeração para número subjacente</span><span class="sxs-lookup"><span data-stu-id="95fa7-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="95fa7-144">`EnumToStringConverter` -Enumeração a cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="95fa7-145">`GuidToBytesConverter` -Guid para a matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="95fa7-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="95fa7-146">`GuidToStringConverter` -Guid para a cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="95fa7-147">`NumberToBytesConverter` -Qualquer valor numérico para a matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="95fa7-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="95fa7-148">`NumberToStringConverter` -Qualquer valor numérico a cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="95fa7-149">`StringToBytesConverter` -Cadeia de caracteres para bytes UTF8</span><span class="sxs-lookup"><span data-stu-id="95fa7-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="95fa7-150">`TimeSpanToStringConverter` -TimeSpan para a cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="95fa7-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="95fa7-151">`TimeSpanToTicksConverter` -TimeSpan para tiques</span><span class="sxs-lookup"><span data-stu-id="95fa7-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="95fa7-152">Observe que `EnumToStringConverter` incluídos nessa lista.</span><span class="sxs-lookup"><span data-stu-id="95fa7-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="95fa7-153">Isso significa que não há necessidade de especificar a conversão explícita, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="95fa7-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="95fa7-154">Em vez disso, basta use o conversor interno:</span><span class="sxs-lookup"><span data-stu-id="95fa7-154">Instead, just use the built-in converter:</span></span>
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="95fa7-155">Observe que todos os conversores internos são sem monitoração de estado e portanto, uma única instância pode ser compartilhada com segurança por várias propriedades.</span><span class="sxs-lookup"><span data-stu-id="95fa7-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="95fa7-156">Conversões predefinidas</span><span class="sxs-lookup"><span data-stu-id="95fa7-156">Pre-defined conversions</span></span>

<span data-ttu-id="95fa7-157">Para conversões comuns para o qual exista um conversor de interno não é necessário especificar explicitamente o conversor.</span><span class="sxs-lookup"><span data-stu-id="95fa7-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="95fa7-158">Em vez disso, configure qual tipo de provedor deve ser usado e o EF usará automaticamente o conversor interno apropriado.</span><span class="sxs-lookup"><span data-stu-id="95fa7-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="95fa7-159">Enum para conversões de cadeia de caracteres são usados como um exemplo acima, mas o EF, na verdade, faz isso automaticamente se o tipo de provedor é configurado:</span><span class="sxs-lookup"><span data-stu-id="95fa7-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="95fa7-160">A mesma coisa pode ser obtida especificando explicitamente o tipo de coluna.</span><span class="sxs-lookup"><span data-stu-id="95fa7-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="95fa7-161">Por exemplo, se o tipo de entidade é definido como modo:</span><span class="sxs-lookup"><span data-stu-id="95fa7-161">For example, if the entity type is defined like so:</span></span>
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="95fa7-162">Em seguida, os valores de enumeração serão salvo como cadeias de caracteres no banco de dados sem qualquer configuração adicional no `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="95fa7-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="95fa7-163">Limitações</span><span class="sxs-lookup"><span data-stu-id="95fa7-163">Limitations</span></span>

<span data-ttu-id="95fa7-164">Há algumas limitações conhecidas de atuais do sistema de conversão do valor:</span><span class="sxs-lookup"><span data-stu-id="95fa7-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="95fa7-165">Conforme observado acima, `null` não pode ser convertido.</span><span class="sxs-lookup"><span data-stu-id="95fa7-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="95fa7-166">Atualmente, não há nenhuma maneira para distribuir uma conversão de uma propriedade em várias colunas ou vice-versa.</span><span class="sxs-lookup"><span data-stu-id="95fa7-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="95fa7-167">Uso de conversões de valor pode impactar a capacidade do EF Core para traduzir expressões para SQL.</span><span class="sxs-lookup"><span data-stu-id="95fa7-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="95fa7-168">Um aviso será registrado para esses casos.</span><span class="sxs-lookup"><span data-stu-id="95fa7-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="95fa7-169">A remoção dessas limitações está sendo considerada para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="95fa7-169">Removal of these limitations is being considered for a future release.</span></span>
