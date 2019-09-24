---
title: Esquema padrão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197137"
---
# <a name="default-schema"></a><span data-ttu-id="feb33-102">Esquema padrão</span><span class="sxs-lookup"><span data-stu-id="feb33-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="feb33-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="feb33-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="feb33-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="feb33-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="feb33-105">O esquema padrão é o esquema de banco de dados no qual os objetos serão criados se um esquema não estiver configurado explicitamente para esse objeto.</span><span class="sxs-lookup"><span data-stu-id="feb33-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="feb33-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="feb33-106">Conventions</span></span>

<span data-ttu-id="feb33-107">Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado.</span><span class="sxs-lookup"><span data-stu-id="feb33-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="feb33-108">Por exemplo, Microsoft SQL Server usará o `dbo` esquema e o SQLite não usará um esquema (já que não há suporte para esquemas no SQLite).</span><span class="sxs-lookup"><span data-stu-id="feb33-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="feb33-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="feb33-109">Data Annotations</span></span>

<span data-ttu-id="feb33-110">Não é possível definir o esquema padrão usando as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="feb33-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="feb33-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="feb33-111">Fluent API</span></span>

<span data-ttu-id="feb33-112">Você pode usar a API fluente para especificar um esquema padrão.</span><span class="sxs-lookup"><span data-stu-id="feb33-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
