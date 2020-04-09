---
title: Como salvar dados relacionados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417541"
---
# <a name="saving-related-data"></a><span data-ttu-id="2d564-102">Como salvar dados relacionados</span><span class="sxs-lookup"><span data-stu-id="2d564-102">Saving Related Data</span></span>

<span data-ttu-id="2d564-103">Além de entidades isoladas, você também pode fazer uso das relações definidas no seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2d564-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="2d564-104">Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="2d564-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="2d564-105">Como adicionar um gráfico de novas entidades</span><span class="sxs-lookup"><span data-stu-id="2d564-105">Adding a graph of new entities</span></span>

<span data-ttu-id="2d564-106">Se você criar várias novas entidades relacionadas, adicionar uma delas ao contexto fará com que as outras também sejam adicionadas.</span><span class="sxs-lookup"><span data-stu-id="2d564-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="2d564-107">No exemplo a seguir, o blog e três postagens relacionadas estão todos inseridos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="2d564-108">As postagens são encontradas e adicionadas, porque estão acessíveis por meio da propriedade de navegação `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="2d564-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="2d564-109">Use a propriedade EntityEntry.State para definir o estado de uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="2d564-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="2d564-110">Por exemplo, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="2d564-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="2d564-111">Como adicionar uma entidade relacionada</span><span class="sxs-lookup"><span data-stu-id="2d564-111">Adding a related entity</span></span>

<span data-ttu-id="2d564-112">Se você referenciar uma nova entidade da propriedade de navegação de uma entidade que já é controlada pelo contexto, a entidade será descoberta e inserida no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="2d564-113">No exemplo a seguir, a entidade `post` é inserida porque ela é adicionada à propriedade `Posts` da entidade `blog` que foi obtida do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="2d564-114">Como alterar relações</span><span class="sxs-lookup"><span data-stu-id="2d564-114">Changing relationships</span></span>

<span data-ttu-id="2d564-115">Se você alterar a propriedade de navegação de uma entidade, as alterações correspondentes serão feitas na coluna de chave estrangeira no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="2d564-116">No exemplo a seguir, a entidade `post` é atualizada para pertencer à nova entidade `blog` porque sua propriedade de navegação `Blog` é configurada para apontar para `blog`.</span><span class="sxs-lookup"><span data-stu-id="2d564-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="2d564-117">Observe que `blog` também será inserido no banco de dados porque ele é uma nova entidade referenciada pela propriedade de navegação de uma entidade que já é controlada pelo contexto (`post`).</span><span class="sxs-lookup"><span data-stu-id="2d564-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="2d564-118">Como remover relações</span><span class="sxs-lookup"><span data-stu-id="2d564-118">Removing relationships</span></span>

<span data-ttu-id="2d564-119">Você pode remover uma relação configurando uma navegação de referência como `null` ou removendo a entidade relacionada de uma navegação de coleção.</span><span class="sxs-lookup"><span data-stu-id="2d564-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="2d564-120">A remoção de uma relação pode ter efeitos colaterais sobre a entidade dependente, de acordo com o comportamento de exclusão em cascata configurado na relação.</span><span class="sxs-lookup"><span data-stu-id="2d564-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="2d564-121">Por padrão, para as relações necessárias, um comportamento de exclusão em cascata é configurado e a entidade dependente/filha será excluída do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="2d564-122">Para relações opcionais, a exclusão em cascata não é configurada por padrão, mas a propriedade de chave estrangeira será definida como nula.</span><span class="sxs-lookup"><span data-stu-id="2d564-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="2d564-123">Confira [Relações Obrigatórias e Opcionais](../modeling/relationships.md#required-and-optional-relationships) para saber como a obrigatoriedade das relações pode ser configurada.</span><span class="sxs-lookup"><span data-stu-id="2d564-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="2d564-124">Confira [Exclusão em Cascata](cascade-delete.md) para obter mais detalhes sobre como os comportamentos de exclusão em cascata funcionam, como eles podem ser configurados explicitamente e como eles são selecionados por convenção.</span><span class="sxs-lookup"><span data-stu-id="2d564-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="2d564-125">No exemplo a seguir, uma exclusão em cascata é configurada na relação entre `Blog` e `Post`, assim a entidade `post` é excluída do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d564-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
