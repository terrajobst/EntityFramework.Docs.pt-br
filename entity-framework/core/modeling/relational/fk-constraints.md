---
title: Restrições de chave estrangeira-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824583"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="ad9c6-102">Restrições de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="ad9c6-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="ad9c6-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ad9c6-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="ad9c6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ad9c6-105">Uma restrição FOREIGN KEY é introduzida para cada relação no modelo.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="ad9c6-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="ad9c6-106">Conventions</span></span>

<span data-ttu-id="ad9c6-107">Por convenção, as restrições FOREIGN KEY são nomeadas `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="ad9c6-108">Para chaves estrangeiras compostas `<foreign key property name>` se torna uma lista separada por sublinhado de nomes de propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ad9c6-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ad9c6-109">Data Annotations</span></span>

<span data-ttu-id="ad9c6-110">Nomes de restrição de chave estrangeira não podem ser configurados usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ad9c6-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="ad9c6-111">Fluent API</span></span>

<span data-ttu-id="ad9c6-112">Você pode usar a API Fluent para configurar o nome da restrição de chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="ad9c6-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
