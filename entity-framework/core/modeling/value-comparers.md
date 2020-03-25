---
title: Comparadores de valor-EF Core
description: Usando os comparadores de valor para controlar como EF Core compara valores de propriedade
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 9dfed7b7ef8163f4f5c94a0c81c510807c53c13d
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148267"
---
# <a name="value-comparers"></a>Comparadores de valor

> [!NOTE]  
> Esse recurso é novo no EF Core 3,0.

> [!TIP]  
> O código deste documento pode ser encontrado no GitHub como um [exemplo executável](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).

## <a name="background"></a>Segundo plano

EF Core precisa comparar valores de propriedade quando:

* Determinando se uma propriedade foi alterada como parte da [detecção de alterações para atualizações](xref:core/saving/basic)
* Determinando se dois valores de chave são os mesmos ao resolver relações 

Isso é manipulado automaticamente para tipos primitivos comuns, como int, bool, DateTime, etc.

Para tipos mais complexos, as opções precisam ser feitas como fazer a comparação.
Por exemplo, uma matriz de bytes poderia ser comparada:

* Por referência, de modo que uma diferença seja detectada apenas se uma nova matriz de bytes for usada
* Por uma comparação profunda, essa mutação dos bytes na matriz é detectada

Por padrão, EF Core usa a primeira dessas abordagens para matrizes de byte não chave.
Ou seja, somente referências são comparadas e uma alteração é detectada somente quando uma matriz de bytes existente é substituída por uma nova.
Essa é uma decisão pragmática que evita uma comparação profunda de muitas matrizes de bytes grandes ao executar SaveChanges.
Mas o cenário comum de substituição, digamos, uma imagem com uma imagem diferente é manipulado de forma de desempenho.

Por outro lado, a igualdade de referência não funcionaria quando matrizes de bytes são usadas para representar chaves binárias.
É muito improvável que uma propriedade FK seja definida como a _mesma instância_ que uma propriedade CP para a qual ela precisa ser comparada.
Portanto, EF Core usa comparações profundas para matrizes de bytes que atuam como chaves.
Isso é improvável de ter um grande impacto de desempenho, pois as chaves binárias são geralmente curtas.

### <a name="snapshots"></a>Instantâneos

Comparações profundas em tipos mutáveis significa que EF Core precisa da capacidade de criar um "instantâneo" profundo do valor da propriedade.
Apenas copiar a referência, em vez disso, resultaria na mutação do valor atual e do instantâneo, já que eles são _o mesmo objeto_.
Portanto, quando comparações profundas são usadas em tipos mutáveis, o instantâneo profundo também é necessário.

## <a name="properties-with-value-converters"></a>Propriedades com conversores de valor

No caso acima, EF Core tem suporte de mapeamento nativo para matrizes de bytes e, portanto, pode escolher automaticamente os padrões apropriados.
No entanto, se a propriedade for mapeada por meio de um [conversor de valor](xref:core/modeling/value-conversions), EF Core nem sempre poderá determinar a comparação apropriada a ser usada.
Em vez disso, EF Core sempre usa a comparação de igualdade padrão definida pelo tipo da propriedade.
Isso geralmente está correto, mas talvez precise ser substituído ao mapear tipos mais complexos.

### <a name="simple-immutable-classes"></a>Classes imutáveis simples

Considere uma propriedade o usa um conversor de valor para mapear uma classe simples e imutável.

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

As propriedades desse tipo não precisam de comparações ou instantâneos especiais porque:
* A igualdade é substituída para que diferentes instâncias sejam comparadas corretamente
* O tipo é imutável, portanto, não há chance de mutar um valor de instantâneo

Nesse caso, o comportamento padrão de EF Core é bem como é.

### <a name="simple-immutable-structs"></a>Structs imutáveis simples

O mapeamento para structs simples também é simples e não requer comparadores especiais ou instantâneos.

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

EF Core tem suporte interno para gerar comparações memberwise compiladas de propriedades de struct.
Isso significa que as estruturas não precisam ter a igualdade substituída para o EF, mas você ainda pode optar por fazer isso por [outros motivos](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).
Além disso, o instantâneo especial não é necessário, pois structs é imutável e sempre são memberwise copiados de qualquer forma.
(Isso também é verdadeiro para structidades mutáveis, mas [structs mutáveis devem ser evitados em geral](/dotnet/csharp/write-safe-efficient-code).)

### <a name="mutable-classes"></a>Classes mutáveis

É recomendável que você use tipos imutáveis (classes ou structs) com conversores de valor quando possível.
Isso geralmente é mais eficiente e tem uma semântica mais limpa do que usar um tipo mutável.

No entanto, isso está sendo dito, é comum usar propriedades de tipos que o aplicativo não pode alterar.
Por exemplo, mapear uma propriedade que contém uma lista de números: 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

A [classe`List<T>`](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):
* Tem igualdade de referência; duas listas contendo os mesmos valores são tratadas como diferentes.
* É mutável; os valores na lista podem ser adicionados e removidos.

Uma conversão de valor comum em uma propriedade de lista pode converter a lista de e para JSON:

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

Isso requer a definição de um `ValueComparer<T>` na propriedade para forçar EF Core usar comparações corretas com essa conversão:

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> A API do construtor de modelos ("Fluent") para definir um comparador de valor ainda não foi implementada.
> Em vez disso, o código acima chama SetValueComparer no IMutableProperty de nível inferior exposto pelo construtor como ' Metadata '.

O construtor de `ValueComparer<T>` aceita três expressões:
* Uma expressão para verificar a qualidade
* Uma expressão para gerar um código hash
* Uma expressão para fazer o instantâneo de um valor  

Nesse caso, a comparação é feita verificando se as sequências de números são as mesmas.

Da mesma forma, o código hash é criado a partir dessa mesma sequência.
(Observe que esse é um código hash sobre valores mutáveis e, portanto, pode [causar problemas](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).
Seja imutável, em vez disso, se possível.)

O instantâneo é criado clonando a lista com ToList.
Novamente, isso só será necessário se as listas estiverem sendo modificadas.
Seja imutável, em vez disso, se você puder. 

> [!NOTE]  
> Conversores de valor e comparadores são construídos usando expressões em vez de delegados simples.
> Isso ocorre porque o EF insere essas expressões em uma árvore de expressão muito mais complexa que é então compilada em um delegado de forma de entidade.
> Conceitualmente, isso é semelhante ao de inalinhamento de compilador.
> Por exemplo, uma conversão simples pode ser apenas uma compilada em conversão, em vez de uma chamada para outro método para fazer a conversão.    

### <a name="key-comparers"></a>Principais comparadores

A seção de plano de fundo aborda por que as comparações de chave podem exigir semântica especial.
Certifique-se de criar um comparador apropriado para chaves ao defini-lo em uma propriedade primária, principal ou chave estrangeira.

Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) nos casos raros em que a semântica diferente é necessária na mesma propriedade.

> [!NOTE]  
> SetStructuralComparer foi obsoleto no EF Core 5,0.
> Use SetKeyValueComparer em vez disso.

### <a name="overriding-defaults"></a>Substituindo padrões

Às vezes, a comparação padrão usada pelo EF Core pode não ser apropriada.
Por exemplo, a mutação de matrizes de bytes não é, por padrão, detectada no EF Core.
Isso pode ser substituído definindo um comparador diferente na propriedade: 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

EF Core agora irá comparar sequências de bytes e, portanto, detectará mutações de matriz de bytes.
