---
title: Code First de anotações de dados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419181"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="4a715-102">Code First anotações de dados</span><span class="sxs-lookup"><span data-stu-id="4a715-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="4a715-103">**Somente EF 4.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="4a715-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="4a715-104">Se você estiver usando uma versão anterior, algumas ou todas essas informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="4a715-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="4a715-105">O conteúdo desta página é adaptado de um artigo escrito originalmente por Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="4a715-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="4a715-106">Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo que o EF depende para executar consultas, controle de alterações e atualizações de funções.</span><span class="sxs-lookup"><span data-stu-id="4a715-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="4a715-107">Code First aproveita um padrão de programação conhecido como ' Convenção sobre configuração '.</span><span class="sxs-lookup"><span data-stu-id="4a715-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="4a715-108">Code First presumirá que suas classes seguem as convenções de Entity Framework e, nesse caso, irão automaticamente resolver como executar seu trabalho.</span><span class="sxs-lookup"><span data-stu-id="4a715-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform its job.</span></span> <span data-ttu-id="4a715-109">No entanto, se suas classes não seguirem essas convenções, você terá a capacidade de adicionar configurações às suas classes para fornecer ao EF as informações necessárias.</span><span class="sxs-lookup"><span data-stu-id="4a715-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="4a715-110">Code First oferece duas maneiras de adicionar essas configurações às suas classes.</span><span class="sxs-lookup"><span data-stu-id="4a715-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="4a715-111">Uma delas é usar atributos simples, chamados Annotations, e a segunda é usar a API fluente do Code First, que fornece uma maneira de descrever as configurações imperativamente, no código.</span><span class="sxs-lookup"><span data-stu-id="4a715-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="4a715-112">Este artigo se concentrará no uso de Annotations (no namespace System. ComponentModel. Annotations) para configurar suas classes – destacando as configurações mais comumente necessárias.</span><span class="sxs-lookup"><span data-stu-id="4a715-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="4a715-113">Annotations também são compreendidos por vários aplicativos .net, como o ASP.NET MVC, que permite que esses aplicativos aproveitem as mesmas anotações para validações do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="4a715-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="4a715-114">O modelo</span><span class="sxs-lookup"><span data-stu-id="4a715-114">The model</span></span>

<span data-ttu-id="4a715-115">Demonstrarei Code First Annotations com um par simples de classes: blog e post.</span><span class="sxs-lookup"><span data-stu-id="4a715-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
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

<span data-ttu-id="4a715-116">Como estão, as classes blog e post seguem convenientemente a Convenção do código e não exigem ajustes para habilitar a compatibilidade do EF.</span><span class="sxs-lookup"><span data-stu-id="4a715-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="4a715-117">No entanto, você também pode usar as anotações para fornecer mais informações ao EF sobre as classes e o banco de dados no qual eles são mapeados.</span><span class="sxs-lookup"><span data-stu-id="4a715-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="4a715-118">Chave</span><span class="sxs-lookup"><span data-stu-id="4a715-118">Key</span></span>

<span data-ttu-id="4a715-119">Entity Framework se baseia em cada entidade com um valor de chave que é usado para acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="4a715-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="4a715-120">Uma Convenção de Code First é Propriedades de chave implícita; Code First procurará uma propriedade chamada "ID" ou uma combinação de nome de classe e "ID", como "blogid".</span><span class="sxs-lookup"><span data-stu-id="4a715-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="4a715-121">Essa propriedade será mapeada para uma coluna de chave primária no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="4a715-122">As classes blog e post seguem essa convenção.</span><span class="sxs-lookup"><span data-stu-id="4a715-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="4a715-123">E se eles não estivessem?</span><span class="sxs-lookup"><span data-stu-id="4a715-123">What if they didn’t?</span></span> <span data-ttu-id="4a715-124">E se o blog usava o nome *PrimaryTrackingKey* , ou até *foo*?</span><span class="sxs-lookup"><span data-stu-id="4a715-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="4a715-125">Se o Code First não encontrar uma propriedade que corresponda a essa convenção, ele lançará uma exceção devido ao requisito de Entity Framework que você deve ter uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="4a715-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="4a715-126">Você pode usar a anotação de chave para especificar qual propriedade deve ser usada como EntityKey.</span><span class="sxs-lookup"><span data-stu-id="4a715-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="4a715-127">Se você estiver usando o recurso de geração de banco de dados de primeiro código, a tabela de blog terá uma coluna de chave primária denominada PrimaryTrackingKey, que também é definida como identidade por padrão.</span><span class="sxs-lookup"><span data-stu-id="4a715-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Tabela de blog com chave primária](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="4a715-129">Chaves compostas</span><span class="sxs-lookup"><span data-stu-id="4a715-129">Composite keys</span></span>

<span data-ttu-id="4a715-130">Entity Framework dá suporte a chaves compostas-chaves primárias que consistem em mais de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="4a715-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="4a715-131">Por exemplo, você poderia ter uma classe do Passport cuja chave primária é uma combinação de PassportNumber e IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="4a715-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="4a715-132">A tentativa de usar a classe acima em seu modelo de EF resultaria em um `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="4a715-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="4a715-133">*Não é possível determinar a ordenação de chave primária composta para o tipo ' Passport '. Use o método ColumnAttribute ou HasKey para especificar uma ordem para chaves primárias compostas.*</span><span class="sxs-lookup"><span data-stu-id="4a715-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="4a715-134">Para usar chaves compostas, Entity Framework exige que você defina uma ordem para as propriedades de chave.</span><span class="sxs-lookup"><span data-stu-id="4a715-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="4a715-135">Você pode fazer isso usando a anotação de coluna para especificar um pedido.</span><span class="sxs-lookup"><span data-stu-id="4a715-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="4a715-136">O valor do pedido é relativo (em vez de baseado em índice) para que qualquer valor possa ser usado.</span><span class="sxs-lookup"><span data-stu-id="4a715-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="4a715-137">Por exemplo, 100 e 200 seriam aceitáveis no lugar de 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="4a715-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="4a715-138">Se você tiver entidades com chaves estrangeiras compostas, deverá especificar a mesma ordenação de coluna usada para as propriedades de chave primária correspondentes.</span><span class="sxs-lookup"><span data-stu-id="4a715-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="4a715-139">Somente a ordenação relativa dentro das propriedades de chave estrangeira precisa ser a mesma, os valores exatos atribuídos à **ordem** não precisam corresponder.</span><span class="sxs-lookup"><span data-stu-id="4a715-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="4a715-140">Por exemplo, na classe a seguir, 3 e 4 podem ser usados no lugar de 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="4a715-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="4a715-141">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="4a715-141">Required</span></span>

<span data-ttu-id="4a715-142">A anotação necessária informa ao EF que uma determinada propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="4a715-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="4a715-143">Adicionar necessário à propriedade Title forçará o EF (e MVC) a garantir que a propriedade tenha dados nele.</span><span class="sxs-lookup"><span data-stu-id="4a715-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="4a715-144">Sem alterações de código ou marcação adicionais no aplicativo, um aplicativo MVC executará a validação do lado do cliente, até mesmo criando dinamicamente uma mensagem usando os nomes da propriedade e da anotação.</span><span class="sxs-lookup"><span data-stu-id="4a715-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Criar página com título é um erro obrigatório](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="4a715-146">O atributo Required também afetará o banco de dados gerado, tornando a propriedade mapeada não anulável.</span><span class="sxs-lookup"><span data-stu-id="4a715-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="4a715-147">Observe que o campo título foi alterado para "não nulo".</span><span class="sxs-lookup"><span data-stu-id="4a715-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="4a715-148">Em alguns casos, talvez não seja possível que a coluna no banco de dados seja não anulável, embora a propriedade seja necessária.</span><span class="sxs-lookup"><span data-stu-id="4a715-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="4a715-149">Por exemplo, ao usar uma estratégia de herança TPH, os dados de vários tipos são armazenados em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="4a715-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="4a715-150">Se um tipo derivado incluir uma propriedade obrigatória, a coluna não poderá se tornar não anulável, pois nem todos os tipos na hierarquia terão essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="4a715-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Tabela de Blogs](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="4a715-152">MaxLength e MinLength</span><span class="sxs-lookup"><span data-stu-id="4a715-152">MaxLength and MinLength</span></span>

<span data-ttu-id="4a715-153">Os atributos MaxLength e MinLength permitem especificar validações de propriedade adicionais, exatamente como você fez com required.</span><span class="sxs-lookup"><span data-stu-id="4a715-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="4a715-154">Aqui está o Bloggername com requisitos de comprimento.</span><span class="sxs-lookup"><span data-stu-id="4a715-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="4a715-155">O exemplo também demonstra como combinar atributos.</span><span class="sxs-lookup"><span data-stu-id="4a715-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="4a715-156">A anotação MaxLength afetará o banco de dados definindo o comprimento da propriedade como 10.</span><span class="sxs-lookup"><span data-stu-id="4a715-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Tabela de Blogs mostrando o comprimento máximo na coluna Bloggername](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="4a715-158">A anotação do lado do cliente do MVC e a anotação do lado do servidor do EF 4,1 respeitarão essa validação, novamente criando uma mensagem de erro de forma dinâmica: "o campo Bloggername deve ser uma cadeia de caracteres ou um tipo de matriz com um comprimento máximo de ' 10 '." Essa mensagem é um pouco longa.</span><span class="sxs-lookup"><span data-stu-id="4a715-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="4a715-159">Muitas anotações permitem que você especifique uma mensagem de erro com o atributo ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="4a715-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="4a715-160">Você também pode especificar ErrorMessage na anotação necessária.</span><span class="sxs-lookup"><span data-stu-id="4a715-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Criar página com mensagem de erro personalizada](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="4a715-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="4a715-162">NotMapped</span></span>

<span data-ttu-id="4a715-163">A Convenção do código First determina que todas as propriedades de um tipo de dados com suporte são representadas no Database.</span><span class="sxs-lookup"><span data-stu-id="4a715-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="4a715-164">Mas esse nem sempre é o caso em seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="4a715-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="4a715-165">Por exemplo, você pode ter uma propriedade na classe blog que cria um código com base nos campos title e Bloggername.</span><span class="sxs-lookup"><span data-stu-id="4a715-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="4a715-166">Essa propriedade pode ser criada dinamicamente e não precisa ser armazenada.</span><span class="sxs-lookup"><span data-stu-id="4a715-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="4a715-167">Você pode marcar todas as propriedades que não são mapeadas para o banco de dados com a anotação não mapeada, como essa propriedade BlogCode.</span><span class="sxs-lookup"><span data-stu-id="4a715-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="4a715-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="4a715-168">ComplexType</span></span>

<span data-ttu-id="4a715-169">Não é incomum descrever suas entidades de domínio em um conjunto de classes e, em seguida, camadar essas classes para descrever uma entidade completa.</span><span class="sxs-lookup"><span data-stu-id="4a715-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="4a715-170">Por exemplo, você pode adicionar uma classe chamada BlogDetails ao seu modelo.</span><span class="sxs-lookup"><span data-stu-id="4a715-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="4a715-171">Observe que BlogDetails não tem nenhum tipo de propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="4a715-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="4a715-172">No design controlado por domínio, BlogDetails é chamado de objeto value.</span><span class="sxs-lookup"><span data-stu-id="4a715-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="4a715-173">Entity Framework se refere a objetos de valor como tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="4a715-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="4a715-174">  Tipos complexos não podem ser rastreados por conta própria.</span><span class="sxs-lookup"><span data-stu-id="4a715-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="4a715-175">No entanto, como uma propriedade na classe blog, BlogDetails ele será acompanhado como parte de um objeto de blog.</span><span class="sxs-lookup"><span data-stu-id="4a715-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="4a715-176">Para que o Code First reconheça isso, você deve marcar a classe BlogDetails como um complexType.</span><span class="sxs-lookup"><span data-stu-id="4a715-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="4a715-177">Agora você pode adicionar uma propriedade na classe blog para representar o BlogDetails para esse blog.</span><span class="sxs-lookup"><span data-stu-id="4a715-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="4a715-178">No banco de dados, a tabela de blog conterá todas as propriedades do blog, incluindo as propriedades contidas em sua propriedade BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="4a715-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="4a715-179">Por padrão, cada um é precedido do nome do tipo complexo, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="4a715-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Tabela de blog com tipo complexo](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="4a715-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="4a715-181">ConcurrencyCheck</span></span>

<span data-ttu-id="4a715-182">A anotação ConcurrencyCheck permite sinalizar uma ou mais propriedades a serem usadas para verificação de simultaneidade no banco de dados quando um usuário edita ou exclui uma entidade.</span><span class="sxs-lookup"><span data-stu-id="4a715-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="4a715-183">Se você trabalha com o designer do EF, isso se alinha com a definição do ConcurrencyMode de uma propriedade como Fixed.</span><span class="sxs-lookup"><span data-stu-id="4a715-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="4a715-184">Vamos ver como o ConcurrencyCheck funciona adicionando-o à propriedade Bloggername.</span><span class="sxs-lookup"><span data-stu-id="4a715-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="4a715-185">Quando SaveChanges é chamado, devido à anotação ConcurrencyCheck no campo Bloggername, o valor original dessa propriedade será usado na atualização.</span><span class="sxs-lookup"><span data-stu-id="4a715-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="4a715-186">O comando tentará localizar a linha correta filtrando não apenas o valor da chave, mas também o valor original de Bloggername.</span><span class="sxs-lookup"><span data-stu-id="4a715-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="4a715-187">  Aqui estão as partes críticas do comando UPDATE enviadas ao banco de dados, onde você pode ver que o comando atualizará a linha que tem um PrimaryTrackingKey é 1 e um Bloggername de "Julie", que era o valor original quando o blog foi recuperado do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="4a715-188">Se alguém tiver alterado o nome do Blogger para esse blog enquanto isso, essa atualização falhará e você receberá um DbUpdateConcurrencyException que você precisará manipular.</span><span class="sxs-lookup"><span data-stu-id="4a715-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="4a715-189">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="4a715-189">TimeStamp</span></span>

<span data-ttu-id="4a715-190">É mais comum usar os campos de IsVersion ou timestamp para verificação de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="4a715-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="4a715-191">Mas, em vez de usar a anotação ConcurrencyCheck, você pode usar a anotação de carimbo de data/hora mais específica, contanto que o tipo da propriedade seja matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="4a715-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="4a715-192">O Code First tratará as propriedades de carimbo de data/hora da mesma forma que as propriedades ConcurrencyCheck, mas também garantirá que o campo de banco de dados que o Code First gera seja não anulável.</span><span class="sxs-lookup"><span data-stu-id="4a715-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="4a715-193">Você só pode ter uma propriedade Timestamp em uma determinada classe.</span><span class="sxs-lookup"><span data-stu-id="4a715-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="4a715-194">Adicionando a seguinte propriedade à classe blog:</span><span class="sxs-lookup"><span data-stu-id="4a715-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="4a715-195">resulta em um código primeiro criando uma coluna de carimbo de data/hora não anulável na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Tabela de Blogs com coluna de carimbo de data/hora](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="4a715-197">Tabela e coluna</span><span class="sxs-lookup"><span data-stu-id="4a715-197">Table and Column</span></span>

<span data-ttu-id="4a715-198">Se você estiver permitindo que Code First crie o banco de dados, convém alterar o nome das tabelas e das colunas que ele está criando.</span><span class="sxs-lookup"><span data-stu-id="4a715-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="4a715-199">Você também pode usar Code First com um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="4a715-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="4a715-200">Mas nem sempre é o caso de os nomes das classes e das propriedades em seu domínio corresponderem aos nomes das tabelas e colunas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="4a715-201">Minha classe é chamada blog e, por convenção, o Code First pressupõe que isso será mapeado para uma tabela chamada Blogs.</span><span class="sxs-lookup"><span data-stu-id="4a715-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="4a715-202">Se esse não for o caso, você poderá especificar o nome da tabela com o atributo Table.</span><span class="sxs-lookup"><span data-stu-id="4a715-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="4a715-203">Aqui, por exemplo, a anotação está especificando que o nome da tabela é InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="4a715-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="4a715-204">A anotação de coluna é mais adepta na especificação dos atributos de uma coluna mapeada.</span><span class="sxs-lookup"><span data-stu-id="4a715-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="4a715-205">Você pode estipular um nome, um tipo de dados ou até mesmo a ordem na qual uma coluna aparece na tabela.</span><span class="sxs-lookup"><span data-stu-id="4a715-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="4a715-206">Aqui está um exemplo do atributo Column.</span><span class="sxs-lookup"><span data-stu-id="4a715-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="4a715-207">Não confunda o atributo TypeName da coluna com a DataAnnotation DataType.</span><span class="sxs-lookup"><span data-stu-id="4a715-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="4a715-208">DataType é uma anotação usada para a interface do usuário e é ignorada por Code First.</span><span class="sxs-lookup"><span data-stu-id="4a715-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="4a715-209">Aqui está a tabela após sua regeneração.</span><span class="sxs-lookup"><span data-stu-id="4a715-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="4a715-210">O nome da tabela foi alterado para InternalBlogs e a coluna de descrição do tipo complexo agora é BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="4a715-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="4a715-211">Como o nome foi especificado na anotação, o Code First não usará a Convenção de iniciar o nome da coluna com o nome do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="4a715-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Tabela de Blogs e coluna renomeada](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="4a715-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="4a715-213">DatabaseGenerated</span></span>

<span data-ttu-id="4a715-214">Um importante recurso de banco de dados é a capacidade de ter propriedades computadas.</span><span class="sxs-lookup"><span data-stu-id="4a715-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="4a715-215">Se você estiver mapeando suas classes de Code First para tabelas que contêm colunas computadas, você não quer que Entity Framework tente atualizar essas colunas.</span><span class="sxs-lookup"><span data-stu-id="4a715-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="4a715-216">Mas você deseja que o EF retorne esses valores do banco de dados depois de inserir ou atualizar os dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="4a715-217">Você pode usar a anotação DatabaseGenerated para sinalizar essas propriedades em sua classe junto com a enumeração computada.</span><span class="sxs-lookup"><span data-stu-id="4a715-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="4a715-218">Outros enums são None e Identity.</span><span class="sxs-lookup"><span data-stu-id="4a715-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="4a715-219">Você pode usar o banco de dados gerado em colunas de byte ou carimbo de data/hora em que o Code First está gerando o banco de dados, caso contrário, você deve usá-lo apenas ao apontar para bancos de dados existentes, pois o Code First não será capaz de determinar a fórmula para a coluna computada.</span><span class="sxs-lookup"><span data-stu-id="4a715-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="4a715-220">Você leu acima que, por padrão, uma propriedade de chave que é um inteiro se tornará uma chave de identidade no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="4a715-221">Isso seria o mesmo que definir DatabaseGenerated como DatabaseGeneratedOption. Identity.</span><span class="sxs-lookup"><span data-stu-id="4a715-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="4a715-222">Se você não quiser que ela seja uma chave de identidade, poderá definir o valor como DatabaseGeneratedOption. None.</span><span class="sxs-lookup"><span data-stu-id="4a715-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="4a715-223">Índice</span><span class="sxs-lookup"><span data-stu-id="4a715-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="4a715-224">**Somente em versões do EF 6.1** – o atributo de índice foi introduzido no Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="4a715-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="4a715-225">Se você estiver usando uma versão anterior, as informações nesta seção não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="4a715-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="4a715-226">Você pode criar um índice em uma ou mais colunas usando o **indexattribute**.</span><span class="sxs-lookup"><span data-stu-id="4a715-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="4a715-227">Adicionar o atributo a uma ou mais propriedades fará com que o EF crie o índice correspondente no banco de dados quando ele criar o banco de dados ou Scaffold as chamadas **CreateIndex** correspondentes se você estiver usando migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="4a715-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="4a715-228">Por exemplo, o código a seguir resultará em um índice sendo criado na coluna **classificação** da tabela de **postagens** no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="4a715-229">Por padrão, o índice será nomeado **IX\_&lt;nome da propriedade&gt;** (IX\_classificação no exemplo acima).</span><span class="sxs-lookup"><span data-stu-id="4a715-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="4a715-230">No entanto, você também pode especificar um nome para o índice.</span><span class="sxs-lookup"><span data-stu-id="4a715-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="4a715-231">O exemplo a seguir especifica que o índice deve ser nomeado **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="4a715-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="4a715-232">Por padrão, os índices são não exclusivos, mas você pode usar o parâmetro **IsUnique** nomeado para especificar que um índice deve ser exclusivo.</span><span class="sxs-lookup"><span data-stu-id="4a715-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="4a715-233">O exemplo a seguir apresenta um índice exclusivo no nome de logon de um **usuário**.</span><span class="sxs-lookup"><span data-stu-id="4a715-233">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="4a715-234">Índices de várias colunas</span><span class="sxs-lookup"><span data-stu-id="4a715-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="4a715-235">Os índices que abrangem várias colunas são especificados usando o mesmo nome em várias anotações de índice para uma determinada tabela.</span><span class="sxs-lookup"><span data-stu-id="4a715-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="4a715-236">Ao criar índices de várias colunas, você precisa especificar uma ordem para as colunas no índice.</span><span class="sxs-lookup"><span data-stu-id="4a715-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="4a715-237">Por exemplo, o código a seguir cria um índice de várias colunas em **classificação** e **blogid** chamado **IX\_BlogIdAndRating**.</span><span class="sxs-lookup"><span data-stu-id="4a715-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="4a715-238">O **blogid** é a primeira coluna no índice e a **classificação** é a segunda.</span><span class="sxs-lookup"><span data-stu-id="4a715-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="4a715-239">Atributos de relação: inversoproperty e ForeignKey</span><span class="sxs-lookup"><span data-stu-id="4a715-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="4a715-240">Esta página fornece informações sobre como configurar relações em seu modelo de Code First usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="4a715-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="4a715-241">Para obter informações gerais sobre relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="4a715-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="4a715-242">A Convenção do código First cuidará das relações mais comuns em seu modelo, mas há alguns casos em que precisa de ajuda.</span><span class="sxs-lookup"><span data-stu-id="4a715-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="4a715-243">A alteração do nome da propriedade de chave na classe de blog criou um problema com sua relação com a postagem.</span><span class="sxs-lookup"><span data-stu-id="4a715-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="4a715-244">Ao gerar o banco de dados, o Code First vê a propriedade blogid na classe post e a reconhece, pela Convenção de que ela corresponde a um nome de classe, mais "ID", como uma chave estrangeira para a classe de blog.</span><span class="sxs-lookup"><span data-stu-id="4a715-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="4a715-245">Mas não há nenhuma propriedade blogid na classe blog.</span><span class="sxs-lookup"><span data-stu-id="4a715-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="4a715-246">A solução para isso é criar uma propriedade de navegação na postagem e usar a anotação externa para ajudar o Code a entender primeiro como criar a relação entre as duas classes — usando a propriedade post. blogid — e também como especificar restrições no banco.</span><span class="sxs-lookup"><span data-stu-id="4a715-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="4a715-247">A restrição no banco de dados mostra uma relação entre InternalBlogs. PrimaryTrackingKey e postes. blogid.</span><span class="sxs-lookup"><span data-stu-id="4a715-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![relação entre InternalBlogs. PrimaryTrackingKey e postes. blogid](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="4a715-249">O inversoproperty é usado quando você tem várias relações entre classes.</span><span class="sxs-lookup"><span data-stu-id="4a715-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="4a715-250">Na classe post, convém acompanhar quem escreveu uma postagem de blog, bem como quem a editou.</span><span class="sxs-lookup"><span data-stu-id="4a715-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="4a715-251">Aqui estão duas novas propriedades de navegação para a classe post.</span><span class="sxs-lookup"><span data-stu-id="4a715-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="4a715-252">Você também precisará adicionar a classe Person referenciada por essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="4a715-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="4a715-253">A classe Person tem propriedades de navegação de volta para a postagem, uma para todas as postagens escritas pela pessoa e uma para todas as postagens atualizadas por essa pessoa.</span><span class="sxs-lookup"><span data-stu-id="4a715-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="4a715-254">O Code First não é capaz de corresponder as propriedades nas duas classes por conta própria.</span><span class="sxs-lookup"><span data-stu-id="4a715-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="4a715-255">A tabela de banco de dados para postagens deve ter uma chave estrangeira para a pessoa CreatedBy e outra para a pessoa UpdatedBy, mas o Code First criará quatro propriedades de chave estrangeira: Person\_ID, Person\_Id1, CreatedBy\_ID e UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="4a715-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Lança a tabela com chaves estrangeiras extras](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="4a715-257">Para corrigir esses problemas, você pode usar a anotação inversoproperty para especificar o alinhamento das propriedades.</span><span class="sxs-lookup"><span data-stu-id="4a715-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="4a715-258">Como a propriedade PostsWritten em Person sabe que isso se refere ao tipo post, ela criará a relação para post. CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="4a715-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="4a715-259">Da mesma forma, PostsUpdated será conectado a post. UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="4a715-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="4a715-260">E o Code First não criará as chaves estrangeiras extras.</span><span class="sxs-lookup"><span data-stu-id="4a715-260">And code first will not create the extra foreign keys.</span></span>

![Lança a tabela sem chaves estrangeiras extras](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="4a715-262">Resumo</span><span class="sxs-lookup"><span data-stu-id="4a715-262">Summary</span></span>

<span data-ttu-id="4a715-263">Annotations não só permitem que você descreva a validação do cliente e do servidor nas classes Code First, mas elas também permitem aprimorar e até mesmo corrigir as suposições que o Code First fará sobre suas classes com base em suas convenções.</span><span class="sxs-lookup"><span data-stu-id="4a715-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="4a715-264">Com as anotações de dados, você não só pode impulsionar a geração do esquema de banco, mas também pode mapear suas primeiras classes code para um banco de dados pré-existente.</span><span class="sxs-lookup"><span data-stu-id="4a715-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="4a715-265">Embora eles sejam muito flexíveis, lembre-se de que Annotations fornecem apenas as alterações de configuração mais comumente necessárias que você pode fazer em suas classes de código primeiro.</span><span class="sxs-lookup"><span data-stu-id="4a715-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="4a715-266">Para configurar suas classes para alguns dos casos de borda, você deve procurar o mecanismo de configuração alternativo, a API Fluent do Code First.</span><span class="sxs-lookup"><span data-stu-id="4a715-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
