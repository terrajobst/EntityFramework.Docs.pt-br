---
title: "Banco de dados SQLite provedor - limitações - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="526b0-102">Limitações de provedor de banco de dados do SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="526b0-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="526b0-103">O provedor do SQLite tem várias limitações de migrações.</span><span class="sxs-lookup"><span data-stu-id="526b0-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="526b0-104">A maioria dessas limitações é o resultado das limitações do mecanismo de banco de dados SQLite subjacente e não é específicas ao EF.</span><span class="sxs-lookup"><span data-stu-id="526b0-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="526b0-105">Limitações de modelagem</span><span class="sxs-lookup"><span data-stu-id="526b0-105">Modeling limitations</span></span>

<span data-ttu-id="526b0-106">A biblioteca de relacional comuns (compartilhada por provedores de banco de dados relacional do Entity Framework) define APIs de modelagem conceitos que são comuns a maioria dos mecanismos de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="526b0-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="526b0-107">Alguns desses conceitos não são suportados pelo provedor do SQLite.</span><span class="sxs-lookup"><span data-stu-id="526b0-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="526b0-108">Esquemas</span><span class="sxs-lookup"><span data-stu-id="526b0-108">Schemas</span></span>
* <span data-ttu-id="526b0-109">Sequências</span><span class="sxs-lookup"><span data-stu-id="526b0-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="526b0-110">Limitações de migrações</span><span class="sxs-lookup"><span data-stu-id="526b0-110">Migrations limitations</span></span>

<span data-ttu-id="526b0-111">O mecanismo de banco de dados SQLite não oferece suporte a um número de operações de esquema compatíveis com a maioria dos outros bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="526b0-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="526b0-112">Se você tentar aplicar uma das operações sem suporte para um banco de dados SQLite um `NotSupportedException` será lançada.</span><span class="sxs-lookup"><span data-stu-id="526b0-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="526b0-113">Operação</span><span class="sxs-lookup"><span data-stu-id="526b0-113">Operation</span></span>            | <span data-ttu-id="526b0-114">Com suporte?</span><span class="sxs-lookup"><span data-stu-id="526b0-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="526b0-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="526b0-115">AddColumn</span></span>            | <span data-ttu-id="526b0-116">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-116">✔</span></span>          |
| <span data-ttu-id="526b0-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="526b0-117">AddForeignKey</span></span>        | <span data-ttu-id="526b0-118">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-118">✗</span></span>          |
| <span data-ttu-id="526b0-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="526b0-119">AddPrimaryKey</span></span>        | <span data-ttu-id="526b0-120">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-120">✗</span></span>          |
| <span data-ttu-id="526b0-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="526b0-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="526b0-122">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-122">✗</span></span>          |
| <span data-ttu-id="526b0-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="526b0-123">AlterColumn</span></span>          | <span data-ttu-id="526b0-124">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-124">✗</span></span>          |
| <span data-ttu-id="526b0-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="526b0-125">CreateIndex</span></span>          | <span data-ttu-id="526b0-126">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-126">✔</span></span>          |
| <span data-ttu-id="526b0-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="526b0-127">CreateTable</span></span>          | <span data-ttu-id="526b0-128">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-128">✔</span></span>          |
| <span data-ttu-id="526b0-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="526b0-129">DropColumn</span></span>           | <span data-ttu-id="526b0-130">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-130">✗</span></span>          |
| <span data-ttu-id="526b0-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="526b0-131">DropForeignKey</span></span>       | <span data-ttu-id="526b0-132">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-132">✗</span></span>          |
| <span data-ttu-id="526b0-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="526b0-133">DropIndex</span></span>            | <span data-ttu-id="526b0-134">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-134">✔</span></span>          |
| <span data-ttu-id="526b0-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="526b0-135">DropPrimaryKey</span></span>       | <span data-ttu-id="526b0-136">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-136">✗</span></span>          |
| <span data-ttu-id="526b0-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="526b0-137">DropTable</span></span>            | <span data-ttu-id="526b0-138">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-138">✔</span></span>          |
| <span data-ttu-id="526b0-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="526b0-139">DropUniqueConstraint</span></span> | <span data-ttu-id="526b0-140">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-140">✗</span></span>          |
| <span data-ttu-id="526b0-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="526b0-141">RenameColumn</span></span>         | <span data-ttu-id="526b0-142">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-142">✗</span></span>          |
| <span data-ttu-id="526b0-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="526b0-143">RenameIndex</span></span>          | <span data-ttu-id="526b0-144">✗</span><span class="sxs-lookup"><span data-stu-id="526b0-144">✗</span></span>          |
| <span data-ttu-id="526b0-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="526b0-145">RenameTable</span></span>          | <span data-ttu-id="526b0-146">✔</span><span class="sxs-lookup"><span data-stu-id="526b0-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="526b0-147">Solução alternativa de limitações de migrações</span><span class="sxs-lookup"><span data-stu-id="526b0-147">Migrations limitations workaround</span></span>

<span data-ttu-id="526b0-148">Você pode solucionar esse problema algumas dessas limitações manualmente escrevendo código em seu migrações para executar uma tabela de recompilar.</span><span class="sxs-lookup"><span data-stu-id="526b0-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="526b0-149">Uma recriação de tabela envolve renomear a tabela existente, criando uma nova tabela, copiando dados para a nova tabela e descartar a tabela antiga.</span><span class="sxs-lookup"><span data-stu-id="526b0-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="526b0-150">Você precisará usar o `Sql(string)` método para executar algumas dessas etapas.</span><span class="sxs-lookup"><span data-stu-id="526b0-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="526b0-151">Consulte [fazer outros tipos de alteração de esquema](http://sqlite.org/lang_altertable.html#otheralter) na documentação do SQLite para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="526b0-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="526b0-152">No futuro, EF pode dar suporte a algumas dessas operações usando a abordagem de recriação de tabela nos bastidores.</span><span class="sxs-lookup"><span data-stu-id="526b0-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="526b0-153">Você pode [controlar esse recurso em nosso projeto GitHub](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="526b0-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
