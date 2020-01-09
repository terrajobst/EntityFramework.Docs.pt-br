---
title: Herança-EF Core
description: Como configurar a herança de tipo de entidade usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502156"
---
# <a name="inheritance"></a><span data-ttu-id="48f49-103">{1&gt;Herança&lt;1}</span><span class="sxs-lookup"><span data-stu-id="48f49-103">Inheritance</span></span>

<span data-ttu-id="48f49-104">O EF pode mapear uma hierarquia de tipo .NET para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48f49-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="48f49-105">Isso permite que você grave suas entidades do .NET no código como de costume, usando tipos base e derivados, e tenha o EF para criar diretamente o esquema de banco de dados apropriado, consultas de problemas, etc. Os detalhes reais de como uma hierarquia de tipo é mapeada são dependentes do provedor; Esta página descreve o suporte de herança no contexto de um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="48f49-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="48f49-106">No momento, EF Core dá suporte apenas ao padrão de tabela por hierarquia (TPH).</span><span class="sxs-lookup"><span data-stu-id="48f49-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="48f49-107">O TPH usa uma única tabela para armazenar os dados de todos os tipos na hierarquia, e uma coluna discriminadora é usada para identificar qual tipo cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="48f49-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="48f49-108">O EF Core (tabela por tipo) e o tipo de tabela por concreto (TPC), que têm suporte de EF6, ainda não têm suporte do TPT.</span><span class="sxs-lookup"><span data-stu-id="48f49-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="48f49-109">TPT é um recurso importante planejado para EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="48f49-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="48f49-110">Mapeamento de hierarquia de tipo de entidade</span><span class="sxs-lookup"><span data-stu-id="48f49-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="48f49-111">Por convenção, o EF irá configurar a herança somente se dois ou mais tipos herdados forem explicitamente incluídos no modelo.</span><span class="sxs-lookup"><span data-stu-id="48f49-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="48f49-112">O EF não examinará automaticamente os tipos base ou derivados que não estão incluídos no modelo de outra forma.</span><span class="sxs-lookup"><span data-stu-id="48f49-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="48f49-113">Você pode incluir tipos no modelo expondo um DbSet para cada tipo na hierarquia de herança:</span><span class="sxs-lookup"><span data-stu-id="48f49-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="48f49-114">Esse modelo deve ser mapeado para o esquema de banco de dados a seguir (Observe a coluna *discriminadora* implicitamente criada, que identifica qual tipo de *blog* está armazenado em cada linha):</span><span class="sxs-lookup"><span data-stu-id="48f49-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![imagem](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="48f49-116">As colunas de banco de dados são automaticamente tornadas anuláveis conforme necessário ao usar o mapeamento TPH.</span><span class="sxs-lookup"><span data-stu-id="48f49-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="48f49-117">Por exemplo, a coluna *RssUrl* é anulável porque as instâncias de *blog* regulares não têm essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="48f49-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="48f49-118">Se você não quiser expor um DbSet para uma ou mais entidades na hierarquia, também poderá usar a API fluente para garantir que elas sejam incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="48f49-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="48f49-119">Se você não depender de convenções, poderá especificar o tipo base explicitamente usando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="48f49-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="48f49-120">Você também pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="48f49-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="48f49-121">Configuração do discriminador</span><span class="sxs-lookup"><span data-stu-id="48f49-121">Discriminator configuration</span></span>

<span data-ttu-id="48f49-122">Você pode configurar o nome e o tipo da coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia:</span><span class="sxs-lookup"><span data-stu-id="48f49-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="48f49-123">Nos exemplos acima, o EF adicionou o discriminador implicitamente como uma [Propriedade Shadow](xref:core/modeling/shadow-properties) na entidade base da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="48f49-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="48f49-124">Essa propriedade pode ser configurada como qualquer outra:</span><span class="sxs-lookup"><span data-stu-id="48f49-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="48f49-125">Por fim, o discriminador também pode ser mapeado para uma propriedade comum do .NET em sua entidade:</span><span class="sxs-lookup"><span data-stu-id="48f49-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="48f49-126">Colunas compartilhadas</span><span class="sxs-lookup"><span data-stu-id="48f49-126">Shared columns</span></span>

<span data-ttu-id="48f49-127">Por padrão, quando dois tipos de entidade irmã na hierarquia têm uma propriedade com o mesmo nome, eles serão mapeados para duas colunas separadas.</span><span class="sxs-lookup"><span data-stu-id="48f49-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="48f49-128">No entanto, se o tipo for idêntico, eles poderão ser mapeados para a mesma coluna de banco de dados:</span><span class="sxs-lookup"><span data-stu-id="48f49-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
