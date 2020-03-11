---
title: Definindo DbSets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419090"
---
# <a name="defining-dbsets"></a><span data-ttu-id="2de02-102">Definindo DbSets</span><span class="sxs-lookup"><span data-stu-id="2de02-102">Defining DbSets</span></span>
<span data-ttu-id="2de02-103">Ao desenvolver com o fluxo de trabalho de Code First, você define um DbContext derivado que representa a sessão com o banco de dados e expõe um DbSet para cada tipo em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2de02-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="2de02-104">Este tópico aborda as várias maneiras pelas quais você pode definir as propriedades DbSet.</span><span class="sxs-lookup"><span data-stu-id="2de02-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="2de02-105">DbContext com propriedades DbSet</span><span class="sxs-lookup"><span data-stu-id="2de02-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="2de02-106">O caso comum mostrado em Code First exemplos é ter um DbContext com propriedades públicas automáticas de DbSet para os tipos de entidade do seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2de02-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="2de02-107">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2de02-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="2de02-108">Quando usado no modo de Code First, isso irá configurar Blogs e postagens como tipos de entidade, bem como configurar outros tipos acessíveis a partir deles.</span><span class="sxs-lookup"><span data-stu-id="2de02-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="2de02-109">Além disso, o DbContext chamará automaticamente o setter para cada uma dessas propriedades para definir uma instância do DbSet apropriado.</span><span class="sxs-lookup"><span data-stu-id="2de02-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="2de02-110">DbContext com propriedades IDbSet</span><span class="sxs-lookup"><span data-stu-id="2de02-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="2de02-111">Há situações, como ao criar simulações ou falsificações, em que é mais útil declarar as propriedades definidas usando uma interface.</span><span class="sxs-lookup"><span data-stu-id="2de02-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="2de02-112">Nesses casos, a interface IDbSet pode ser usada no lugar de DbSet.</span><span class="sxs-lookup"><span data-stu-id="2de02-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="2de02-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2de02-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="2de02-114">Esse contexto funciona exatamente da mesma forma que o contexto que usa a classe DbSet para suas propriedades set.</span><span class="sxs-lookup"><span data-stu-id="2de02-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="2de02-115">DbContext com propriedades de conjunto somente leitura</span><span class="sxs-lookup"><span data-stu-id="2de02-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="2de02-116">Se você não deseja expor os setters públicos para suas propriedades DbSet ou IDbSet, você pode criar propriedades somente leitura e criar as instâncias definidas por conta própria.</span><span class="sxs-lookup"><span data-stu-id="2de02-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="2de02-117">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2de02-117">For example:</span></span>  

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

<span data-ttu-id="2de02-118">Observe que DbContext armazena em cache a instância de DbSet retornada do método set para que cada uma dessas propriedades retorne a mesma instância toda vez que for chamada.</span><span class="sxs-lookup"><span data-stu-id="2de02-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="2de02-119">A descoberta de tipos de entidade para Code First funciona da mesma maneira aqui para propriedades com getters e setters públicos.</span><span class="sxs-lookup"><span data-stu-id="2de02-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
