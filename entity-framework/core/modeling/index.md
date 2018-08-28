---
title: Criar um Modelo – Core EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 9f702d5833b88e6eb77c0afefdae0ed3bc162ec8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993927"
---
# <a name="creating-a-model"></a><span data-ttu-id="e7fdb-102">Criar um Modelo</span><span class="sxs-lookup"><span data-stu-id="e7fdb-102">Creating a Model</span></span>

<span data-ttu-id="e7fdb-103">O Entity Framework usa um conjunto de convenções para criar um modelo com base na forma de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="e7fdb-104">Você pode especificar configurações adicionais para complementar e/ou substituir o que foi descoberto por convenção.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="e7fdb-105">Este artigo aborda a configuração que pode ser aplicada a um modelo que tenha como destino qualquer armazenamento de dados e que possa ser aplicado ao ter qualquer banco de dados relacional como destino.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="e7fdb-106">Os provedores também podem habilitar configuração específica de um armazenamento de dados em particular.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="e7fdb-107">Para obter documentação sobre configuração específica do provedor, consulte a seção [Provedores de Banco de Dados](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e7fdb-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="e7fdb-108">Você pode exibir o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="e7fdb-109">Métodos de configuração</span><span class="sxs-lookup"><span data-stu-id="e7fdb-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="e7fdb-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="e7fdb-110">Fluent API</span></span>

<span data-ttu-id="e7fdb-111">Você pode substituir o método `OnModelCreating` no contexto derivado e usar o `ModelBuilder API` para configurar seu modelo.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="e7fdb-112">Este é o método mais eficiente de configuração e permite que a configuração seja especificada sem modificação de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="e7fdb-113">A configuração da API fluente tem a precedência mais alta e substituirá as anotações de dados e as convenções.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="e7fdb-114">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e7fdb-114">Data Annotations</span></span>

<span data-ttu-id="e7fdb-115">Você também pode aplicar atributos (conhecidos como Anotações de Dados) a classes e propriedades.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="e7fdb-116">As anotações de dados substituirão as convenções, mas serão substituídas pela configuração da API Fluente.</span><span class="sxs-lookup"><span data-stu-id="e7fdb-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
