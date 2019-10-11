---
title: Consultar Dados – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181923"
---
# <a name="querying-data"></a><span data-ttu-id="473ff-102">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="473ff-102">Querying Data</span></span>

<span data-ttu-id="473ff-103">O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="473ff-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="473ff-104">O LINQ permite que você use o C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="473ff-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="473ff-105">Ele usa o contexto derivado e as classes de entidade para fazer referência a objetos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="473ff-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="473ff-106">O EF Core passa uma representação da consulta LINQ para o provedor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="473ff-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="473ff-107">Os provedores de banco de dados, por sua vez, a convertem em uma linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional).</span><span class="sxs-lookup"><span data-stu-id="473ff-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="473ff-108">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="473ff-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="473ff-109">Os snippets a seguir mostram alguns exemplos de como realizar tarefas comuns com o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="473ff-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="473ff-110">Como carregar todos os dados</span><span class="sxs-lookup"><span data-stu-id="473ff-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="473ff-111">Como carregar uma única entidade</span><span class="sxs-lookup"><span data-stu-id="473ff-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="473ff-112">Filtragem</span><span class="sxs-lookup"><span data-stu-id="473ff-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="473ff-113">Leituras adicionais</span><span class="sxs-lookup"><span data-stu-id="473ff-113">Further readings</span></span>

- <span data-ttu-id="473ff-114">Saiba mais sobre as [expressões de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="473ff-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="473ff-115">Para obter mais informações sobre como uma consulta é processada no EF Core, confira [Como funciona a consulta](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="473ff-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
