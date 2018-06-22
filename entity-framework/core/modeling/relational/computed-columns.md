---
title: Colunas computadas - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052476"
---
# <a name="computed-columns"></a><span data-ttu-id="c4762-102">Colunas computadas</span><span class="sxs-lookup"><span data-stu-id="c4762-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="c4762-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="c4762-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c4762-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="c4762-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c4762-105">Uma coluna computada é uma coluna cujo valor é calculado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c4762-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="c4762-106">Uma coluna computada pode usar outras colunas na tabela para calcular o valor.</span><span class="sxs-lookup"><span data-stu-id="c4762-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="c4762-107">Convenções</span><span class="sxs-lookup"><span data-stu-id="c4762-107">Conventions</span></span>

<span data-ttu-id="c4762-108">Por convenção, as colunas computadas não são criadas no modelo.</span><span class="sxs-lookup"><span data-stu-id="c4762-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c4762-109">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c4762-109">Data Annotations</span></span>

<span data-ttu-id="c4762-110">Colunas computadas não podem ser configuradas com as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="c4762-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c4762-111">API fluente</span><span class="sxs-lookup"><span data-stu-id="c4762-111">Fluent API</span></span>

<span data-ttu-id="c4762-112">Você pode usar a API fluente para especificar que uma propriedade deve ser mapeado para uma coluna computada.</span><span class="sxs-lookup"><span data-stu-id="c4762-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
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
