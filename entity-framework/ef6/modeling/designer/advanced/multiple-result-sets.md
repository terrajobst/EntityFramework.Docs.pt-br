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
# <a name="stored-procedures-with-multiple-result-sets"></a>Procedimentos armazenados com vários conjuntos de resultados
Às vezes, ao usar procedimentos armazenados, será necessário retornar mais de um conjunto de resultados. Esse cenário é comumente usado para reduzir o número de viagens de ida e volta necessárias para compor uma única tela. Antes do EF5, Entity Framework permitiria que o procedimento armazenado fosse chamado, mas só retornaria o primeiro conjunto de resultados ao código de chamada.

Este artigo mostrará duas maneiras que você pode usar para acessar mais de um conjunto de resultados de um procedimento armazenado no Entity Framework. Um que usa apenas o código e trabalha com o Code First e o designer do EF e outro que funciona apenas com o designer do EF. O suporte a ferramentas e API para isso deve melhorar em versões futuras do Entity Framework.

## <a name="model"></a>Modelo

Os exemplos neste artigo usam um blog básico e o modelo de postagens em que um blog tem muitas postagens e uma postagem pertence a um único blog. Usaremos um procedimento armazenado no banco de dados que retorna todos os Blogs e postagens, algo assim:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Acessando vários conjuntos de resultados com código

Podemos executar o código de uso para emitir um comando SQL bruto para executar nosso procedimento armazenado. A vantagem dessa abordagem é que ela funciona com o Code First e o designer do EF.

Para obter vários conjuntos de resultados trabalhando, precisamos descartar para a API ObjectContext usando a interface IObjectContextAdapter.

Assim que tivermos um ObjectContext, podemos usar o método translate para converter os resultados do nosso procedimento armazenado em entidades que podem ser rastreadas e usadas no EF normalmente. O exemplo de código a seguir demonstra isso em ação.

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

O método translate aceita o leitor que recebemos quando executamos o procedimento, um nome de EntitySet e um MergeOption. O nome do EntitySet será o mesmo que a propriedade DbSet em seu contexto derivado. A enumeração MergeOption controla como os resultados serão manipulados se a mesma entidade já existir na memória.

Aqui, iteramos na coleção de Blogs antes de chamarmos NextResult, isso é importante Considerando o código acima porque o primeiro conjunto de resultados deve ser consumido antes de passar para o próximo conjunto de resultados.

Depois que os dois métodos de conversão são chamados, as entidades blog e post são rastreadas pelo EF da mesma maneira que qualquer outra entidade e, portanto, podem ser modificadas ou excluídas e salvas como normais.

>[!NOTE]
> O EF não pega nenhum mapeamento em conta quando cria entidades usando o método translate. Ele simplesmente fará a correspondência de nomes de coluna no conjunto de resultados com nomes de propriedade em suas classes.

>[!NOTE]
> Se você tiver o carregamento lento habilitado, acessando a propriedade postagens em uma das entidades do blog e, em seguida, o EF se conectará ao banco de dados para carregar lentamente todas as postagens, mesmo que já tenhamos carregado todas elas. Isso ocorre porque o EF não pode saber se você carregou ou não todas as postagens ou se há mais no banco de dados. Se você quiser evitar isso, será necessário desabilitar o carregamento lento.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Vários conjuntos de resultados com configurados em EDMX

>[!NOTE]
> Você deve direcionar .NET Framework 4,5 para poder configurar vários conjuntos de resultados em EDMX. Se você estiver visando o .NET 4,0, poderá usar o método baseado em código mostrado na seção anterior.

Se você estiver usando o designer do EF, também poderá modificar seu modelo para que ele saiba sobre os diferentes conjuntos de resultados que serão retornados. Uma coisa a ser lembrada antes da mão é que as ferramentas não reconhecem vários conjuntos de resultados, portanto, você precisará editar manualmente o arquivo edmx. Editar o arquivo EDMX como isso funcionará, mas também interromperá a validação do modelo no VS. Portanto, se você validar seu modelo, sempre receberá erros.

-   Para fazer isso, você precisa adicionar o procedimento armazenado ao seu modelo como faria para uma única consulta de conjunto de resultados.
-   Depois de fazer isso, você precisará clicar com o botão direito do mouse em seu modelo e selecionar **abrir com..** em seguida, **XML**

    ![Abrir como](~/ef6/media/openas.png)

Depois que o modelo for aberto como XML, você precisará executar as seguintes etapas:

-   Localize o tipo complexo e a importação de função em seu modelo:

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

 

-   Remover o tipo complexo
-   Atualize a importação de função para que ela seja mapeada para suas entidades, em nosso caso será semelhante ao seguinte:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Isso informa ao modelo que o procedimento armazenado retornará duas coleções, uma das entradas do blog e uma das entradas de post.

-   Localize o elemento de mapeamento de função:

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

-   Substitua o mapeamento de resultado por um para cada entidade que está sendo retornada, como a seguinte:

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

Também é possível mapear os conjuntos de resultados para tipos complexos, como aquele criado por padrão. Para fazer isso, você cria um novo tipo complexo, em vez de removê-los e usa os tipos complexos em todos os lugares em que você usou os nomes de entidade nos exemplos acima.

Depois que esses mapeamentos tiverem sido alterados, você poderá salvar o modelo e executar o seguinte código para usar o procedimento armazenado:

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
> Se você editar manualmente o arquivo EDMX para seu modelo, ele será substituído se você já regenerar o modelo do banco de dados.

## <a name="summary"></a>Resumo

Aqui, mostramos dois métodos diferentes de acesso a vários conjuntos de resultados usando Entity Framework. Ambos são igualmente válidos, dependendo de sua situação e preferências, e você deve escolher aquele que pareça melhor para suas circunstâncias. Está planejado que o suporte para vários conjuntos de resultados será melhorado em versões futuras do Entity Framework e que a execução das etapas neste documento não será mais necessária.
