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
# <a name="default-schema"></a>Esquema padrão

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O esquema padrão é o esquema de banco de dados que objetos serão criados em se um esquema não é explicitamente configurado para esse objeto.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado. Por exemplo, Microsoft SQL Server usará o `dbo` esquema e o SQLite não usará um esquema (já que não há suporte para esquemas em SQLite).

## <a name="data-annotations"></a>Anotações de dados

Não é possível definir o esquema padrão usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para especificar um esquema padrão.

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
