---
title: Herança (banco de dados relacional)-EF Core
description: Como configurar a herança de tipo de entidade em um banco de dados relacional usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824748"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="297a8-103">Herança (banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="297a8-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="297a8-104">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="297a8-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="297a8-105">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="297a8-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="297a8-106">A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="297a8-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="297a8-107">Atualmente, somente o padrão de tabela por hierarquia (TPH) é implementado em EF Core.</span><span class="sxs-lookup"><span data-stu-id="297a8-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="297a8-108">Outros padrões comuns, como tabela por tipo (TPT) e tipo de tabela por concreto (TPC), ainda não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="297a8-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="297a8-109">Convenções</span><span class="sxs-lookup"><span data-stu-id="297a8-109">Conventions</span></span>

<span data-ttu-id="297a8-110">Por padrão, a herança será mapeada usando o padrão de tabela por hierarquia (TPH).</span><span class="sxs-lookup"><span data-stu-id="297a8-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="297a8-111">TPH usa uma única tabela para armazenar os dados de todos os tipos na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="297a8-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="297a8-112">Uma coluna discriminadora é usada para identificar o tipo que cada linha representa.</span><span class="sxs-lookup"><span data-stu-id="297a8-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="297a8-113">EF Core só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).</span><span class="sxs-lookup"><span data-stu-id="297a8-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="297a8-114">Veja abaixo um exemplo que mostra um cenário de herança simples e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH.</span><span class="sxs-lookup"><span data-stu-id="297a8-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="297a8-115">A coluna *discriminador* identifica qual tipo de *blog* é armazenado em cada linha.</span><span class="sxs-lookup"><span data-stu-id="297a8-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="297a8-117">As colunas de banco de dados são automaticamente tornadas anuláveis conforme necessário ao usar o mapeamento TPH.</span><span class="sxs-lookup"><span data-stu-id="297a8-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="297a8-118">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="297a8-118">Data Annotations</span></span>

<span data-ttu-id="297a8-119">Você não pode usar anotações de dados para configurar a herança.</span><span class="sxs-lookup"><span data-stu-id="297a8-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="297a8-120">API fluente</span><span class="sxs-lookup"><span data-stu-id="297a8-120">Fluent API</span></span>

<span data-ttu-id="297a8-121">Você pode usar a API fluente para configurar o nome e o tipo da coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="297a8-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="297a8-122">Configurando a propriedade discriminador</span><span class="sxs-lookup"><span data-stu-id="297a8-122">Configuring the discriminator property</span></span>

<span data-ttu-id="297a8-123">Nos exemplos acima, o discriminador é criado como uma [Propriedade Shadow](xref:core/modeling/shadow-properties) na entidade base da hierarquia.</span><span class="sxs-lookup"><span data-stu-id="297a8-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="297a8-124">Como é uma propriedade no modelo, ela pode ser configurada assim como outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="297a8-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="297a8-125">Por exemplo, para definir o comprimento máximo quando o discriminador padrão por convenção está sendo usado:</span><span class="sxs-lookup"><span data-stu-id="297a8-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="297a8-126">O discriminador também pode ser mapeado para uma propriedade do .NET em sua entidade e configurá-lo.</span><span class="sxs-lookup"><span data-stu-id="297a8-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="297a8-127">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="297a8-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="297a8-128">Colunas compartilhadas</span><span class="sxs-lookup"><span data-stu-id="297a8-128">Shared columns</span></span>

<span data-ttu-id="297a8-129">Quando dois tipos de entidade irmãos têm uma propriedade com o mesmo nome, eles serão mapeados para duas colunas separadas por padrão.</span><span class="sxs-lookup"><span data-stu-id="297a8-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="297a8-130">Mas se eles forem compatíveis, eles poderão ser mapeados para a mesma coluna:</span><span class="sxs-lookup"><span data-stu-id="297a8-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]