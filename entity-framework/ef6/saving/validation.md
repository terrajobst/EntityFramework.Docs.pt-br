---
title: Validação-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 4162c2eb60109459c799da7cf4c1a9c8e84548b6
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182127"
---
# <a name="data-validation"></a>Validação de dados
> [!NOTE]
> **Somente EF 4.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 4,1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão

O conteúdo desta página é adaptado de um artigo escrito originalmente por Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com)).

O Entity Framework fornece uma grande variedade de recursos de validação que podem ser alimentados em uma interface do usuário para validação no lado do cliente ou usados para validação no lado do servidor. Ao usar o Code First, você pode especificar validações usando anotações ou configurações de API fluente. Validações adicionais e mais complexas podem ser especificadas no código e funcionarão independentemente de seu modelo Suz primeiro, primeiro modelo ou banco de dados.

## <a name="the-model"></a>O modelo

Demonstrarei as validações com um par simples de classes: Blog e post.

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

## <a name="data-annotations"></a>Anotações de dados

Code First usa anotações do assembly `System.ComponentModel.DataAnnotations` como um meio de configurar classes Code First. Entre essas anotações estão aquelas que fornecem regras como `Required`, `MaxLength` e `MinLength`. Vários aplicativos cliente .NET também reconhecem essas anotações, por exemplo, ASP.NET MVC. Você pode obter a validação do lado do cliente e do servidor com essas anotações. Por exemplo, você pode forçar a propriedade título do blog a ser uma propriedade necessária.

``` csharp
[Required]
public string Title { get; set; }
```

Sem alterações de código ou marcação adicionais no aplicativo, um aplicativo MVC existente executará a validação do lado do cliente, até mesmo criando dinamicamente uma mensagem usando os nomes da propriedade e da anotação.

![Figura 1](~/ef6/media/figure01.png)

No método de postagem desse modo de exibição de criação, Entity Framework é usado para salvar o novo blog no banco de dados, mas a validação do lado do cliente do MVC é disparada antes de o aplicativo atingir esse código.

No entanto, a validação do lado do cliente não é uma prova de sinal. Os usuários podem impactar os recursos de seu navegador ou, pior ainda, um hacker pode usar um truque para evitar as validações da interface do usuário. Mas Entity Framework também reconhecerá a anotação `Required` e a validará.

Uma maneira simples de testar isso é desabilitar o recurso de validação do lado do cliente do MVC. Você pode fazer isso no arquivo Web. config do aplicativo MVC. A seção appSettings tem uma chave para ClientValidationEnabled. Definir essa chave como false impedirá que a interface do usuário execute validações.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

Mesmo com a validação do lado do cliente desabilitada, você receberá a mesma resposta em seu aplicativo. A mensagem de erro "o campo título é obrigatório" será exibida como antes. Exceto agora, será resultado da validação do lado do servidor. Entity Framework executará a validação na anotação de `Required` (antes mesmo dos dois para criar um comando `INSERT` para enviar ao banco de dados) e retornar o erro para o MVC, que exibirá a mensagem.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent do Code First em vez de anotações para obter o mesmo lado do cliente & validação do lado do servidor. Em vez de usar `Required`, mostrarei isso usando uma validação de MaxLength.

As configurações de API fluente são aplicadas à medida que o Code First está criando o modelo a partir das classes. Você pode injetar as configurações substituindo o método OnModelCreating da classe DbContext. Aqui está uma configuração que especifica que a propriedade Bloggername não pode ter mais de 10 caracteres.

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

Os erros de validação gerados com base nas configurações de API fluente não acessarão automaticamente a interface do usuário, mas você pode capturá-los no código e, em seguida, respondê-los adequadamente.

Aqui está um código de erro de manipulação de exceção na classe BlogController do aplicativo que captura esse erro de validação quando Entity Framework tenta salvar um blog com um Bloggername que excede o máximo de 10 caracteres.

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

A validação não é repassada automaticamente para a exibição, porque o código adicional que usa `ModelState.AddModelError` está sendo usado. Isso garante que os detalhes do erro o façam na exibição, o que usará o `ValidationMessageFor` HtmlHelper para exibir o erro.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject` é uma interface que reside em `System.ComponentModel.DataAnnotations`. Embora não faça parte da API de Entity Framework, você ainda pode aproveitá-la para validação no lado do servidor em suas classes de Entity Framework. `IValidatableObject` fornece um método `Validate` que Entity Framework chamará durante SaveChanges ou você poderá chamar a qualquer momento que desejar validar as classes.

Configurações como `Required` e `MaxLength` executam a validação em um único campo. No método `Validate`, você pode ter uma lógica ainda mais complexa, por exemplo, comparando dois campos.

No exemplo a seguir, a classe `Blog` foi estendida para implementar `IValidatableObject` e, em seguida, fornecer uma regra que o `Title` e `BloggerName` não podem corresponder.

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

O Construtor `ValidationResult` usa um `string` que representa a mensagem de erro e uma matriz de `string`s que representam os nomes de membro associados à validação. Como essa validação verifica tanto o `Title` quanto o `BloggerName`, ambos os nomes de propriedade são retornados.

Ao contrário da validação fornecida pela API fluente, esse resultado de validação será reconhecido pela exibição e o manipulador de exceção que usei anteriormente para adicionar o erro em `ModelState` será desnecessário. Como defini os dois nomes de propriedade no `ValidationResult`, o MVC HtmlHelpers exibe a mensagem de erro para ambas as propriedades.

![Figura 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

`DbContext` tem um método substituível chamado `ValidateEntity`. Quando você chama `SaveChanges`, Entity Framework chamará esse método para cada entidade em seu cache cujo estado não é `Unchanged`. Você pode colocar a lógica de validação diretamente aqui ou até usar esse método para chamar, por exemplo, o método `Blog.Validate` adicionado na seção anterior.

Aqui está um exemplo de uma substituição `ValidateEntity` que valida novos `Post`s para garantir que o título de postagem ainda não tenha sido usado. Ele verifica primeiro se a entidade é uma postagem e se seu estado é adicionado. Se esse for o caso, ele procurará no banco de dados para ver se já existe uma postagem com o mesmo título. Se já houver uma postagem existente, um novo `DbEntityValidationResult` será criado.

`DbEntityValidationResult` aloja um `DbEntityEntry` e um `ICollection<DbValidationErrors>` para uma única entidade. No início desse método, um `DbEntityValidationResult` é instanciado e, em seguida, os erros descobertos são adicionados à sua coleção `ValidationErrors`.

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

## <a name="explicitly-triggering-validation"></a>Gatilho de validação explícita

Uma chamada para `SaveChanges` dispara todas as validações abordadas neste artigo. Mas você não precisa contar com `SaveChanges`. Você pode preferir validar em outro lugar em seu aplicativo.

`DbContext.GetValidationErrors` disparará todas as validações, aquelas definidas por anotações ou a API fluente, a validação criada em `IValidatableObject` (por exemplo, `Blog.Validate`) e as validações executadas no método `DbContext.ValidateEntity`.

O código a seguir chamará `GetValidationErrors` na instância atual de um `DbContext`. `ValidationErrors` são agrupados por tipo de entidade em `DbEntityValidationResult`. O código é iterado primeiro por meio de @no__t 0s retornados pelo método e, em seguida, por cada `DbValidationError` dentro de.

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

## <a name="other-considerations-when-using-validation"></a>Outras considerações ao usar a validação

Aqui estão alguns outros pontos a serem considerados ao usar a validação de Entity Framework:

- O carregamento lento está desabilitado durante a validação
- O EF validará as anotações de dados em Propriedades não mapeadas (propriedades que não são mapeadas para uma coluna no banco de dados)
- A validação é executada Depois que as alterações são detectadas durante `SaveChanges`. Se você fizer alterações durante a validação, será sua responsabilidade notificar o rastreador de alterações
- `DbUnexpectedValidationException` será gerado se ocorrerem erros durante a validação
- As facetas que Entity Framework incluem no modelo (comprimento máximo, obrigatório, etc.) causarão a validação, mesmo se não houver nenhuma anotação de dados em suas classes e/ou se você tiver usado o designer do EF para criar seu modelo
- Regras de precedência:
  - Chamadas à API fluente substituem as anotações de dados correspondentes
- Ordem de execução:
  - A validação da propriedade ocorre antes da validação de tipo
  - A validação de tipo ocorrerá somente se a validação de propriedade for realizada com sucesso
- Se uma propriedade for complexa, sua validação também incluirá:
  - Validação em nível de propriedade nas propriedades de tipo complexo
  - Validação de nível de tipo no tipo complexo, incluindo validação de `IValidatableObject` no tipo complexo

## <a name="summary"></a>Resumo

A API de validação no Entity Framework é executada muito bem com a validação do lado do cliente no MVC, mas você não precisa contar com a validação do lado do cliente. Entity Framework cuidará da validação no lado do servidor para Annotations ou configurações que você aplicou com a API Fluent do Code First.

Você também viu vários pontos de extensibilidade para personalizar o comportamento se usar a interface `IValidatableObject` ou tocar no método `DbContext.ValidateEntity`. E esses dois últimos meios de validação estão disponíveis por meio do `DbContext`, quer você use o Code First, Model First ou Database First fluxo de trabalho para descrever seu modelo conceitual.
