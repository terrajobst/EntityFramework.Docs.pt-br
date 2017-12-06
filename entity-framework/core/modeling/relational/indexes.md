---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="d12df-102">Índices</span><span class="sxs-lookup"><span data-stu-id="d12df-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="d12df-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="d12df-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d12df-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="d12df-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d12df-105">Um índice em um banco de dados relacional é mapeado para o mesmo conceito que um índice no núcleo do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d12df-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="d12df-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="d12df-106">Conventions</span></span>

<span data-ttu-id="d12df-107">Por convenção, os índices são denominados `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="d12df-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="d12df-108">Para índices compostos `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="d12df-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d12df-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d12df-109">Data Annotations</span></span>

<span data-ttu-id="d12df-110">Índices não podem ser configurados usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="d12df-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d12df-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="d12df-111">Fluent API</span></span>

<span data-ttu-id="d12df-112">Você pode usar a API fluente para configurar o nome de um índice.</span><span class="sxs-lookup"><span data-stu-id="d12df-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
