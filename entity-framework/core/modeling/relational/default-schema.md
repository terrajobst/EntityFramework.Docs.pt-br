---
title: Esquema padrão, o EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995360"
---
# <a name="default-schema"></a><span data-ttu-id="ca1cb-102">Esquema padrão</span><span class="sxs-lookup"><span data-stu-id="ca1cb-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="ca1cb-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="ca1cb-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ca1cb-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="ca1cb-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ca1cb-105">O esquema padrão é o esquema de banco de dados que objetos serão criados em se um esquema não é explicitamente configurado para esse objeto.</span><span class="sxs-lookup"><span data-stu-id="ca1cb-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="ca1cb-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="ca1cb-106">Conventions</span></span>

<span data-ttu-id="ca1cb-107">Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado.</span><span class="sxs-lookup"><span data-stu-id="ca1cb-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="ca1cb-108">Por exemplo, Microsoft SQL Server usará o `dbo` esquema e o SQLite não usará um esquema (já que não há suporte para esquemas em SQLite).</span><span class="sxs-lookup"><span data-stu-id="ca1cb-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ca1cb-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="ca1cb-109">Data Annotations</span></span>

<span data-ttu-id="ca1cb-110">Não é possível definir o esquema padrão usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="ca1cb-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ca1cb-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="ca1cb-111">Fluent API</span></span>

<span data-ttu-id="ca1cb-112">Você pode usar a API Fluent para especificar um esquema padrão.</span><span class="sxs-lookup"><span data-stu-id="ca1cb-112">You can use the Fluent API to specify a default schema.</span></span>

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
