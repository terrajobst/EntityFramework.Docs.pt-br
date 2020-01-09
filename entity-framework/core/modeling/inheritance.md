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
# <a name="inheritance"></a>{1&gt;Herança&lt;1}

O EF pode mapear uma hierarquia de tipo .NET para um banco de dados. Isso permite que você grave suas entidades do .NET no código como de costume, usando tipos base e derivados, e tenha o EF para criar diretamente o esquema de banco de dados apropriado, consultas de problemas, etc. Os detalhes reais de como uma hierarquia de tipo é mapeada são dependentes do provedor; Esta página descreve o suporte de herança no contexto de um banco de dados relacional.

No momento, EF Core dá suporte apenas ao padrão de tabela por hierarquia (TPH). O TPH usa uma única tabela para armazenar os dados de todos os tipos na hierarquia, e uma coluna discriminadora é usada para identificar qual tipo cada linha representa.

> [!NOTE]
> O EF Core (tabela por tipo) e o tipo de tabela por concreto (TPC), que têm suporte de EF6, ainda não têm suporte do TPT. TPT é um recurso importante planejado para EF Core 5,0.

## <a name="entity-type-hierarchy-mapping"></a>Mapeamento de hierarquia de tipo de entidade

Por convenção, o EF irá configurar a herança somente se dois ou mais tipos herdados forem explicitamente incluídos no modelo. O EF não examinará automaticamente os tipos base ou derivados que não estão incluídos no modelo de outra forma.

Você pode incluir tipos no modelo expondo um DbSet para cada tipo na hierarquia de herança:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Esse modelo deve ser mapeado para o esquema de banco de dados a seguir (Observe a coluna *discriminadora* implicitamente criada, que identifica qual tipo de *blog* está armazenado em cada linha):

![imagem](_static/inheritance-tph-data.png)

>[!NOTE]
> As colunas de banco de dados são automaticamente tornadas anuláveis conforme necessário ao usar o mapeamento TPH. Por exemplo, a coluna *RssUrl* é anulável porque as instâncias de *blog* regulares não têm essa propriedade.

Se você não quiser expor um DbSet para uma ou mais entidades na hierarquia, também poderá usar a API fluente para garantir que elas sejam incluídas no modelo.

> [!TIP]
> Se você não depender de convenções, poderá especificar o tipo base explicitamente usando `HasBaseType`. Você também pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.

## <a name="discriminator-configuration"></a>Configuração do discriminador

Você pode configurar o nome e o tipo da coluna discriminadora e os valores que são usados para identificar cada tipo na hierarquia:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

Nos exemplos acima, o EF adicionou o discriminador implicitamente como uma [Propriedade Shadow](xref:core/modeling/shadow-properties) na entidade base da hierarquia. Essa propriedade pode ser configurada como qualquer outra:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Por fim, o discriminador também pode ser mapeado para uma propriedade comum do .NET em sua entidade:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Colunas compartilhadas

Por padrão, quando dois tipos de entidade irmã na hierarquia têm uma propriedade com o mesmo nome, eles serão mapeados para duas colunas separadas. No entanto, se o tipo for idêntico, eles poderão ser mapeados para a mesma coluna de banco de dados:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
