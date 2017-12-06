---
title: "Esquema padrão - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="default-schema"></a><span data-ttu-id="64296-102">Esquema Padrão</span><span class="sxs-lookup"><span data-stu-id="64296-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="64296-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="64296-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="64296-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="64296-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="64296-105">O esquema padrão é o esquema de banco de dados que objetos serão criados no se um esquema não é explicitamente configurado para esse objeto.</span><span class="sxs-lookup"><span data-stu-id="64296-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="64296-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="64296-106">Conventions</span></span>

<span data-ttu-id="64296-107">Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado.</span><span class="sxs-lookup"><span data-stu-id="64296-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="64296-108">Por exemplo, Microsoft SQL Server usará o `dbo` esquema e SQLite não usará um esquema (já que não há suporte para esquemas em SQLite).</span><span class="sxs-lookup"><span data-stu-id="64296-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="64296-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="64296-109">Data Annotations</span></span>

<span data-ttu-id="64296-110">Não é possível definir o esquema padrão usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="64296-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="64296-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="64296-111">Fluent API</span></span>

<span data-ttu-id="64296-112">Você pode usar a API fluente para especificar um esquema padrão.</span><span class="sxs-lookup"><span data-stu-id="64296-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
