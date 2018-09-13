---
title: Código primeiro inserir, atualizar e excluir procedimentos armazenados – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489616"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="54f0e-102">Código primeiro inserir, atualizar e excluir procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="54f0e-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="54f0e-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="54f0e-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="54f0e-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="54f0e-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="54f0e-105">Por padrão, Code First configurará todas as entidades para executar a inserção, atualizar e excluir os comandos que usam acesso direto à tabela.</span><span class="sxs-lookup"><span data-stu-id="54f0e-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="54f0e-106">Começando no EF6, você pode configurar seu modelo Code First para usar procedimentos armazenados para algumas ou todas as entidades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="54f0e-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="54f0e-107">Mapeamento de entidade básica</span><span class="sxs-lookup"><span data-stu-id="54f0e-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="54f0e-108">Você pode aceitar o uso de procedimentos armazenados para inserir, atualizar e excluir usando a API do Fluent.</span><span class="sxs-lookup"><span data-stu-id="54f0e-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="54f0e-109">Isso fará com que Code First usar algumas convenções para criar a forma esperada dos procedimentos armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="54f0e-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="54f0e-110">Chamada de procedimentos armazenados de três  **\<type_name\>inserir**,  **\<type_name\>atualizar** e  **\<type_ nome da\>excluir** (por exemplo, Blog_Insert, Blog_Update e Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="54f0e-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="54f0e-111">Nomes de parâmetro correspondem aos nomes de propriedade.</span><span class="sxs-lookup"><span data-stu-id="54f0e-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="54f0e-112">Se você usar HasColumnName() ou o atributo de coluna para renomear a coluna para uma determinada propriedade, em seguida, esse nome é usado para parâmetros em vez do nome de propriedade.</span><span class="sxs-lookup"><span data-stu-id="54f0e-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="54f0e-113">**O procedimento armazenado insert** terá um parâmetro para cada propriedade, exceto para aqueles marcados como geradas pelo repositório (identidade ou computadas).</span><span class="sxs-lookup"><span data-stu-id="54f0e-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="54f0e-114">O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="54f0e-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="54f0e-115">**Procedimento armazenado de atualização** terá um parâmetro para cada propriedade, exceto para aqueles marcados com um padrão de repositório gerado de 'Computed'.</span><span class="sxs-lookup"><span data-stu-id="54f0e-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="54f0e-116">Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte o *Tokens de simultaneidade* seção abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="54f0e-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="54f0e-117">O procedimento armazenado deve retornar um conjunto de resultados com uma coluna para cada propriedade computada.</span><span class="sxs-lookup"><span data-stu-id="54f0e-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="54f0e-118">**Procedimento armazenado de exclusão** deve ter um parâmetro para o valor da chave de entidade (ou vários parâmetros, se a entidade tem uma chave composta).</span><span class="sxs-lookup"><span data-stu-id="54f0e-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="54f0e-119">Além disso, o procedimento de exclusão também deve ter parâmetros para nenhuma chave estrangeira da associação independente na tabela de destino (relações que não têm propriedades de chave estrangeira correspondentes declaradas na entidade).</span><span class="sxs-lookup"><span data-stu-id="54f0e-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="54f0e-120">Alguns tokens de simultaneidade exigem um parâmetro para o valor original, consulte o *Tokens de simultaneidade* seção abaixo para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="54f0e-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="54f0e-121">Usando a classe a seguir como exemplo:</span><span class="sxs-lookup"><span data-stu-id="54f0e-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="54f0e-122">Os procedimentos armazenado de padrão seria:</span><span class="sxs-lookup"><span data-stu-id="54f0e-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="54f0e-123">Substituindo padrões</span><span class="sxs-lookup"><span data-stu-id="54f0e-123">Overriding the Defaults</span></span>  

<span data-ttu-id="54f0e-124">Você pode substituir todo ou parte do que foi configurado por padrão.</span><span class="sxs-lookup"><span data-stu-id="54f0e-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="54f0e-125">Você pode alterar o nome de um ou mais procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="54f0e-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="54f0e-126">Este exemplo renomeia o procedimento armazenado de atualização apenas.</span><span class="sxs-lookup"><span data-stu-id="54f0e-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="54f0e-127">Este exemplo renomeia todos os três procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="54f0e-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="54f0e-128">Nestes exemplos as chamadas são encadeadas juntos, mas você também pode usar a sintaxe de bloco de lambda.</span><span class="sxs-lookup"><span data-stu-id="54f0e-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="54f0e-129">Este exemplo renomeia o parâmetro para a propriedade BlogId no procedimento armazenado de atualização.</span><span class="sxs-lookup"><span data-stu-id="54f0e-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="54f0e-130">Essas chamadas são todos encadeável e combinável.</span><span class="sxs-lookup"><span data-stu-id="54f0e-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="54f0e-131">Aqui está um exemplo que renomeia todos os três procedimentos armazenados e seus parâmetros.</span><span class="sxs-lookup"><span data-stu-id="54f0e-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="54f0e-132">Você também pode alterar o nome das colunas no conjunto de resultados que contém os valores de banco de dados gerado.</span><span class="sxs-lookup"><span data-stu-id="54f0e-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="54f0e-133">Relações sem uma chave estrangeira na classe (associações independentes)</span><span class="sxs-lookup"><span data-stu-id="54f0e-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="54f0e-134">Quando uma propriedade de chave estrangeira é incluída na definição de classe, o parâmetro correspondente pode ser renomeado da mesma forma como qualquer outra propriedade.</span><span class="sxs-lookup"><span data-stu-id="54f0e-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="54f0e-135">Quando uma relação existente sem uma propriedade de chave estrangeira na classe, o nome do parâmetro padrão é  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="54f0e-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="54f0e-136">Por exemplo, as seguintes definições de classe resultaria em um parâmetro de Blog_BlogId sendo esperado nos procedimentos armazenados para inserir e atualizar as postagens.</span><span class="sxs-lookup"><span data-stu-id="54f0e-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="54f0e-137">Substituindo padrões</span><span class="sxs-lookup"><span data-stu-id="54f0e-137">Overriding the Defaults</span></span>  

<span data-ttu-id="54f0e-138">Você pode alterar os parâmetros para chaves estrangeiras que não estão incluídos na classe, fornecendo o caminho para a propriedade de chave primária para o método de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="54f0e-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="54f0e-139">Se você não tiver uma propriedade de navegação na entidade dependente (isto é</span><span class="sxs-lookup"><span data-stu-id="54f0e-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="54f0e-140">nenhuma propriedade Post.Blog) e em seguida, você pode usar o método de associação para identificar a outra extremidade da relação e, em seguida, configure os parâmetros que correspondem a cada uma da propriedade de chave (s).</span><span class="sxs-lookup"><span data-stu-id="54f0e-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="54f0e-141">Tokens de simultaneidade</span><span class="sxs-lookup"><span data-stu-id="54f0e-141">Concurrency Tokens</span></span>  

<span data-ttu-id="54f0e-142">Update e delete armazenados procedimentos talvez precise lidar com simultaneidade:</span><span class="sxs-lookup"><span data-stu-id="54f0e-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="54f0e-143">Se a entidade contém tokens de simultaneidade, o procedimento armazenado pode, opcionalmente, ter um parâmetro de saída que retorna o número de linhas atualizado/excluído (linhas afetadas).</span><span class="sxs-lookup"><span data-stu-id="54f0e-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="54f0e-144">Esse parâmetro deve ser configurado usando o método RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="54f0e-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="54f0e-145">Por padrão o EF usa o valor retornado de ExecuteNonQuery para determinar quantas linhas foram afetadas.</span><span class="sxs-lookup"><span data-stu-id="54f0e-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="54f0e-146">Especificando um parâmetro de saída de linhas afetadas é útil se você executar qualquer lógica em seu sproc que resultaria no valor de retorno de ExecuteNonQuery sendo incorreto (da perspectiva do EF) no final da execução.</span><span class="sxs-lookup"><span data-stu-id="54f0e-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="54f0e-147">Para cada simultaneidade de token de lá será um parâmetro denominado  **\<property_name\>_Original** (por exemplo, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="54f0e-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="54f0e-148">Isso será passado o valor original dessa propriedade – o valor quando consultado do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="54f0e-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="54f0e-149">Tokens de simultaneidade são calculados pelo banco de dados – como os carimbos de hora – terá apenas um parâmetro de valor original.</span><span class="sxs-lookup"><span data-stu-id="54f0e-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="54f0e-150">Propriedades não são computadas que são definidas como tokens de simultaneidade também terá um parâmetro para o novo valor no procedimento de atualização.</span><span class="sxs-lookup"><span data-stu-id="54f0e-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="54f0e-151">Isso usa as convenções de nomenclatura já discutidas para novos valores.</span><span class="sxs-lookup"><span data-stu-id="54f0e-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="54f0e-152">Um exemplo de como esse token usariam URL do como um token de simultaneidade, o novo valor é necessário porque isso pode ser atualizado para um novo valor pelo seu código (ao contrário de um token de carimbo de hora que só é atualizado pelo banco de dados).</span><span class="sxs-lookup"><span data-stu-id="54f0e-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="54f0e-153">Este é um exemplo de classe e atualize o procedimento armazenado com um token de simultaneidade de carimbo de hora.</span><span class="sxs-lookup"><span data-stu-id="54f0e-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="54f0e-154">Aqui está um exemplo de classe e atualize o procedimento armazenado com o token de simultaneidade não computadas.</span><span class="sxs-lookup"><span data-stu-id="54f0e-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="54f0e-155">Substituindo padrões</span><span class="sxs-lookup"><span data-stu-id="54f0e-155">Overriding the Defaults</span></span>  

<span data-ttu-id="54f0e-156">Opcionalmente, você pode introduzir um parâmetro de linhas afetadas.</span><span class="sxs-lookup"><span data-stu-id="54f0e-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="54f0e-157">Para tokens de simultaneidade de banco de dados calculado – em que apenas o valor original é passado – você pode usar apenas o parâmetro padrão renomeando um mecanismo para renomear o parâmetro do valor original.</span><span class="sxs-lookup"><span data-stu-id="54f0e-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="54f0e-158">Para tokens de simultaneidade não computadas – em que tanto o valor original e novo é passado – você pode usar uma sobrecarga de parâmetro que permite que você forneça um nome para cada parâmetro.</span><span class="sxs-lookup"><span data-stu-id="54f0e-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="54f0e-159">Relações Muitos para Muitos</span><span class="sxs-lookup"><span data-stu-id="54f0e-159">Many to Many Relationships</span></span>  

<span data-ttu-id="54f0e-160">Usaremos as classes a seguir como exemplo nesta seção.</span><span class="sxs-lookup"><span data-stu-id="54f0e-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="54f0e-161">Muitas para muitas relações podem ser mapeadas para procedimentos armazenados com a sintaxe a seguir.</span><span class="sxs-lookup"><span data-stu-id="54f0e-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="54f0e-162">Se nenhuma outra configuração é fornecida na forma de procedimento armazenado a seguir é usada por padrão.</span><span class="sxs-lookup"><span data-stu-id="54f0e-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="54f0e-163">Dois chamados de procedimentos armazenados  **\<type_one\>\<type_two\>inserir** e  **\<type_one\>\<type_two \>Excluir** (por exemplo, PostTag_Insert e PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="54f0e-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="54f0e-164">Os parâmetros serão os valores de chave para cada tipo.</span><span class="sxs-lookup"><span data-stu-id="54f0e-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="54f0e-165">O nome de cada parâmetro que está sendo **\<type_name\>_\<property_name\>** (por exemplo, Post_PostId e Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="54f0e-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="54f0e-166">Estes são exemplos de inserção e atualização de procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="54f0e-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="54f0e-167">Substituindo padrões</span><span class="sxs-lookup"><span data-stu-id="54f0e-167">Overriding the Defaults</span></span>  

<span data-ttu-id="54f0e-168">Os nomes de procedimento e o parâmetro podem ser configurados de forma semelhante aos procedimentos armazenado de entidade.</span><span class="sxs-lookup"><span data-stu-id="54f0e-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
