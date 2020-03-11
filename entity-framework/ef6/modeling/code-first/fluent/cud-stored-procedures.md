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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="f9ac6-102">Code First inserir, atualizar e excluir procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="f9ac6-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="f9ac6-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f9ac6-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f9ac6-105">Por padrão, Code First configurará todas as entidades para executar comandos INSERT, Update e DELETE usando acesso direto à tabela.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="f9ac6-106">A partir do EF6, você pode configurar seu modelo de Code First para usar procedimentos armazenados para algumas ou todas as entidades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="f9ac6-107">Mapeamento de entidade básica</span><span class="sxs-lookup"><span data-stu-id="f9ac6-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="f9ac6-108">Você pode optar por usar procedimentos armazenados para inserir, atualizar e excluir usando a API fluente.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="f9ac6-109">Isso fará com que Code First Use algumas convenções para criar a forma esperada dos procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="f9ac6-110">Três procedimentos armazenados chamados **\<type_name\>_Insert**, **\<type_name\>_Update** e **\<** type_name\>_Delete (por exemplo, Blog_Insert, Blog_Update e Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="f9ac6-111">Os nomes de parâmetro correspondem aos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="f9ac6-112">Se você usar HasColumnName () ou o atributo de coluna para renomear a coluna para uma determinada propriedade, esse nome será usado para parâmetros em vez do nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="f9ac6-113">**O procedimento armazenado INSERT** terá um parâmetro para cada propriedade, exceto aqueles marcados como Store generated (identidade ou computado).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="f9ac6-114">O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade gerada pelo armazenamento.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="f9ac6-115">**O procedimento armazenado de atualização** terá um parâmetro para cada propriedade, exceto aqueles marcados com um padrão de armazenamento gerado de ' computado '.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="f9ac6-116">Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte a seção *tokens de simultaneidade* abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="f9ac6-117">O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade computada.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="f9ac6-118">**O procedimento armazenado Delete** deve ter um parâmetro para o valor de chave da entidade (ou vários parâmetros se a entidade tiver uma chave composta).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="f9ac6-119">Além disso, o procedimento Delete também deve ter parâmetros para qualquer chave estrangeira de associação independente na tabela de destino (relações que não têm propriedades de chave estrangeira correspondentes declaradas na entidade).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="f9ac6-120">Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte a seção *tokens de simultaneidade* abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="f9ac6-121">Usando a seguinte classe como exemplo:</span><span class="sxs-lookup"><span data-stu-id="f9ac6-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="f9ac6-122">Os procedimentos armazenados padrão seriam:</span><span class="sxs-lookup"><span data-stu-id="f9ac6-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="f9ac6-123">Substituindo os padrões</span><span class="sxs-lookup"><span data-stu-id="f9ac6-123">Overriding the Defaults</span></span>  

<span data-ttu-id="f9ac6-124">Você pode substituir parte ou tudo o que foi configurado por padrão.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="f9ac6-125">Você pode alterar o nome de um ou mais procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="f9ac6-126">Este exemplo renomeia apenas o procedimento armazenado Update.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="f9ac6-127">Este exemplo renomeia todos os três procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="f9ac6-128">Nesses exemplos, as chamadas são encadeadas, mas você também pode usar a sintaxe de bloco lambda.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="f9ac6-129">Este exemplo renomeia o parâmetro para a propriedade blogid no procedimento armazenado de atualização.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="f9ac6-130">Essas chamadas são encadeadas e combináveis.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="f9ac6-131">Aqui está um exemplo que renomeia todos os três procedimentos armazenados e seus parâmetros.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="f9ac6-132">Você também pode alterar o nome das colunas no conjunto de resultados que contém valores gerados pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="f9ac6-133">Relações sem uma chave estrangeira na classe (associações independentes)</span><span class="sxs-lookup"><span data-stu-id="f9ac6-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="f9ac6-134">Quando uma propriedade de chave estrangeira é incluída na definição de classe, o parâmetro correspondente pode ser renomeado da mesma maneira que qualquer outra propriedade.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="f9ac6-135">Quando existe uma relação sem uma propriedade de chave estrangeira na classe, o nome do parâmetro padrão é **\<navigation_property_name\>_\<primary_key_name** \>.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="f9ac6-136">Por exemplo, as seguintes definições de classe resultariam na espera de um parâmetro Blog_BlogId nos procedimentos armazenados para inserir e atualizar postagens.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="f9ac6-137">Substituindo os padrões</span><span class="sxs-lookup"><span data-stu-id="f9ac6-137">Overriding the Defaults</span></span>  

<span data-ttu-id="f9ac6-138">Você pode alterar parâmetros para chaves estrangeiras que não estão incluídas na classe fornecendo o caminho para a propriedade de chave primária para o método de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="f9ac6-139">Se você não tiver uma propriedade de navegação na entidade dependente (ou seja,</span><span class="sxs-lookup"><span data-stu-id="f9ac6-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="f9ac6-140">nenhuma propriedade post. blog). em seguida, você pode usar o método de associação para identificar a outra extremidade da relação e, em seguida, configurar os parâmetros que correspondem a cada uma das propriedades de chave.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="f9ac6-141">Tokens de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="f9ac6-141">Concurrency Tokens</span></span>  

<span data-ttu-id="f9ac6-142">Os procedimentos armazenados Update e Delete também podem precisar lidar com a simultaneidade:</span><span class="sxs-lookup"><span data-stu-id="f9ac6-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="f9ac6-143">Se a entidade contiver tokens de simultaneidade, o procedimento armazenado poderá, opcionalmente, ter um parâmetro de saída que retorna o número de linhas atualizadas/excluídas (linhas afetadas).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="f9ac6-144">Esse parâmetro deve ser configurado usando o método RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="f9ac6-145">Por padrão, o EF usa o valor de retorno de ExecuteNonQuery para determinar quantas linhas foram afetadas.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="f9ac6-146">A especificação de um parâmetro de saída de linhas afetadas será útil se você executar qualquer lógica em seu SPROC que resultaria no valor de retorno de ExecuteNonQuery como incorreto (da perspectiva do EF) no final da execução.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="f9ac6-147">Para cada token de simultaneidade, haverá um parâmetro chamado **\<property_name\>_Original** (por exemplo, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="f9ac6-148">Isso passará o valor original dessa propriedade – o valor quando consultado do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="f9ac6-149">Os tokens de simultaneidade que são computados pelo banco de dados – como carimbos de data/hora – terão apenas um parâmetro de valor original.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="f9ac6-150">As propriedades não computadas definidas como tokens de simultaneidade também terão um parâmetro para o novo valor no procedimento de atualização.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="f9ac6-151">Isso usa as convenções de nomenclatura já discutidas para novos valores.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="f9ac6-152">Um exemplo de tal token seria usar a URL de um blog como um token de simultaneidade, o novo valor é necessário porque isso pode ser atualizado para um novo valor pelo seu código (ao contrário de um token de carimbo de data/hora que é atualizado apenas pelo banco de dados).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="f9ac6-153">Esta é uma classe de exemplo e atualiza o procedimento armazenado com um token de simultaneidade de carimbo de data/hora.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="f9ac6-154">Aqui está uma classe de exemplo e um procedimento armazenado de atualização com um token de simultaneidade não computado.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="f9ac6-155">Substituindo os padrões</span><span class="sxs-lookup"><span data-stu-id="f9ac6-155">Overriding the Defaults</span></span>  

<span data-ttu-id="f9ac6-156">Opcionalmente, você pode introduzir um parâmetro de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="f9ac6-157">Para tokens de simultaneidade computados de banco de dados – onde apenas o valor original é passado – você pode usar apenas o mecanismo de renomeação de parâmetro padrão para renomear o parâmetro para o valor original.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="f9ac6-158">Para tokens de simultaneidade não computados – onde o valor original e o novo são passados – você pode usar uma sobrecarga de parâmetro que permite fornecer um nome para cada parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="f9ac6-159">Relações Muitos para Muitos</span><span class="sxs-lookup"><span data-stu-id="f9ac6-159">Many to Many Relationships</span></span>  

<span data-ttu-id="f9ac6-160">Usaremos as seguintes classes como um exemplo nesta seção.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="f9ac6-161">Muitos para muitos relacionamentos podem ser mapeados para procedimentos armazenados com a seguinte sintaxe.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="f9ac6-162">Se nenhuma outra configuração for fornecida, a forma de procedimento armazenado a seguir será usada por padrão.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="f9ac6-163">Dois procedimentos armazenados chamados **\<type_one\>\<type_two\>_Insert** e **\<type_one\>\<type_two\>** _Delete (por exemplo, PostTag_Insert e PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="f9ac6-164">Os parâmetros serão os valores de chave para cada tipo.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="f9ac6-165">O nome de cada parâmetro sendo **\<type_name\>_\<** property_name\>(por exemplo, Post_PostId e Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="f9ac6-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="f9ac6-166">Aqui estão exemplos de inserir e atualizar procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="f9ac6-167">Substituindo os padrões</span><span class="sxs-lookup"><span data-stu-id="f9ac6-167">Overriding the Defaults</span></span>  

<span data-ttu-id="f9ac6-168">Os nomes de procedimento e parâmetro podem ser configurados de forma semelhante aos procedimentos armazenados de entidade.</span><span class="sxs-lookup"><span data-stu-id="f9ac6-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
