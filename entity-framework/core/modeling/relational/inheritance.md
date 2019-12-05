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
# <a name="inheritance-relational-database"></a>Herança (banco de dados relacional)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.

> [!NOTE]  
> Atualmente, somente o padrão de tabela por hierarquia (TPH) é implementado em EF Core. Outros padrões comuns, como tabela por tipo (TPT) e tipo de tabela por concreto (TPC), ainda não estão disponíveis.

## <a name="conventions"></a>Convenções

Por padrão, a herança será mapeada usando o padrão de tabela por hierarquia (TPH). TPH usa uma única tabela para armazenar os dados de todos os tipos na hierarquia. Uma coluna discriminadora é usada para identificar o tipo que cada linha representa.

EF Core só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo (consulte [herança](../inheritance.md) para obter mais detalhes).

Veja abaixo um exemplo que mostra um cenário de herança simples e os dados armazenados em uma tabela de banco de dados relacional usando o padrão TPH. A coluna *discriminador* identifica qual tipo de *blog* é armazenado em cada linha.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> As colunas de banco de dados são automaticamente tornadas anuláveis conforme necessário ao usar o mapeamento TPH.

## <a name="data-annotations"></a>Anotações de dados

Você não pode usar anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome e o tipo da coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configurando a propriedade discriminador

Nos exemplos acima, o discriminador é criado como uma [Propriedade Shadow](xref:core/modeling/shadow-properties) na entidade base da hierarquia. Como é uma propriedade no modelo, ela pode ser configurada assim como outras propriedades. Por exemplo, para definir o comprimento máximo quando o discriminador padrão por convenção está sendo usado:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

O discriminador também pode ser mapeado para uma propriedade do .NET em sua entidade e configurá-lo. Por exemplo:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Colunas compartilhadas

Quando dois tipos de entidade irmãos têm uma propriedade com o mesmo nome, eles serão mapeados para duas colunas separadas por padrão. Mas se eles forem compatíveis, eles poderão ser mapeados para a mesma coluna:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]