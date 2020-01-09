---
title: Valores gerados-EF Core
description: Como configurar a geração de valores para propriedades ao usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502026"
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

Dependendo do provedor de banco de dados que está sendo usado, os valores podem ser gerados no lado do cliente pelo EF ou no banco de dados. Se o valor for gerado pelo banco de dados, o EF poderá atribuir um valor temporário quando você adicionar a entidade ao contexto. Esse valor temporário será então substituído pelo valor gerado pelo banco de dados durante a `SaveChanges()`.

Se você adicionar uma entidade ao contexto que tem um valor atribuído à propriedade, o EF tentará inserir esse valor em vez de gerar um novo. Uma propriedade será considerada como tendo um valor atribuído se não for atribuído o valor padrão CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etc.). Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Como o valor é gerado para entidades adicionadas dependerá do provedor de banco de dados que está sendo usado. Os provedores de banco de dados podem configurar automaticamente a geração de valores para alguns tipos de propriedade, mas outros podem exigir que você configure manualmente como o valor é gerado.
>
> Por exemplo, ao usar SQL Server, os valores serão gerados automaticamente para propriedades de `GUID` (usando o algoritmo GUID sequencial SQL Server). No entanto, se você especificar que uma propriedade `DateTime` é gerada em Adicionar, você deve configurar uma maneira para os valores a serem gerados. Uma maneira de fazer isso é configurar um valor padrão de `GETDATE()`, consulte [valores padrão](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor gerado em Adicionar ou atualizar

O valor gerado em Adicionar ou atualizar significa que um novo valor é gerado toda vez que o registro é salvo (inserir ou atualizar).

Assim como `value generated on add`, se você especificar um valor para a propriedade em uma instância recém-adicionada de uma entidade, esse valor será inserido em vez de um valor que está sendo gerado. Também é possível definir um valor explícito ao atualizar. Para obter mais informações, consulte [valores explícitos para propriedades geradas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Como o valor é gerado para entidades adicionadas e atualizadas dependerá do provedor de banco de dados que está sendo usado. Os provedores de banco de dados podem configurar automaticamente a geração de valores para alguns tipos de propriedade, enquanto outros exigirão que você configure manualmente como o valor é gerado.
>
> Por exemplo, ao usar SQL Server, as propriedades `byte[]` definidas como geradas em Adicionar ou atualizar e marcadas como tokens de simultaneidade serão configuradas com o tipo de dados `rowversion`, de modo que os valores serão gerados no banco de dado. No entanto, se você especificar que uma propriedade `DateTime` é gerada em Adicionar ou atualizar, você deve configurar uma maneira para que os valores sejam gerados. Uma maneira de fazer isso é configurar um valor padrão de `GETDATE()` (consulte [valores padrão](relational/default-values.md)) para gerar valores para novas linhas. Em seguida, você pode usar um gatilho de banco de dados para gerar valores durante atualizações (como o seguinte exemplo de gatilho).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="value-generated-on-add"></a>Valor gerado em Adicionar

Por convenção, chaves primárias não compostas do tipo curto, int, Long ou GUID são configuradas para ter valores gerados para entidades inseridas, se um valor não for fornecido pelo aplicativo. Seu provedor de banco de dados normalmente cuida da configuração necessária; por exemplo, uma chave primária numérica em SQL Server é automaticamente configurada para ser uma coluna de identidade.

Você pode configurar qualquer propriedade para que seu valor seja gerado para entidades inseridas da seguinte maneira:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> Isso apenas permite que o EF saiba que os valores são gerados para entidades adicionadas, ele não garante que o EF configurará o mecanismo real para gerar valores. Consulte o [valor gerado na seção Adicionar](#value-generated-on-add) para obter mais detalhes.

### <a name="default-values"></a>Valores padrão

Em bancos de dados relacionais, uma coluna pode ser configurada com um valor padrão; se uma linha for inserida sem um valor para essa coluna, o valor padrão será usado.

Você pode configurar um valor padrão em uma propriedade:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

Você também pode especificar um fragmento SQL que é usado para calcular o valor padrão:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

A especificação de um valor padrão configurará implicitamente a propriedade como valor gerado em Adicionar.

## <a name="value-generated-on-add-or-update"></a>Valor gerado em Adicionar ou atualizar

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> Isso apenas permite que o EF saiba que os valores são gerados para entidades adicionadas ou atualizadas, ele não garante que o EF configurará o mecanismo real para gerar valores. Consulte o [valor gerado na seção Adicionar ou atualizar](#value-generated-on-add-or-update) para obter mais detalhes.

### <a name="computed-columns"></a>Colunas computadas

Em alguns bancos de dados relacionais, uma coluna pode ser configurada para ter seu valor computado no banco de dados, normalmente com uma expressão referindo-se a outras colunas:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> Em alguns casos, o valor da coluna é calculado sempre que é buscado (às vezes chamado de colunas *virtuais* ) e, em outros, é computado em todas as atualizações da linha e armazenadas (às vezes chamadas de colunas *armazenadas* ou *persistentes* ). Isso varia entre os provedores de banco de dados.

## <a name="no-value-generation"></a>Nenhuma geração de valor

A desabilitação da geração de valor em uma propriedade normalmente é necessária se uma convenção a configura para geração de valor. Por exemplo, se você tiver uma chave primária do tipo int, ela será definida implicitamente como o valor gerado em Add; Você pode desabilitar isso por meio do seguinte:

### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
