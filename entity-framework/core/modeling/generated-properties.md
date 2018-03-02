---
title: Valores gerados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 892494461bcf49ee10d05c972da0ba19ca003c35
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="generated-values"></a>Valores gerados

## <a name="value-generation-patterns"></a>Padrões de geração de valor

Há três padrões de geração de valor que podem ser usados para propriedades.

### <a name="no-value-generation"></a>Não há geração de valor

Não há geração de valor significa que você sempre fornecerá um valor válido a ser salvo no banco de dados. Esse valor válido deve ser atribuído para novas entidades antes de serem adicionados ao contexto.

### <a name="value-generated-on-add"></a>Adicionar valor gerado em

Valor gerado em Adicionar significa que um valor é gerado para novas entidades.

Dependendo do provedor de banco de dados que está sendo usado, os valores podem ser gerados do lado do cliente por EF ou no banco de dados. Se o valor é gerado pelo banco de dados, EF pode atribuir um valor temporário quando você adiciona a entidade ao contexto. Esse valor temporário, em seguida, será substituído pelo valor do banco de dados gerado durante `SaveChanges()`.

Se você adicionar uma entidade ao contexto que tem um valor atribuído à propriedade, o EF tentará inserir esse valor em vez de gerar um novo. Uma propriedade é considerada para receber um valor se ele não está atribuído o valor padrão CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etc.). Para obter mais informações, consulte [valores explícitos de propriedades gerado](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Como o valor é gerado para entidades adicionadas dependem do provedor de banco de dados que está sendo usado. Provedores de banco de dados podem configurar automaticamente a geração de valor para alguns tipos de propriedade, mas outras pessoas podem exigir a instalação manualmente como o valor é gerado.
>
> Por exemplo, ao usar o SQL Server, valores serão gerados automaticamente para `GUID` propriedades (usando o algoritmo GUID sequencial do SQL Server). No entanto, se você especificar que um `DateTime` propriedade é gerada em Adicionar, em seguida, você deve configurar uma maneira para que os valores a serem gerados. Uma maneira de fazer isso, é configurar um valor padrão de `GETDATE()`, consulte [valores padrão](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor gerado em Adicionar ou atualizar

Valor gerado ao adicionar ou atualizar significa que um novo valor é gerado sempre que o registro é salvo (insert ou update).

Como `value generated on add`, se você especificar um valor para a propriedade em uma instância recém adicionada de uma entidade, que o valor será inserido em vez de um valor que está sendo gerado. Também é possível definir um valor explícito ao atualizar. Para obter mais informações, consulte [valores explícitos de propriedades gerado](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Como o valor é gerado para entidades adicionadas e atualizadas dependem do provedor de banco de dados que está sendo usado. Provedores de banco de dados automaticamente podem configurar a geração de valor para alguns tipos de propriedade, enquanto outros exigem instalação manualmente como o valor é gerado.
>
> Por exemplo, ao usar o SQL Server, `byte[]` propriedades que são definidas como gerado em Adicionar ou atualizar e marcado como tokens de simultaneidade, será instalado com o `rowversion` de tipo de dados - para que os valores serão criados no banco de dados. No entanto, se você especificar que um `DateTime` propriedade é gerada em Adicionar ou atualizar, em seguida, você deve configurar uma maneira para que os valores a serem gerados. Uma maneira de fazer isso, é configurar um valor padrão de `GETDATE()` (consulte [valores padrão](relational/default-values.md)) para gerar valores para novas linhas. Você pode usar um gatilho de banco de dados para gerar valores de durante as atualizações (como o gatilho de exemplo a seguir).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenções

Por convenção, chaves primárias de não composição do tipo short, int, longas ou Guid será instalado para ter valores gerados em Adicionar. Todas as outras propriedades será instalado com nenhuma geração de valor.

## <a name="data-annotations"></a>Anotações de dados

### <a name="no-value-generation-data-annotations"></a>Não há geração de valor (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor gerado em Adicionar (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Isso apenas permite EF souber que valores são gerados para entidades adicionadas, ele não garante que o EF irá configurar o mecanismo real para gerar os valores. Consulte [Adicionar valor gerado no](#value-generated-on-add) seção para obter mais detalhes.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor gerado em Adicionar ou atualizar (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite EF souber que os valores são gerados para entidades foram adicionadas ou atualizadas, isso não garante que o EF irá configurar o mecanismo real para gerar os valores. Consulte [valor gerado em Adicionar ou atualizar](#value-generated-on-add-or-update) seção para obter mais detalhes.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para alterar o padrão de geração de valor para uma determinada propriedade.

### <a name="no-value-generation-fluent-api"></a>Não há geração de valor (API fluente)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor gerado em Adicionar (API fluente)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` apenas permite que o EF que valores são gerados para entidades adicionadas, eles não garantem que o EF irá configurar o mecanismo real para gerar os valores.  Consulte [Adicionar valor gerado no](#value-generated-on-add) seção para obter mais detalhes.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valor gerado em Adicionar ou atualização (API fluente)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite EF souber que os valores são gerados para entidades foram adicionadas ou atualizadas, isso não garante que o EF irá configurar o mecanismo real para gerar os valores. Consulte [valor gerado em Adicionar ou atualizar](#value-generated-on-add-or-update) seção para obter mais detalhes.
