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
# <a name="value-conversions"></a>Conversões de valor

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

Os conversores de valor permitem que os valores de propriedade sejam convertidos ao ler ou gravar no banco de dados. Essa conversão pode ser de um valor para outro do mesmo tipo (por exemplo, criptografia de cadeias de caracteres) ou de um valor de um tipo para um valor de outro tipo (por exemplo, converter valores de enumeração de e para cadeias de caracteres no banco de dados).

## <a name="fundamentals"></a>Princípios básicos

Os conversores de valor são especificados em termos de um `ModelClrType` e um `ProviderClrType`. O tipo de modelo é o tipo .NET da propriedade no tipo de entidade. O tipo de provedor é o tipo .NET compreendido pelo provedor de banco de dados. Por exemplo, para salvar enums como cadeias de caracteres no banco de dados, o tipo de modelo é o tipo de enum e o tipo de provedor é `String`. Esses dois tipos podem ser iguais.

As conversões são definidas usando duas árvores de expressão de `Func`: uma de `ModelClrType` para `ProviderClrType` e a outra de `ProviderClrType` para `ModelClrType`. As árvores de expressão são usadas para que possam ser compiladas no código de acesso ao banco de dados para conversões eficientes. Para conversões complexas, a árvore de expressão pode ser uma chamada simples para um método que executa a conversão.

## <a name="configuring-a-value-converter"></a>Configurando um conversor de valor

Conversões de valor são definidas nas propriedades no `OnModelCreating` do seu `DbContext`. Por exemplo, considere uma enumeração e um tipo de entidade definidos como:

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

Em seguida, as conversões podem ser definidas no `OnModelCreating` para armazenar os valores de enumeração como cadeias de caracteres (por exemplo, "Donkey", "Mule",...) no banco de dados:

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
> Um valor `null` nunca será passado para um conversor de valor. Isso torna a implementação de conversões mais fácil e permite que elas sejam compartilhadas entre Propriedades anuláveis e não anuláveis.

## <a name="the-valueconverter-class"></a>A classe ValueConverter

Chamar `HasConversion`, conforme mostrado acima, criará uma instância de `ValueConverter` e a definirá na propriedade. Em vez disso, o `ValueConverter` pode ser criado explicitamente. Por exemplo:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Isso pode ser útil quando várias propriedades usam a mesma conversão.

> [!NOTE]  
> Atualmente, não há nenhuma maneira de especificar em um lugar que cada propriedade de um determinado tipo deve usar o mesmo conversor de valor. Esse recurso será considerado para uma versão futura.

## <a name="built-in-converters"></a>Conversores internos

O EF Core é fornecido com um conjunto de classes de `ValueConverter` predefinidas, encontrado no namespace `Microsoft.EntityFrameworkCore.Storage.ValueConversion`. Elas são:

* `BoolToZeroOneConverter`-bool para zero e um
* `BoolToStringConverter`-bool para cadeias de caracteres como "Y" e "N"
* `BoolToTwoValuesConverter`-bool para quaisquer dois valores
* matriz de `BytesToStringConverter` bytes para cadeia de caracteres codificada em base64
* `CastingConverter`-conversões que exigem apenas uma conversão de tipo
* `CharToStringConverter`-char para cadeia de caracteres única
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset para o valor de 64 bits codificado em binário
* `DateTimeOffsetToBytesConverter`-DateTimeOffset para matriz de bytes
* `DateTimeOffsetToStringConverter`-DateTimeOffset para cadeia de caracteres
* `DateTimeToBinaryConverter`-DateTime para o valor de 64 bits, incluindo DateTimeKind
* `DateTimeToStringConverter`-DateTime para cadeia de caracteres
* `DateTimeToTicksConverter`-DateTime para tiques
* `EnumToNumberConverter`-enum para o número subjacente
* `EnumToStringConverter`-enumeração para cadeia de caracteres
* `GuidToBytesConverter`-GUID para matriz de bytes
* `GuidToStringConverter` GUID para cadeia de caracteres
* `NumberToBytesConverter`-qualquer valor numérico para a matriz de bytes
* `NumberToStringConverter`-qualquer valor numérico para cadeia de caracteres
* Cadeia de caracteres `StringToBytesConverter` a UTF8 bytes
* `TimeSpanToStringConverter`-TimeSpan para cadeia de caracteres
* `TimeSpanToTicksConverter`-TimeSpan para tiques

Observe que `EnumToStringConverter` está incluído nessa lista. Isso significa que não há necessidade de especificar a conversão explicitamente, como mostrado acima. Em vez disso, basta usar o conversor interno:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Observe que todos os conversores internos são sem estado e, portanto, uma única instância pode ser compartilhada com segurança por várias propriedades.

## <a name="pre-defined-conversions"></a>Conversões predefinidas

Para conversões comuns para as quais existe um conversor interno, não é necessário especificar o conversor explicitamente. Em vez disso, basta configurar qual tipo de provedor deve ser usado e o EF usará automaticamente o conversor interno apropriado. Enum para conversões de cadeias de caracteres são usadas como um exemplo acima, mas o EF fará isso automaticamente se o tipo de provedor estiver configurado:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

A mesma coisa pode ser obtida especificando explicitamente o tipo de coluna. Por exemplo, se o tipo de entidade for definido da seguinte forma:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Em seguida, os valores de enumeração serão salvos como cadeias de caracteres no banco de dados sem nenhuma configuração adicional no `OnModelCreating`.

## <a name="limitations"></a>Limitações

Há algumas limitações atuais conhecidas do sistema de conversão de valor:

* Conforme observado acima, `null` não pode ser convertido.
* Atualmente, não há nenhuma maneira de distribuir uma conversão de uma propriedade para várias colunas ou vice-versa.
* O uso de conversões de valor pode afetar a capacidade de EF Core de converter expressões para SQL. Um aviso será registrado em log para esses casos.
A remoção dessas limitações está sendo considerada para uma versão futura.
