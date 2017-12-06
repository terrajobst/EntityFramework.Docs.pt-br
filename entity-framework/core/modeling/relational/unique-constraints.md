---
title: "Chaves alternativas (restrições exclusivas) - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="d0287-102">Chaves alternativas (restrições exclusivas)</span><span class="sxs-lookup"><span data-stu-id="d0287-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="d0287-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="d0287-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d0287-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="d0287-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d0287-105">Uma restrição exclusiva é introduzida para cada chave alternativa no modelo.</span><span class="sxs-lookup"><span data-stu-id="d0287-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="d0287-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="d0287-106">Conventions</span></span>

<span data-ttu-id="d0287-107">Por convenção, o índice e a restrição que são apresentados para uma chave alternativa serão nomeados `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="d0287-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="d0287-108">Para chaves compostas de alternativas `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="d0287-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d0287-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d0287-109">Data Annotations</span></span>

<span data-ttu-id="d0287-110">Restrições UNIQUE não podem ser configuradas usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="d0287-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d0287-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="d0287-111">Fluent API</span></span>

<span data-ttu-id="d0287-112">Você pode usar a API fluente para configurar o nome de índice e de restrição para uma chave alternativa.</span><span class="sxs-lookup"><span data-stu-id="d0287-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
