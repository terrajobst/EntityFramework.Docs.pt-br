---
title: Relações-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197655"
---
# <a name="relationships"></a>Relações

Uma relação define como duas entidades se relacionam entre si. Em um banco de dados relacional, isso é representado por uma restrição FOREIGN KEY.

> [!NOTE]  
> A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos. Para obter exemplos de relações um-para-um e muitos para muitos, consulte a seção [outros padrões de relação](#other-relationship-patterns) no final do artigo.

## <a name="definition-of-terms"></a>Definição de termos

Há vários termos usados para descrever as relações

* **Entidade dependente:** Esta é a entidade que contém as propriedades de chave estrangeira. Às vezes, chamado de ' filho ' da relação.

* **Entidade principal:** Esta é a entidade que contém as propriedades de chave primária/alternativa. Às vezes, chamado de ' pai ' da relação.

* **Chave estrangeira:** As propriedades na entidade dependente que é usada para armazenar os valores da propriedade de chave principal à qual a entidade está relacionada.

* **Chave principal:** As propriedades que identificam exclusivamente a entidade principal. Essa pode ser a chave primária ou uma chave alternativa.

* **Propriedade de navegação:** Uma propriedade definida na entidade principal e/ou dependente que contém uma referência (s) para as entidades relacionadas.

  * **Propriedade de navegação da coleção:** Uma propriedade de navegação que contém referências a muitas entidades relacionadas.

  * **Propriedade de navegação de referência:** Uma propriedade de navegação que mantém uma referência a uma única entidade relacionada.

  * **Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação específica, esse termo refere-se à propriedade de navegação na outra extremidade da relação.

A listagem de código a seguir mostra uma relação um-para- `Blog` muitos entre e`Post`

* `Post`é a entidade dependente

* `Blog`é a entidade principal

* `Post.BlogId`é a chave estrangeira

* `Blog.BlogId`é a chave principal (nesse caso, é uma chave primária em vez de uma chave alternativa)

* `Post.Blog`é uma propriedade de navegação de referência

* `Blog.Posts`é uma propriedade de navegação de coleção

* `Post.Blog`é a propriedade de navegação inversa `Blog.Posts` de (e vice-versa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenções

Por convenção, uma relação será criada quando houver uma propriedade de navegação descoberta em um tipo. Uma propriedade é considerada uma propriedade de navegação se o tipo que ele aponta não puder ser mapeado como um tipo escalar pelo provedor de banco de dados atual.

> [!NOTE]  
> As relações descobertas por convenção serão sempre direcionadas à chave primária da entidade principal. Para direcionar uma chave alternativa, a configuração adicional deve ser executada usando a API Fluent.

### <a name="fully-defined-relationships"></a>Relações totalmente definidas

O padrão mais comum para relações é ter propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.

* Se um par de propriedades de navegação for encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversas da mesma relação.

* Se a entidade dependente contiver uma propriedade `<primary key property name>`chamada `<navigation property name><primary key property name>`, ou `<principal entity name><primary key property name>` , ela será configurada como a chave estrangeira.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Se houver várias propriedades de navegação definidas entre dois tipos (ou seja, mais de um par distinto de navegações que apontam entre si), nenhuma relação será criada por convenção e você precisará configurá-las manualmente para identificar como o Propriedades de navegação emparelhadas.

### <a name="no-foreign-key-property"></a>Nenhuma propriedade de chave estrangeira

Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, ela não é necessária. Se nenhuma propriedade de chave estrangeira for encontrada, uma propriedade de chave estrangeira de sombra será introduzida `<navigation property name><principal key property name>` com o nome (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propriedade de navegação única

Incluir apenas uma propriedade de navegação (sem navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção. Você também pode ter uma única propriedade de navegação e uma propriedade de chave estrangeira.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Excluir em cascata

Por convenção, a exclusão em cascata será definida como *em cascata* para relações necessárias e *ClientSetNull* para relações opcionais. *Cascade* significa que as entidades dependentes também são excluídas. *ClientSetNull* significa que as entidades dependentes que não estão carregadas na memória permanecerão inalteradas e deverão ser excluídas manualmente ou atualizadas para apontar para uma entidade principal válida. Para entidades que são carregadas na memória, EF Core tentará definir as propriedades de chave estrangeira como NULL.

Consulte a seção [relações obrigatórias e opcionais](#required-and-optional-relationships) para obter a diferença entre as relações obrigatórias e opcionais.

Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos de exclusão e os padrões usados pela Convenção.

## <a name="data-annotations"></a>Anotações de dados

Há duas anotações de dados que podem ser usadas para configurar relações `[ForeignKey]` e. `[InverseProperty]` Eles estão disponíveis no `System.ComponentModel.DataAnnotations.Schema` namespace.

### <a name="foreignkey"></a>ForeignKey

Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação. Isso normalmente é feito quando a propriedade de chave estrangeira não é descoberta pela Convenção.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> A `[ForeignKey]` anotação pode ser colocada em qualquer propriedade de navegação na relação. Ele não precisa ir para a propriedade de navegação na classe de entidade dependente.

### <a name="inverseproperty"></a>[Inversoproperty]

Você pode usar as anotações de dados para configurar como as propriedades de navegação nas entidades dependentes e de entidade emparelham. Isso normalmente é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>API fluente

Para configurar uma relação na API fluente, você começa identificando as propriedades de navegação que compõem a relação. `HasOne`ou `HasMany` identifica a propriedade de navegação no tipo de entidade no qual você está iniciando a configuração. Em seguida, você encadea uma `WithOne` chamada `WithMany` para ou para identificar a navegação inversa. `HasOne`/`WithOne`são usadas para propriedades de navegação de `HasMany` referência e / `WithMany` são usadas para propriedades de navegação de coleção.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Propriedade de navegação única

Se você tiver apenas uma propriedade de navegação, haverá sobrecargas sem parâmetros de `WithOne` e `WithMany`. Isso indica que há uma referência ou uma coleção conceitualmente na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Chave estrangeira

Você pode usar a API fluente para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

A listagem de código a seguir mostra como configurar uma chave estrangeira composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Você pode usar a sobrecarga de cadeia `HasForeignKey(...)` de caracteres de para configurar uma propriedade de sombra como uma chave estrangeira (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações). É recomendável adicionar explicitamente a propriedade Shadow ao modelo antes de usá-la como uma chave estrangeira (como mostrado abaixo).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Sem propriedade de navegação

Você não precisa necessariamente fornecer uma propriedade de navegação. Você pode simplesmente fornecer uma chave estrangeira em um lado da relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Chave principal

Se desejar que a chave estrangeira referencie uma propriedade diferente da chave primária, você poderá usar a API Fluent para configurar a propriedade principal de chave para a relação. A propriedade que você configurar como a chave principal será automaticamente configurada como uma chave alternativa (consulte [chaves alternativas](alternate-keys.md) para obter mais informações).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> A ordem na qual você especifica as propriedades da chave de entidade de segurança deve corresponder à ordem na qual elas são especificadas para a chave estrangeira.

### <a name="required-and-optional-relationships"></a>Relações obrigatórias e opcionais

Você pode usar a API fluente para configurar se a relação é necessária ou opcional. Por fim, isso controla se a propriedade de chave estrangeira é necessária ou opcional. Isso é mais útil quando você está usando uma chave estrangeira de estado de sombra. Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, a exigência da relação será determinada com base em se a propriedade de chave estrangeira é necessária ou opcional (consulte [as propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

Consulte [exclusão em cascata](../saving/cascade-delete.md) na seção salvando dados para obter uma discussão detalhada de cada opção.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

### <a name="one-to-one"></a>Um-para-um

Relações um para um têm uma propriedade de navegação de referência em ambos os lados. Eles seguem as mesmas convenções que as relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade Foreign Key para garantir que apenas um dependente esteja relacionado a cada entidade de segurança.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> O EF escolherá uma das entidades como dependente, com base em sua capacidade de detectar uma propriedade de chave estrangeira. Se a entidade incorreta for escolhida como dependente, você poderá usar a API fluente para corrigir isso.

Ao configurar a relação com a API Fluent, você usa os `HasOne` métodos `WithOne` e.

Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente-Observe o parâmetro genérico fornecido `HasForeignKey` na lista abaixo. Em uma relação um-para-muitos, fica claro que a entidade com a navegação de referência é a dependente e aquela com a coleção é a principal. Mas isso não é tão em relação um-para-um-portanto, a necessidade de defini-lo explicitamente.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a>Muitos para muitos

Relações muitos para muitos sem uma classe de entidade para representar a tabela de junção ainda não têm suporte. No entanto, você pode representar uma relação muitos para muitos incluindo uma classe de entidade para a tabela de junção e mapeando duas relações um-para-muitos separadas.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
