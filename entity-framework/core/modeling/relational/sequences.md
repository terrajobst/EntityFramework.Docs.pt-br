---
title: Sequências-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656108"
---
# <a name="sequences"></a><span data-ttu-id="2d7fc-102">Sequências</span><span class="sxs-lookup"><span data-stu-id="2d7fc-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="2d7fc-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2d7fc-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="2d7fc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2d7fc-105">Uma sequência gera valores numéricos sequenciais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="2d7fc-106">As sequências não estão associadas a uma tabela específica.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="2d7fc-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="2d7fc-107">Conventions</span></span>

<span data-ttu-id="2d7fc-108">Por convenção, as sequências não são introduzidas no modelo.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2d7fc-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="2d7fc-109">Data Annotations</span></span>

<span data-ttu-id="2d7fc-110">Você não pode configurar uma sequência usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2d7fc-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="2d7fc-111">Fluent API</span></span>

<span data-ttu-id="2d7fc-112">Você pode usar a API fluente para criar uma sequência no modelo.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="2d7fc-113">Você também pode configurar o aspecto adicional da sequência, como seu esquema, valor inicial e incremento.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="2d7fc-114">Depois que uma sequência é introduzida, você pode usá-la para gerar valores para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="2d7fc-115">Por exemplo, você pode usar [valores padrão](default-values.md) para inserir o próximo valor da sequência.</span><span class="sxs-lookup"><span data-stu-id="2d7fc-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
