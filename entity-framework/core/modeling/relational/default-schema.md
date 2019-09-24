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
# <a name="default-schema"></a>Esquema padrão

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O esquema padrão é o esquema de banco de dados no qual os objetos serão criados se um esquema não estiver configurado explicitamente para esse objeto.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado. Por exemplo, Microsoft SQL Server usará o `dbo` esquema e o SQLite não usará um esquema (já que não há suporte para esquemas no SQLite).

## <a name="data-annotations"></a>Anotações de dados

Não é possível definir o esquema padrão usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar um esquema padrão.

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
