---
title: Restrições de chave estrangeira-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655991"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="c39b4-102">Restrições de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="c39b4-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="c39b4-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="c39b4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c39b4-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="c39b4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c39b4-105">Uma restrição FOREIGN KEY é introduzida para cada relação no modelo.</span><span class="sxs-lookup"><span data-stu-id="c39b4-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="c39b4-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="c39b4-106">Conventions</span></span>

<span data-ttu-id="c39b4-107">Por convenção, as restrições FOREIGN KEY são nomeadas `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c39b4-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="c39b4-108">Para chaves estrangeiras compostas `<foreign key property name>` se torna uma lista separada por sublinhado de nomes de propriedade de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="c39b4-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c39b4-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c39b4-109">Data Annotations</span></span>

<span data-ttu-id="c39b4-110">Nomes de restrição de chave estrangeira não podem ser configurados usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="c39b4-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c39b4-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="c39b4-111">Fluent API</span></span>

<span data-ttu-id="c39b4-112">Você pode usar a API Fluent para configurar o nome da restrição de chave estrangeira para uma relação.</span><span class="sxs-lookup"><span data-stu-id="c39b4-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
