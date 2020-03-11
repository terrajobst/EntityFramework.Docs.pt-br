---
title: Procedimentos armazenados com vários conjuntos de resultados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418700"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="84450-102">Procedimentos armazenados com vários conjuntos de resultados</span><span class="sxs-lookup"><span data-stu-id="84450-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="84450-103">Às vezes, ao usar procedimentos armazenados, será necessário retornar mais de um conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="84450-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="84450-104">Esse cenário é comumente usado para reduzir o número de viagens de ida e volta necessárias para compor uma única tela.</span><span class="sxs-lookup"><span data-stu-id="84450-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="84450-105"> Antes do EF5, Entity Framework permitiria que o procedimento armazenado fosse chamado, mas só retornaria o primeiro conjunto de resultados ao código de chamada.</span><span class="sxs-lookup"><span data-stu-id="84450-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="84450-106">Este artigo mostrará duas maneiras que você pode usar para acessar mais de um conjunto de resultados de um procedimento armazenado no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84450-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="84450-107">Um que usa apenas o código e trabalha com o Code First e o designer do EF e outro que funciona apenas com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="84450-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="84450-108">O suporte a ferramentas e API para isso deve melhorar em versões futuras do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84450-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="84450-109">Modelo</span><span class="sxs-lookup"><span data-stu-id="84450-109">Model</span></span>

<span data-ttu-id="84450-110">Os exemplos neste artigo usam um blog básico e o modelo de postagens em que um blog tem muitas postagens e uma postagem pertence a um único blog.</span><span class="sxs-lookup"><span data-stu-id="84450-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="84450-111">Usaremos um procedimento armazenado no banco de dados que retorna todos os Blogs e postagens, algo assim:</span><span class="sxs-lookup"><span data-stu-id="84450-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="84450-112">Acessando vários conjuntos de resultados com código</span><span class="sxs-lookup"><span data-stu-id="84450-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="84450-113">Podemos executar o código de uso para emitir um comando SQL bruto para executar nosso procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="84450-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="84450-114">A vantagem dessa abordagem é que ela funciona com o Code First e o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="84450-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="84450-115">Para obter vários conjuntos de resultados trabalhando, precisamos descartar para a API ObjectContext usando a interface IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="84450-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="84450-116">Assim que tivermos um ObjectContext, podemos usar o método translate para converter os resultados do nosso procedimento armazenado em entidades que podem ser rastreadas e usadas no EF normalmente.</span><span class="sxs-lookup"><span data-stu-id="84450-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="84450-117">O exemplo de código a seguir demonstra isso em ação.</span><span class="sxs-lookup"><span data-stu-id="84450-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="84450-118">O método translate aceita o leitor que recebemos quando executamos o procedimento, um nome de EntitySet e um MergeOption.</span><span class="sxs-lookup"><span data-stu-id="84450-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="84450-119">O nome do EntitySet será o mesmo que a propriedade DbSet em seu contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="84450-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="84450-120">A enumeração MergeOption controla como os resultados serão manipulados se a mesma entidade já existir na memória.</span><span class="sxs-lookup"><span data-stu-id="84450-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="84450-121">Aqui, iteramos na coleção de Blogs antes de chamarmos NextResult, isso é importante Considerando o código acima porque o primeiro conjunto de resultados deve ser consumido antes de passar para o próximo conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="84450-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="84450-122">Depois que os dois métodos de conversão são chamados, as entidades blog e post são rastreadas pelo EF da mesma maneira que qualquer outra entidade e, portanto, podem ser modificadas ou excluídas e salvas como normais.</span><span class="sxs-lookup"><span data-stu-id="84450-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="84450-123">O EF não pega nenhum mapeamento em conta quando cria entidades usando o método translate.</span><span class="sxs-lookup"><span data-stu-id="84450-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="84450-124">Ele simplesmente fará a correspondência de nomes de coluna no conjunto de resultados com nomes de propriedade em suas classes.</span><span class="sxs-lookup"><span data-stu-id="84450-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="84450-125">Se você tiver o carregamento lento habilitado, acessando a propriedade postagens em uma das entidades do blog e, em seguida, o EF se conectará ao banco de dados para carregar lentamente todas as postagens, mesmo que já tenhamos carregado todas elas.</span><span class="sxs-lookup"><span data-stu-id="84450-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="84450-126">Isso ocorre porque o EF não pode saber se você carregou ou não todas as postagens ou se há mais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="84450-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="84450-127">Se você quiser evitar isso, será necessário desabilitar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="84450-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="84450-128">Vários conjuntos de resultados com configurados em EDMX</span><span class="sxs-lookup"><span data-stu-id="84450-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="84450-129">Você deve direcionar .NET Framework 4,5 para poder configurar vários conjuntos de resultados em EDMX.</span><span class="sxs-lookup"><span data-stu-id="84450-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="84450-130">Se você estiver visando o .NET 4,0, poderá usar o método baseado em código mostrado na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="84450-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="84450-131">Se você estiver usando o designer do EF, também poderá modificar seu modelo para que ele saiba sobre os diferentes conjuntos de resultados que serão retornados.</span><span class="sxs-lookup"><span data-stu-id="84450-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="84450-132">Uma coisa a ser lembrada antes da mão é que as ferramentas não reconhecem vários conjuntos de resultados, portanto, você precisará editar manualmente o arquivo edmx.</span><span class="sxs-lookup"><span data-stu-id="84450-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="84450-133">Editar o arquivo EDMX como isso funcionará, mas também interromperá a validação do modelo no VS.</span><span class="sxs-lookup"><span data-stu-id="84450-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="84450-134">Portanto, se você validar seu modelo, sempre receberá erros.</span><span class="sxs-lookup"><span data-stu-id="84450-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="84450-135">Para fazer isso, você precisa adicionar o procedimento armazenado ao seu modelo como faria para uma única consulta de conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="84450-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="84450-136">Depois de fazer isso, você precisará clicar com o botão direito do mouse em seu modelo e selecionar **abrir com..**</span><span class="sxs-lookup"><span data-stu-id="84450-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="84450-137">em seguida, **XML**</span><span class="sxs-lookup"><span data-stu-id="84450-137">then **Xml**</span></span>

    ![Abrir como](~/ef6/media/openas.png)

<span data-ttu-id="84450-139">Depois que o modelo for aberto como XML, você precisará executar as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="84450-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="84450-140">Localize o tipo complexo e a importação de função em seu modelo:</span><span class="sxs-lookup"><span data-stu-id="84450-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="84450-141">Remover o tipo complexo</span><span class="sxs-lookup"><span data-stu-id="84450-141">Remove the complex type</span></span>
-   <span data-ttu-id="84450-142">Atualize a importação de função para que ela seja mapeada para suas entidades, em nosso caso será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="84450-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="84450-143">Isso informa ao modelo que o procedimento armazenado retornará duas coleções, uma das entradas do blog e uma das entradas de post.</span><span class="sxs-lookup"><span data-stu-id="84450-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="84450-144">Localize o elemento de mapeamento de função:</span><span class="sxs-lookup"><span data-stu-id="84450-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="84450-145">Substitua o mapeamento de resultado por um para cada entidade que está sendo retornada, como a seguinte:</span><span class="sxs-lookup"><span data-stu-id="84450-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="84450-146">Também é possível mapear os conjuntos de resultados para tipos complexos, como aquele criado por padrão.</span><span class="sxs-lookup"><span data-stu-id="84450-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="84450-147">Para fazer isso, você cria um novo tipo complexo, em vez de removê-los e usa os tipos complexos em todos os lugares em que você usou os nomes de entidade nos exemplos acima.</span><span class="sxs-lookup"><span data-stu-id="84450-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="84450-148">Depois que esses mapeamentos tiverem sido alterados, você poderá salvar o modelo e executar o seguinte código para usar o procedimento armazenado:</span><span class="sxs-lookup"><span data-stu-id="84450-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="84450-149">Se você editar manualmente o arquivo EDMX para seu modelo, ele será substituído se você já regenerar o modelo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="84450-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="84450-150">Resumo</span><span class="sxs-lookup"><span data-stu-id="84450-150">Summary</span></span>

<span data-ttu-id="84450-151">Aqui, mostramos dois métodos diferentes de acesso a vários conjuntos de resultados usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="84450-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="84450-152">Ambos são igualmente válidos, dependendo de sua situação e preferências, e você deve escolher aquele que pareça melhor para suas circunstâncias.</span><span class="sxs-lookup"><span data-stu-id="84450-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="84450-153">Está planejado que o suporte para vários conjuntos de resultados será melhorado em versões futuras do Entity Framework e que a execução das etapas neste documento não será mais necessária.</span><span class="sxs-lookup"><span data-stu-id="84450-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
