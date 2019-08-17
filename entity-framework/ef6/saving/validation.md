---
title: Validação-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565370"
---
# <a name="data-validation"></a><span data-ttu-id="780f6-102">Validação de dados</span><span class="sxs-lookup"><span data-stu-id="780f6-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="780f6-103">**Somente EF 4.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="780f6-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="780f6-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão</span><span class="sxs-lookup"><span data-stu-id="780f6-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="780f6-105">O conteúdo desta página é adaptado de um artigo escrito originalmente por Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="780f6-105">The content on this page is adapted from an article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="780f6-106">O Entity Framework fornece uma grande variedade de recursos de validação que podem ser alimentados em uma interface do usuário para validação no lado do cliente ou usados para validação no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="780f6-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="780f6-107">Ao usar o Code First, você pode especificar validações usando anotações ou configurações de API fluente.</span><span class="sxs-lookup"><span data-stu-id="780f6-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="780f6-108">Validações adicionais e mais complexas podem ser especificadas no código e funcionarão independentemente de seu modelo Suz primeiro, primeiro modelo ou banco de dados.</span><span class="sxs-lookup"><span data-stu-id="780f6-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="780f6-109">O modelo</span><span class="sxs-lookup"><span data-stu-id="780f6-109">The model</span></span>

<span data-ttu-id="780f6-110">Demonstrarei as validações com um par simples de classes: Blog e post.</span><span class="sxs-lookup"><span data-stu-id="780f6-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

## <a name="data-annotations"></a><span data-ttu-id="780f6-111">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="780f6-111">Data Annotations</span></span>

<span data-ttu-id="780f6-112">Code First usa anotações do `System.ComponentModel.DataAnnotations` assembly como um meio de configurar classes Code First.</span><span class="sxs-lookup"><span data-stu-id="780f6-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="780f6-113">Entre essas anotações estão aquelas que fornecem regras como `Required`, `MaxLength` e `MinLength`.</span><span class="sxs-lookup"><span data-stu-id="780f6-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="780f6-114">Vários aplicativos cliente .NET também reconhecem essas anotações, por exemplo, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="780f6-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="780f6-115">Você pode obter a validação do lado do cliente e do servidor com essas anotações.</span><span class="sxs-lookup"><span data-stu-id="780f6-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="780f6-116">Por exemplo, você pode forçar a propriedade título do blog a ser uma propriedade necessária.</span><span class="sxs-lookup"><span data-stu-id="780f6-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="780f6-117">Sem alterações de código ou marcação adicionais no aplicativo, um aplicativo MVC existente executará a validação do lado do cliente, até mesmo criando dinamicamente uma mensagem usando os nomes da propriedade e da anotação.</span><span class="sxs-lookup"><span data-stu-id="780f6-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Figura 1](~/ef6/media/figure01.png)

<span data-ttu-id="780f6-119">No método de postagem desse modo de exibição de criação, Entity Framework é usado para salvar o novo blog no banco de dados, mas a validação do lado do cliente do MVC é disparada antes de o aplicativo atingir esse código.</span><span class="sxs-lookup"><span data-stu-id="780f6-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="780f6-120">No entanto, a validação do lado do cliente não é uma prova de sinal.</span><span class="sxs-lookup"><span data-stu-id="780f6-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="780f6-121">Os usuários podem impactar os recursos de seu navegador ou, pior ainda, um hacker pode usar um truque para evitar as validações da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="780f6-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="780f6-122">Mas Entity Framework também reconhecerá `Required` a anotação e a validará.</span><span class="sxs-lookup"><span data-stu-id="780f6-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="780f6-123">Uma maneira simples de testar isso é desabilitar o recurso de validação do lado do cliente do MVC.</span><span class="sxs-lookup"><span data-stu-id="780f6-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="780f6-124">Você pode fazer isso no arquivo Web. config do aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="780f6-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="780f6-125">A seção appSettings tem uma chave para ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="780f6-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="780f6-126">Definir essa chave como false impedirá que a interface do usuário execute validações.</span><span class="sxs-lookup"><span data-stu-id="780f6-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="780f6-127">Mesmo com a validação do lado do cliente desabilitada, você receberá a mesma resposta em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="780f6-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="780f6-128">A mensagem de erro "o campo título é obrigatório" será exibida como antes.</span><span class="sxs-lookup"><span data-stu-id="780f6-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="780f6-129">Exceto agora, será resultado da validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="780f6-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="780f6-130">Entity Framework executará a validação na `Required` anotação (antes mesmo dos dois para criar um `INSERT` comando para enviar ao banco de dados) e retornar o erro para MVC, que exibirá a mensagem.</span><span class="sxs-lookup"><span data-stu-id="780f6-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="780f6-131">API fluente</span><span class="sxs-lookup"><span data-stu-id="780f6-131">Fluent API</span></span>

<span data-ttu-id="780f6-132">Você pode usar a API Fluent do Code First em vez de anotações para obter o mesmo lado do cliente & validação do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="780f6-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="780f6-133">Em vez de `Required`usar, mostrarei isso usando uma validação MaxLength.</span><span class="sxs-lookup"><span data-stu-id="780f6-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="780f6-134">As configurações de API fluente são aplicadas à medida que o Code First está criando o modelo a partir das classes.</span><span class="sxs-lookup"><span data-stu-id="780f6-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="780f6-135">Você pode injetar as configurações substituindo o método OnModelCreating da classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="780f6-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="780f6-136">Aqui está uma configuração que especifica que a propriedade Bloggername não pode ter mais de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="780f6-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="780f6-137">Os erros de validação gerados com base nas configurações de API fluente não acessarão automaticamente a interface do usuário, mas você pode capturá-los no código e, em seguida, respondê-los adequadamente.</span><span class="sxs-lookup"><span data-stu-id="780f6-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="780f6-138">Aqui está um código de erro de manipulação de exceção na classe BlogController do aplicativo que captura esse erro de validação quando Entity Framework tenta salvar um blog com um Bloggername que excede o máximo de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="780f6-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="780f6-139">A validação não é passada automaticamente para a exibição, e é por isso que o código adicional `ModelState.AddModelError` que usa o está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="780f6-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="780f6-140">Isso garante que os detalhes do erro o façam na exibição, o que usará `ValidationMessageFor` o HtmlHelper para exibir o erro.</span><span class="sxs-lookup"><span data-stu-id="780f6-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="780f6-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="780f6-141">IValidatableObject</span></span>

<span data-ttu-id="780f6-142">`IValidatableObject`é uma interface que reside no `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="780f6-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="780f6-143">Embora não faça parte da API de Entity Framework, você ainda pode aproveitá-la para validação no lado do servidor em suas classes de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="780f6-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="780f6-144">`IValidatableObject`fornece um `Validate` método que Entity Framework será chamado durante SaveChanges ou você poderá chamar a qualquer momento que desejar validar as classes.</span><span class="sxs-lookup"><span data-stu-id="780f6-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="780f6-145">Configurações como `Required` e `MaxLength` executam a validação em um único campo.</span><span class="sxs-lookup"><span data-stu-id="780f6-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="780f6-146">`Validate` No método, você pode ter uma lógica ainda mais complexa, por exemplo, comparando dois campos.</span><span class="sxs-lookup"><span data-stu-id="780f6-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="780f6-147">No exemplo a seguir, a `Blog` classe foi estendida para implementar `IValidatableObject` e, em seguida, fornecer uma `Title` regra `BloggerName` que o e o não podem corresponder.</span><span class="sxs-lookup"><span data-stu-id="780f6-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="780f6-148">O `ValidationResult` construtor usa um `string` que representa a mensagem de erro e uma matriz `string`de s que representam os nomes de membro associados à validação.</span><span class="sxs-lookup"><span data-stu-id="780f6-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="780f6-149">Como essa validação verifica tanto o `Title` quanto o `BloggerName`, ambos os nomes de propriedade são retornados.</span><span class="sxs-lookup"><span data-stu-id="780f6-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="780f6-150">Ao contrário da validação fornecida pela API fluente, esse resultado de validação será reconhecido pela exibição e o manipulador de exceção que usei anteriormente para adicionar o erro `ModelState` é desnecessário.</span><span class="sxs-lookup"><span data-stu-id="780f6-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="780f6-151">Como defini os dois nomes de propriedade no `ValidationResult`, o MVC HtmlHelpers exibe a mensagem de erro para ambas as propriedades.</span><span class="sxs-lookup"><span data-stu-id="780f6-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Figura 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="780f6-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="780f6-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="780f6-154">`DbContext`tem um método substituível `ValidateEntity`chamado.</span><span class="sxs-lookup"><span data-stu-id="780f6-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="780f6-155">Quando você chamar `SaveChanges`, Entity Framework chamará esse método para cada entidade em seu cache cujo estado não `Unchanged`é.</span><span class="sxs-lookup"><span data-stu-id="780f6-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="780f6-156">Você pode colocar a lógica de validação diretamente aqui ou até mesmo usar esse método para chamar, por exemplo `Blog.Validate` , o método adicionado na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="780f6-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="780f6-157">Aqui está um exemplo de uma `ValidateEntity` substituição que valida novas `Post`s para garantir que o título da postagem ainda não tenha sido usado.</span><span class="sxs-lookup"><span data-stu-id="780f6-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="780f6-158">Ele verifica primeiro se a entidade é uma postagem e se seu estado é adicionado.</span><span class="sxs-lookup"><span data-stu-id="780f6-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="780f6-159">Se esse for o caso, ele procurará no banco de dados para ver se já existe uma postagem com o mesmo título.</span><span class="sxs-lookup"><span data-stu-id="780f6-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="780f6-160">Se já houver uma postagem existente, um novo `DbEntityValidationResult` será criado.</span><span class="sxs-lookup"><span data-stu-id="780f6-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="780f6-161">`DbEntityValidationResult`hospeda um `DbEntityEntry` e um `ICollection<DbValidationErrors>` para uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="780f6-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="780f6-162">No início desse método, um `DbEntityValidationResult` é instanciado e, em seguida, quaisquer erros descobertos são adicionados à sua `ValidationErrors` coleção.</span><span class="sxs-lookup"><span data-stu-id="780f6-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="780f6-163">Gatilho de validação explícita</span><span class="sxs-lookup"><span data-stu-id="780f6-163">Explicitly triggering validation</span></span>

<span data-ttu-id="780f6-164">Uma chamada para `SaveChanges` dispara todas as validações abordadas neste artigo.</span><span class="sxs-lookup"><span data-stu-id="780f6-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="780f6-165">Mas você não precisa contar `SaveChanges`com o.</span><span class="sxs-lookup"><span data-stu-id="780f6-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="780f6-166">Você pode preferir validar em outro lugar em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="780f6-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="780f6-167">`DbContext.GetValidationErrors`o disparará todas as validações, aquelas definidas por anotações ou a API Fluent, a validação criada em `IValidatableObject` (por exemplo, `Blog.Validate`) e as `DbContext.ValidateEntity` validações executadas no método.</span><span class="sxs-lookup"><span data-stu-id="780f6-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="780f6-168">O código a seguir chamará `GetValidationErrors` a instância atual de a. `DbContext`</span><span class="sxs-lookup"><span data-stu-id="780f6-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="780f6-169">`ValidationErrors`são agrupados por tipo de entidade `DbEntityValidationResult`em.</span><span class="sxs-lookup"><span data-stu-id="780f6-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="780f6-170">O código é iterado primeiro por `DbEntityValidationResult`meio dos s retornados pelo método e, em `DbValidationError` seguida, por meio de cada dentro.</span><span class="sxs-lookup"><span data-stu-id="780f6-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="780f6-171">Outras considerações ao usar a validação</span><span class="sxs-lookup"><span data-stu-id="780f6-171">Other considerations when using validation</span></span>

<span data-ttu-id="780f6-172">Aqui estão alguns outros pontos a serem considerados ao usar a validação de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="780f6-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="780f6-173">O carregamento lento está desabilitado durante a validação</span><span class="sxs-lookup"><span data-stu-id="780f6-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="780f6-174">O EF validará as anotações de dados em Propriedades não mapeadas (propriedades que não são mapeadas para uma coluna no banco de dados)</span><span class="sxs-lookup"><span data-stu-id="780f6-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="780f6-175">A validação é executada Depois que as alterações `SaveChanges`são detectadas durante.</span><span class="sxs-lookup"><span data-stu-id="780f6-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="780f6-176">Se você fizer alterações durante a validação, será sua responsabilidade notificar o rastreador de alterações</span><span class="sxs-lookup"><span data-stu-id="780f6-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="780f6-177">`DbUnexpectedValidationException`será gerado se ocorrerem erros durante a validação</span><span class="sxs-lookup"><span data-stu-id="780f6-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="780f6-178">As facetas que Entity Framework incluem no modelo (comprimento máximo, obrigatório, etc.) causarão a validação, mesmo se não houver nenhuma anotação de dados em suas classes e/ou se você tiver usado o designer do EF para criar seu modelo</span><span class="sxs-lookup"><span data-stu-id="780f6-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="780f6-179">Regras de precedência:</span><span class="sxs-lookup"><span data-stu-id="780f6-179">Precedence rules:</span></span>
  - <span data-ttu-id="780f6-180">Chamadas à API fluente substituem as anotações de dados correspondentes</span><span class="sxs-lookup"><span data-stu-id="780f6-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="780f6-181">Ordem de execução:</span><span class="sxs-lookup"><span data-stu-id="780f6-181">Execution order:</span></span>
  - <span data-ttu-id="780f6-182">A validação da propriedade ocorre antes da validação de tipo</span><span class="sxs-lookup"><span data-stu-id="780f6-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="780f6-183">A validação de tipo ocorrerá somente se a validação de propriedade for realizada com sucesso</span><span class="sxs-lookup"><span data-stu-id="780f6-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="780f6-184">Se uma propriedade for complexa, sua validação também incluirá:</span><span class="sxs-lookup"><span data-stu-id="780f6-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="780f6-185">Validação em nível de propriedade nas propriedades de tipo complexo</span><span class="sxs-lookup"><span data-stu-id="780f6-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="780f6-186">Validação de nível de tipo no tipo complexo, `IValidatableObject` incluindo validação no tipo complexo</span><span class="sxs-lookup"><span data-stu-id="780f6-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="780f6-187">Resumo</span><span class="sxs-lookup"><span data-stu-id="780f6-187">Summary</span></span>

<span data-ttu-id="780f6-188">A API de validação no Entity Framework é executada muito bem com a validação do lado do cliente no MVC, mas você não precisa contar com a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="780f6-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="780f6-189">Entity Framework cuidará da validação no lado do servidor para Annotations ou configurações que você aplicou com a API Fluent do Code First.</span><span class="sxs-lookup"><span data-stu-id="780f6-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="780f6-190">Você também viu vários pontos de extensibilidade para personalizar o comportamento se usar a `IValidatableObject` interface ou tocar `DbContext.ValidateEntity` no método.</span><span class="sxs-lookup"><span data-stu-id="780f6-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="780f6-191">E esses dois últimos meios de validação estão disponíveis por meio `DbContext`do, independentemente de você usar o Code First, Model First ou Database First fluxo de trabalho para descrever seu modelo conceitual.</span><span class="sxs-lookup"><span data-stu-id="780f6-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
