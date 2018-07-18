---
title: Definindo DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39120194"
---
# <a name="defining-dbsets"></a><span data-ttu-id="28096-102">Definindo DbSets</span><span class="sxs-lookup"><span data-stu-id="28096-102">Defining DbSets</span></span>
<span data-ttu-id="28096-103">Ao desenvolver com o fluxo de trabalho do Code First você definir um DbContext derivado que representa a sessão com o banco de dados e expõe um DbSet para cada tipo em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="28096-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="28096-104">Este tópico aborda as várias maneiras que você pode definir as propriedades DbSet.</span><span class="sxs-lookup"><span data-stu-id="28096-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="28096-105">DbContext com propriedades DbSet</span><span class="sxs-lookup"><span data-stu-id="28096-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="28096-106">É o caso comum mostrado nos exemplos de código primeiro ter um DbContext com propriedades públicas de DbSet automática para os tipos de entidade do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="28096-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="28096-107">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="28096-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="28096-108">Quando usado no modo de Code First, isso irá configurar Blogs e postagens como tipos de entidade, bem como configurar outros tipos pode ser acessados a partir desses.</span><span class="sxs-lookup"><span data-stu-id="28096-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="28096-109">Além disso DbContext automaticamente chamará o setter para cada uma dessas propriedades para definir uma instância do DbSet apropriada.</span><span class="sxs-lookup"><span data-stu-id="28096-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="28096-110">DbContext com propriedades IDbSet</span><span class="sxs-lookup"><span data-stu-id="28096-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="28096-111">Há situações, como quando criar simulações ou falsificações, onde é mais útil declarar as propriedades de conjunto usando uma interface.</span><span class="sxs-lookup"><span data-stu-id="28096-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="28096-112">Em tais casos o IDbSet interface pode ser usado no lugar do DbSet.</span><span class="sxs-lookup"><span data-stu-id="28096-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="28096-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="28096-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="28096-114">Nesse contexto funciona exatamente da mesma maneira como o contexto que usa a classe DbSet para suas propriedades de conjunto.</span><span class="sxs-lookup"><span data-stu-id="28096-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="28096-115">DbContext com as propriedades definidas somente leitura</span><span class="sxs-lookup"><span data-stu-id="28096-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="28096-116">Se você não quiser expor setters públicos para suas propriedades DbSet ou IDbSet em vez disso, você pode criar propriedades somente leitura e criar as instâncias do conjunto por conta própria.</span><span class="sxs-lookup"><span data-stu-id="28096-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="28096-117">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="28096-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="28096-118">Observe que o DbContext armazena em cache a instância do DbSet retornado do método de conjunto, de modo que cada uma dessas propriedades retornarão a mesma instância sempre que ele é chamado.</span><span class="sxs-lookup"><span data-stu-id="28096-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="28096-119">Descoberta de tipos de entidade do Code First funciona da mesma maneira como faz em propriedades com públicos getters e setters.</span><span class="sxs-lookup"><span data-stu-id="28096-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
