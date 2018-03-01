---
title: "Conversões de valor - Core EF"
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 50acba39cdec16caa9300fcaf47ab6242a4f69fb
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="value-conversions"></a>Conversões de valor

> [!NOTE]  
> Esse recurso é novo no EF Core 2.1.

Conversores de valor permitem que os valores de propriedade a ser convertido durante a leitura ou gravação no banco de dados. Esta conversão pode ser de um valor para outra do mesmo tipo (por exemplo, cadeias de caracteres com criptografia) ou de um valor de um tipo para um valor de outro tipo (por exemplo, converter valores enum para e de cadeias de caracteres no banco de dados.)

## <a name="fundamentals"></a>Princípios básicos

Conversores de valor são especificados em termos de um `ModelClrType` e um `ProviderClrType`. O tipo de modelo é o tipo de .NET da propriedade no tipo de entidade. O tipo de provedor é o tipo de .NET entendido pelo provedor de banco de dados. Por exemplo, para salvar enums como cadeias de caracteres no banco de dados, o tipo de modelo é o tipo de enum e o tipo de provedor é `String`. Esses dois tipos podem ser os mesmos.

Conversões são definidas usando dois `Func` árvores de expressão: um do `ModelClrType` para `ProviderClrType` e outro de `ProviderClrType` para `ModelClrType`. Árvores de expressão são usadas para que eles podem ser compilados em código de acesso a banco de dados para conversões eficientes. Para conversões complexas, a árvore de expressão pode ser uma chamada simples para um método que executa a conversão.

## <a name="configuring-a-value-converter"></a>Configurando um conversor de valor

Conversões de valor são definidas nas propriedades de OnModelCreating de seu DbContext. Por exemplo, considere um tipo enum e entidade definido como:
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
Em seguida, conversões podem ser definidas em OnModelCreating para armazenar os valores de enum como cadeias de caracteres (por exemplo "Burro", "outras" mulas","...) no banco de dados:
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
> Um `null` valor nunca será passado para um conversor de valor. Isso facilita a implementação de conversões e permite que seja compartilhada entre propriedades anuláveis e não anulável.

## <a name="the-valueconverter-class"></a>A classe ValueConverter

Chamando `HasConversion` como mostrado acima, criará um `ValueConverter` de instância e defina-o na propriedade. O `ValueConverter` em vez disso, podem ser criadas explicitamente. Por exemplo:
```Csharp
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
> Atualmente, não há nenhuma maneira de especificar em um local que todas as propriedades de um determinado tipo devem usar o mesmo conversor de valor. Este recurso será considerado para uma versão futura.

## <a name="built-in-converters"></a>Conversores internos

EF Core é fornecido com um conjunto de predefinidas `ValueConverter` classes, encontradas no `Microsoft.EntityFrameworkCore.Storage.Converters` namespace. Elas são:
* `BoolToZeroOneConverter` -Bool em zero e um
* `BoolToStringConverter` -Bool em cadeias de caracteres como "Y" e "N"
* `BoolToTwoValuesConverter` -Bool em quaisquer dois valores
* `BytesToStringConverter` -Matriz de byte a cadeia de caracteres codificada em Base64
* `CastingConverter` -Conversões que exigem uma conversão de Csharp
* `CharToStringConverter` -Char única cadeia de caracteres
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset valor binário codificado de 64 bits
* `DateTimeOffsetToBytesConverter` -DateTimeOffset para matriz de bytes
* `DateTimeOffsetToStringConverter` -DateTimeOffset em cadeia de caracteres
* `DateTimeToBinaryConverter` -Data e hora para o valor de 64 bits, incluindo DateTimeKind
* `DateTimeToStringConverter` -DateTime para string
* `DateTimeToTicksConverter` -Data e hora em tiques
* `EnumToNumberConverter` -Enumeração número subjacente
* `EnumToStringConverter` -Enum a cadeia de caracteres
* `GuidToBytesConverter` -Guid para a matriz de bytes
* `GuidToStringConverter` -Guid para a cadeia de caracteres
* `NumberToBytesConverter` -Qualquer valor numérico para a matriz de bytes
* `NumberToStringConverter` -Qualquer valor numérico para a cadeia de caracteres
* `StringToBytesConverter` -Cadeia de caracteres de bytes UTF8
* `TimeSpanToStringConverter` -TimeSpan para a cadeia de caracteres
* `TimeSpanToTicksConverter` -TimeSpan para escalas

Observe que `EnumToStringConverter` está incluído nesta lista. Isso significa que não há necessidade de especificar a conversão explícita, como mostrado acima. Em vez disso, use o conversor interno:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Observe que todos os conversores internos são sem monitoração de estado e assim por uma única instância pode ser compartilhada com segurança por várias propriedades.

## <a name="pre-defined-conversions"></a>Conversões predefinidos

Para conversões comuns para o qual exista um conversor interno não é necessário especificar o conversor explicitamente. Em vez disso, configure o tipo de provedor deve ser usado e EF usará automaticamente o conversor de compilação apropriado. Enumeração para conversões de cadeia de caracteres são usados como um exemplo acima, mas EF realmente faz isso automaticamente se o tipo de provedor é configurado:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
A mesma coisa pode ser obtida especificando explicitamente o tipo de coluna. Por exemplo, se o tipo de entidade é definido como modo:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Em seguida, os valores de enumeração serão salvo como cadeias de caracteres no banco de dados sem qualquer configuração adicional no OnModelCreating.

## <a name="limitations"></a>Limitações

Há algumas limitações conhecidas de atuais do sistema de conversão do valor:
* Conforme observado acima, `null` não pode ser convertido.
* Atualmente, não há nenhuma maneira de distribuir uma conversão de uma propriedade multuple colunas ou vice-versa.
* Uso de conversões de valor pode afetar a capacidade do núcleo do EF para traduzir expressões SQL. Um aviso será registrado para esses casos.
A remoção dessas limitações está sendo considerada para uma versão futura.
