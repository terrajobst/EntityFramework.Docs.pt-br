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
# <a name="default-schema"></a>Esquema Padrão

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

O esquema padrão é o esquema de banco de dados que objetos serão criados no se um esquema não é explicitamente configurado para esse objeto.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado. Por exemplo, Microsoft SQL Server usará o `dbo` esquema e SQLite não usará um esquema (já que não há suporte para esquemas em SQLite).

## <a name="data-annotations"></a>Anotações de dados

Não é possível definir o esquema padrão usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar um esquema padrão.

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
