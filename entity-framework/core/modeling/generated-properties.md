---
title: Valores gerados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250693"
---
# <a name="generated-values"></a>Valores gerados

## <a name="value-generation-patterns"></a>Padrões de geração de valor

Existem três padrões de geração de valor que podem ser usados para propriedades:
* Não há geração de valor
* Adicionar valor gerado em
* Valor gerado em Adicionar ou atualizar

### <a name="no-value-generation"></a>Não há geração de valor

Não há geração de valor significa que você sempre será fornecer um valor válido a ser salvo no banco de dados. Esse valor válido deve ser atribuído às novas entidades antes de serem adicionados ao contexto.

### <a name="value-generated-on-add"></a>Adicionar valor gerado em

Valor gerado em Adicionar significa que um valor é gerado para novas entidades.

Dependendo do provedor de banco de dados que está sendo usado, os valores podem ser geradas do lado do cliente pelo EF ou no banco de dados. Se o valor é gerado pelo banco de dados, o EF pode atribuir um valor temporário quando você adiciona a entidade ao contexto. Esse valor temporário, em seguida, será substituído pelo valor de banco de dados gerado durante a `SaveChanges()`.

Se você adicionar uma entidade ao contexto que tem um valor atribuído à propriedade, o EF tentará inserir esse valor em vez de gerar um novo. Uma propriedade é considerada como tendo um valor atribuído se ele não é atribuído o valor padrão do CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etc.). Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Como o valor é gerado para entidades adicionadas dependerá do provedor de banco de dados que está sendo usado. Provedores de banco de dados automaticamente poderão configurar a geração de valor para alguns tipos de propriedade, mas outras podem exigir que você configurar manualmente a como o valor é gerado.
>
> Por exemplo, ao usar o SQL Server, valores serão gerados automaticamente para `GUID` propriedades (usando o algoritmo GUID sequencial do SQL Server). No entanto, se você especificar que um `DateTime` propriedade é gerada em Adicionar, em seguida, você deve configurar uma maneira para os valores a serem gerados. Uma maneira de fazer isso, é configurar um valor padrão de `GETDATE()`, consulte [valores padrão](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor gerado em Adicionar ou atualizar

Valor gerado ao adicionar ou atualização significa que um novo valor é gerado sempre que o registro é salvo (insert ou update).

Como `value generated on add`, se você especificar um valor para a propriedade em uma instância recém adicionada de uma entidade, que o valor será inserido em vez de um valor que está sendo gerado. Também é possível definir um valor explícito ao atualizar. Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Como o valor é gerado para entidades adicionadas e atualizadas dependerá do provedor de banco de dados que está sendo usado. Provedores de banco de dados poderão configurar automaticamente a geração de valor para alguns tipos de propriedade, enquanto outros exigirão que você configurar manualmente a como o valor é gerado.
> 
> Por exemplo, ao usar o SQL Server `byte[]` propriedades que são definidas como gerada em Adicionar ou atualizar e marcado como tokens de simultaneidade, será configurado com o `rowversion` de tipo de dados - para que os valores serão gerados no banco de dados. No entanto, se você especificar que um `DateTime` propriedade é gerada em Adicionar ou atualizar, em seguida, você deve configurar uma maneira para os valores a serem gerados. Uma maneira de fazer isso, é configurar um valor padrão de `GETDATE()` (consulte [valores padrão](relational/default-values.md)) para gerar valores para novas linhas. Em seguida, você pode usar um gatilho de banco de dados para gerar valores durante as atualizações (por exemplo, o gatilho de exemplo a seguir).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenções

Por convenção, chaves primárias de não composição do tipo short, int, long ou Guid será configurado para ter valores gerados em Adicionar. Todas as outras propriedades será configurado com nenhuma geração de valor.

## <a name="data-annotations"></a>Anotações de dados

### <a name="no-value-generation-data-annotations"></a>Não há geração de valor (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor gerado em add (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF Saiba que valores são gerados para entidades adicionadas, ele não garante que o EF irá configurar o mecanismo real para gerar os valores. Ver [Adicionar valor gerado no](#value-generated-on-add) seção para obter mais detalhes.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor gerado em Adicionar ou atualizar (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF Saiba que valores são gerados para entidades adicionadas ou atualizadas, ele não garante que o EF irá configurar o mecanismo real para gerar os valores. Ver [valor gerado em Adicionar ou atualizar](#value-generated-on-add-or-update) para obter mais detalhes.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para alterar o padrão de geração de valor para uma determinada propriedade.

### <a name="no-value-generation-fluent-api"></a>Não há geração de valor (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor gerado em add (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` apenas permite que o EF Saiba que os valores são gerados para entidades adicionadas, ele não garante que o EF irá configurar o mecanismo real para gerar os valores.  Ver [Adicionar valor gerado no](#value-generated-on-add) seção para obter mais detalhes.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Adicionar valor gerado no ou atualização (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF Saiba que valores são gerados para entidades adicionadas ou atualizadas, ele não garante que o EF irá configurar o mecanismo real para gerar os valores. Ver [valor gerado em Adicionar ou atualizar](#value-generated-on-add-or-update) para obter mais detalhes.
