---
title: Valores gerados-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197435"
---
# <a name="generated-values"></a>Valores gerados

## <a name="value-generation-patterns"></a>Padrões de geração de valor

Há três padrões de geração de valor que podem ser usados para propriedades:
* Nenhuma geração de valor
* Valor gerado em Adicionar
* Valor gerado em Adicionar ou atualizar

### <a name="no-value-generation"></a>Nenhuma geração de valor

Nenhuma geração de valor significa que você sempre fornecerá um valor válido a ser salvo no banco de dados. Esse valor válido deve ser atribuído a novas entidades antes de serem adicionadas ao contexto.

### <a name="value-generated-on-add"></a>Valor gerado em Adicionar

O valor gerado em Add significa que um valor é gerado para novas entidades.

Dependendo do provedor de banco de dados que está sendo usado, os valores podem ser gerados no lado do cliente pelo EF ou no banco de dados. Se o valor for gerado pelo banco de dados, o EF poderá atribuir um valor temporário quando você adicionar a entidade ao contexto. Esse valor temporário será então substituído pelo valor gerado pelo banco de dados `SaveChanges()`durante.

Se você adicionar uma entidade ao contexto que tem um valor atribuído à propriedade, o EF tentará inserir esse valor em vez de gerar um novo. Uma propriedade será considerada como tendo um valor atribuído se não for atribuído o valor padrão CLR (`null` `0` for `Guid.Empty` `string` `int`, for, for `Guid`, etc.). Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Como o valor é gerado para entidades adicionadas dependerá do provedor de banco de dados que está sendo usado. Os provedores de banco de dados podem configurar automaticamente a geração de valores para alguns tipos de propriedade, mas outros podem exigir que você configure manualmente como o valor é gerado.
>
> Por exemplo, ao usar SQL Server, os valores serão gerados automaticamente para `GUID` Propriedades (usando o algoritmo GUID sequencial SQL Server). No entanto, se você especificar `DateTime` que uma propriedade é gerada em Adicionar, será necessário configurar uma maneira de gerar os valores a serem gerados. Uma maneira de fazer isso é configurar um valor padrão de `GETDATE()`, consulte [valores padrão](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor gerado em Adicionar ou atualizar

O valor gerado em Adicionar ou atualizar significa que um novo valor é gerado toda vez que o registro é salvo (inserir ou atualizar).

Assim `value generated on add`como, se você especificar um valor para a propriedade em uma instância recém-adicionada de uma entidade, esse valor será inserido em vez de um valor que está sendo gerado. Também é possível definir um valor explícito ao atualizar. Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Como o valor é gerado para entidades adicionadas e atualizadas dependerá do provedor de banco de dados que está sendo usado. Os provedores de banco de dados podem configurar automaticamente a geração de valores para alguns tipos de propriedade, enquanto outros exigirão que você configure manualmente como o valor é gerado.
> 
> Por exemplo, ao usar SQL Server, `byte[]` as propriedades definidas como geradas em Adicionar ou atualizar e marcadas como tokens de simultaneidade serão configuradas com o `rowversion` tipo de dados, de modo que os valores serão gerados no banco de dado. No entanto, se você especificar `DateTime` que uma propriedade é gerada em Adicionar ou atualizar, deverá configurar uma maneira para os valores a serem gerados. Uma maneira de fazer isso é configurar um valor padrão de `GETDATE()` (consulte [valores padrão](relational/default-values.md)) para gerar valores para novas linhas. Em seguida, você pode usar um gatilho de banco de dados para gerar valores durante atualizações (como o seguinte exemplo de gatilho).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenções

Por convenção, chaves primárias não compostas do tipo curto, int, Long ou GUID serão configuradas para ter valores gerados na adição. Todas as outras propriedades serão configuradas sem nenhuma geração de valor.

## <a name="data-annotations"></a>Anotações de dados

### <a name="no-value-generation-data-annotations"></a>Nenhuma geração de valor (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor gerado em Add (data Annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF saiba que os valores são gerados para entidades adicionadas, ele não garante que o EF configurará o mecanismo real para gerar valores. Consulte o [valor gerado na seção Adicionar](#value-generated-on-add) para obter mais detalhes.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor gerado em Adicionar ou atualizar (anotações de dados)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF saiba que os valores são gerados para entidades adicionadas ou atualizadas, ele não garante que o EF configurará o mecanismo real para gerar valores. Consulte o [valor gerado na seção Adicionar ou atualizar](#value-generated-on-add-or-update) para obter mais detalhes.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para alterar o padrão de geração de valor de uma determinada propriedade.

### <a name="no-value-generation-fluent-api"></a>Nenhuma geração de valor (API fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor gerado em Add (API fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`apenas permite ao EF saber que os valores são gerados para entidades adicionadas, ele não garante que o EF configurará o mecanismo real para gerar valores.  Consulte o [valor gerado na seção Adicionar](#value-generated-on-add) para obter mais detalhes.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valor gerado em Adicionar ou atualizar (API fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Isso apenas permite que o EF saiba que os valores são gerados para entidades adicionadas ou atualizadas, ele não garante que o EF configurará o mecanismo real para gerar valores. Consulte o [valor gerado na seção Adicionar ou atualizar](#value-generated-on-add-or-update) para obter mais detalhes.
