---
title: Validação - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 65639b0f91f54ee2cd1336f6b6cd4caf45ede680
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251018"
---
# <a name="data-validation"></a><span data-ttu-id="48977-102">Validação de dados</span><span class="sxs-lookup"><span data-stu-id="48977-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="48977-103">**EF4.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="48977-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="48977-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplica</span><span class="sxs-lookup"><span data-stu-id="48977-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="48977-105">O conteúdo desta página é adaptado do e do artigo escrito originalmente de Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="48977-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="48977-106">Entity Framework fornece uma grande variedade de recursos de validação que podem alimentar por meio de uma interface de usuário para validação do lado do cliente ou a ser usado para validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="48977-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="48977-107">Ao usar o código pela primeira vez, você pode especificar usando a anotação ou configurações fluentes da API de validações.</span><span class="sxs-lookup"><span data-stu-id="48977-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="48977-108">Validações adicionais e mais complexa, podem ser especificados no código e funcionarão se seu modelo tem código pela primeira vez, o modelo pela primeira vez ou banco de dados pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="48977-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="48977-109">O modelo</span><span class="sxs-lookup"><span data-stu-id="48977-109">The model</span></span>

<span data-ttu-id="48977-110">Vou demonstrar as validações com um par simple de classes: Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="48977-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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
## <a name="data-annotations"></a><span data-ttu-id="48977-111">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="48977-111">Data Annotations</span></span>

<span data-ttu-id="48977-112">Primeiro, o código usa as anotações do assembly DataAnnotations como um meio de classes do primeiro código de configuração.</span><span class="sxs-lookup"><span data-stu-id="48977-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="48977-113">Entre essas anotações são aquelas que fornecem regras como necessária, MinLength e MaxLength.</span><span class="sxs-lookup"><span data-stu-id="48977-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="48977-114">Um número de aplicativos do cliente .NET também reconhece essas anotações, por exemplo, o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="48977-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="48977-115">Você pode obter ambos os lado e o servidor de validação do cliente com essas anotações.</span><span class="sxs-lookup"><span data-stu-id="48977-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="48977-116">Por exemplo, você pode forçar a propriedade de título do Blog para ser uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="48977-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="48977-117">Sem código adicional ou alterações de marcação no aplicativo, um aplicativo MVC existente executará validação do lado do cliente, até mesmo dinamicamente a criação de uma mensagem usando os nomes de propriedade e a anotação.</span><span class="sxs-lookup"><span data-stu-id="48977-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Figura 1](~/ef6/media/figure01.png)

<span data-ttu-id="48977-119">Método dessa exibição criar a fazer a postagem, Entity Framework é usado para salvar o novo blog no banco de dados, mas validação do lado do cliente do MVC é disparada antes que o código de alcance o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="48977-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="48977-120">Validação do lado do cliente entretanto não é à prova de marcador.</span><span class="sxs-lookup"><span data-stu-id="48977-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="48977-121">Os usuários podem ter impacto nos recursos do navegador ou, pior ainda, um hacker pode usar alguns truque para evitar as validações de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="48977-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="48977-122">Mas o Entity Framework também reconhecerá a anotação necessária e validá-la.</span><span class="sxs-lookup"><span data-stu-id="48977-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="48977-123">Uma maneira simples de testar isso é desabilitar o recurso de validação do lado do cliente do MVC.</span><span class="sxs-lookup"><span data-stu-id="48977-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="48977-124">Você pode fazer isso no arquivo de Web. config do aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="48977-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="48977-125">A seção appSettings tem uma chave para ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="48977-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="48977-126">Definir essa chave como falso impedirá que a interface do usuário realizar validações.</span><span class="sxs-lookup"><span data-stu-id="48977-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="48977-127">Mesmo com a validação do lado do cliente desabilitada, você receberá a mesma resposta em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="48977-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="48977-128">A mensagem de erro "o campo título é obrigatório" será exibido como.</span><span class="sxs-lookup"><span data-stu-id="48977-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="48977-129">Exceto que agora será um resultado da validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="48977-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="48977-130">Entity Framework executará a validação na anotação necessária (antes de ele até mesmo incômodo para compilação e o comando de inserção para enviar para o banco de dados) e retornará o erro ao MVC que exibirá a mensagem.</span><span class="sxs-lookup"><span data-stu-id="48977-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="48977-131">API fluente</span><span class="sxs-lookup"><span data-stu-id="48977-131">Fluent API</span></span>

<span data-ttu-id="48977-132">Você pode usar o código de validação de lado a API fluente do primeiro em vez de anotações para obter o cliente mesmo lado do & servidor.</span><span class="sxs-lookup"><span data-stu-id="48977-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="48977-133">Em vez de uso necessárias, vou mostrar isso usando uma validação de MaxLength.</span><span class="sxs-lookup"><span data-stu-id="48977-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="48977-134">Configurações da API Fluent são aplicadas conforme o código pela primeira vez é criar o modelo de classes.</span><span class="sxs-lookup"><span data-stu-id="48977-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="48977-135">Você pode injetar as configurações, substituindo o método OnModelCreating da classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="48977-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="48977-136">Aqui está uma configuração que especifica que a propriedade BloggerName pode ser não mais de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="48977-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="48977-137">Erros de validação gerados com base nas configurações de API Fluent será não automaticamente alcance a interface do usuário, mas você pode capturá-la no código e, em seguida, responder adequadamente.</span><span class="sxs-lookup"><span data-stu-id="48977-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="48977-138">Um tratamento de exceções aqui está código de erro na classe BlogController do aplicativo que captura esse erro de validação quando o Entity Framework tenta salvar um blog com um BloggerName que excede o máximo de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="48977-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="48977-139">A validação não obter passada automaticamente volta para o modo de exibição é por isso que o código adicional que usa ModelState.AddModelError está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="48977-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="48977-140">Isso garante que os detalhes do erro torná-lo para o modo de exibição, em seguida, usará o ValidationMessageFor Htmlhelper para exibir o erro.</span><span class="sxs-lookup"><span data-stu-id="48977-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="48977-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="48977-141">IValidatableObject</span></span>

<span data-ttu-id="48977-142">IValidatableObject é uma interface que reside no DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="48977-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="48977-143">Embora não seja parte da API do Entity Framework, você ainda pode aproveitá-lo para a validação do lado do servidor em suas classes de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48977-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="48977-144">IValidatableObject fornece um método de validação do Entity Framework chamará durante SaveChanges, ou você pode chamar a mesmo sempre que quiser para validar as classes.</span><span class="sxs-lookup"><span data-stu-id="48977-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="48977-145">MaxLength e configurações necessárias, como executam a validação em um único campo.</span><span class="sxs-lookup"><span data-stu-id="48977-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="48977-146">No método Validate você pode ter até mesmo uma lógica mais complexa, por exemplo, comparando os dois campos.</span><span class="sxs-lookup"><span data-stu-id="48977-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="48977-147">No exemplo a seguir, a classe de Blog foi estendida para implementar IValidatableObject e, em seguida, fornecer uma regra que não podem coincidir com o título e BloggerName.</span><span class="sxs-lookup"><span data-stu-id="48977-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="48977-148">O construtor de ValidationResult aceita uma cadeia de caracteres que representa a mensagem de erro e uma matriz de cadeias de caracteres que representam os nomes de membros que estão associados com a validação.</span><span class="sxs-lookup"><span data-stu-id="48977-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="48977-149">Uma vez que essa validação verifica o título e o BloggerName, ambos os nomes de propriedade são retornados.</span><span class="sxs-lookup"><span data-stu-id="48977-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="48977-150">Ao contrário de validação fornecida pela API do Fluent, esse resultado de validação será reconhecido pelo modo de exibição e o manipulador de exceção que usei anteriormente para adicionar o erro em ModelState é desnecessário.</span><span class="sxs-lookup"><span data-stu-id="48977-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="48977-151">Como posso definir ambos os nomes de propriedade em ValidationResult, o HtmlHelpers o MVC exibe a mensagem de erro para essas duas propriedades.</span><span class="sxs-lookup"><span data-stu-id="48977-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Figura 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="48977-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="48977-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="48977-154">DbContext tem um método substituível chamado ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="48977-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="48977-155">Quando você chamar SaveChanges, Entity Framework irá chamar este método para cada entidade em seu cache cujo estado não é alterado.</span><span class="sxs-lookup"><span data-stu-id="48977-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="48977-156">Você pode colocar a lógica de validação diretamente em aqui ou até mesmo usar esse método para chamar, por exemplo, o método Blog.Validate adicionado na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="48977-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="48977-157">Aqui está um exemplo de uma substituição ValidateEntity valida novas postagens para garantir que o título da postagem já não tiver sido usado.</span><span class="sxs-lookup"><span data-stu-id="48977-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="48977-158">Ele primeiro verifica para ver se a entidade é uma postagem e que seu estado é adicionado.</span><span class="sxs-lookup"><span data-stu-id="48977-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="48977-159">Se esse for o caso, em seguida, ele examina o banco de dados para ver se já houver uma postagem com o mesmo título.</span><span class="sxs-lookup"><span data-stu-id="48977-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="48977-160">Se já houver uma postagem existente, um novo DbEntityValidationResult é criado.</span><span class="sxs-lookup"><span data-stu-id="48977-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="48977-161">DbEntityValidationResult abriga uma DbEntityEntry e uma ICollection de DbValidationErrors para uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="48977-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="48977-162">No início desse método, um DbEntityValidationResult é instanciada e, em seguida, todos os erros que são descobertos são adicionados à sua coleção ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="48977-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="48977-163">Acionar a validação explicitamente</span><span class="sxs-lookup"><span data-stu-id="48977-163">Explicitly triggering validation</span></span>

<span data-ttu-id="48977-164">Uma chamada para SaveChanges dispara todas as validações abordadas neste artigo.</span><span class="sxs-lookup"><span data-stu-id="48977-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="48977-165">Mas você não precisa contar com SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="48977-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="48977-166">Talvez você prefira a ser validado em outro lugar no seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="48977-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="48977-167">DbContext.GetValidationErrors disparará a todas as validações, aquelas definidas por anotações ou a API Fluent, a validação criado no IValidatableObject (por exemplo, Blog.Validate) e as validações executadas no DbContext.ValidateEntity método.</span><span class="sxs-lookup"><span data-stu-id="48977-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="48977-168">O código a seguir chamará GetValidationErrors na instância atual de um DbContext.</span><span class="sxs-lookup"><span data-stu-id="48977-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="48977-169">ValidationErrors são agrupados por tipo de entidade em DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="48977-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="48977-170">Primeiro, o código itera por meio de DbValidationResults retornado pelo método e, em seguida, cada ValidationError dentro.</span><span class="sxs-lookup"><span data-stu-id="48977-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="48977-171">Outras considerações ao usar a validação</span><span class="sxs-lookup"><span data-stu-id="48977-171">Other considerations when using validation</span></span>

<span data-ttu-id="48977-172">Aqui estão alguns outros pontos a considerar ao usar a validação do Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="48977-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="48977-173">Carregamento lento está desabilitado durante a validação.</span><span class="sxs-lookup"><span data-stu-id="48977-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="48977-174">O EF validará as anotações de dados nas propriedades não mapeadas (propriedades que não são mapeadas para uma coluna no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="48977-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="48977-175">Validação é executada depois que as alterações são detectadas durante SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="48977-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="48977-176">Se você fizer alterações durante a validação é sua responsabilidade para notificar o rastreador de alteração.</span><span class="sxs-lookup"><span data-stu-id="48977-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="48977-177">DbUnexpectedValidationException será lançada se ocorrerem erros durante a validação.</span><span class="sxs-lookup"><span data-stu-id="48977-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="48977-178">As facetas que o Entity Framework inclui no modelo (comprimento máximo, exigido, etc.) fará com que a validação, mesmo se não há anotações de dados em suas classes de e/ou usado o EF Designer para criar seu modelo.</span><span class="sxs-lookup"><span data-stu-id="48977-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="48977-179">Regras de precedência:</span><span class="sxs-lookup"><span data-stu-id="48977-179">Precedence rules:</span></span>
    -   <span data-ttu-id="48977-180">Chamadas à API Fluent substituem as anotações de dados correspondente</span><span class="sxs-lookup"><span data-stu-id="48977-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="48977-181">Ordem de execução:</span><span class="sxs-lookup"><span data-stu-id="48977-181">Execution order:</span></span>
    -   <span data-ttu-id="48977-182">Validação de propriedade ocorre antes da validação de tipo</span><span class="sxs-lookup"><span data-stu-id="48977-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="48977-183">Tipo de validação ocorre apenas se a validação de propriedade for bem-sucedida</span><span class="sxs-lookup"><span data-stu-id="48977-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="48977-184">Se uma propriedade for complexa sua validação também incluirá:</span><span class="sxs-lookup"><span data-stu-id="48977-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="48977-185">Validação de nível de propriedade nas propriedades de tipo complexo</span><span class="sxs-lookup"><span data-stu-id="48977-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="48977-186">Validação de nível de tipo no tipo complexo, incluindo validação IValidatableObject no tipo complexo</span><span class="sxs-lookup"><span data-stu-id="48977-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="48977-187">Resumo</span><span class="sxs-lookup"><span data-stu-id="48977-187">Summary</span></span>

<span data-ttu-id="48977-188">A API de validação no Entity Framework é reproduzido muito bem com a validação do lado do cliente no MVC, mas você não precisa contar com validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="48977-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="48977-189">Entity Framework cuidará da validação no lado do servidor para DataAnnotations ou configurações que você tiver aplicado com a API Fluent do code first.</span><span class="sxs-lookup"><span data-stu-id="48977-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="48977-190">Você também viu um número de pontos de extensibilidade para personalizar o comportamento se você usar a interface IValidatableObject ou aproveita o método DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="48977-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="48977-191">E esses últimos dois meios de validação estão disponíveis por meio do DbContext, se você usar o fluxo de trabalho do Code First, Model First ou Database First para descrever o modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="48977-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
