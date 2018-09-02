---
title: Como salvar dados relacionados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 7349c57c0dccd3c911178641d3b34a478a4f6194
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994738"
---
# <a name="saving-related-data"></a>Como salvar dados relacionados

Além de entidades isoladas, você também pode fazer uso das relações definidas no seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) deste artigo no GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Como adicionar um gráfico de novas entidades

Se você criar várias novas entidades relacionadas, adicionar uma delas ao contexto fará com que as outras também sejam adicionadas.

No exemplo a seguir, o blog e três postagens relacionadas estão todos inseridos no banco de dados. As postagens são encontradas e adicionadas, porque estão acessíveis por meio da propriedade de navegação `Blog.Posts`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Use a propriedade EntityEntry.State para definir o estado de uma única entidade. Por exemplo, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Como adicionar uma entidade relacionada

Se você referenciar uma nova entidade da propriedade de navegação de uma entidade que já é controlada pelo contexto, a entidade será descoberta e inserida no banco de dados.

No exemplo a seguir, a entidade `post` é inserida porque ela é adicionada à propriedade `Posts` da entidade `blog` que foi obtida do banco de dados.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Como alterar relações

Se você alterar a propriedade de navegação de uma entidade, as alterações correspondentes serão feitas na coluna de chave estrangeira no banco de dados.

No exemplo a seguir, a entidade `post` é atualizada para pertencer à nova entidade `blog` porque sua propriedade de navegação `Blog` é configurada para apontar para `blog`. Observe que `blog` também será inserido no banco de dados porque ele é uma nova entidade referenciada pela propriedade de navegação de uma entidade que já é controlada pelo contexto (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Como remover relações

Você pode remover uma relação configurando uma navegação de referência como `null` ou removendo a entidade relacionada de uma navegação de coleção.

A remoção de uma relação pode ter efeitos colaterais sobre a entidade dependente, de acordo com o comportamento de exclusão em cascata configurado na relação.

Por padrão, para as relações necessárias, um comportamento de exclusão em cascata é configurado e a entidade dependente/filha será excluída do banco de dados. Para relações opcionais, a exclusão em cascata não é configurada por padrão, mas a propriedade de chave estrangeira será definida como nula.

Confira [Relações Obrigatórias e Opcionais](../modeling/relationships.md#required-and-optional-relationships) para saber como a obrigatoriedade das relações pode ser configurada.

Confira [Exclusão em Cascata](cascade-delete.md) para obter mais detalhes sobre como os comportamentos de exclusão em cascata funcionam, como eles podem ser configurados explicitamente e como eles são selecionados por convenção.

No exemplo a seguir, uma exclusão em cascata é configurada na relação entre `Blog` e `Post`, assim a entidade `post` é excluída do banco de dados.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
