---
title: Os procedimentos armazenados com vários conjuntos de resultados - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489304"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Procedimentos armazenados com vários conjuntos de resultados
Às vezes, ao usar armazenados procedimentos, você precisará retornar mais de um resultado definem. Esse cenário normalmente é usado para reduzir o número de banco de dados de viagens de ida e necessárias para compor uma única tela. Antes do EF5, o Entity Framework permitiria que o procedimento armazenado a ser chamado, mas apenas retornaria o primeiro conjunto de resultados para o código de chamada.

Este artigo mostra duas maneiras que você pode usar para acessar mais de um conjunto de resultados de um procedimento armazenado no Entity Framework. Um que usa apenas o código e funciona com o código primeiro e o EF Designer e outro que funciona apenas com o EF Designer. As ferramentas e suporte de API para isso devem melhorar em futuras versões do Entity Framework.

## <a name="model"></a>Modelo

Os exemplos neste artigo usam um Blog básico e modelo de postagens em que um blog tem muitas postagens e uma postagem pertencer a um único blog. Usaremos um procedimento armazenado no banco de dados que retorna todos os blogs e postagens, algo assim:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Acessar resultados múltiplos conjuntos com código

Podemos executar código de uso para emitir um comando SQL bruto para executar nosso procedimento armazenado. A vantagem dessa abordagem é que ele funciona com o código primeiro e o EF Designer.

Para obter resultados múltiplos define o trabalho que é necessário para descartar a API ObjectContext usando a interface IObjectContextAdapter.

Assim que tivermos uma ObjectContext, em seguida, usamos o método de conversão para converter os resultados do nosso procedimento armazenado em entidades que podem ser controladas e usadas no EF como de costume. O exemplo de código a seguir demonstra isso em ação.

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

O método de traduzir aceita o leitor que recebemos quando executamos o procedimento, um nome de EntitySet e um MergeOption. O nome de EntitySet será o mesmo que a propriedade DbSet em seu contexto derivado. O enum MergeOption controla como os resultados são tratados se a mesma entidade já existe na memória.

Aqui vamos iterar pela coleção de blogs antes que podemos chamar NextResult, isso é importante, considerando o código acima porque o primeiro conjunto de resultados deve ser consumido antes de passar para o próximo conjunto de resultados.

Depois de convertem os dois métodos são chamados, em seguida, as entidades de Blog e Post são controladas pelo EF da mesma maneira que qualquer outra entidade e, portanto, podem ser modificados ou excluídos e salvos como normal.

>[!NOTE]
> O EF não leva nenhum mapeamento em consideração quando ele cria entidades usando o método Translate. Ela simplesmente será compatível com nomes de coluna no conjunto de resultados com os nomes de propriedade em suas classes.

>[!NOTE]
> Que se você tiver habilitado, o carregamento lento ao acessar a propriedade de postagens em uma das entidades de blog, em seguida, EF se conectará ao banco de dados para carregar lentamente todas as postagens, mesmo que nós já carregados todos eles. Isso ocorre porque o EF não é possível saber se você carregou todas as postagens ou se houver mais no banco de dados. Se você quiser evitar isso, em seguida, você precisará desabilitar o carregamento lento.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Vários conjuntos de resultados com configurado em EDMX

>[!NOTE]
> Você deve ter como destino .NET Framework 4.5 para que seja possível configurar vários conjuntos de resultados em EDMX. Se você estiver direcionando o .NET 4.0, você pode usar o método com base no código mostrado na seção anterior.

Se você estiver usando o EF Designer, você também pode modificar o modelo para que ele saiba sobre os diferentes conjuntos de resultados que serão retornados. Uma coisa a saber antes de mão é que as ferramentas não resultados múltiplos definido sem suporte, portanto, você precisará editar manualmente o arquivo edmx. Editando o arquivo edmx que isso vai funcionar, mas também interromperá a validação do modelo no VS. Portanto, se você validar seu modelo sempre receberá erros.

-   Para fazer isso, você precisará adicionar o procedimento armazenado ao seu modelo, como você faria para uma consulta de conjunto de resultados único.
-   Depois de ter isso em seguida, você precisará clicar em seu modelo e selecione **abrir com...** em seguida, **Xml**

    ![Abrir como](~/ef6/media/openas.png)

Depois do modelo aberto como XML, em seguida, você precisa fazer as seguintes etapas:

-   Localize a importação de tipo e a função complexa em seu modelo:

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
-   Atualize a importação de função para que ele seja mapeado para suas entidades, no nosso caso, que ele se parecerá com o seguinte:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Isso informa o modelo que o procedimento armazenado retornará duas coleções, uma das entradas de blog e uma das entradas de postagem.

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

-   Substitua o mapeamento de resultado com uma para cada entidade que está sendo retornada, como o seguinte:

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

Também é possível mapear os conjuntos de resultados para tipos complexos, como aquele criado por padrão. Para fazer isso, você cria um novo tipo complexo, em vez de removê-los e usa os tipos complexos em todos os lugares que você tivesse usado os nomes de entidades nos exemplos acima.

Depois que esses mapeamentos foram alterados, em seguida, você pode salvar o modelo e execute o seguinte código para usar o procedimento armazenado:

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
> Se você editar manualmente o arquivo edmx para seu modelo ele será substituído se você nunca regenerar o modelo do banco de dados.

## <a name="summary"></a>Resumo

Aqui, mostramos dois métodos diferentes para acessar resultados múltiplos conjuntos usando o Entity Framework. Ambos são igualmente válidos dependendo da sua situação e as preferências e você deve escolher o que parece melhor para suas circunstâncias. Ele está planejado que o suporte para vários resultados conjuntos será aprimorada em futuras versões do Entity Framework e que executar as etapas neste documento não serão necessária.
