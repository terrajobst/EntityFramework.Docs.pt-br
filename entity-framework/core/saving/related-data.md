---
title: Salvar relacionadas a dados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a>Salvando dados relacionados

Além de entidades isoladas, você também pode fazer uso das relações definidas em seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) deste artigo no GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Adicionando um gráfico de novas entidades

Se você criar várias novas entidades relacionadas, adicionar um para o contexto fará com que os demais ser adicionado muito.

No exemplo a seguir, o blog e três postagens relacionadas são todos inseridas no banco de dados. Postagens forem encontradas e adicionadas, porque eles estão acessíveis por meio de `Blog.Posts` propriedade de navegação.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Use a propriedade EntityEntry.State para definir o estado de apenas uma única entidade. Por exemplo, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Adicionando uma entidade relacionada

Se você referenciar a propriedade de navegação de uma entidade que já é controlada pelo contexto de uma nova entidade, a entidade será descoberta e inserida no banco de dados.

No exemplo a seguir, o `post` entidade é inserida porque ele é adicionado ao `Posts` propriedade o `blog` entidade que foi obtida do banco de dados.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Alterando relações

Se você alterar a propriedade de navegação de uma entidade, as alterações correspondentes serão feitas para a coluna de chave estrangeira no banco de dados.

No exemplo a seguir, o `post` entidade é atualizada para pertencer ao novo `blog` entidade porque seu `Blog` propriedade de navegação é definida para apontar para `blog`. Observe que `blog` também serão inseridos no banco de dados porque ele é uma nova entidade que é referenciada pela propriedade de navegação de uma entidade que já é controlada pelo contexto (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Removendo relações

Você pode remover uma relação definindo uma navegação de referência como `null`, ou remover a entidade relacionada da navegação coleção.

Para remover uma relação pode ter efeitos colaterais na entidade dependente, acordo com cascade excluir comportamento configurado na relação.

Por padrão, para as relações necessárias, um comportamento de exclusão em cascata está configurado e a entidade dependente/filho será excluída do banco de dados. Para relações opcionais, a exclusão em cascata não está configurado por padrão, mas a propriedade de chave estrangeira será definida como null.

Consulte [relações necessárias e opcionais](../modeling/relationships.md#required-and-optional-relationships) para saber mais sobre como o requiredness de relações podem ser configuradas.

Consulte [Cascade Delete](cascade-delete.md) para obter mais detalhes sobre como comportamentos de exclusão em cascata de trabalho, como eles podem ser configurados explicitamente e como eles são selecionados por convenção.

No exemplo a seguir, uma exclusão em cascata é configurada na relação entre `Blog` e `Post`, portanto, o `post` entidade é excluída do banco de dados.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
