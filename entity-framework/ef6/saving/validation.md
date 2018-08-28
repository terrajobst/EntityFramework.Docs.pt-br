---
title: Validação - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: eec834888e2e3efaadc8acf9d4f64307f394ea4a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994439"
---
# <a name="data-validation"></a>Validação de dados
> [!NOTE]
> **EF4.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 4.1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplica

O conteúdo desta página é adaptado do e do artigo escrito originalmente de Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework fornece uma grande variedade de recursos de validação que podem alimentar por meio de uma interface de usuário para validação do lado do cliente ou a ser usado para validação do lado do servidor. Ao usar o código pela primeira vez, você pode especificar usando a anotação ou configurações fluentes da API de validações. Validações adicionais e mais complexa, podem ser especificados no código e funcionarão se seu modelo tem código pela primeira vez, o modelo pela primeira vez ou banco de dados pela primeira vez.

## <a name="the-model"></a>O modelo

Vou demonstrar as validações com um par simple de classes: Blog e Post.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>Anotações de dados

Primeiro, o código usa as anotações do assembly DataAnnotations como um meio de classes do primeiro código de configuração. Entre essas anotações são aquelas que fornecem regras como necessária, MinLength e MaxLength. Um número de aplicativos do cliente .NET também reconhece essas anotações, por exemplo, o ASP.NET MVC. Você pode obter ambos os lado e o servidor de validação do cliente com essas anotações. Por exemplo, você pode forçar a propriedade de título do Blog para ser uma propriedade necessária.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sem código adicional ou alterações de marcação no aplicativo, um aplicativo MVC existente executará validação do lado do cliente, até mesmo dinamicamente a criação de uma mensagem usando os nomes de propriedade e a anotação.

![figure01](~/ef6/media/figure01.png)

Método dessa exibição criar a fazer a postagem, Entity Framework é usado para salvar o novo blog no banco de dados, mas validação do lado do cliente do MVC é disparada antes que o código de alcance o aplicativo.

Validação do lado do cliente entretanto não é à prova de marcador. Os usuários podem ter impacto nos recursos do navegador ou, pior ainda, um hacker pode usar alguns truque para evitar as validações de interface do usuário. Mas o Entity Framework também reconhecerá a anotação necessária e validá-la.

Uma maneira simples de testar isso é desabilitar o recurso de validação do lado do cliente do MVC. Você pode fazer isso no arquivo de Web. config do aplicativo MVC. A seção appSettings tem uma chave para ClientValidationEnabled. Definir essa chave como falso impedirá que a interface do usuário realizar validações.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Mesmo com a validação do lado do cliente desabilitada, você receberá a mesma resposta em seu aplicativo. A mensagem de erro "o campo título é obrigatório" será exibido como. Exceto que agora será um resultado da validação do lado do servidor. Entity Framework executará a validação na anotação necessária (antes de ele até mesmo incômodo para compilação e o comando de inserção para enviar para o banco de dados) e retornará o erro ao MVC que exibirá a mensagem.

## <a name="fluent-api"></a>API fluente

Você pode usar o código de validação de lado a API fluente do primeiro em vez de anotações para obter o cliente mesmo lado do & servidor. Em vez de uso necessárias, vou mostrar isso usando uma validação de MaxLength.

Configurações da API Fluent são aplicadas conforme o código pela primeira vez é criar o modelo de classes. Você pode injetar as configurações, substituindo o método OnModelCreating da classe DbContext. Aqui está uma configuração que especifica que a propriedade BloggerName pode ser não mais de 10 caracteres.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Erros de validação gerados com base nas configurações de API Fluent será não automaticamente alcance a interface do usuário, mas você pode capturá-la no código e, em seguida, responder adequadamente.

Um tratamento de exceções aqui está código de erro na classe BlogController do aplicativo que captura esse erro de validação quando o Entity Framework tenta salvar um blog com um BloggerName que excede o máximo de 10 caracteres.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

A validação não obter passada automaticamente volta para o modo de exibição é por isso que o código adicional que usa ModelState.AddModelError está sendo usado. Isso garante que os detalhes do erro torná-lo para o modo de exibição, em seguida, usará o ValidationMessageFor Htmlhelper para exibir o erro.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject é uma interface que reside no DataAnnotations. Embora não seja parte da API do Entity Framework, você ainda pode aproveitá-lo para a validação do lado do servidor em suas classes de Entity Framework. IValidatableObject fornece um método de validação do Entity Framework chamará durante SaveChanges, ou você pode chamar a mesmo sempre que quiser para validar as classes.

MaxLength e configurações necessárias, como executam a validação em um único campo. No método Validate você pode ter até mesmo uma lógica mais complexa, por exemplo, comparando os dois campos.

No exemplo a seguir, a classe de Blog foi estendida para implementar IValidatableObject e, em seguida, fornecer uma regra que não podem coincidir com o título e BloggerName.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

O construtor de ValidationResult aceita uma cadeia de caracteres que representa a mensagem de erro e uma matriz de cadeias de caracteres que representam os nomes de membros que estão associados com a validação. Uma vez que essa validação verifica o título e o BloggerName, ambos os nomes de propriedade são retornados.

Ao contrário de validação fornecida pela API do Fluent, esse resultado de validação será reconhecido pelo modo de exibição e o manipulador de exceção que usei anteriormente para adicionar o erro em ModelState é desnecessário. Como posso definir ambos os nomes de propriedade em ValidationResult, o HtmlHelpers o MVC exibe a mensagem de erro para essas duas propriedades.

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext tem um método substituível chamado ValidateEntity. Quando você chamar SaveChanges, Entity Framework irá chamar este método para cada entidade em seu cache cujo estado não é alterado. Você pode colocar a lógica de validação diretamente em aqui ou até mesmo usar esse método para chamar, por exemplo, o método Blog.Validate adicionado na seção anterior.

Aqui está um exemplo de uma substituição ValidateEntity valida novas postagens para garantir que o título da postagem já não tiver sido usado. Ele primeiro verifica para ver se a entidade é uma postagem e que seu estado é adicionado. Se esse for o caso, em seguida, ele examina o banco de dados para ver se já houver uma postagem com o mesmo título. Se já houver uma postagem existente, um novo DbEntityValidationResult é criado.

DbEntityValidationResult abriga uma DbEntityEntry e uma ICollection de DbValidationErrors para uma única entidade. No início desse método, um DbEntityValidationResult é instanciada e, em seguida, todos os erros que são descobertos são adicionados à sua coleção ValidationErrors.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Acionar a validação explicitamente

Uma chamada para SaveChanges dispara todas as validações abordadas neste artigo. Mas você não precisa contar com SaveChanges. Talvez você prefira a ser validado em outro lugar no seu aplicativo.

DbContext.GetValidationErrors disparará a todas as validações, aquelas definidas por anotações ou a API Fluent, a validação criado no IValidatableObject (por exemplo, Blog.Validate) e as validações executadas no DbContext.ValidateEntity método.

O código a seguir chamará GetValidationErrors na instância atual de um DbContext. ValidationErrors são agrupados por tipo de entidade em DbValidationRestuls. Primeiro, o código itera por meio de DbValidationResults retornado pelo método e, em seguida, cada ValidationError dentro.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Outras considerações ao usar a validação

Aqui estão alguns outros pontos a considerar ao usar a validação do Entity Framework:

-   Carregamento lento está desabilitado durante a validação.
-   O EF validará as anotações de dados nas propriedades não mapeadas (propriedades que não são mapeadas para uma coluna no banco de dados).
-   Validação é executada depois que as alterações são detectadas durante SaveChanges. Se você fizer alterações durante a validação é sua responsabilidade para notificar o rastreador de alteração.
-   DbUnexpectedValidationException será lançada se ocorrerem erros durante a validação.
-   As facetas que o Entity Framework inclui no modelo (comprimento máximo, exigido, etc.) fará com que a validação, mesmo se não há anotações de dados em suas classes de e/ou usado o EF Designer para criar seu modelo.
-   Regras de precedência:
    -   Chamadas à API Fluent substituem as anotações de dados correspondente
-   Ordem de execução:
    -   Validação de propriedade ocorre antes da validação de tipo
    -   Tipo de validação ocorre apenas se a validação de propriedade for bem-sucedida
-   Se uma propriedade for complexa sua validação também incluirá:
    -   Validação de nível de propriedade nas propriedades de tipo complexo
    -   Validação de nível de tipo no tipo complexo, incluindo validação IValidatableObject no tipo complexo

## <a name="summary"></a>Resumo

A API de validação no Entity Framework é reproduzido muito bem com a validação do lado do cliente no MVC, mas você não precisa contar com validação do lado do cliente. Entity Framework cuidará da validação no lado do servidor para DataAnnotations ou configurações que você tiver aplicado com a API Fluent do code first.

Você também viu um número de pontos de extensibilidade para personalizar o comportamento se você usar a interface IValidatableObject ou aproveita o método DbContet.ValidateEntity. E esses últimos dois meios de validação estão disponíveis por meio do DbContext, se você usar o fluxo de trabalho do Code First, Model First ou Database First para descrever o modelo conceitual.
