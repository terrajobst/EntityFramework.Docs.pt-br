---
title: Trabalhando com os estados da entidade - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490622"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="ee9c7-102">Trabalhando com os estados da entidade</span><span class="sxs-lookup"><span data-stu-id="ee9c7-102">Working with entity states</span></span>
<span data-ttu-id="ee9c7-103">Este tópico abordará como adicionar e anexar entidades a um contexto e como o Entity Framework processa esses durante SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="ee9c7-104">Entity Framework se encarrega de acompanhamento para o estado das entidades enquanto eles estiverem conectados a um contexto, mas em cenários desconectados ou de N camadas você pode permitir que o EF sabe que estado suas entidades deve estar no.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="ee9c7-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="ee9c7-106">Estados da entidade e SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ee9c7-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="ee9c7-107">Uma entidade pode estar em um dos cinco estados, conforme definido pela enumeração EntityState.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="ee9c7-108">Esses estados são:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-108">These states are:</span></span>  

- <span data-ttu-id="ee9c7-109">Adicionado: a entidade está sendo acompanhada pelo contexto, mas ainda não existe no banco de dados</span><span class="sxs-lookup"><span data-stu-id="ee9c7-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="ee9c7-110">Inalterado: a entidade está sendo acompanhada pelo contexto e existe no banco de dados e seus valores de propriedade não foram alterados entre os valores no banco de dados</span><span class="sxs-lookup"><span data-stu-id="ee9c7-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="ee9c7-111">Modificado: a entidade está sendo acompanhada pelo contexto e existe no banco de dados, e alguns ou todos os seus valores de propriedade foram modificados</span><span class="sxs-lookup"><span data-stu-id="ee9c7-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="ee9c7-112">Excluído: a entidade está sendo acompanhada pelo contexto e existe no banco de dados, mas foi marcada para exclusão do banco de dados na próxima vez em que o SaveChanges é chamado</span><span class="sxs-lookup"><span data-stu-id="ee9c7-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="ee9c7-113">Desanexado: a entidade não está sendo acompanhada pelo contexto</span><span class="sxs-lookup"><span data-stu-id="ee9c7-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="ee9c7-114">SaveChanges faz coisas diferentes para entidades em diferentes estados:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="ee9c7-115">Entidades inalteradas não são tocadas pelo SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="ee9c7-116">Atualizações não são enviadas ao banco de dados para entidades no estado inalterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="ee9c7-117">Adicionada a entidades são inseridas no banco de dados e, em seguida, se tornam inalteradas quando SaveChanges retorna.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="ee9c7-118">Entidades modificadas são atualizadas no banco de dados e, em seguida, se tornam inalteradas quando SaveChanges retorna.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="ee9c7-119">Entidades excluídas são excluídas do banco de dados e, em seguida, são desanexadas do contexto.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="ee9c7-120">Os exemplos a seguir mostram maneiras em que o estado de uma entidade ou um grafo de entidade pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="ee9c7-121">Adicionar uma nova entidade ao contexto</span><span class="sxs-lookup"><span data-stu-id="ee9c7-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="ee9c7-122">Uma nova entidade pode ser adicionada ao contexto chamando o método Add no DbSet.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="ee9c7-123">Isso coloca a entidade no estado adicionado, que significa que ele será inserido no banco de dados da próxima vez que o SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="ee9c7-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-125">É outra maneira de adicionar uma nova entidade ao contexto alterar seu estado como adicionado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="ee9c7-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-127">Por fim, você pode adicionar uma nova entidade ao contexto, conectá-lo a outra entidade que já está sendo rastreada.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="ee9c7-128">Isso pode ser, adicionando a nova entidade para a propriedade de navegação de coleção de outra entidade ou definindo uma propriedade de navegação de referência de outra entidade para apontar para a nova entidade.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="ee9c7-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-130">Observe que, para todos esses exemplos se a entidade que está sendo adicionada tiver referências a outras entidades que não são rastreadas ainda, em seguida, essas novas entidades também será adicionado ao contexto e será inserido no banco de dados da próxima vez que o SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="ee9c7-131">Anexar uma entidade existente ao contexto</span><span class="sxs-lookup"><span data-stu-id="ee9c7-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="ee9c7-132">Se você tiver uma entidade que você sabe que já existe no banco de dados, mas que não está atualmente sendo acompanhado pelo contexto, em seguida, você pode informar o contexto para controlar a entidade usando o método anexar no DbSet.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="ee9c7-133">A entidade será no estado inalterado no contexto.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="ee9c7-134">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-135">Observe que nenhuma alteração será feita no banco de dados se SaveChanges for chamado sem fazer qualquer outra manipulação da entidade anexada.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="ee9c7-136">Isso ocorre porque a entidade está no estado inalterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="ee9c7-137">É outra maneira de anexar uma entidade existente ao contexto alterar seu estado como não alterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="ee9c7-138">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-139">Observe que para ambos os exemplos se a entidade que está sendo anexada tem referências a outras entidades que ainda não são controladas, em seguida, essas novas entidades serão também anexado ao contexto no estado inalterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="ee9c7-140">Anexando um existente, mas a entidade modificada para o contexto</span><span class="sxs-lookup"><span data-stu-id="ee9c7-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="ee9c7-141">Se você tiver uma entidade que você sabe que já existe no banco de dados, mas as alterações que talvez tenham sido feitas, em seguida, você pode informar o contexto para anexar a entidade e definir seu estado para modificado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="ee9c7-142">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-143">Quando você altera o estado para modificado todas as propriedades da entidade serão marcadas como modificada e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="ee9c7-144">Observe que, se a entidade que está sendo anexada tem referências a outras entidades que ainda não são controladas, essas novas entidades será anexado ao contexto no estado inalterado — eles não serão automaticamente feitos modificado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="ee9c7-145">Se você tiver várias entidades que precisam ser marcadas modificado deve definir o estado individualmente para cada uma dessas entidades.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="ee9c7-146">Alterando o estado de uma entidade rastreada</span><span class="sxs-lookup"><span data-stu-id="ee9c7-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="ee9c7-147">Você pode alterar o estado de uma entidade que já está sendo controlado definindo a propriedade State em sua entrada.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="ee9c7-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ee9c7-149">Observe que o Add ou Attach de chamada para uma entidade que já é controlada também pode ser usada para alterar o estado da entidade.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="ee9c7-150">Por exemplo, a anexação de chamada para uma entidade que está atualmente no estado adicionado mudará seu estado como não alterado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="ee9c7-151">Inserir ou atualizar padrão</span><span class="sxs-lookup"><span data-stu-id="ee9c7-151">Insert or update pattern</span></span>  

<span data-ttu-id="ee9c7-152">Um padrão comum para alguns aplicativos é adicionar uma entidade como novo (resultando em uma inserção de banco de dados) ou anexar uma entidade já existente e marcá-la como modificada (resultando em uma atualização de banco de dados), dependendo do valor da chave primária.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="ee9c7-153">Por exemplo, ao usar chaves primárias do banco de dados gerado inteiro é comum para tratar de uma entidade com uma chave zero como novo e uma entidade com uma chave diferente de zero já existente.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="ee9c7-154">Esse padrão pode ser obtido, definindo o estado da entidade com base em uma verificação do valor de chave primária.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="ee9c7-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="ee9c7-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="ee9c7-156">Observe que, quando você altera o estado para modificado todas as propriedades da entidade serão marcadas como modificada e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.</span><span class="sxs-lookup"><span data-stu-id="ee9c7-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
