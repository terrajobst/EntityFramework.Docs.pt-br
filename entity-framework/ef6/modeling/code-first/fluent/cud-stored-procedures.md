---
title: Código primeiro inserir, atualizar e excluir procedimentos armazenados – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: a0448fb44dabb2e03b2358aa7b4f69d92cffa15a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994567"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Código primeiro inserir, atualizar e excluir procedimentos armazenados
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Por padrão, Code First configurará todas as entidades para executar a inserção, atualizar e excluir os comandos que usam acesso direto à tabela. Começando no EF6, você pode configurar seu modelo Code First para usar procedimentos armazenados para algumas ou todas as entidades em seu modelo.  

## <a name="basic-entity-mapping"></a>Mapeamento de entidade básica  

Você pode aceitar o uso de procedimentos armazenados para inserir, atualizar e excluir usando a API do Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Isso fará com que Code First usar algumas convenções para criar a forma esperada dos procedimentos armazenados no banco de dados.  

- Chamada de procedimentos armazenados de três  **\<type_name\>inserir**,  **\<type_name\>atualizar** e  **\<type_ nome da\>excluir** (por exemplo, Blog_Insert, Blog_Update e Blog_Delete).  
- Nomes de parâmetro correspondem aos nomes de propriedade.  
  > [!NOTE]
  > Se você usar HasColumnName() ou o atributo de coluna para renomear a coluna para uma determinada propriedade, em seguida, esse nome é usado para parâmetros em vez do nome de propriedade.  
- **O procedimento armazenado insert** terá um parâmetro para cada propriedade, exceto para aqueles marcados como geradas pelo repositório (identidade ou computadas). O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade geradas pelo repositório.  
- **Procedimento armazenado de atualização** terá um parâmetro para cada propriedade, exceto para aqueles marcados com um padrão de repositório gerado de 'Computed'. Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte o *Tokens de simultaneidade* seção abaixo para obter detalhes. O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade computada.  
- **Procedimento armazenado de exclusão** deve ter um parâmetro para o valor da chave de entidade (ou vários parâmetros, se a entidade tem uma chave composta). Além disso, o procedimento de exclusão também deve ter parâmetros para nenhuma chave estrangeira da associação independente na tabela de destino (relações que não têm propriedades de chave estrangeira correspondentes declaradas na entidade). Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte o *Tokens de simultaneidade* seção abaixo para obter detalhes.  

Usando a classe a seguir como exemplo:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Os procedimentos armazenado de padrão seria:  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>Substituindo padrões  

Você pode substituir todo ou parte do que foi configurado por padrão.  

Você pode alterar o nome de um ou mais procedimentos armazenados. Este exemplo renomeia o procedimento armazenado de atualização apenas.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Este exemplo renomeia todos os três procedimentos armazenados.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

Nestes exemplos as chamadas são encadeadas juntos, mas você também pode usar a sintaxe de bloco de lambda.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

Este exemplo renomeia o parâmetro para a propriedade BlogId no procedimento armazenado de atualização.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Essas chamadas são todos encadeável e combinável. Aqui está um exemplo que renomeia todos os três procedimentos armazenados e seus parâmetros.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

Você também pode alterar o nome das colunas no conjunto de resultados que contém os valores de banco de dados gerado.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relações sem uma chave estrangeira na classe (associações independentes)  

Quando uma propriedade de chave estrangeira é incluída na definição de classe, o parâmetro correspondente pode ser renomeado da mesma forma como qualquer outra propriedade. Quando uma relação existente sem uma propriedade de chave estrangeira na classe, o nome do parâmetro padrão é  **\<navigation_property_name\>_\<primary_key_name\>**.  

Por exemplo, as seguintes definições de classe resultaria em um parâmetro de Blog_BlogId sendo esperado nos procedimentos armazenados para inserir e atualizar as postagens.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>Substituindo padrões  

Você pode alterar os parâmetros para chaves estrangeiras que não estão incluídos na classe, fornecendo o caminho para a propriedade de chave primária para o método de parâmetro.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Se você não tiver uma propriedade de navegação na entidade dependente (isto é nenhuma propriedade Post.Blog) e em seguida, você pode usar o método de associação para identificar a outra extremidade da relação e, em seguida, configure os parâmetros que correspondem a cada uma da propriedade de chave (s).  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokens de simultaneidade  

Update e delete armazenados procedimentos talvez precise lidar com simultaneidade:  

- Se a entidade contém tokens de simultaneidade, o procedimento armazenado pode, opcionalmente, ter um parâmetro de saída que retorna o número de linhas atualizado/excluído (linhas afetadas). Esse parâmetro deve ser configurado usando o método RowsAffectedParameter.  
Por padrão o EF usa o valor retornado de ExecuteNonQuery para determinar quantas linhas foram afetadas. Especificando um parâmetro de saída de linhas afetadas é útil se você executar qualquer lógica em seu sproc que resultaria no valor de retorno de ExecuteNonQuery sendo incorreto (da perspectiva do EF) no final da execução.  
- Para cada simultaneidade de token de lá será um parâmetro denominado  **\<property_name\>_Original** (por exemplo, Timestamp_Original). Isso será passado o valor original dessa propriedade – o valor quando consultado do banco de dados.  
    - Tokens de simultaneidade são calculados pelo banco de dados – como os carimbos de hora – terá apenas um parâmetro de valor original.  
    - Propriedades não são computadas que são definidas como tokens de simultaneidade também terá um parâmetro para o novo valor no procedimento de atualização. Isso usa as convenções de nomenclatura já discutidas para novos valores. Um exemplo de como esse token usariam URL do como um token de simultaneidade, o novo valor é necessário porque isso pode ser atualizado para um novo valor pelo seu código (ao contrário de um token de carimbo de hora que só é atualizado pelo banco de dados).  

Este é um exemplo de classe e atualize o procedimento armazenado com um token de simultaneidade de carimbo de hora.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

Aqui está um exemplo de classe e atualize o procedimento armazenado com o token de simultaneidade não computadas.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>Substituindo padrões  

Opcionalmente, você pode introduzir um parâmetro de linhas afetadas.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Para tokens de simultaneidade de banco de dados calculado – em que apenas o valor original é passado – você pode usar apenas o parâmetro padrão renomeando um mecanismo para renomear o parâmetro do valor original.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Para tokens de simultaneidade não computadas – em que tanto o valor original e novo é passado – você pode usar uma sobrecarga de parâmetro que permite que você forneça um nome para cada parâmetro.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Relações Muitos para Muitos  

Usaremos as classes a seguir como exemplo nesta seção.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

Muitas para muitas relações podem ser mapeadas para procedimentos armazenados com a sintaxe a seguir.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Se nenhuma outra configuração é fornecida na forma de procedimento armazenado a seguir é usada por padrão.  

- Dois chamados de procedimentos armazenados  **\<type_one\>\<type_two\>inserir** e  **\<type_one\>\<type_two \>Excluir** (por exemplo, PostTag_Insert e PostTag_Delete).  
- Os parâmetros serão os valores de chave para cada tipo. O nome de cada parâmetro que está sendo **\<type_name\>_\<property_name\>** (por exemplo, Post_PostId e Tag_TagId).

Estes são exemplos de inserção e atualização de procedimentos armazenados.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>Substituindo padrões  

Os nomes de procedimento e o parâmetro podem ser configurados de forma semelhante aos procedimentos armazenado de entidade.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
