---
title: Tratando conflitos de simultaneidade - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489148"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="e28c6-102">Como tratar conflitos de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="e28c6-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="e28c6-103">Otimista de simultaneidade suporá envolve a tentativa de salvar a entidade no banco de dados na esperança de que os dados lá não foram alterados desde a entidade foi carregada.</span><span class="sxs-lookup"><span data-stu-id="e28c6-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="e28c6-104">Se for descoberto que os dados foram alterados, em seguida, uma exceção será lançada e você deve resolver o conflito antes de tentar salvar novamente.</span><span class="sxs-lookup"><span data-stu-id="e28c6-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="e28c6-105">Este tópico aborda como lidar com essas exceções no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e28c6-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="e28c6-106">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="e28c6-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="e28c6-107">Esta postagem não é o local apropriado para uma discussão completa de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="e28c6-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="e28c6-108">As seções a seguir pressupõem algum conhecimento de resolução de simultaneidade e mostram os padrões para tarefas comuns.</span><span class="sxs-lookup"><span data-stu-id="e28c6-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="e28c6-109">Muitos desses padrões fazem usam os tópicos discutidos [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="e28c6-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="e28c6-110">Resolver problemas de simultaneidade quando você estiver usando associações independentes (onde a chave estrangeira não é mapeada para uma propriedade em sua entidade) é muito mais difícil do que quando você estiver usando associações de chave estrangeiras.</span><span class="sxs-lookup"><span data-stu-id="e28c6-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="e28c6-111">Portanto, se você pretende fazer a resolução de simultaneidade em seu aplicativo é aconselhável sempre mapear chaves estrangeiras em suas entidades.</span><span class="sxs-lookup"><span data-stu-id="e28c6-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="e28c6-112">Todos os exemplos abaixo pressupõem que você está usando associações de chave estrangeiras.</span><span class="sxs-lookup"><span data-stu-id="e28c6-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="e28c6-113">Um DbUpdateConcurrencyException é gerada por SaveChanges quando uma exceção de simultaneidade otimista é detectada ao tentar salvar uma entidade que usa associações de chave estrangeiras.</span><span class="sxs-lookup"><span data-stu-id="e28c6-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="e28c6-114">Resolver as exceções de simultaneidade otimista com recarregar (wins do banco de dados)</span><span class="sxs-lookup"><span data-stu-id="e28c6-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="e28c6-115">O método Reload pode ser usado para substituir os valores atuais da entidade com os valores agora no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e28c6-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="e28c6-116">A entidade é, em seguida, geralmente dada de volta para o usuário de alguma forma e ele devem tentar fazer novamente suas alterações e salve novamente.</span><span class="sxs-lookup"><span data-stu-id="e28c6-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="e28c6-117">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e28c6-117">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

<span data-ttu-id="e28c6-118">É uma boa maneira de simular uma exceção de simultaneidade definir um ponto de interrupção na chamada SaveChanges e, em seguida, modificar uma entidade que está sendo salvo no banco de dados usando outra ferramenta como o SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="e28c6-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="e28c6-119">Você também pode inserir uma linha antes SaveChanges para atualizar o banco de dados utilizando o SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="e28c6-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="e28c6-120">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e28c6-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="e28c6-121">O método de entradas no DbUpdateConcurrencyException retorna as instâncias de DbEntityEntry para as entidades que falha ao atualizar.</span><span class="sxs-lookup"><span data-stu-id="e28c6-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="e28c6-122">(Essa propriedade atualmente sempre retorna um valor único para problemas de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="e28c6-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="e28c6-123">Ele pode retornar vários valores para as exceções de atualização geral.) Uma alternativa para algumas situações pode ser para obter entradas para todas as entidades que podem precisar ser recarregados do banco de dados e recarregar de chamada para cada um deles.</span><span class="sxs-lookup"><span data-stu-id="e28c6-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="e28c6-124">Resolver as exceções de simultaneidade otimista como cliente ganha</span><span class="sxs-lookup"><span data-stu-id="e28c6-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="e28c6-125">O exemplo acima que usa o recarregamento às vezes é chamado de wins do banco de dados ou armazenamento vence porque os valores da entidade são substituídos pelos valores do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e28c6-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="e28c6-126">Às vezes, você poderá fazer o oposto e substituir os valores no banco de dados com os valores atualmente na entidade.</span><span class="sxs-lookup"><span data-stu-id="e28c6-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="e28c6-127">Isso é chamado, às vezes, o cliente vence e pode ser feito obtendo os valores atuais do banco de dados e defini-los como os valores originais da entidade.</span><span class="sxs-lookup"><span data-stu-id="e28c6-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="e28c6-128">(Consulte [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md) para obter informações sobre os valores atuais e originais.) Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e28c6-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="e28c6-129">Resolução personalizada de exceções de simultaneidade otimista</span><span class="sxs-lookup"><span data-stu-id="e28c6-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="e28c6-130">Às vezes, você talvez queira combinar os valores atualmente no banco de dados com os valores atualmente na entidade.</span><span class="sxs-lookup"><span data-stu-id="e28c6-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="e28c6-131">Isso geralmente requer alguma interação lógica ou de usuário personalizada.</span><span class="sxs-lookup"><span data-stu-id="e28c6-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="e28c6-132">Por exemplo, você pode apresentar um formulário ao usuário que contém os valores atuais, os valores no banco de dados, e um padrão definido de valores resolvidos.</span><span class="sxs-lookup"><span data-stu-id="e28c6-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="e28c6-133">O usuário, em seguida, edita os valores resolvidos conforme necessário e seria esses valores resolvidos que obterem salvos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e28c6-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="e28c6-134">Isso pode ser feito usando os objetos DbPropertyValues retornado de CurrentValues e GetDatabaseValues na entrada da entidade.</span><span class="sxs-lookup"><span data-stu-id="e28c6-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="e28c6-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e28c6-135">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="e28c6-136">Resolução personalizada de exceções de simultaneidade otimista usando objetos</span><span class="sxs-lookup"><span data-stu-id="e28c6-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="e28c6-137">O código acima usa instâncias DbPropertyValues para passar ao redor de atual, o banco de dados e valores resolvidos.</span><span class="sxs-lookup"><span data-stu-id="e28c6-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="e28c6-138">Às vezes, talvez seja mais fácil usar instâncias de seu tipo de entidade para isso.</span><span class="sxs-lookup"><span data-stu-id="e28c6-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="e28c6-139">Isso pode ser feito usando os métodos de DbPropertyValues ToObject e SetValues.</span><span class="sxs-lookup"><span data-stu-id="e28c6-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="e28c6-140">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e28c6-140">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
