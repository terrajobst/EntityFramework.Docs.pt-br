---
title: Chaves alternativas (restrições exclusivas) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994185"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="2cda0-102">Chaves alternativas (restrições exclusivas)</span><span class="sxs-lookup"><span data-stu-id="2cda0-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="2cda0-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="2cda0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2cda0-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="2cda0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2cda0-105">Uma restrição unique é introduzida para cada chave alternativa no modelo.</span><span class="sxs-lookup"><span data-stu-id="2cda0-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="2cda0-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="2cda0-106">Conventions</span></span>

<span data-ttu-id="2cda0-107">Por convenção, o índice e a restrição que são apresentados para uma chave alternativa serão nomeados `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="2cda0-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="2cda0-108">Para chaves compostas de alternativas `<property name>` se tornará uma lista de sublinhado separado de nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="2cda0-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2cda0-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="2cda0-109">Data Annotations</span></span>

<span data-ttu-id="2cda0-110">As restrições UNIQUE não podem ser configuradas usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="2cda0-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2cda0-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="2cda0-111">Fluent API</span></span>

<span data-ttu-id="2cda0-112">Você pode usar a API Fluent para configurar o nome de índice e de restrição para uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="2cda0-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
