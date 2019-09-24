---
title: Propriedades da sombra – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197706"
---
# <a name="shadow-properties"></a><span data-ttu-id="c6dda-102">Propriedades da sombra</span><span class="sxs-lookup"><span data-stu-id="c6dda-102">Shadow Properties</span></span>

<span data-ttu-id="c6dda-103">Propriedades de sombra são propriedades que não são definidas em sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c6dda-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="c6dda-104">O valor e o estado dessas propriedades são mantidos puramente no controlador de alterações.</span><span class="sxs-lookup"><span data-stu-id="c6dda-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="c6dda-105">As propriedades de sombra são úteis quando há dados no banco que não devem ser expostos nos tipos de entidade mapeada.</span><span class="sxs-lookup"><span data-stu-id="c6dda-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="c6dda-106">Eles são usados com mais frequência em Propriedades de chave estrangeira, em que a relação entre duas entidades é representada por um valor de chave estrangeira no banco de dados, mas a relação é gerenciada nos tipos de entidade usando propriedades de navegação entre os tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="c6dda-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="c6dda-107">Os valores de propriedade de sombra podem ser obtidos e `ChangeTracker` alterados por meio da API.</span><span class="sxs-lookup"><span data-stu-id="c6dda-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="c6dda-108">As propriedades de sombra podem ser referenciadas em consultas `EF.Property` LINQ por meio do método estático.</span><span class="sxs-lookup"><span data-stu-id="c6dda-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="c6dda-109">Convenções</span><span class="sxs-lookup"><span data-stu-id="c6dda-109">Conventions</span></span>

<span data-ttu-id="c6dda-110">As propriedades de sombra podem ser criadas por convenção quando uma relação é descoberta, mas nenhuma propriedade de chave estrangeira é encontrada na classe de entidade dependente.</span><span class="sxs-lookup"><span data-stu-id="c6dda-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="c6dda-111">Nesse caso, uma propriedade de chave estrangeira de sombra será introduzida.</span><span class="sxs-lookup"><span data-stu-id="c6dda-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="c6dda-112">A propriedade de chave estrangeira de sombra será `<navigation property name><principal key property name>` nomeada (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomenclatura).</span><span class="sxs-lookup"><span data-stu-id="c6dda-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="c6dda-113">Se o nome da propriedade da chave principal incluir o nome da propriedade de navegação, o nome será apenas `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c6dda-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="c6dda-114">Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança será usado em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="c6dda-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="c6dda-115">Por exemplo, a listagem de código a seguir resultará `BlogId` em uma propriedade de sombra sendo `Post` introduzida na entidade.</span><span class="sxs-lookup"><span data-stu-id="c6dda-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

## <a name="data-annotations"></a><span data-ttu-id="c6dda-116">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="c6dda-116">Data Annotations</span></span>

<span data-ttu-id="c6dda-117">Não é possível criar propriedades de sombra com anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="c6dda-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c6dda-118">API fluente</span><span class="sxs-lookup"><span data-stu-id="c6dda-118">Fluent API</span></span>

<span data-ttu-id="c6dda-119">Você pode usar a API Fluent para configurar propriedades de sombra.</span><span class="sxs-lookup"><span data-stu-id="c6dda-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="c6dda-120">Depois de ter chamado a sobrecarga de cadeia `Property` de caracteres, você pode encadear qualquer uma das chamadas de configuração para outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="c6dda-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="c6dda-121">Se o nome fornecido ao `Property` método corresponder ao nome de uma propriedade existente (uma propriedade de sombra ou uma definida na classe de entidade), o código configurará essa propriedade existente em vez de introduzir uma nova propriedade de sombra.</span><span class="sxs-lookup"><span data-stu-id="c6dda-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
