---
title: Os procedimentos armazenados com vários conjuntos de resultados - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119850"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="d51e1-102">Procedimentos armazenados com vários conjuntos de resultados</span><span class="sxs-lookup"><span data-stu-id="d51e1-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="d51e1-103">Às vezes, ao usar armazenados procedimentos, você precisará retornar mais de um resultado definem.</span><span class="sxs-lookup"><span data-stu-id="d51e1-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="d51e1-104">Esse cenário normalmente é usado para reduzir o número de banco de dados de viagens de ida e necessárias para compor uma única tela.</span><span class="sxs-lookup"><span data-stu-id="d51e1-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="d51e1-105">Antes do EF5, o Entity Framework permitiria que o procedimento armazenado a ser chamado, mas apenas retornaria o primeiro conjunto de resultados para o código de chamada.</span><span class="sxs-lookup"><span data-stu-id="d51e1-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="d51e1-106">Este artigo mostra duas maneiras que você pode usar para acessar mais de um conjunto de resultados de um procedimento armazenado no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d51e1-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="d51e1-107">Um que usa apenas o código e funciona com o código primeiro e o EF Designer e outro que funciona apenas com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="d51e1-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="d51e1-108">As ferramentas e suporte de API para isso devem melhorar em futuras versões do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d51e1-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="d51e1-109">Modelo</span><span class="sxs-lookup"><span data-stu-id="d51e1-109">Model</span></span>

<span data-ttu-id="d51e1-110">Os exemplos neste artigo usam um Blog básico e modelo de postagens em que um blog tem muitas postagens e uma postagem pertencer a um único blog.</span><span class="sxs-lookup"><span data-stu-id="d51e1-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="d51e1-111">Usaremos um procedimento armazenado no banco de dados que retorna todos os blogs e postagens, algo assim:</span><span class="sxs-lookup"><span data-stu-id="d51e1-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="d51e1-112">Acessar resultados múltiplos conjuntos com código</span><span class="sxs-lookup"><span data-stu-id="d51e1-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="d51e1-113">Podemos executar código de uso para emitir um comando SQL bruto para executar nosso procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="d51e1-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="d51e1-114">A vantagem dessa abordagem é que ele funciona com o código primeiro e o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="d51e1-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="d51e1-115">Para obter resultados múltiplos define o trabalho que é necessário para descartar a API ObjectContext usando a interface IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="d51e1-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="d51e1-116">Assim que tivermos uma ObjectContext, em seguida, usamos o método de conversão para converter os resultados do nosso procedimento armazenado em entidades que podem ser controladas e usadas no EF como de costume.</span><span class="sxs-lookup"><span data-stu-id="d51e1-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="d51e1-117">O exemplo de código a seguir demonstra isso em ação.</span><span class="sxs-lookup"><span data-stu-id="d51e1-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="d51e1-118">O método de traduzir aceita o leitor que recebemos quando executamos o procedimento, um nome de EntitySet e um MergeOption.</span><span class="sxs-lookup"><span data-stu-id="d51e1-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="d51e1-119">O nome de EntitySet será o mesmo que a propriedade DbSet em seu contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="d51e1-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="d51e1-120">O enum MergeOption controla como os resultados são tratados se a mesma entidade já existe na memória.</span><span class="sxs-lookup"><span data-stu-id="d51e1-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="d51e1-121">Aqui vamos iterar pela coleção de blogs antes que podemos chamar NextResult, isso é importante, considerando o código acima porque o primeiro conjunto de resultados deve ser consumido antes de passar para o próximo conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="d51e1-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="d51e1-122">Depois de convertem os dois métodos são chamados, em seguida, as entidades de Blog e Post são controladas pelo EF da mesma maneira que qualquer outra entidade e, portanto, podem ser modificados ou excluídos e salvos como normal.</span><span class="sxs-lookup"><span data-stu-id="d51e1-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="d51e1-123">O EF não leva nenhum mapeamento em consideração quando ele cria entidades usando o método Translate.</span><span class="sxs-lookup"><span data-stu-id="d51e1-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="d51e1-124">Ela simplesmente será compatível com nomes de coluna no conjunto de resultados com os nomes de propriedade em suas classes.</span><span class="sxs-lookup"><span data-stu-id="d51e1-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="d51e1-125">Que se você tiver habilitado, o carregamento lento ao acessar a propriedade de postagens em uma das entidades de blog, em seguida, EF se conectará ao banco de dados para carregar lentamente todas as postagens, mesmo que nós já carregados todos eles.</span><span class="sxs-lookup"><span data-stu-id="d51e1-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="d51e1-126">Isso ocorre porque o EF não é possível saber se você carregou todas as postagens ou se houver mais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d51e1-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="d51e1-127">Se você quiser evitar isso, em seguida, você precisará desabilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="d51e1-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="d51e1-128">Vários conjuntos de resultados com configurado em EDMX</span><span class="sxs-lookup"><span data-stu-id="d51e1-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="d51e1-129">Você deve ter como destino .NET Framework 4.5 para que seja possível configurar vários conjuntos de resultados em EDMX.</span><span class="sxs-lookup"><span data-stu-id="d51e1-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="d51e1-130">Se você estiver direcionando o .NET 4.0, você pode usar o método com base no código mostrado na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="d51e1-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="d51e1-131">Se você estiver usando o EF Designer, você também pode modificar o modelo para que ele saiba sobre os diferentes conjuntos de resultados que serão retornados.</span><span class="sxs-lookup"><span data-stu-id="d51e1-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="d51e1-132">Uma coisa a saber antes de mão é que as ferramentas não resultados múltiplos definido sem suporte, portanto, você precisará editar manualmente o arquivo edmx.</span><span class="sxs-lookup"><span data-stu-id="d51e1-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="d51e1-133">Editando o arquivo edmx que isso vai funcionar, mas também interromperá a validação do modelo no VS.</span><span class="sxs-lookup"><span data-stu-id="d51e1-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="d51e1-134">Portanto, se você validar seu modelo sempre receberá erros.</span><span class="sxs-lookup"><span data-stu-id="d51e1-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="d51e1-135">Para fazer isso, você precisará adicionar o procedimento armazenado ao seu modelo, como você faria para uma consulta de conjunto de resultados único.</span><span class="sxs-lookup"><span data-stu-id="d51e1-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="d51e1-136">Depois de ter isso em seguida, você precisará clicar em seu modelo e selecione **abrir com...**</span><span class="sxs-lookup"><span data-stu-id="d51e1-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="d51e1-137">em seguida, **Xml**</span><span class="sxs-lookup"><span data-stu-id="d51e1-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="d51e1-139">Depois do modelo aberto como XML, em seguida, você precisa fazer as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="d51e1-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="d51e1-140">Localize a importação de tipo e a função complexa em seu modelo:</span><span class="sxs-lookup"><span data-stu-id="d51e1-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="d51e1-141">Remover o tipo complexo</span><span class="sxs-lookup"><span data-stu-id="d51e1-141">Remove the complex type</span></span>
-   <span data-ttu-id="d51e1-142">Atualize a importação de função para que ele seja mapeado para suas entidades, no nosso caso, que ele se parecerá com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d51e1-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="d51e1-143">Isso informa o modelo que o procedimento armazenado retornará duas coleções, uma das entradas de blog e uma das entradas de postagem.</span><span class="sxs-lookup"><span data-stu-id="d51e1-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="d51e1-144">Localize o elemento de mapeamento de função:</span><span class="sxs-lookup"><span data-stu-id="d51e1-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="d51e1-145">Substitua o mapeamento de resultado com uma para cada entidade que está sendo retornada, como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d51e1-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="d51e1-146">Também é possível mapear os conjuntos de resultados para tipos complexos, como aquele criado por padrão.</span><span class="sxs-lookup"><span data-stu-id="d51e1-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="d51e1-147">Para fazer isso, você cria um novo tipo complexo, em vez de removê-los e usa os tipos complexos em todos os lugares que você tivesse usado os nomes de entidades nos exemplos acima.</span><span class="sxs-lookup"><span data-stu-id="d51e1-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="d51e1-148">Depois que esses mapeamentos foram alterados, em seguida, você pode salvar o modelo e execute o seguinte código para usar o procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="d51e1-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="d51e1-149">Se você editar manualmente o arquivo edmx para seu modelo ele será substituído se você nunca regenerar o modelo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d51e1-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="d51e1-150">Resumo</span><span class="sxs-lookup"><span data-stu-id="d51e1-150">Summary</span></span>

<span data-ttu-id="d51e1-151">Aqui, mostramos dois métodos diferentes para acessar resultados múltiplos conjuntos usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d51e1-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="d51e1-152">Ambos são igualmente válidos dependendo da sua situação e as preferências e você deve escolher o que parece melhor para suas circunstâncias.</span><span class="sxs-lookup"><span data-stu-id="d51e1-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="d51e1-153">Ele está planejado que o suporte para vários resultados conjuntos será aprimorada em futuras versões do Entity Framework e que executar as etapas neste documento não serão necessária.</span><span class="sxs-lookup"><span data-stu-id="d51e1-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
