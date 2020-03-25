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
# <a name="value-comparers"></a><span data-ttu-id="0ef8e-103">Comparadores de valor</span><span class="sxs-lookup"><span data-stu-id="0ef8e-103">Value Comparers</span></span>

> [!NOTE]  
> <span data-ttu-id="0ef8e-104">Esse recurso é novo no EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-104">This feature is new in EF Core 3.0.</span></span>

> [!TIP]  
> <span data-ttu-id="0ef8e-105">O código deste documento pode ser encontrado no GitHub como um [exemplo executável](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span><span class="sxs-lookup"><span data-stu-id="0ef8e-105">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="background"></a><span data-ttu-id="0ef8e-106">Segundo plano</span><span class="sxs-lookup"><span data-stu-id="0ef8e-106">Background</span></span>

<span data-ttu-id="0ef8e-107">EF Core precisa comparar valores de propriedade quando:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-107">EF Core needs to compare property values when:</span></span>

* <span data-ttu-id="0ef8e-108">Determinando se uma propriedade foi alterada como parte da [detecção de alterações para atualizações](xref:core/saving/basic)</span><span class="sxs-lookup"><span data-stu-id="0ef8e-108">Determining whether a property has been changed as part of [detecting changes for updates](xref:core/saving/basic)</span></span>
* <span data-ttu-id="0ef8e-109">Determinando se dois valores de chave são os mesmos ao resolver relações</span><span class="sxs-lookup"><span data-stu-id="0ef8e-109">Determining whether two key values are the same when resolving relationships</span></span> 

<span data-ttu-id="0ef8e-110">Isso é manipulado automaticamente para tipos primitivos comuns, como int, bool, DateTime, etc.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-110">This is handled automatically for common primitive types such as int, bool, DateTime, etc.</span></span>

<span data-ttu-id="0ef8e-111">Para tipos mais complexos, as opções precisam ser feitas como fazer a comparação.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-111">For more complex types, choices need to be made as to how to do the comparison.</span></span>
<span data-ttu-id="0ef8e-112">Por exemplo, uma matriz de bytes poderia ser comparada:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-112">For example, a byte array could be compared:</span></span>

* <span data-ttu-id="0ef8e-113">Por referência, de modo que uma diferença seja detectada apenas se uma nova matriz de bytes for usada</span><span class="sxs-lookup"><span data-stu-id="0ef8e-113">By reference, such that a difference is only detected if a new byte array is used</span></span>
* <span data-ttu-id="0ef8e-114">Por uma comparação profunda, essa mutação dos bytes na matriz é detectada</span><span class="sxs-lookup"><span data-stu-id="0ef8e-114">By deep comparison, such that mutation of the bytes in the array is detected</span></span>

<span data-ttu-id="0ef8e-115">Por padrão, EF Core usa a primeira dessas abordagens para matrizes de byte não chave.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-115">By default, EF Core uses the first of these approaches for non-key byte arrays.</span></span>
<span data-ttu-id="0ef8e-116">Ou seja, somente referências são comparadas e uma alteração é detectada somente quando uma matriz de bytes existente é substituída por uma nova.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-116">That is, only references are compared and a change is detected only when an existing byte array is replaced with a new one.</span></span>
<span data-ttu-id="0ef8e-117">Essa é uma decisão pragmática que evita uma comparação profunda de muitas matrizes de bytes grandes ao executar SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-117">This is a pragmatic decision that avoids deep comparison of many large byte arrays when executing SaveChanges.</span></span>
<span data-ttu-id="0ef8e-118">Mas o cenário comum de substituição, digamos, uma imagem com uma imagem diferente é manipulado de forma de desempenho.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-118">But the common scenario of replacing, say, an image with a different image is handled in a performant way.</span></span>

<span data-ttu-id="0ef8e-119">Por outro lado, a igualdade de referência não funcionaria quando matrizes de bytes são usadas para representar chaves binárias.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-119">On the other hand, reference equality would not work when byte arrays are used to represent binary keys.</span></span>
<span data-ttu-id="0ef8e-120">É muito improvável que uma propriedade FK seja definida como a _mesma instância_ que uma propriedade CP para a qual ela precisa ser comparada.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-120">It's very unlikely that an FK property is set to the _same instance_ as a PK property to which it needs to be compared.</span></span>
<span data-ttu-id="0ef8e-121">Portanto, EF Core usa comparações profundas para matrizes de bytes que atuam como chaves.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-121">Therefore, EF Core uses deep comparisons for byte arrays acting as keys.</span></span>
<span data-ttu-id="0ef8e-122">Isso é improvável de ter um grande impacto de desempenho, pois as chaves binárias são geralmente curtas.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-122">This is unlikely to have a big performance hit since binary keys are usually short.</span></span>

### <a name="snapshots"></a><span data-ttu-id="0ef8e-123">Instantâneos</span><span class="sxs-lookup"><span data-stu-id="0ef8e-123">Snapshots</span></span>

<span data-ttu-id="0ef8e-124">Comparações profundas em tipos mutáveis significa que EF Core precisa da capacidade de criar um "instantâneo" profundo do valor da propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-124">Deep comparisons on mutable types means that EF Core needs the ability to create a deep "snapshot" of the property value.</span></span>
<span data-ttu-id="0ef8e-125">Apenas copiar a referência, em vez disso, resultaria na mutação do valor atual e do instantâneo, já que eles são _o mesmo objeto_.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-125">Just copying the reference instead would result in mutating both the current value and the snapshot, since they are _the same object_.</span></span>
<span data-ttu-id="0ef8e-126">Portanto, quando comparações profundas são usadas em tipos mutáveis, o instantâneo profundo também é necessário.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-126">Therefore, when deep comparisons are used on mutable types, deep snapshotting is also required.</span></span>

## <a name="properties-with-value-converters"></a><span data-ttu-id="0ef8e-127">Propriedades com conversores de valor</span><span class="sxs-lookup"><span data-stu-id="0ef8e-127">Properties with value converters</span></span>

<span data-ttu-id="0ef8e-128">No caso acima, EF Core tem suporte de mapeamento nativo para matrizes de bytes e, portanto, pode escolher automaticamente os padrões apropriados.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-128">In the case above, EF Core has native mapping support for byte arrays and so can automatically choose appropriate defaults.</span></span>
<span data-ttu-id="0ef8e-129">No entanto, se a propriedade for mapeada por meio de um [conversor de valor](xref:core/modeling/value-conversions), EF Core nem sempre poderá determinar a comparação apropriada a ser usada.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-129">However, if the property is mapped through a [value converter](xref:core/modeling/value-conversions), then EF Core can't always determine the appropriate comparison to use.</span></span>
<span data-ttu-id="0ef8e-130">Em vez disso, EF Core sempre usa a comparação de igualdade padrão definida pelo tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-130">Instead, EF Core always uses the default equality comparison defined by the type of the property.</span></span>
<span data-ttu-id="0ef8e-131">Isso geralmente está correto, mas talvez precise ser substituído ao mapear tipos mais complexos.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-131">This is often correct, but may need to be overridden when mapping more complex types.</span></span>

### <a name="simple-immutable-classes"></a><span data-ttu-id="0ef8e-132">Classes imutáveis simples</span><span class="sxs-lookup"><span data-stu-id="0ef8e-132">Simple immutable classes</span></span>

<span data-ttu-id="0ef8e-133">Considere uma propriedade o usa um conversor de valor para mapear uma classe simples e imutável.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-133">Consider a property the uses a value converter to map a simple, immutable class.</span></span>

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

<span data-ttu-id="0ef8e-134">As propriedades desse tipo não precisam de comparações ou instantâneos especiais porque:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-134">Properties of this type do not need special comparisons or snapshots because:</span></span>
* <span data-ttu-id="0ef8e-135">A igualdade é substituída para que diferentes instâncias sejam comparadas corretamente</span><span class="sxs-lookup"><span data-stu-id="0ef8e-135">Equality is overridden so that different instances will compare correctly</span></span>
* <span data-ttu-id="0ef8e-136">O tipo é imutável, portanto, não há chance de mutar um valor de instantâneo</span><span class="sxs-lookup"><span data-stu-id="0ef8e-136">The type is immutable, so there is no chance of mutating a snapshot value</span></span>

<span data-ttu-id="0ef8e-137">Nesse caso, o comportamento padrão de EF Core é bem como é.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-137">So in this case the default behavior of EF Core is fine as it is.</span></span>

### <a name="simple-immutable-structs"></a><span data-ttu-id="0ef8e-138">Structs imutáveis simples</span><span class="sxs-lookup"><span data-stu-id="0ef8e-138">Simple immutable Structs</span></span>

<span data-ttu-id="0ef8e-139">O mapeamento para structs simples também é simples e não requer comparadores especiais ou instantâneos.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-139">The mapping for simple structs is also simple and requires no special comparers or snapshotting.</span></span>

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

<span data-ttu-id="0ef8e-140">EF Core tem suporte interno para gerar comparações memberwise compiladas de propriedades de struct.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-140">EF Core has built-in support for generating compiled, memberwise comparisons of struct properties.</span></span>
<span data-ttu-id="0ef8e-141">Isso significa que as estruturas não precisam ter a igualdade substituída para o EF, mas você ainda pode optar por fazer isso por [outros motivos](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span><span class="sxs-lookup"><span data-stu-id="0ef8e-141">This means structs don't need to have equality overridden for EF, but you may still choose to do this for [other reasons](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span></span>
<span data-ttu-id="0ef8e-142">Além disso, o instantâneo especial não é necessário, pois structs é imutável e sempre são memberwise copiados de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-142">Also, special snapshotting is not needed since structs immutable and are always memberwise copied anyway.</span></span>
<span data-ttu-id="0ef8e-143">(Isso também é verdadeiro para structidades mutáveis, mas [structs mutáveis devem ser evitados em geral](/dotnet/csharp/write-safe-efficient-code).)</span><span class="sxs-lookup"><span data-stu-id="0ef8e-143">(This is also true for mutable structs, but [mutable structs should in general be avoided](/dotnet/csharp/write-safe-efficient-code).)</span></span>

### <a name="mutable-classes"></a><span data-ttu-id="0ef8e-144">Classes mutáveis</span><span class="sxs-lookup"><span data-stu-id="0ef8e-144">Mutable classes</span></span>

<span data-ttu-id="0ef8e-145">É recomendável que você use tipos imutáveis (classes ou structs) com conversores de valor quando possível.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-145">It is recommended that you use immutable types (classes or structs) with value converters when possible.</span></span>
<span data-ttu-id="0ef8e-146">Isso geralmente é mais eficiente e tem uma semântica mais limpa do que usar um tipo mutável.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-146">This is usually more efficient and has cleaner semantics than using a mutable type.</span></span>

<span data-ttu-id="0ef8e-147">No entanto, isso está sendo dito, é comum usar propriedades de tipos que o aplicativo não pode alterar.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-147">However, that being said, it is common to use properties of types that the application cannot change.</span></span>
<span data-ttu-id="0ef8e-148">Por exemplo, mapear uma propriedade que contém uma lista de números:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-148">For example, mapping a property containing a list of numbers:</span></span> 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

<span data-ttu-id="0ef8e-149">A [classe`List<T>`](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span><span class="sxs-lookup"><span data-stu-id="0ef8e-149">The [`List<T>` class](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span></span>
* <span data-ttu-id="0ef8e-150">Tem igualdade de referência; duas listas contendo os mesmos valores são tratadas como diferentes.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-150">Has reference equality; two lists containing the same values are treated as different.</span></span>
* <span data-ttu-id="0ef8e-151">É mutável; os valores na lista podem ser adicionados e removidos.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-151">Is mutable; values in the list can be added and removed.</span></span>

<span data-ttu-id="0ef8e-152">Uma conversão de valor comum em uma propriedade de lista pode converter a lista de e para JSON:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-152">A typical value conversion on a list property might convert the list to and from JSON:</span></span>

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

<span data-ttu-id="0ef8e-153">Isso requer a definição de um `ValueComparer<T>` na propriedade para forçar EF Core usar comparações corretas com essa conversão:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-153">This then requires setting a `ValueComparer<T>` on the property to force EF Core use correct comparisons with this conversion:</span></span>

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> <span data-ttu-id="0ef8e-154">A API do construtor de modelos ("Fluent") para definir um comparador de valor ainda não foi implementada.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-154">The model builder ("fluent") API to set a value comparer has not yet been implemented.</span></span>
> <span data-ttu-id="0ef8e-155">Em vez disso, o código acima chama SetValueComparer no IMutableProperty de nível inferior exposto pelo construtor como ' Metadata '.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-155">Instead, the code above calls SetValueComparer on the lower-level IMutableProperty exposed by the builder as 'Metadata'.</span></span>

<span data-ttu-id="0ef8e-156">O construtor de `ValueComparer<T>` aceita três expressões:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-156">The `ValueComparer<T>` constructor accepts three expressions:</span></span>
* <span data-ttu-id="0ef8e-157">Uma expressão para verificar a qualidade</span><span class="sxs-lookup"><span data-stu-id="0ef8e-157">An expression for checking quality</span></span>
* <span data-ttu-id="0ef8e-158">Uma expressão para gerar um código hash</span><span class="sxs-lookup"><span data-stu-id="0ef8e-158">An expression for generating a hash code</span></span>
* <span data-ttu-id="0ef8e-159">Uma expressão para fazer o instantâneo de um valor</span><span class="sxs-lookup"><span data-stu-id="0ef8e-159">An expression to snapshot a value</span></span>  

<span data-ttu-id="0ef8e-160">Nesse caso, a comparação é feita verificando se as sequências de números são as mesmas.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-160">In this case the comparison is done by checking if the sequences of numbers are the same.</span></span>

<span data-ttu-id="0ef8e-161">Da mesma forma, o código hash é criado a partir dessa mesma sequência.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-161">Likewise, the hash code is built from this same sequence.</span></span>
<span data-ttu-id="0ef8e-162">(Observe que esse é um código hash sobre valores mutáveis e, portanto, pode [causar problemas](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span><span class="sxs-lookup"><span data-stu-id="0ef8e-162">(Note that this is a hash code over mutable values and hence can [cause problems](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span></span>
<span data-ttu-id="0ef8e-163">Seja imutável, em vez disso, se possível.)</span><span class="sxs-lookup"><span data-stu-id="0ef8e-163">Be immutable instead if you can.)</span></span>

<span data-ttu-id="0ef8e-164">O instantâneo é criado clonando a lista com ToList.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-164">The snapshot is created by cloning the list with ToList.</span></span>
<span data-ttu-id="0ef8e-165">Novamente, isso só será necessário se as listas estiverem sendo modificadas.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-165">Again, this is only needed if the lists are going to be mutated.</span></span>
<span data-ttu-id="0ef8e-166">Seja imutável, em vez disso, se você puder.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-166">Be immutable instead if you can.</span></span> 

> [!NOTE]  
> <span data-ttu-id="0ef8e-167">Conversores de valor e comparadores são construídos usando expressões em vez de delegados simples.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-167">Value converters and comparers are constructed using expressions rather than simple delegates.</span></span>
> <span data-ttu-id="0ef8e-168">Isso ocorre porque o EF insere essas expressões em uma árvore de expressão muito mais complexa que é então compilada em um delegado de forma de entidade.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-168">This is because EF inserts these expressions into a much more complex expression tree that is then compiled into an entity shaper delegate.</span></span>
> <span data-ttu-id="0ef8e-169">Conceitualmente, isso é semelhante ao de inalinhamento de compilador.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-169">Conceptually, this is similar to compiler inlining.</span></span>
> <span data-ttu-id="0ef8e-170">Por exemplo, uma conversão simples pode ser apenas uma compilada em conversão, em vez de uma chamada para outro método para fazer a conversão.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-170">For example, a simple conversion may just be a compiled in cast, rather than a call to another method to do the conversion.</span></span>    

### <a name="key-comparers"></a><span data-ttu-id="0ef8e-171">Principais comparadores</span><span class="sxs-lookup"><span data-stu-id="0ef8e-171">Key comparers</span></span>

<span data-ttu-id="0ef8e-172">A seção de plano de fundo aborda por que as comparações de chave podem exigir semântica especial.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-172">The background section covers why key comparisons may require special semantics.</span></span>
<span data-ttu-id="0ef8e-173">Certifique-se de criar um comparador apropriado para chaves ao defini-lo em uma propriedade primária, principal ou chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-173">Make sure to create a comparer that is appropriate for keys when setting it on a primary, principal, or foreign key property.</span></span>

<span data-ttu-id="0ef8e-174">Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) nos casos raros em que a semântica diferente é necessária na mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-174">Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) in the rare cases where different semantics is required on the same property.</span></span>

> [!NOTE]  
> <span data-ttu-id="0ef8e-175">SetStructuralComparer foi obsoleto no EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-175">SetStructuralComparer has been obsoleted in EF Core 5.0.</span></span>
> <span data-ttu-id="0ef8e-176">Use SetKeyValueComparer em vez disso.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-176">Use SetKeyValueComparer instead.</span></span>

### <a name="overriding-defaults"></a><span data-ttu-id="0ef8e-177">Substituindo padrões</span><span class="sxs-lookup"><span data-stu-id="0ef8e-177">Overriding defaults</span></span>

<span data-ttu-id="0ef8e-178">Às vezes, a comparação padrão usada pelo EF Core pode não ser apropriada.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-178">Sometimes the default comparison used by EF Core may not be appropriate.</span></span>
<span data-ttu-id="0ef8e-179">Por exemplo, a mutação de matrizes de bytes não é, por padrão, detectada no EF Core.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-179">For example, mutation of byte arrays is not, by default, detected in EF Core.</span></span>
<span data-ttu-id="0ef8e-180">Isso pode ser substituído definindo um comparador diferente na propriedade:</span><span class="sxs-lookup"><span data-stu-id="0ef8e-180">This can be overridden by setting a different comparer on the property:</span></span> 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

<span data-ttu-id="0ef8e-181">EF Core agora irá comparar sequências de bytes e, portanto, detectará mutações de matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="0ef8e-181">EF Core will now compare byte sequences and will therefore detect byte array mutations.</span></span>
