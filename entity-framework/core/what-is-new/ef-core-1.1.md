---
title: Novidades no EF Core 1.1 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995429"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="2f890-102">Novos recursos no EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="2f890-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="2f890-103">Modelagem</span><span class="sxs-lookup"><span data-stu-id="2f890-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="2f890-104">Mapeamento de campo</span><span class="sxs-lookup"><span data-stu-id="2f890-104">Field mapping</span></span>
<span data-ttu-id="2f890-105">Permite que você configure um campo de suporte para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="2f890-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="2f890-106">Isso pode ser útil para propriedades somente leitura, ou para dados que tenham os métodos Get/Set em vez de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="2f890-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="2f890-107">Mapeamento de tabelas com otimização de memória no SQL Server</span><span class="sxs-lookup"><span data-stu-id="2f890-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="2f890-108">Você pode especificar que a tabela para a qual uma entidade está mapeada tem otimização de memória.</span><span class="sxs-lookup"><span data-stu-id="2f890-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="2f890-109">Ao usar o EF Core para criar e manter um banco de dados com base em seu modelo (com migrações ou `Database.EnsureCreated()`), uma tabela com otimização de memória será criada para essas entidades.</span><span class="sxs-lookup"><span data-stu-id="2f890-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="2f890-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="2f890-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="2f890-111">APIs de controle de alterações adicionais do EF6</span><span class="sxs-lookup"><span data-stu-id="2f890-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="2f890-112">Como `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span><span class="sxs-lookup"><span data-stu-id="2f890-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="2f890-113">Consulta</span><span class="sxs-lookup"><span data-stu-id="2f890-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="2f890-114">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="2f890-114">Explicit Loading</span></span>
<span data-ttu-id="2f890-115">Permite que você dispare o preenchimento de uma propriedade de navegação em uma entidade que carregada previamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2f890-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="2f890-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="2f890-116">DbSet.Find</span></span>
<span data-ttu-id="2f890-117">Fornece uma maneira fácil de obter uma entidade com base em seu valor de chave primária.</span><span class="sxs-lookup"><span data-stu-id="2f890-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="2f890-118">Outros</span><span class="sxs-lookup"><span data-stu-id="2f890-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="2f890-119">Resiliência da conexão</span><span class="sxs-lookup"><span data-stu-id="2f890-119">Connection resiliency</span></span>
<span data-ttu-id="2f890-120">Automaticamente repete comandos de banco de dados com falha.</span><span class="sxs-lookup"><span data-stu-id="2f890-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="2f890-121">Isso é especialmente útil ao se conectar com o SQL Azure, em que falhas transitórias são comuns.</span><span class="sxs-lookup"><span data-stu-id="2f890-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="2f890-122">Substituição de serviço simplificada</span><span class="sxs-lookup"><span data-stu-id="2f890-122">Simplified service replacement</span></span>
<span data-ttu-id="2f890-123">Facilita a substituição dos serviços internos usados pelo EF.</span><span class="sxs-lookup"><span data-stu-id="2f890-123">Makes it easier to replace internal services that EF uses.</span></span>
