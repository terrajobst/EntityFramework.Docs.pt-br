---
title: Primeira Data Annotations - EF6 de código
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120101"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="61c47-102">Anotações de dados Code First</span><span class="sxs-lookup"><span data-stu-id="61c47-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="61c47-103">**EF4.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="61c47-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="61c47-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="61c47-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="61c47-105">O conteúdo desta página é adaptado do e do artigo escrito originalmente de Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="61c47-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="61c47-106">Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo que o EF depende para executar a consulta, altere de acompanhamento e a atualização de funções.</span><span class="sxs-lookup"><span data-stu-id="61c47-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="61c47-107">Primeiro, o código aproveita um padrão de programação conhecido como convenção em detrimento da configuração.</span><span class="sxs-lookup"><span data-stu-id="61c47-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="61c47-108">Isso significa que o código primeiro pressupor que suas classes seguem as convenções que usa o EF.</span><span class="sxs-lookup"><span data-stu-id="61c47-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="61c47-109">Nesse caso, o EF será capaz de trabalhar em detalhes em que ele precisa para fazer seu trabalho.</span><span class="sxs-lookup"><span data-stu-id="61c47-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="61c47-110">No entanto, se suas classes não seguir essas convenções, você tem a capacidade de adicionar as configurações para suas classes para fornecer o EF com as informações necessárias.</span><span class="sxs-lookup"><span data-stu-id="61c47-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="61c47-111">Código primeiro oferece duas maneiras de adicionar essas configurações para suas classes.</span><span class="sxs-lookup"><span data-stu-id="61c47-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="61c47-112">Uma é usando atributos simples chamados DataAnnotations e a outra é usando o código é a API Fluent, que fornece uma maneira de descrever as configurações de modo imperativo, no código.</span><span class="sxs-lookup"><span data-stu-id="61c47-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="61c47-113">Este artigo se concentra no uso de DataAnnotations (no namespace DataAnnotations) para configurar classes – realçando as configurações mais necessárias.</span><span class="sxs-lookup"><span data-stu-id="61c47-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="61c47-114">DataAnnotations também são compreendidas por um número de aplicativos .NET, como o ASP.NET MVC, que permite que esses aplicativos para aproveitar as mesmas anotações para validações do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="61c47-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="61c47-115">O modelo</span><span class="sxs-lookup"><span data-stu-id="61c47-115">The model</span></span>

<span data-ttu-id="61c47-116">Vou demonstrar DataAnnotations primeiro com um par simple de classes de código: Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="61c47-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="61c47-117">Como estão, as classes de postagem de Blog convenientemente seguem a convenção de código primeiro e necessários sem ajustes para ajudar o EF trabalhar com eles.</span><span class="sxs-lookup"><span data-stu-id="61c47-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="61c47-118">Mas você também pode usar as anotações para fornecer mais informações para o EF sobre as classes e eles são mapeados para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="61c47-119">Chave</span><span class="sxs-lookup"><span data-stu-id="61c47-119">Key</span></span>

<span data-ttu-id="61c47-120">Entity Framework se baseia em cada entidade que tem um valor de chave que ele utiliza para entidades de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="61c47-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="61c47-121">Um das convenções de código primeiro depende é como ele implica a qual propriedade é a chave em cada uma das classes código primeiro.</span><span class="sxs-lookup"><span data-stu-id="61c47-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="61c47-122">Essa convenção é procurar uma propriedade chamada "Id" ou que combina o nome de classe e a "Id", como "BlogId".</span><span class="sxs-lookup"><span data-stu-id="61c47-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="61c47-123">A propriedade será mapeada para uma coluna de chave primária no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="61c47-124">As classes no Blog e Post seguem Esta convenção.</span><span class="sxs-lookup"><span data-stu-id="61c47-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="61c47-125">Mas e se eles não?</span><span class="sxs-lookup"><span data-stu-id="61c47-125">But what if they didn’t?</span></span> <span data-ttu-id="61c47-126">E se o Blog usou o nome *PrimaryTrackingKey* em vez disso, ou até mesmo *foo*?</span><span class="sxs-lookup"><span data-stu-id="61c47-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="61c47-127">Se o código primeiro não encontrar uma propriedade que corresponde a essa convenção, ele lançará uma exceção devido ao requisito do Entity Framework que você deve ter uma propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="61c47-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="61c47-128">Você pode usar a anotação de chave para especificar qual propriedade deve ser usado como EntityKey.</span><span class="sxs-lookup"><span data-stu-id="61c47-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="61c47-129">Se você estiver usando o código é o recurso de geração de banco de dados, a tabela Blog terá uma coluna de chave primária denominada PrimaryTrackingKey que também é definido como identidade, por padrão.</span><span class="sxs-lookup"><span data-stu-id="61c47-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="61c47-131">Chaves compostas</span><span class="sxs-lookup"><span data-stu-id="61c47-131">Composite keys</span></span>

<span data-ttu-id="61c47-132">Entity Framework oferece suporte a chaves compostas - chaves primárias que consistem em mais de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="61c47-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="61c47-133">Por exemplo, seu poderia ter uma classe de Passport cuja chave primária é uma combinação de PassportNumber e IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="61c47-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="61c47-134">Se você tentar e usar a classe acima em seu modelo EF você obteria um informando InvalidOperationExceptions;</span><span class="sxs-lookup"><span data-stu-id="61c47-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="61c47-135">*Não é possível determinar a composição primário ordenação de chave para o tipo 'Passport'. Use o ColumnAttribute ou o método HasKey para especificar uma ordem para chaves primárias compostas.*</span><span class="sxs-lookup"><span data-stu-id="61c47-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="61c47-136">Quando você tiver chaves compostas, o Entity Framework exige que você pode definir uma ordem das propriedades de chave.</span><span class="sxs-lookup"><span data-stu-id="61c47-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="61c47-137">Você pode fazer isso usando a anotação de coluna para especificar uma ordem.</span><span class="sxs-lookup"><span data-stu-id="61c47-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="61c47-138">O valor de ordem é relativo (em vez de índice com base) para que todos os valores possam ser usados.</span><span class="sxs-lookup"><span data-stu-id="61c47-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="61c47-139">Por exemplo, 100 e 200 seria aceitável no lugar de 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="61c47-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="61c47-140">Se você tiver entidades com chaves estrangeiras compostas, em seguida, você deve especificar a mesma coluna de classificação que você usou para as propriedades de chave primárias correspondentes.</span><span class="sxs-lookup"><span data-stu-id="61c47-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="61c47-141">Somente a ordenação relativa dentro das propriedades de chave estrangeiras deve ser o mesmo, os valores exatos atribuídos a **ordem** não precisa corresponder.</span><span class="sxs-lookup"><span data-stu-id="61c47-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="61c47-142">Por exemplo, a seguinte classe, 3 e 4 poderiam ser usados no lugar de 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="61c47-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="61c47-143">Necessária</span><span class="sxs-lookup"><span data-stu-id="61c47-143">Required</span></span>

<span data-ttu-id="61c47-144">A anotação necessária informa ao EF que uma propriedade específica é necessária.</span><span class="sxs-lookup"><span data-stu-id="61c47-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="61c47-145">Adicionando necessárias para a propriedade Title forçará o EF (e MVC) para garantir que a propriedade tem dados nele.</span><span class="sxs-lookup"><span data-stu-id="61c47-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="61c47-146">Sem nenhum adicional nenhum código ou marcação alterações no aplicativo, um aplicativo MVC executará validação do lado do cliente, até mesmo dinamicamente a criação de uma mensagem usando os nomes de propriedade e a anotação.</span><span class="sxs-lookup"><span data-stu-id="61c47-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="61c47-148">O atributo Required também afetará o banco de dados gerado, tornando a propriedade mapeada não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="61c47-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="61c47-149">Observe que o campo de título foi alterado para "not null".</span><span class="sxs-lookup"><span data-stu-id="61c47-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="61c47-150">Em alguns casos, pode não ser possível para a coluna no banco de dados a ser não anuláveis, mesmo que a propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="61c47-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="61c47-151">Por exemplo, quando usar uma data de estratégia de herança TPH para vários tipos é armazenado em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="61c47-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="61c47-152">Se um tipo derivado inclui uma propriedade necessária que a coluna não pode se tornar não anulável, pois nem todos os tipos na hierarquia terá essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="61c47-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="61c47-154">MaxLength e MinLength</span><span class="sxs-lookup"><span data-stu-id="61c47-154">MaxLength and MinLength</span></span>

<span data-ttu-id="61c47-155">Os atributos MaxLength e MinLength permitem que você especifique validações de propriedade adicional, assim como você fez com necessários.</span><span class="sxs-lookup"><span data-stu-id="61c47-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="61c47-156">Aqui está o BloggerName com requisitos de comprimento.</span><span class="sxs-lookup"><span data-stu-id="61c47-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="61c47-157">O exemplo também demonstra como combinar atributos.</span><span class="sxs-lookup"><span data-stu-id="61c47-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="61c47-158">A anotação de MaxLength terá impacto sobre o banco de dados, definindo o comprimento da propriedade como 10.</span><span class="sxs-lookup"><span data-stu-id="61c47-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="61c47-160">Anotação do lado do cliente MVC e anotação do lado do servidor de EF 4.1 ambos respeitará essa validação novamente dinamicamente a criação de uma mensagem de erro: "o campo BloggerName deve ser um tipo de cadeia de caracteres ou matriz com um comprimento máximo de '10'". Essa mensagem é um pouco longa.</span><span class="sxs-lookup"><span data-stu-id="61c47-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="61c47-161">Muitas anotações permitem que você especifique uma mensagem de erro com o atributo de ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="61c47-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="61c47-162">Você também pode especificar ErrorMessage na anotação necessária.</span><span class="sxs-lookup"><span data-stu-id="61c47-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="61c47-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="61c47-164">NotMapped</span></span>

<span data-ttu-id="61c47-165">Convenção de primeiro código dita que cada propriedade que é de um tipo de dados com suporte é representada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="61c47-166">Mas isso nem sempre é o caso em seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="61c47-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="61c47-167">Por exemplo, você pode ter uma propriedade na classe de Blog que cria um código com base nos campos de título e BloggerName.</span><span class="sxs-lookup"><span data-stu-id="61c47-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="61c47-168">Essa propriedade pode ser criada dinamicamente e não precisam ser armazenados.</span><span class="sxs-lookup"><span data-stu-id="61c47-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="61c47-169">Você pode marcar qualquer propriedade que não é mapeados para o banco de dados com a anotação NotMapped como essa propriedade BlogCode.</span><span class="sxs-lookup"><span data-stu-id="61c47-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="61c47-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="61c47-170">ComplexType</span></span>

<span data-ttu-id="61c47-171">Não é incomum para descrever as entidades de domínio em um conjunto de classes e, em seguida, essas classes para descrever uma entidade completa de camada.</span><span class="sxs-lookup"><span data-stu-id="61c47-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="61c47-172">Por exemplo, você pode adicionar uma classe chamada BlogDetails ao seu modelo.</span><span class="sxs-lookup"><span data-stu-id="61c47-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="61c47-173">Observe que BlogDetails não tem qualquer tipo de propriedade de chave.</span><span class="sxs-lookup"><span data-stu-id="61c47-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="61c47-174">No design controlado por domínio, BlogDetails é conhecido como um objeto de valor.</span><span class="sxs-lookup"><span data-stu-id="61c47-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="61c47-175">Entity Framework se refere a objetos de valor como tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="61c47-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="61c47-176">Tipos complexos não podem ser controlados por conta própria.</span><span class="sxs-lookup"><span data-stu-id="61c47-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="61c47-177">No entanto como uma propriedade na classe de Blog, BlogDetails serão rastreada como parte de um objeto de Blog.</span><span class="sxs-lookup"><span data-stu-id="61c47-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="61c47-178">Em ordem para o code first para reconhecer isso, você deve marcar a classe BlogDetails como um ComplexType.</span><span class="sxs-lookup"><span data-stu-id="61c47-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="61c47-179">Agora você pode adicionar uma propriedade na classe para representar o BlogDetails para esse blog Blog.</span><span class="sxs-lookup"><span data-stu-id="61c47-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="61c47-180">No banco de dados, a tabela Blog conterá todas as propriedades do blog, incluindo as propriedades contidas na sua propriedade BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="61c47-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="61c47-181">Por padrão, cada uma delas é precedida pelo nome do tipo complexo, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="61c47-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="61c47-183">Outra observação interessante é que, embora a propriedade DateCreated foi definida como uma data e hora não anuláveis na classe, o campo de banco de dados relevante é anulável.</span><span class="sxs-lookup"><span data-stu-id="61c47-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="61c47-184">Você deve usar a anotação necessária se você quiser afetar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="61c47-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="61c47-185">ConcurrencyCheck</span></span>

<span data-ttu-id="61c47-186">A anotação ConcurrencyCheck permite sinalizar uma ou mais propriedades a serem usados para verificação no banco de dados quando um usuário edita ou exclui uma entidade de concorrência.</span><span class="sxs-lookup"><span data-stu-id="61c47-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="61c47-187">Se você já trabalhou com o EF Designer, isso se alinha com a configuração ConcurrencyMode da propriedade como Fixed.</span><span class="sxs-lookup"><span data-stu-id="61c47-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="61c47-188">Vamos ver como ConcurrencyCheck funciona ao adicioná-lo à propriedade BloggerName.</span><span class="sxs-lookup"><span data-stu-id="61c47-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="61c47-189">Quando SaveChanges for chamado, devido à anotação ConcurrencyCheck no campo BloggerName, o valor original da propriedade será usado na atualização.</span><span class="sxs-lookup"><span data-stu-id="61c47-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="61c47-190">O comando tentará localizar a linha correta por meio da filtragem não apenas no valor da chave, mas também no valor original de BloggerName.</span><span class="sxs-lookup"><span data-stu-id="61c47-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="61c47-191">Aqui estão as partes críticas do comando UPDATE enviados para o banco de dados, onde você pode ver o comando atualizará a linha que tem um PrimaryTrackingKey é 1 e um BloggerName de "Julie", que era o valor original quando esse blog foi recuperado do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="61c47-192">Se alguém alterou o nome do blogger para esse blog enquanto isso, essa atualização falhará e você obterá um DbUpdateConcurrencyException que você precisará para administrar.</span><span class="sxs-lookup"><span data-stu-id="61c47-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="61c47-193">Carimbo de hora</span><span class="sxs-lookup"><span data-stu-id="61c47-193">TimeStamp</span></span>

<span data-ttu-id="61c47-194">É mais comum usar campos de rowversion ou carimbo de hora para verificação de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="61c47-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="61c47-195">Mas, em vez de usar a anotação ConcurrencyCheck, você pode usar a anotação de carimbo de hora mais específica como o tipo da propriedade é a matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="61c47-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="61c47-196">Código primeiro tratarão propriedades de carimbo de hora da mesma como propriedades ConcurrencyCheck, mas ele garantirá que o campo de banco de dados que o código gera primeiro é não anulável.</span><span class="sxs-lookup"><span data-stu-id="61c47-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="61c47-197">Você só pode ter uma propriedade de carimbo de hora em uma determinada classe.</span><span class="sxs-lookup"><span data-stu-id="61c47-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="61c47-198">Adicionando a seguinte propriedade à classe de Blog:</span><span class="sxs-lookup"><span data-stu-id="61c47-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="61c47-199">resultados no código criando primeiro uma coluna de carimbo de hora não anulável na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="61c47-201">Tabela e coluna</span><span class="sxs-lookup"><span data-stu-id="61c47-201">Table and Column</span></span>

<span data-ttu-id="61c47-202">Se você está permitindo que o Code First cria o banco de dados, você talvez queira alterar o nome das tabelas e colunas que ele está criando.</span><span class="sxs-lookup"><span data-stu-id="61c47-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="61c47-203">Você também pode usar Code First com um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="61c47-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="61c47-204">Mas nem sempre é o caso que os nomes das classes e propriedades em seu domínio de corresponderem aos nomes das tabelas e colunas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="61c47-205">Minha classe é chamado Blog e por convenção, código primeiro supõe que isso será mapeado para uma tabela chamada Blogs.</span><span class="sxs-lookup"><span data-stu-id="61c47-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="61c47-206">Se não for o caso, você pode especificar o nome da tabela com o atributo de tabela.</span><span class="sxs-lookup"><span data-stu-id="61c47-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="61c47-207">Aqui, por exemplo, a anotação está especificando que o nome da tabela é InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="61c47-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="61c47-208">A anotação de coluna é um mais adeptos em especificando os atributos de uma coluna mapeada.</span><span class="sxs-lookup"><span data-stu-id="61c47-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="61c47-209">Você pode estipular um nome, tipo de dados ou até mesmo a ordem na qual uma coluna aparece na tabela.</span><span class="sxs-lookup"><span data-stu-id="61c47-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="61c47-210">Aqui está um exemplo de atributo de coluna.</span><span class="sxs-lookup"><span data-stu-id="61c47-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="61c47-211">Não confunda o atributo de TypeName da coluna com o DataType DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="61c47-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="61c47-212">Tipo de dados é uma anotação, usada para a interface do usuário e é ignorado pelo Code First.</span><span class="sxs-lookup"><span data-stu-id="61c47-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="61c47-213">Aqui está a tabela depois que ele é foi regenerado.</span><span class="sxs-lookup"><span data-stu-id="61c47-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="61c47-214">O nome da tabela foi alterado para InternalBlogs e a coluna de descrição de tipo complexo é agora BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="61c47-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="61c47-215">Como o nome foi especificado na anotação, código pela primeira vez não usará a convenção de iniciar o nome da coluna com o nome do tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="61c47-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="61c47-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="61c47-217">DatabaseGenerated</span></span>

<span data-ttu-id="61c47-218">Recursos de um banco de dados importante é a capacidade de ter propriedades computadas.</span><span class="sxs-lookup"><span data-stu-id="61c47-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="61c47-219">Se estiver mapeando suas classes de Code First para tabelas que contêm as colunas computadas, você não deseja o Entity Framework ao tentar atualizar as colunas.</span><span class="sxs-lookup"><span data-stu-id="61c47-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="61c47-220">Mas se você desejar EF para retornar esses valores do banco de dados depois de inserido ou dados atualizados.</span><span class="sxs-lookup"><span data-stu-id="61c47-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="61c47-221">Você pode usar a anotação DatabaseGenerated para sinalizar essas propriedades em sua classe, juntamente com o enum computado.</span><span class="sxs-lookup"><span data-stu-id="61c47-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="61c47-222">Outras enumerações são None e identidade.</span><span class="sxs-lookup"><span data-stu-id="61c47-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="61c47-223">Você pode usar o banco de dados gerado em colunas timestamp ou byte ao código pela primeira vez é gerar o banco de dados, caso contrário, você deve usar isso apenas quando apontando para bancos de dados existentes, porque o código primeiro não será capaz de determinar a fórmula para a coluna computada.</span><span class="sxs-lookup"><span data-stu-id="61c47-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="61c47-224">Ler acima, por padrão, uma propriedade de chave que é um número inteiro se torna uma chave de identidade no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="61c47-225">Que seria o mesmo que definir DatabaseGenerated como DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="61c47-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="61c47-226">Se você não quiser que ele seja uma chave de identidade, você pode definir o valor para DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="61c47-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="61c47-227">Índice</span><span class="sxs-lookup"><span data-stu-id="61c47-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="61c47-228">**EF6.1 em diante, somente** -atributo o índice foi introduzido no Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="61c47-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="61c47-229">Se você estiver usando uma versão anterior, as informações nesta seção não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="61c47-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="61c47-230">Você pode criar um índice em uma ou mais colunas usando o **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="61c47-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="61c47-231">Adicionando o atributo a uma ou mais propriedades será fazer com que o EF criar o índice correspondente no banco de dados quando ele cria o banco de dados ou criar o scaffolding correspondente **CreateIndex** chama se você estiver usando as migrações Code First.</span><span class="sxs-lookup"><span data-stu-id="61c47-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="61c47-232">Por exemplo, o código a seguir resultará em um índice que está sendo criado o **Rating** coluna da **postagens** tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="61c47-233">Por padrão, o índice será nomeado **IX\_&lt;nome da propriedade&gt;**  (IX\_de classificação no exemplo acima).</span><span class="sxs-lookup"><span data-stu-id="61c47-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="61c47-234">Você também pode especificar um nome para o índice no entanto.</span><span class="sxs-lookup"><span data-stu-id="61c47-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="61c47-235">O exemplo a seguir especifica que o índice deve ser nomeado **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="61c47-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="61c47-236">Por padrão, os índices não forem exclusivas, mas você pode usar o **IsUnique** chamado parâmetro para especificar que um índice deve ser exclusivo.</span><span class="sxs-lookup"><span data-stu-id="61c47-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="61c47-237">O exemplo a seguir apresenta um índice exclusivo em uma **usuário**do nome de logon.</span><span class="sxs-lookup"><span data-stu-id="61c47-237">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="61c47-238">Índices de várias colunas</span><span class="sxs-lookup"><span data-stu-id="61c47-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="61c47-239">Os índices que abrangem várias colunas são especificados usando o mesmo nome em várias anotações de índice para uma determinada tabela.</span><span class="sxs-lookup"><span data-stu-id="61c47-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="61c47-240">Ao criar índices com múltiplas colunas, você precisa especificar uma ordem para as colunas no índice.</span><span class="sxs-lookup"><span data-stu-id="61c47-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="61c47-241">Por exemplo, o código a seguir cria um índice de várias colunas em **Rating** e **BlogId** chamado **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="61c47-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="61c47-242">**BlogId** é a primeira coluna no índice e **classificação** é o segundo.</span><span class="sxs-lookup"><span data-stu-id="61c47-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="61c47-243">Atributos de relação: InverseProperty e ForeignKey</span><span class="sxs-lookup"><span data-stu-id="61c47-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="61c47-244">Esta página fornece informações sobre como configurar as relações em seu modelo Code First usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="61c47-245">Para obter informações gerais sobre as relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="61c47-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="61c47-246">Convenção de primeiro código se encarregará das relações mais comuns em seu modelo, mas há alguns casos em que ele precisa de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="61c47-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="61c47-247">Alterando o nome da propriedade chave na classe de Blog que criou um problema com sua relação com o Post.</span><span class="sxs-lookup"><span data-stu-id="61c47-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="61c47-248">Ao gerar o banco de dados, o código primeiro vê a propriedade BlogId na classe Post e reconheça, pela convenção de que ele corresponda a um nome de classe além da "Id", como uma chave estrangeira para a classe de Blog.</span><span class="sxs-lookup"><span data-stu-id="61c47-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="61c47-249">Mas não há nenhuma propriedade BlogId na classe blog.</span><span class="sxs-lookup"><span data-stu-id="61c47-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="61c47-250">A solução para isso é criar uma propriedade de navegação na postagem e usar o DataAnnotation externa para ajudar o código primeiro entender como criar a relação entre as duas classes — usando a propriedade Post.BlogId — bem como especificar as restrições no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61c47-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="61c47-251">A restrição no banco de dados mostra uma relação entre InternalBlogs.PrimaryTrackingKey e Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="61c47-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="61c47-253">O InverseProperty é usado quando você tiver várias relações entre classes.</span><span class="sxs-lookup"><span data-stu-id="61c47-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="61c47-254">Na classe Post, você talvez queira manter o controle de quem o escreveu uma postagem de blog, bem como quem editou.</span><span class="sxs-lookup"><span data-stu-id="61c47-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="61c47-255">Aqui estão duas novas propriedades de navegação para a classe de postagem.</span><span class="sxs-lookup"><span data-stu-id="61c47-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="61c47-256">Você também precisará adicionar a classe Person referenciada por essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="61c47-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="61c47-257">A classe Person tem as propriedades de navegação de volta para a postagem, uma para todas as postagens escritas por pessoa e outro para todas as postagens atualizadas por aquela pessoa.</span><span class="sxs-lookup"><span data-stu-id="61c47-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="61c47-258">Code first não é capaz de corresponder as propriedades nas duas classes por conta própria.</span><span class="sxs-lookup"><span data-stu-id="61c47-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="61c47-259">A tabela de banco de dados para postagens deve ter uma chave estrangeira para a pessoa CreatedBy e outro para a pessoa UpdatedBy mas o código primeiro criar propriedades de chave estrangeira quatro do será: pessoa\_Id, a pessoa\_Id1, CreatedBy\_Id e UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="61c47-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="61c47-261">Para corrigir esses problemas, você pode usar a anotação InverseProperty para especificar o alinhamento das propriedades.</span><span class="sxs-lookup"><span data-stu-id="61c47-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="61c47-262">Como a propriedade PostsWritten pessoalmente sabe que isso se refere ao tipo de postagem, ele criará a relação para Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="61c47-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="61c47-263">Da mesma forma, PostsUpdated será conectado ao Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="61c47-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="61c47-264">E o código primeiro não criará as chaves estrangeiras extra.</span><span class="sxs-lookup"><span data-stu-id="61c47-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="61c47-266">Resumo</span><span class="sxs-lookup"><span data-stu-id="61c47-266">Summary</span></span>

<span data-ttu-id="61c47-267">DataAnnotations não só permitem que você descreva a validação do lado do cliente e servidor nas suas classes de primeiro código, mas eles também permitem que você melhore e até mesmo corrigir as suposições que código fará primeiro sobre suas classes com base nas suas convenções.</span><span class="sxs-lookup"><span data-stu-id="61c47-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="61c47-268">Com DataAnnotations é possível não apenas conduzir geração de esquema de banco de dados, mas você também pode mapear suas classes de primeiro código para um banco de dados já existente.</span><span class="sxs-lookup"><span data-stu-id="61c47-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="61c47-269">Enquanto eles são muito flexíveis, tenha em mente que DataAnnotations fornecem apenas mais comumente necessárias alterações de configuração, que você pode fazer em suas classes de código primeiro.</span><span class="sxs-lookup"><span data-stu-id="61c47-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="61c47-270">Para configurar suas classes de alguns dos casos de borda, você deve procurar para o mecanismo de configuração alternativa, a API Fluent do Code First.</span><span class="sxs-lookup"><span data-stu-id="61c47-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
