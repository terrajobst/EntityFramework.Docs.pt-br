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
# <a name="saving-related-data"></a><span data-ttu-id="6dddc-102">Salvando dados relacionados</span><span class="sxs-lookup"><span data-stu-id="6dddc-102">Saving Related Data</span></span>

<span data-ttu-id="6dddc-103">Além de entidades isoladas, você também pode fazer uso das relações definidas em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="6dddc-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="6dddc-104">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="6dddc-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="6dddc-105">Adicionando um gráfico de novas entidades</span><span class="sxs-lookup"><span data-stu-id="6dddc-105">Adding a graph of new entities</span></span>

<span data-ttu-id="6dddc-106">Se você criar várias novas entidades relacionadas, adicionar um para o contexto fará com que os demais ser adicionado muito.</span><span class="sxs-lookup"><span data-stu-id="6dddc-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="6dddc-107">No exemplo a seguir, o blog e três postagens relacionadas são todos inseridas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="6dddc-108">Postagens forem encontradas e adicionadas, porque eles estão acessíveis por meio de `Blog.Posts` propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="6dddc-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="6dddc-109">Use a propriedade EntityEntry.State para definir o estado de apenas uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="6dddc-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="6dddc-110">Por exemplo, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="6dddc-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="6dddc-111">Adicionando uma entidade relacionada</span><span class="sxs-lookup"><span data-stu-id="6dddc-111">Adding a related entity</span></span>

<span data-ttu-id="6dddc-112">Se você referenciar a propriedade de navegação de uma entidade que já é controlada pelo contexto de uma nova entidade, a entidade será descoberta e inserida no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="6dddc-113">No exemplo a seguir, o `post` entidade é inserida porque ele é adicionado ao `Posts` propriedade o `blog` entidade que foi obtida do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="6dddc-114">Alterando relações</span><span class="sxs-lookup"><span data-stu-id="6dddc-114">Changing relationships</span></span>

<span data-ttu-id="6dddc-115">Se você alterar a propriedade de navegação de uma entidade, as alterações correspondentes serão feitas para a coluna de chave estrangeira no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="6dddc-116">No exemplo a seguir, o `post` entidade é atualizada para pertencer ao novo `blog` entidade porque seu `Blog` propriedade de navegação é definida para apontar para `blog`.</span><span class="sxs-lookup"><span data-stu-id="6dddc-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="6dddc-117">Observe que `blog` também serão inseridos no banco de dados porque ele é uma nova entidade que é referenciada pela propriedade de navegação de uma entidade que já é controlada pelo contexto (`post`).</span><span class="sxs-lookup"><span data-stu-id="6dddc-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="6dddc-118">Removendo relações</span><span class="sxs-lookup"><span data-stu-id="6dddc-118">Removing relationships</span></span>

<span data-ttu-id="6dddc-119">Você pode remover uma relação definindo uma navegação de referência como `null`, ou remover a entidade relacionada da navegação coleção.</span><span class="sxs-lookup"><span data-stu-id="6dddc-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="6dddc-120">Para remover uma relação pode ter efeitos colaterais na entidade dependente, acordo com cascade excluir comportamento configurado na relação.</span><span class="sxs-lookup"><span data-stu-id="6dddc-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="6dddc-121">Por padrão, para as relações necessárias, um comportamento de exclusão em cascata está configurado e a entidade dependente/filho será excluída do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="6dddc-122">Para relações opcionais, a exclusão em cascata não está configurado por padrão, mas a propriedade de chave estrangeira será definida como null.</span><span class="sxs-lookup"><span data-stu-id="6dddc-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="6dddc-123">Consulte [relações necessárias e opcionais](../modeling/relationships.md#required-and-optional-relationships) para saber mais sobre como o requiredness de relações podem ser configuradas.</span><span class="sxs-lookup"><span data-stu-id="6dddc-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="6dddc-124">Consulte [Cascade Delete](cascade-delete.md) para obter mais detalhes sobre como comportamentos de exclusão em cascata de trabalho, como eles podem ser configurados explicitamente e como eles são selecionados por convenção.</span><span class="sxs-lookup"><span data-stu-id="6dddc-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="6dddc-125">No exemplo a seguir, uma exclusão em cascata é configurada na relação entre `Blog` e `Post`, portanto, o `post` entidade é excluída do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6dddc-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
