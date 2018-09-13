---
title: Trabalhar com proxies - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489811"
---
# <a name="working-with-proxies"></a><span data-ttu-id="a4e55-102">Trabalhar com proxies</span><span class="sxs-lookup"><span data-stu-id="a4e55-102">Working with proxies</span></span>
<span data-ttu-id="a4e55-103">Ao criar instâncias de tipos de entidade POCO, o Entity Framework geralmente cria instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade.</span><span class="sxs-lookup"><span data-stu-id="a4e55-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="a4e55-104">Esse proxy substitui alguns virtuais propriedades da entidade para inserir os ganchos para executar as ações automaticamente quando a propriedade é acessada.</span><span class="sxs-lookup"><span data-stu-id="a4e55-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="a4e55-105">Por exemplo, esse mecanismo é usado para dar suporte a carregamento lento de relações.</span><span class="sxs-lookup"><span data-stu-id="a4e55-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="a4e55-106">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a4e55-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="a4e55-107">Desabilitando a criação de proxy</span><span class="sxs-lookup"><span data-stu-id="a4e55-107">Disabling proxy creation</span></span>  

<span data-ttu-id="a4e55-108">Às vezes é útil para evitar que o Entity Framework desde a criação de instâncias de proxy.</span><span class="sxs-lookup"><span data-stu-id="a4e55-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="a4e55-109">Por exemplo, serializar instâncias de proxy não é consideravelmente mais fácil do que serializar instâncias do proxy.</span><span class="sxs-lookup"><span data-stu-id="a4e55-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="a4e55-110">Criação de proxy pode ser desativada desmarcando o sinalizador ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="a4e55-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="a4e55-111">Um único lugar, você pode fazer isso é no construtor de seu contexto.</span><span class="sxs-lookup"><span data-stu-id="a4e55-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="a4e55-112">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a4e55-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="a4e55-113">Observe que o EF não criará proxies para os tipos em que não há nada para fazer o proxy.</span><span class="sxs-lookup"><span data-stu-id="a4e55-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="a4e55-114">Isso significa que você também pode evitar proxies tendo tipos que são lacrados e/ou não têm nenhuma propriedade virtual.</span><span class="sxs-lookup"><span data-stu-id="a4e55-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="a4e55-115">Criar explicitamente uma instância de um proxy</span><span class="sxs-lookup"><span data-stu-id="a4e55-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="a4e55-116">Uma instância do proxy não será criada se você criar uma instância de uma entidade usando o operador new.</span><span class="sxs-lookup"><span data-stu-id="a4e55-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="a4e55-117">Isso pode não ser um problema, mas se você precisar criar uma instância do proxy (por exemplo, de modo que funcionarão lento carregamento ou proxy de controle de alterações), em seguida, você pode fazer isso usando o método Create da DbSet.</span><span class="sxs-lookup"><span data-stu-id="a4e55-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="a4e55-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a4e55-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="a4e55-119">A versão genérica de criação pode ser usada se você quiser criar uma instância de um tipo de entidade derivada.</span><span class="sxs-lookup"><span data-stu-id="a4e55-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="a4e55-120">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a4e55-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="a4e55-121">Observe que o método Create não adicionar ou anexar a entidade criada ao contexto.</span><span class="sxs-lookup"><span data-stu-id="a4e55-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="a4e55-122">Observe que o método Create apenas criar uma instância do tipo de entidade própria se a criação de um tipo de proxy para a entidade não teria nenhum valor porque ele não faria nada.</span><span class="sxs-lookup"><span data-stu-id="a4e55-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="a4e55-123">Por exemplo, se o tipo de entidade é selado e/ou não tem nenhuma propriedade virtual, em seguida, criar apenas criará uma instância do tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="a4e55-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="a4e55-124">Obtendo o tipo de entidade real de um tipo de proxy</span><span class="sxs-lookup"><span data-stu-id="a4e55-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="a4e55-125">Tipos de proxy têm nomes que algo parecido com isto:</span><span class="sxs-lookup"><span data-stu-id="a4e55-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="a4e55-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="a4e55-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="a4e55-127">Você pode encontrar o tipo de entidade para este tipo de proxy usando o método GetObjectType de ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a4e55-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="a4e55-128">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a4e55-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="a4e55-129">Observe que, se o tipo passado para GetObjectType é uma instância de um tipo de entidade que não é um tipo de proxy, em seguida, o tipo de entidade ainda é retornada.</span><span class="sxs-lookup"><span data-stu-id="a4e55-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="a4e55-130">Isso significa que você sempre pode usar esse método para obter o tipo de entidade real sem qualquer outra verificação para ver se o tipo for um tipo de proxy ou não.</span><span class="sxs-lookup"><span data-stu-id="a4e55-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
