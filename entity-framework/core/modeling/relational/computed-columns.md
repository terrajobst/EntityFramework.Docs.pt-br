---
title: Colunas computadas-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197240"
---
# <a name="computed-columns"></a><span data-ttu-id="db531-102">Colunas computadas</span><span class="sxs-lookup"><span data-stu-id="db531-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="db531-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="db531-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="db531-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="db531-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="db531-105">Uma coluna computada é uma coluna cujo valor é calculado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="db531-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="db531-106">Uma coluna computada pode usar outras colunas na tabela para calcular seu valor.</span><span class="sxs-lookup"><span data-stu-id="db531-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="db531-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="db531-107">Conventions</span></span>

<span data-ttu-id="db531-108">Por convenção, as colunas computadas não são criadas no modelo.</span><span class="sxs-lookup"><span data-stu-id="db531-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="db531-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="db531-109">Data Annotations</span></span>

<span data-ttu-id="db531-110">Colunas computadas não podem ser configuradas com anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="db531-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="db531-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="db531-111">Fluent API</span></span>

<span data-ttu-id="db531-112">Você pode usar a API fluente para especificar que uma propriedade deve ser mapeada para uma coluna computada.</span><span class="sxs-lookup"><span data-stu-id="db531-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
