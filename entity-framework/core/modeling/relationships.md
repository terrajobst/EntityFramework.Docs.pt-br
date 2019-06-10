---
title: Relações – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 793401362788e865c89ce01b6246b1ba14c36c8a
ms.sourcegitcommit: 8b9568211d37a1c36da9533fa1ac2ef063b0bf8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2019
ms.locfileid: "66815004"
---
# <a name="relationships"></a>Relações

Uma relação define como duas entidades se relacionam entre si. Em um banco de dados relacional, isso é representado por uma restrição foreign key.

> [!NOTE]  
> A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos. Para obter exemplos de relações um para um e muitos-para-muitos, consulte o [outros padrões de relação](#other-relationship-patterns) seção no final do artigo.

## <a name="definition-of-terms"></a>Definição de termos

Há um número de termos usados para descrever relações

* **Entidade dependente:** Essa é a entidade que contém a propriedade de chave estrangeira (s). Às vezes chamado de "filho" da relação.

* **Entidade principal:** Essa é a entidade que contém as propriedades de chave alternativo/primário. Às vezes chamado de 'parent' da relação.

* **Chave estrangeira:** Propriedade (s) em que é usada para armazenar os valores da propriedade de chave principal que a entidade está relacionada à entidade dependente.

* **Chave da entidade:** A propriedade (s) que identifica exclusivamente a entidade principal. Isso pode ser a chave primária ou uma chave alternativa.

* **Propriedade de navegação:** Uma propriedade definida na entidade dependente e/ou entidade de segurança que contém uma referência (s) para a entidade relacionada (s).

  * **Propriedade de navegação de coleção:** Uma propriedade de navegação que contém referências a várias entidades relacionadas.

  * **Propriedade de navegação de referência:** Uma propriedade de navegação que contém uma referência a uma única entidade relacionada.

  * **Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação em particular, este termo refere-se a propriedade de navegação na outra extremidade da relação.

A listagem de código a seguir mostra uma relação um-para-muitos entre `Blog` e `Post`

* `Post` é a entidade dependente

* `Blog` é a entidade principal

* `Post.BlogId` é a chave estrangeira

* `Blog.BlogId` é a chave de entidade (nesse caso é uma chave primária em vez de uma chave alternativa)

* `Post.Blog` é uma propriedade de navegação de referência

* `Blog.Posts` é uma propriedade de navegação de coleção

* `Post.Blog` é a propriedade de navegação inverso do `Blog.Posts` (e vice-versa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenções

Por convenção, uma relação será criada quando há uma propriedade de navegação descoberta em um tipo. Uma propriedade é considerada uma propriedade de navegação, se o tipo que ele aponta não pode ser mapeado como um tipo escalar pelo provedor de banco de dados atual.

> [!NOTE]  
> As relações que são descobertas pela convenção sempre serão direcionada para a chave primária da entidade principal. Para direcionar para uma chave alternativa, uma configuração adicional deve ser executada usando a API do Fluent.

### <a name="fully-defined-relationships"></a>Relações totalmente definidas

O padrão mais comum para relações é ter as propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.

* Se um par de propriedades de navegação é encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversa da mesma relação.

* Se a entidade dependente contém uma propriedade chamada `<primary key property name>`, `<navigation property name><primary key property name>`, ou `<principal entity name><primary key property name>` e em seguida, ele será configurado como a chave estrangeira.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Se houver várias propriedades de navegação, definidas entre dois tipos (ou seja, mais de um par distinto de navegações que apontam para si), em seguida, nenhuma relação será criada por convenção, e você precisará configurá-las para identificar manualmente como o par de propriedades de navegação para cima.

### <a name="no-foreign-key-property"></a>Nenhuma propriedade de chave estrangeira

Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, não é necessário. Se nenhuma propriedade de chave estrangeira for encontrada, uma propriedade de chave estrangeira de sombra será introduzida com o nome `<navigation property name><principal key property name>` (consulte [propriedades de sombra](shadow-properties.md) para obter mais informações).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propriedade de navegação único

Incluindo apenas uma propriedade de navegação (nenhuma navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção. Você também pode ter uma propriedade de navegação e uma propriedade de chave estrangeira.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Excluir em cascata

Por convenção, a exclusão em cascata será definida como *Cascade* para as relações necessárias e *ClientSetNull* para relações opcionais. *CASCADE* significa entidades dependentes também serão excluídas. *ClientSetNull* significa que as entidades dependentes que não são carregadas na memória permanecerá inalterado e deve ser excluídas manualmente ou atualizada para apontar para uma entidade principal válida. Para entidades que são carregadas na memória, o EF Core tentará definir as propriedades de chave estrangeiras como nulo.

Consulte a [relações obrigatórios e opcionais](#required-and-optional-relationships) seção para a diferença entre as relações obrigatórios e opcionais.

Ver [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos e os padrões usados pela convenção de exclusão.

## <a name="data-annotations"></a>Anotações de dados

Há duas anotações de dados que podem ser usadas para configurar as relações, `[ForeignKey]` e `[InverseProperty]`. Eles estão disponíveis no `System.ComponentModel.DataAnnotations.Schema` namespace.

### <a name="foreignkey"></a>[ForeignKey]

Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação. Normalmente, isso é feito quando a propriedade de chave estrangeira não for descoberta por convenção.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> O `[ForeignKey]` anotação pode ser colocada em uma das propriedades de navegação na relação. Ele não precisa ir na propriedade de navegação na classe de entidade dependente.

### <a name="inverseproperty"></a>[InverseProperty]

Você pode usar as anotações de dados para configurar como as propriedades de navegação em entidades dependentes e principais ao emparelhar. Normalmente, isso é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>API fluente

Para configurar uma relação na API do Fluent, inicie identificando as propriedades de navegação que compõem a relação. `HasOne` ou `HasMany` identifica a propriedade de navegação no tipo de entidade que você estiver iniciando a configuração em. Uma chamada para então encadear `WithOne` ou `WithMany` para identificar a navegação inversa. `HasOne`/`WithOne` são usados para as propriedades de navegação de referência e `HasMany` / `WithMany` são usados para as propriedades de navegação de coleção.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Propriedade de navegação único

Se você tiver apenas uma propriedade de navegação, há sobrecargas sem parâmetro de `WithOne` e `WithMany`. Isso indica que há conceitualmente uma referência ou uma coleção na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Chave estrangeira

Você pode usar a API Fluent para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

A listagem de código a seguir mostra como configurar uma chave estrangeira composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

Você pode usar a sobrecarga de cadeia de caracteres de `HasForeignKey(...)` para configurar uma propriedade de sombra como uma chave estrangeira (consulte [propriedades de sombra](shadow-properties.md) para obter mais informações). É recomendável adicionar explicitamente a propriedade de sombra para o modelo antes de usá-lo como uma chave estrangeira (conforme mostrado abaixo).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Sem a propriedade de navegação

Você não precisa necessariamente fornecem uma propriedade de navegação. Você simplesmente pode fornecer uma chave estrangeira em um lado da relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Chave da entidade

Se você quiser que a chave estrangeira para fazer referência a uma propriedade que não seja a chave primária, você pode usar a API Fluent para configurar a propriedade de chave de entidade de segurança para a relação. A propriedade que você configurar como a chave será a entidade de segurança automaticamente ser configurado como uma chave alternativa (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

A listagem de código a seguir mostra como configurar uma chave de entidade composta.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> A ordem em que você especificar propriedades de chave de entidade de segurança deve corresponder à ordem na qual eles são especificados para a chave estrangeira.

### <a name="required-and-optional-relationships"></a>Relações obrigatórios e opcionais

Você pode usar a API Fluent para configurar se a relação é obrigatório ou opcional. Por fim, isso controla se a propriedade de chave estrangeira é obrigatório ou opcional. Isso é mais útil quando você estiver usando uma chave estrangeira do estado de sombra. Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, o requiredness da relação é determinado com base em se a propriedade de chave estrangeira é obrigatória ou opcional (consulte [propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações informações).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
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

### <a name="cascade-delete"></a>Excluir em cascata

Você pode usar a API Fluent para configurar o comportamento de exclusão em cascata para uma determinada relação explicitamente.

Ver [exclusão em cascata](../saving/cascade-delete.md) na seção salvando dados para uma discussão detalhada de cada opção.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Outros padrões de relação

### <a name="one-to-one"></a>Um para um

Relações um-para-um têm uma propriedade de navegação de referência em ambos os lados. Eles seguem as mesmas convenções de relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade de chave estrangeira para garantir que apenas um dependente está relacionado a cada entidade de segurança.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> O EF escolherá uma das entidades para ser dependente com base em sua capacidade de detectar uma propriedade de chave estrangeira. Se a entidade errada é escolhida como o dependente, você pode usar a API Fluent para corrigir isso.

Ao configurar a relação com a API Fluent, que você usar o `HasOne` e `WithOne` métodos.

Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente - Observe o parâmetro genérico fornecido para `HasForeignKey` na lista abaixo. Em uma relação um-para-muitos, é claro que a entidade com a navegação de referência é dependente e o com a coleção é a entidade de segurança. Mas isso não é então uma relação individual -, portanto, a necessidade de defini-lo explicitamente.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Muitos-para-muitos

Ainda não há suporte para relações muitos-para-muitos sem uma classe de entidade para representar a tabela de junção. No entanto, você pode representar uma relação muitos-para-muitos com a inclusão de uma classe de entidade para a tabela de junção e duas relações de um-para-muitos separado mapeamento.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
