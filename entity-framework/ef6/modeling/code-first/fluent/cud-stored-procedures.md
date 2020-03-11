---
title: Code First inserir, atualizar e excluir procedimentos armazenados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419083"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code First inserir, atualizar e excluir procedimentos armazenados
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Por padrão, Code First configurará todas as entidades para executar comandos INSERT, Update e DELETE usando acesso direto à tabela. A partir do EF6, você pode configurar seu modelo de Code First para usar procedimentos armazenados para algumas ou todas as entidades em seu modelo.  

## <a name="basic-entity-mapping"></a>Mapeamento de entidade básica  

Você pode optar por usar procedimentos armazenados para inserir, atualizar e excluir usando a API fluente.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Isso fará com que Code First Use algumas convenções para criar a forma esperada dos procedimentos armazenados no banco de dados.  

- Três procedimentos armazenados chamados **\<type_name\>_Insert**, **\<type_name\>_Update** e **\<** type_name\>_Delete (por exemplo, Blog_Insert, Blog_Update e Blog_Delete).  
- Os nomes de parâmetro correspondem aos nomes de propriedade.  
  > [!NOTE]
  > Se você usar HasColumnName () ou o atributo de coluna para renomear a coluna para uma determinada propriedade, esse nome será usado para parâmetros em vez do nome da propriedade.  
- **O procedimento armazenado INSERT** terá um parâmetro para cada propriedade, exceto aqueles marcados como Store generated (identidade ou computado). O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade gerada pelo armazenamento.  
- **O procedimento armazenado de atualização** terá um parâmetro para cada propriedade, exceto aqueles marcados com um padrão de armazenamento gerado de ' computado '. Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte a seção *tokens de simultaneidade* abaixo para obter detalhes. O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade computada.  
- **O procedimento armazenado Delete** deve ter um parâmetro para o valor de chave da entidade (ou vários parâmetros se a entidade tiver uma chave composta). Além disso, o procedimento Delete também deve ter parâmetros para qualquer chave estrangeira de associação independente na tabela de destino (relações que não têm propriedades de chave estrangeira correspondentes declaradas na entidade). Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte a seção *tokens de simultaneidade* abaixo para obter detalhes.  

Usando a seguinte classe como exemplo:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Os procedimentos armazenados padrão seriam:  

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

### <a name="overriding-the-defaults"></a>Substituindo os padrões  

Você pode substituir parte ou tudo o que foi configurado por padrão.  

Você pode alterar o nome de um ou mais procedimentos armazenados. Este exemplo renomeia apenas o procedimento armazenado Update.  

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

Nesses exemplos, as chamadas são encadeadas, mas você também pode usar a sintaxe de bloco lambda.  

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

Este exemplo renomeia o parâmetro para a propriedade blogid no procedimento armazenado de atualização.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Essas chamadas são encadeadas e combináveis. Aqui está um exemplo que renomeia todos os três procedimentos armazenados e seus parâmetros.  

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

Você também pode alterar o nome das colunas no conjunto de resultados que contém valores gerados pelo banco de dados.  

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

Quando uma propriedade de chave estrangeira é incluída na definição de classe, o parâmetro correspondente pode ser renomeado da mesma maneira que qualquer outra propriedade. Quando existe uma relação sem uma propriedade de chave estrangeira na classe, o nome do parâmetro padrão é **\<navigation_property_name\>_\<primary_key_name** \>.  

Por exemplo, as seguintes definições de classe resultariam na espera de um parâmetro Blog_BlogId nos procedimentos armazenados para inserir e atualizar postagens.  

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

### <a name="overriding-the-defaults"></a>Substituindo os padrões  

Você pode alterar parâmetros para chaves estrangeiras que não estão incluídas na classe fornecendo o caminho para a propriedade de chave primária para o método de parâmetro.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Se você não tiver uma propriedade de navegação na entidade dependente (ou seja, nenhuma propriedade post. blog). em seguida, você pode usar o método de associação para identificar a outra extremidade da relação e, em seguida, configurar os parâmetros que correspondem a cada uma das propriedades de chave.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokens de simultaneidade  

Os procedimentos armazenados Update e Delete também podem precisar lidar com a simultaneidade:  

- Se a entidade contiver tokens de simultaneidade, o procedimento armazenado poderá, opcionalmente, ter um parâmetro de saída que retorna o número de linhas atualizadas/excluídas (linhas afetadas). Esse parâmetro deve ser configurado usando o método RowsAffectedParameter.  
Por padrão, o EF usa o valor de retorno de ExecuteNonQuery para determinar quantas linhas foram afetadas. A especificação de um parâmetro de saída de linhas afetadas será útil se você executar qualquer lógica em seu SPROC que resultaria no valor de retorno de ExecuteNonQuery como incorreto (da perspectiva do EF) no final da execução.  
- Para cada token de simultaneidade, haverá um parâmetro chamado **\<property_name\>_Original** (por exemplo, Timestamp_Original). Isso passará o valor original dessa propriedade – o valor quando consultado do banco de dados.  
    - Os tokens de simultaneidade que são computados pelo banco de dados – como carimbos de data/hora – terão apenas um parâmetro de valor original.  
    - As propriedades não computadas definidas como tokens de simultaneidade também terão um parâmetro para o novo valor no procedimento de atualização. Isso usa as convenções de nomenclatura já discutidas para novos valores. Um exemplo de tal token seria usar a URL de um blog como um token de simultaneidade, o novo valor é necessário porque isso pode ser atualizado para um novo valor pelo seu código (ao contrário de um token de carimbo de data/hora que é atualizado apenas pelo banco de dados).  

Esta é uma classe de exemplo e atualiza o procedimento armazenado com um token de simultaneidade de carimbo de data/hora.  

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

Aqui está uma classe de exemplo e um procedimento armazenado de atualização com um token de simultaneidade não computado.  

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

### <a name="overriding-the-defaults"></a>Substituindo os padrões  

Opcionalmente, você pode introduzir um parâmetro de linhas afetadas.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Para tokens de simultaneidade computados de banco de dados – onde apenas o valor original é passado – você pode usar apenas o mecanismo de renomeação de parâmetro padrão para renomear o parâmetro para o valor original.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Para tokens de simultaneidade não computados – onde o valor original e o novo são passados – você pode usar uma sobrecarga de parâmetro que permite fornecer um nome para cada parâmetro.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Relações Muitos para Muitos  

Usaremos as seguintes classes como um exemplo nesta seção.  

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

Muitos para muitos relacionamentos podem ser mapeados para procedimentos armazenados com a seguinte sintaxe.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Se nenhuma outra configuração for fornecida, a forma de procedimento armazenado a seguir será usada por padrão.  

- Dois procedimentos armazenados chamados **\<type_one\>\<type_two\>_Insert** e **\<type_one\>\<type_two\>** _Delete (por exemplo, PostTag_Insert e PostTag_Delete).  
- Os parâmetros serão os valores de chave para cada tipo. O nome de cada parâmetro sendo **\<type_name\>_\<** property_name\>(por exemplo, Post_PostId e Tag_TagId).

Aqui estão exemplos de inserir e atualizar procedimentos armazenados.  

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

### <a name="overriding-the-defaults"></a>Substituindo os padrões  

Os nomes de procedimento e parâmetro podem ser configurados de forma semelhante aos procedimentos armazenados de entidade.  

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
