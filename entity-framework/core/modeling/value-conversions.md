---
title: Conversões de valor-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654794"
---
# <a name="value-conversions"></a><span data-ttu-id="5f3fa-102">Conversões de valor</span><span class="sxs-lookup"><span data-stu-id="5f3fa-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="5f3fa-103">Este recurso é novo no EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="5f3fa-104">Os conversores de valor permitem que os valores de propriedade sejam convertidos ao ler ou gravar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="5f3fa-105">Essa conversão pode ser de um valor para outro do mesmo tipo (por exemplo, criptografia de cadeias de caracteres) ou de um valor de um tipo para um valor de outro tipo (por exemplo, converter valores de enumeração de e para cadeias de caracteres no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="5f3fa-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="5f3fa-106">Princípios básicos</span><span class="sxs-lookup"><span data-stu-id="5f3fa-106">Fundamentals</span></span>

<span data-ttu-id="5f3fa-107">Os conversores de valor são especificados em termos de um `ModelClrType` e um `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="5f3fa-108">O tipo de modelo é o tipo .NET da propriedade no tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="5f3fa-109">O tipo de provedor é o tipo .NET compreendido pelo provedor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="5f3fa-110">Por exemplo, para salvar enums como cadeias de caracteres no banco de dados, o tipo de modelo é o tipo de enum e o tipo de provedor é `String`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="5f3fa-111">Esses dois tipos podem ser iguais.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-111">These two types can be the same.</span></span>

<span data-ttu-id="5f3fa-112">As conversões são definidas usando duas árvores de expressão de `Func`: uma de `ModelClrType` para `ProviderClrType` e a outra de `ProviderClrType` para `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="5f3fa-113">As árvores de expressão são usadas para que possam ser compiladas no código de acesso ao banco de dados para conversões eficientes.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="5f3fa-114">Para conversões complexas, a árvore de expressão pode ser uma chamada simples para um método que executa a conversão.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="5f3fa-115">Configurando um conversor de valor</span><span class="sxs-lookup"><span data-stu-id="5f3fa-115">Configuring a value converter</span></span>

<span data-ttu-id="5f3fa-116">Conversões de valor são definidas nas propriedades no `OnModelCreating` do seu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="5f3fa-117">Por exemplo, considere uma enumeração e um tipo de entidade definidos como:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-117">For example, consider an enum and entity type defined as:</span></span>

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

<span data-ttu-id="5f3fa-118">Em seguida, as conversões podem ser definidas no `OnModelCreating` para armazenar os valores de enumeração como cadeias de caracteres (por exemplo, "Donkey", "Mule",...) no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

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
> <span data-ttu-id="5f3fa-119">Um valor `null` nunca será passado para um conversor de valor.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="5f3fa-120">Isso torna a implementação de conversões mais fácil e permite que elas sejam compartilhadas entre Propriedades anuláveis e não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="5f3fa-121">A classe ValueConverter</span><span class="sxs-lookup"><span data-stu-id="5f3fa-121">The ValueConverter class</span></span>

<span data-ttu-id="5f3fa-122">Chamar `HasConversion`, conforme mostrado acima, criará uma instância de `ValueConverter` e a definirá na propriedade.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="5f3fa-123">Em vez disso, o `ValueConverter` pode ser criado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="5f3fa-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="5f3fa-125">Isso pode ser útil quando várias propriedades usam a mesma conversão.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="5f3fa-126">Atualmente, não há nenhuma maneira de especificar em um lugar que cada propriedade de um determinado tipo deve usar o mesmo conversor de valor.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="5f3fa-127">Esse recurso será considerado para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="5f3fa-128">Conversores internos</span><span class="sxs-lookup"><span data-stu-id="5f3fa-128">Built-in converters</span></span>

<span data-ttu-id="5f3fa-129">O EF Core é fornecido com um conjunto de classes de `ValueConverter` predefinidas, encontrado no namespace `Microsoft.EntityFrameworkCore.Storage.ValueConversion`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="5f3fa-130">Elas são:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-130">These are:</span></span>

* <span data-ttu-id="5f3fa-131">`BoolToZeroOneConverter`-bool para zero e um</span><span class="sxs-lookup"><span data-stu-id="5f3fa-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="5f3fa-132">`BoolToStringConverter`-bool para cadeias de caracteres como "Y" e "N"</span><span class="sxs-lookup"><span data-stu-id="5f3fa-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="5f3fa-133">`BoolToTwoValuesConverter`-bool para quaisquer dois valores</span><span class="sxs-lookup"><span data-stu-id="5f3fa-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="5f3fa-134">matriz de `BytesToStringConverter` bytes para cadeia de caracteres codificada em base64</span><span class="sxs-lookup"><span data-stu-id="5f3fa-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="5f3fa-135">`CastingConverter`-conversões que exigem apenas uma conversão de tipo</span><span class="sxs-lookup"><span data-stu-id="5f3fa-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="5f3fa-136">`CharToStringConverter`-char para cadeia de caracteres única</span><span class="sxs-lookup"><span data-stu-id="5f3fa-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="5f3fa-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset para o valor de 64 bits codificado em binário</span><span class="sxs-lookup"><span data-stu-id="5f3fa-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="5f3fa-138">`DateTimeOffsetToBytesConverter`-DateTimeOffset para matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="5f3fa-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="5f3fa-139">`DateTimeOffsetToStringConverter`-DateTimeOffset para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="5f3fa-140">`DateTimeToBinaryConverter`-DateTime para o valor de 64 bits, incluindo DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="5f3fa-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="5f3fa-141">`DateTimeToStringConverter`-DateTime para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="5f3fa-142">`DateTimeToTicksConverter`-DateTime para tiques</span><span class="sxs-lookup"><span data-stu-id="5f3fa-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="5f3fa-143">`EnumToNumberConverter`-enum para o número subjacente</span><span class="sxs-lookup"><span data-stu-id="5f3fa-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="5f3fa-144">`EnumToStringConverter`-enumeração para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="5f3fa-145">`GuidToBytesConverter`-GUID para matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="5f3fa-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="5f3fa-146">`GuidToStringConverter` GUID para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="5f3fa-147">`NumberToBytesConverter`-qualquer valor numérico para a matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="5f3fa-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="5f3fa-148">`NumberToStringConverter`-qualquer valor numérico para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="5f3fa-149">Cadeia de caracteres `StringToBytesConverter` a UTF8 bytes</span><span class="sxs-lookup"><span data-stu-id="5f3fa-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="5f3fa-150">`TimeSpanToStringConverter`-TimeSpan para cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5f3fa-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="5f3fa-151">`TimeSpanToTicksConverter`-TimeSpan para tiques</span><span class="sxs-lookup"><span data-stu-id="5f3fa-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="5f3fa-152">Observe que `EnumToStringConverter` está incluído nessa lista.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="5f3fa-153">Isso significa que não há necessidade de especificar a conversão explicitamente, como mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="5f3fa-154">Em vez disso, basta usar o conversor interno:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="5f3fa-155">Observe que todos os conversores internos são sem estado e, portanto, uma única instância pode ser compartilhada com segurança por várias propriedades.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="5f3fa-156">Conversões predefinidas</span><span class="sxs-lookup"><span data-stu-id="5f3fa-156">Pre-defined conversions</span></span>

<span data-ttu-id="5f3fa-157">Para conversões comuns para as quais existe um conversor interno, não é necessário especificar o conversor explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="5f3fa-158">Em vez disso, basta configurar qual tipo de provedor deve ser usado e o EF usará automaticamente o conversor interno apropriado.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="5f3fa-159">Enum para conversões de cadeias de caracteres são usadas como um exemplo acima, mas o EF fará isso automaticamente se o tipo de provedor estiver configurado:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="5f3fa-160">A mesma coisa pode ser obtida especificando explicitamente o tipo de coluna.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="5f3fa-161">Por exemplo, se o tipo de entidade for definido da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="5f3fa-162">Em seguida, os valores de enumeração serão salvos como cadeias de caracteres no banco de dados sem nenhuma configuração adicional no `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="5f3fa-163">Limitações</span><span class="sxs-lookup"><span data-stu-id="5f3fa-163">Limitations</span></span>

<span data-ttu-id="5f3fa-164">Há algumas limitações atuais conhecidas do sistema de conversão de valor:</span><span class="sxs-lookup"><span data-stu-id="5f3fa-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="5f3fa-165">Conforme observado acima, `null` não pode ser convertido.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="5f3fa-166">Atualmente, não há nenhuma maneira de distribuir uma conversão de uma propriedade para várias colunas ou vice-versa.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="5f3fa-167">O uso de conversões de valor pode afetar a capacidade de EF Core de converter expressões para SQL.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="5f3fa-168">Um aviso será registrado em log para esses casos.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="5f3fa-169">A remoção dessas limitações está sendo considerada para uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="5f3fa-169">Removal of these limitations is being considered for a future release.</span></span>
