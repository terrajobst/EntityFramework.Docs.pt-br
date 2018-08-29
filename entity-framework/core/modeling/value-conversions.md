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
# <a name="value-conversions"></a>Conversões de valor

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

Conversores de valor permitem que os valores de propriedade a ser convertido durante a leitura ou gravação no banco de dados. Essa conversão pode ser de um valor para outra do mesmo tipo (por exemplo, cadeias de caracteres com criptografia) ou de um valor de um tipo para um valor de outro tipo (por exemplo, converter enum valores em cadeias de caracteres no banco de dados).

## <a name="fundamentals"></a>Princípios básicos

Conversores de valor são especificados em termos de um `ModelClrType` e um `ProviderClrType`. O tipo de modelo é o tipo de .NET da propriedade no tipo de entidade. O tipo de provedor é o tipo de .NET entendido pelo provedor de banco de dados. Por exemplo, para salvar as enumerações como cadeias de caracteres no banco de dados, o tipo de modelo é o tipo de enumeração, e o tipo de provedor é `String`. Esses dois tipos podem ser o mesmo.

As conversões são definidas usando duas `Func` árvores de expressão: um da `ModelClrType` para `ProviderClrType` e o outro da `ProviderClrType` para `ModelClrType`. Árvores de expressão são usadas para que eles podem ser compilados em código de acesso a banco de dados para conversões eficientes. Para conversões complexas, a árvore de expressão pode ser uma chamada simples para um método que executa a conversão.

## <a name="configuring-a-value-converter"></a>Configurando um conversor de valor

Conversões de valor são definidas nas propriedades na `OnModelCreating` de seu `DbContext`. Por exemplo, considere um tipo enum e a entidade definido como:
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
Em seguida, as conversões podem ser definidas em `OnModelCreating` para armazenar os valores de enum como cadeias de caracteres (por exemplo, "Burro", "outras" mulas "",...) no banco de dados:
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
> Um `null` valor nunca será passado para um conversor de valor. Isso facilita a implementação de conversões e lhes permite ser compartilhado entre as propriedades que permitem valor nulas e não anuláveis.

## <a name="the-valueconverter-class"></a>A classe ValueConverter

Chamando `HasConversion` conforme mostrado acima criará um `ValueConverter` da instância e defini-lo na propriedade. O `ValueConverter` em vez disso, podem ser criadas explicitamente. Por exemplo:
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
> Atualmente, não há nenhuma maneira de especificar em um só lugar que todas as propriedades de um determinado tipo devem usar o mesmo conversor de valor. Esse recurso será considerado para uma versão futura.

## <a name="built-in-converters"></a>Conversores internos

EF Core envia com um conjunto de predefinidos `ValueConverter` classes, encontradas no `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace. Elas são:
* `BoolToZeroOneConverter` -Bool para zero e um
* `BoolToStringConverter` -Bool em cadeias de caracteres como "Y" e "N"
* `BoolToTwoValuesConverter` -Bool em quaisquer dois valores
* `BytesToStringConverter` -Matriz de byte a cadeia de caracteres codificada em Base64
* `CastingConverter` -Conversões que exigem apenas uma conversão de tipo
* `CharToStringConverter` -Char para única cadeia de caracteres
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset para o valor codificado por binário de 64 bits
* `DateTimeOffsetToBytesConverter` -DateTimeOffset para matriz de bytes
* `DateTimeOffsetToStringConverter` -DateTimeOffset em cadeia de caracteres
* `DateTimeToBinaryConverter` -Data e hora para o valor de 64 bits, incluindo DateTimeKind
* `DateTimeToStringConverter` -DateTime como cadeia de caracteres
* `DateTimeToTicksConverter` -Data e hora para tiques
* `EnumToNumberConverter` -Enumeração para número subjacente
* `EnumToStringConverter` -Enumeração a cadeia de caracteres
* `GuidToBytesConverter` -Guid para a matriz de bytes
* `GuidToStringConverter` -Guid para a cadeia de caracteres
* `NumberToBytesConverter` -Qualquer valor numérico para a matriz de bytes
* `NumberToStringConverter` -Qualquer valor numérico a cadeia de caracteres
* `StringToBytesConverter` -Cadeia de caracteres para bytes UTF8
* `TimeSpanToStringConverter` -TimeSpan para a cadeia de caracteres
* `TimeSpanToTicksConverter` -TimeSpan para tiques

Observe que `EnumToStringConverter` incluídos nessa lista. Isso significa que não há necessidade de especificar a conversão explícita, conforme mostrado acima. Em vez disso, basta use o conversor interno:
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Observe que todos os conversores internos são sem monitoração de estado e portanto, uma única instância pode ser compartilhada com segurança por várias propriedades.

## <a name="pre-defined-conversions"></a>Conversões predefinidas

Para conversões comuns para o qual exista um conversor de interno não é necessário especificar explicitamente o conversor. Em vez disso, configure qual tipo de provedor deve ser usado e o EF usará automaticamente o conversor interno apropriado. Enum para conversões de cadeia de caracteres são usados como um exemplo acima, mas o EF, na verdade, faz isso automaticamente se o tipo de provedor é configurado:
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
A mesma coisa pode ser obtida especificando explicitamente o tipo de coluna. Por exemplo, se o tipo de entidade é definido como modo:
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Em seguida, os valores de enumeração serão salvo como cadeias de caracteres no banco de dados sem qualquer configuração adicional no `OnModelCreating`.

## <a name="limitations"></a>Limitações

Há algumas limitações conhecidas de atuais do sistema de conversão do valor:
* Conforme observado acima, `null` não pode ser convertido.
* Atualmente, não há nenhuma maneira para distribuir uma conversão de uma propriedade em várias colunas ou vice-versa.
* Uso de conversões de valor pode impactar a capacidade do EF Core para traduzir expressões para SQL. Um aviso será registrado para esses casos.
A remoção dessas limitações está sendo considerada para uma versão futura.
