---
title: Chaves primárias-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196953"
---
# <a name="primary-keys"></a><span data-ttu-id="f97af-102">Chaves primárias</span><span class="sxs-lookup"><span data-stu-id="f97af-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="f97af-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="f97af-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f97af-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="f97af-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f97af-105">Uma restrição PRIMARY KEY é introduzida para a chave de cada tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="f97af-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="f97af-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="f97af-106">Conventions</span></span>

<span data-ttu-id="f97af-107">Por convenção, a chave primária no banco de dados será nomeada `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="f97af-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f97af-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="f97af-108">Data Annotations</span></span>

<span data-ttu-id="f97af-109">Nenhum aspecto específico do banco de dados relacional de uma chave primária pode ser configurado usando as anotações.</span><span class="sxs-lookup"><span data-stu-id="f97af-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f97af-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="f97af-110">Fluent API</span></span>

<span data-ttu-id="f97af-111">Você pode usar a API fluente para configurar o nome da restrição de chave primária no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f97af-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
