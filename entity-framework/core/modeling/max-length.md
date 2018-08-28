---
title: Comprimento máximo – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996185"
---
# <a name="maximum-length"></a><span data-ttu-id="b005c-102">Comprimento máximo</span><span class="sxs-lookup"><span data-stu-id="b005c-102">Maximum Length</span></span>

<span data-ttu-id="b005c-103">A configuração de um comprimento máximo fornece uma dica para o armazenamento de dados sobre o tipo de dados apropriado a ser usado para uma determinada propriedade.</span><span class="sxs-lookup"><span data-stu-id="b005c-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="b005c-104">Comprimento máximo só se aplica a tipos de dados de matriz, como `string` e `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b005c-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="b005c-105">Não faz nenhuma validação de comprimento máximo antes de passar dados para o provedor do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b005c-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="b005c-106">É responsabilidade do provedor ou repositório de dados para validar se apropriado.</span><span class="sxs-lookup"><span data-stu-id="b005c-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="b005c-107">Por exemplo, quando o SQL Server, que excede o comprimento máximo de direcionamento resultará em uma exceção como o tipo de dados da coluna subjacente não permitirá excesso de dados a ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="b005c-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="b005c-108">Convenções</span><span class="sxs-lookup"><span data-stu-id="b005c-108">Conventions</span></span>

<span data-ttu-id="b005c-109">Por convenção, é deixado para o provedor de banco de dados para escolher um tipo de dados apropriado para as propriedades.</span><span class="sxs-lookup"><span data-stu-id="b005c-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="b005c-110">Para propriedades que têm um comprimento, o provedor de banco de dados geralmente escolherá um tipo de dados que permite que o maior tamanho de dados.</span><span class="sxs-lookup"><span data-stu-id="b005c-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="b005c-111">Por exemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` se a coluna é usada como uma chave).</span><span class="sxs-lookup"><span data-stu-id="b005c-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b005c-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="b005c-112">Data Annotations</span></span>

<span data-ttu-id="b005c-113">Você pode usar as anotações de dados para configurar um comprimento máximo para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="b005c-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="b005c-114">Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="b005c-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="b005c-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="b005c-115">Fluent API</span></span>

<span data-ttu-id="b005c-116">Você pode usar a API Fluent para configurar um comprimento máximo para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="b005c-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="b005c-117">Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="b005c-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
