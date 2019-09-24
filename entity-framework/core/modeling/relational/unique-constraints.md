---
title: Chaves alternativas (restrições exclusivas)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197606"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="d4ae3-102">Chaves alternativas (restrições exclusivas)</span><span class="sxs-lookup"><span data-stu-id="d4ae3-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="d4ae3-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d4ae3-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="d4ae3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d4ae3-105">Uma restrição UNIQUE é introduzida para cada chave alternativa no modelo.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="d4ae3-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="d4ae3-106">Conventions</span></span>

<span data-ttu-id="d4ae3-107">Por convenção, o índice e a restrição que são introduzidos para uma chave alternativa serão `AK_<type name>_<property name>`nomeados.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="d4ae3-108">Para chaves `<property name>` alternativas compostas se torna uma lista de nomes de propriedade separados por sublinhado.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d4ae3-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d4ae3-109">Data Annotations</span></span>

<span data-ttu-id="d4ae3-110">Restrições exclusivas não podem ser configuradas usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d4ae3-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="d4ae3-111">Fluent API</span></span>

<span data-ttu-id="d4ae3-112">Você pode usar a API fluente para configurar o índice e o nome da restrição para uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="d4ae3-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
