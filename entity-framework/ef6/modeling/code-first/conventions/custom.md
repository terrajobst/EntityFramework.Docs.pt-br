---
title: Convenções de Code First personalizadas-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419223"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="3e290-102">Convenções de Code First personalizadas</span><span class="sxs-lookup"><span data-stu-id="3e290-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="3e290-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3e290-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="3e290-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="3e290-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="3e290-105">Ao usar Code First seu modelo é calculado de suas classes usando um conjunto de convenções.</span><span class="sxs-lookup"><span data-stu-id="3e290-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="3e290-106">As [convenções de Code First](~/ef6/modeling/code-first/conventions/built-in.md) padrão determinam coisas como a propriedade que se torna a chave primária de uma entidade, o nome da tabela à qual uma entidade mapeia e em que precisão e escala uma coluna decimal tem por padrão.</span><span class="sxs-lookup"><span data-stu-id="3e290-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="3e290-107">Às vezes, essas convenções padrão não são ideais para seu modelo e você precisa contorná-las Configurando muitas entidades individuais usando as anotações de dados ou a API fluente.</span><span class="sxs-lookup"><span data-stu-id="3e290-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="3e290-108">As convenções de Code First personalizadas permitem que você defina suas próprias convenções que fornecem padrões de configuração para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="3e290-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="3e290-109">Neste tutorial, exploraremos os diferentes tipos de convenções personalizadas e como criar cada uma delas.</span><span class="sxs-lookup"><span data-stu-id="3e290-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="3e290-110">Convenções baseadas em modelo</span><span class="sxs-lookup"><span data-stu-id="3e290-110">Model-Based Conventions</span></span>

<span data-ttu-id="3e290-111">Esta página aborda a API DbModelBuilder para convenções personalizadas.</span><span class="sxs-lookup"><span data-stu-id="3e290-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="3e290-112">Essa API deve ser suficiente para criar a maioria das convenções personalizadas.</span><span class="sxs-lookup"><span data-stu-id="3e290-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="3e290-113">No entanto, também há a capacidade de criar convenções baseadas em modelo – convenções que manipulam o modelo final após sua criação – para lidar com cenários avançados.</span><span class="sxs-lookup"><span data-stu-id="3e290-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="3e290-114">Para obter mais informações, consulte [convenções baseadas em modelos](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="3e290-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="3e290-115">Nosso modelo</span><span class="sxs-lookup"><span data-stu-id="3e290-115">Our Model</span></span>

<span data-ttu-id="3e290-116">Vamos começar definindo um modelo simples que podemos usar com nossas convenções.</span><span class="sxs-lookup"><span data-stu-id="3e290-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="3e290-117">Adicione as seguintes classes ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3e290-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="3e290-118">Apresentando convenções personalizadas</span><span class="sxs-lookup"><span data-stu-id="3e290-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="3e290-119">Vamos escrever uma convenção que configure qualquer propriedade chamada Key para ser a chave primária para seu tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="3e290-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="3e290-120">As convenções são habilitadas no construtor de modelos, que pode ser acessado por meio da substituição de OnModelCreating no contexto.</span><span class="sxs-lookup"><span data-stu-id="3e290-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="3e290-121">Atualize a classe ProductContext da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3e290-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="3e290-122">Agora, qualquer propriedade em nosso modelo chamado Key será configurada como a chave primária de qualquer entidade de sua parte.</span><span class="sxs-lookup"><span data-stu-id="3e290-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="3e290-123">Também poderíamos tornar nossas convenções mais específicas filtrando o tipo de propriedade que iremos configurar:</span><span class="sxs-lookup"><span data-stu-id="3e290-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="3e290-124">Isso configurará todas as propriedades chamadas Key para ser a chave primária de sua entidade, mas somente se forem um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="3e290-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="3e290-125">Um recurso interessante do método IsKey é que ele é aditivo.</span><span class="sxs-lookup"><span data-stu-id="3e290-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="3e290-126">Isso significa que, se você chamar IsKey em várias propriedades e todas elas se tornarão parte de uma chave composta.</span><span class="sxs-lookup"><span data-stu-id="3e290-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="3e290-127">A única limitação disso é que, ao especificar várias propriedades para uma chave, você também deve especificar uma ordem para essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="3e290-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="3e290-128">Você pode fazer isso chamando o método HasColumnOrder como abaixo:</span><span class="sxs-lookup"><span data-stu-id="3e290-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="3e290-129">Esse código configurará os tipos em nosso modelo para ter uma chave composta que consiste na coluna de chave int e na coluna de nome da cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3e290-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="3e290-130">Se exibirmos o modelo no designer, ele ficaria assim:</span><span class="sxs-lookup"><span data-stu-id="3e290-130">If we view the model in the designer it would look like this:</span></span>

![Chave composta](~/ef6/media/compositekey.png)

<span data-ttu-id="3e290-132">Outro exemplo de convenções de propriedade é configurar todas as propriedades DateTime em meu modelo para mapear para o tipo datetime2 em SQL Server em vez de DateTime.</span><span class="sxs-lookup"><span data-stu-id="3e290-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="3e290-133">Você pode conseguir isso com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="3e290-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="3e290-134">Classes de Convenção</span><span class="sxs-lookup"><span data-stu-id="3e290-134">Convention Classes</span></span>

<span data-ttu-id="3e290-135">Outra maneira de definir convenções é usar uma classe de Convenção para encapsular sua Convenção.</span><span class="sxs-lookup"><span data-stu-id="3e290-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="3e290-136">Ao usar uma classe de Convenção, você cria um tipo que herda da classe de Convenção no namespace System. Data. Entity. ModelConfiguration. convenções.</span><span class="sxs-lookup"><span data-stu-id="3e290-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="3e290-137">Podemos criar uma classe de convenção com a Convenção datetime2 que mostramos anteriormente fazendo o seguinte:</span><span class="sxs-lookup"><span data-stu-id="3e290-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="3e290-138">Para instruir o EF a usar essa convenção, adicione-a à coleção de convenções no OnModelCreating, que, se você tiver trabalhado a seguir, com o passo a passos, terá a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="3e290-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="3e290-139">Como você pode ver, adicionamos uma instância de nossa Convenção à coleção de convenções.</span><span class="sxs-lookup"><span data-stu-id="3e290-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="3e290-140">A herança de Convenção fornece uma maneira conveniente de agrupar e compartilhar convenções entre equipes ou projetos.</span><span class="sxs-lookup"><span data-stu-id="3e290-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="3e290-141">Você poderia, por exemplo, ter uma biblioteca de classes com um conjunto comum de convenções que todos os seus projetos de organizações usam.</span><span class="sxs-lookup"><span data-stu-id="3e290-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="3e290-142">Atributos personalizados</span><span class="sxs-lookup"><span data-stu-id="3e290-142">Custom Attributes</span></span>

<span data-ttu-id="3e290-143">Outro grande uso de convenções é permitir que novos atributos sejam usados ao configurar um modelo.</span><span class="sxs-lookup"><span data-stu-id="3e290-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="3e290-144">Para ilustrar isso, vamos criar um atributo que possamos usar para marcar as propriedades de cadeia de caracteres como não-Unicode.</span><span class="sxs-lookup"><span data-stu-id="3e290-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="3e290-145">Agora, vamos criar uma Convenção para aplicar esse atributo ao nosso modelo:</span><span class="sxs-lookup"><span data-stu-id="3e290-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="3e290-146">Com essa convenção, podemos adicionar o atributo nonunicode a qualquer uma de nossas propriedades de cadeia de caracteres, o que significa que a coluna no banco de dados será armazenada como varchar em vez de nvarchar.</span><span class="sxs-lookup"><span data-stu-id="3e290-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="3e290-147">Uma coisa a ser observada sobre essa Convenção é que, se você colocar o atributo não Unicode em algo diferente de uma propriedade de cadeia de caracteres, ele gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="3e290-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="3e290-148">Ele faz isso porque você não pode configurar isunicode em qualquer tipo que não seja uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3e290-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="3e290-149">Se isso acontecer, você poderá tornar sua Convenção mais específica, para que ela filtre tudo que não seja uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3e290-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="3e290-150">Embora a Convenção acima funcione para definir atributos personalizados, há outra API que pode ser muito mais fácil de usar, especialmente quando você deseja usar propriedades da classe Attribute.</span><span class="sxs-lookup"><span data-stu-id="3e290-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="3e290-151">Para este exemplo, vamos atualizar nosso atributo e alterá-lo para um atributo isunicode, portanto, ele tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="3e290-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="3e290-152">Assim que tivermos isso, podemos definir um bool em nosso atributo para informar à Convenção se uma propriedade deve ser Unicode ou não.</span><span class="sxs-lookup"><span data-stu-id="3e290-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="3e290-153">Poderíamos fazer isso na Convenção que já temos acessando o ClrProperty da classe de configuração como esta:</span><span class="sxs-lookup"><span data-stu-id="3e290-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="3e290-154">Isso é fácil o suficiente, mas há uma maneira mais sucinta de conseguir isso usando o método com da API de convenções.</span><span class="sxs-lookup"><span data-stu-id="3e290-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="3e290-155">O método with tem um parâmetro do tipo Func&lt;PropertyInfo, T&gt; que aceita o PropertyInfo da mesma forma que o método Where, mas é esperado que ele retorne um objeto.</span><span class="sxs-lookup"><span data-stu-id="3e290-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="3e290-156">Se o objeto retornado for nulo, a propriedade não será configurada, o que significa que você pode filtrar Propriedades com ele, exatamente como, mas é diferente, pois ele também capturará o objeto retornado e o passará para o método configure.</span><span class="sxs-lookup"><span data-stu-id="3e290-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="3e290-157">Isso funciona da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3e290-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="3e290-158">Os atributos personalizados não são o único motivo para usar o método com, ele é útil em qualquer lugar que você precise dizer sobre algo que você está filtrando ao configurar seus tipos ou propriedades.</span><span class="sxs-lookup"><span data-stu-id="3e290-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="3e290-159">Configurando tipos</span><span class="sxs-lookup"><span data-stu-id="3e290-159">Configuring Types</span></span>

<span data-ttu-id="3e290-160">Até agora, todas as nossas convenções foram para propriedades, mas há outra área da API de convenções para configurar os tipos em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="3e290-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="3e290-161">A experiência é semelhante às convenções que vimos até agora, mas as opções dentro da configuração estarão na entidade em vez de no nível de propriedade.</span><span class="sxs-lookup"><span data-stu-id="3e290-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="3e290-162">Uma das coisas com as quais as convenções de nível de tipo pode ser realmente útil é alterar a Convenção de nomenclatura de tabela, mapear para um esquema existente que seja diferente do padrão do EF ou criar um novo banco de dados com uma Convenção de nomenclatura diferente.</span><span class="sxs-lookup"><span data-stu-id="3e290-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="3e290-163">Para fazer isso, primeiro precisamos de um método que possa aceitar o TypeInfo para um tipo em nosso modelo e retornar o que o nome da tabela para esse tipo deve ser:</span><span class="sxs-lookup"><span data-stu-id="3e290-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="3e290-164">Esse método usa um tipo e retorna uma cadeia de caracteres que usa letras minúsculas com sublinhados em vez de CamelCase.</span><span class="sxs-lookup"><span data-stu-id="3e290-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="3e290-165">Em nosso modelo, isso significa que a classe ProductCategory será mapeada para uma tabela chamada Product\_Category em vez de ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="3e290-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="3e290-166">Assim que tivermos esse método, podemos chamá-lo em uma convenção como esta:</span><span class="sxs-lookup"><span data-stu-id="3e290-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="3e290-167">Essa convenção configura todos os tipos em nosso modelo para mapear para o nome da tabela que é retornada do nosso método getTableName.</span><span class="sxs-lookup"><span data-stu-id="3e290-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="3e290-168">Essa convenção é o equivalente a chamar o método ToTable para cada entidade no modelo usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e290-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="3e290-169">Uma coisa a ser observada sobre isso é que, quando você chamar a tarefa, o EF usará a cadeia de caracteres que você fornece como o nome exato da tabela, sem nenhuma das pluralização que normalmente faria ao determinar os nomes das tabelas.</span><span class="sxs-lookup"><span data-stu-id="3e290-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="3e290-170">É por isso que o nome da tabela de nossa Convenção é produto\_categoria em vez de categorias de\_de produto.</span><span class="sxs-lookup"><span data-stu-id="3e290-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="3e290-171">Podemos resolver isso em nossa Convenção fazendo uma chamada para o serviço de pluralização por si mesmos.</span><span class="sxs-lookup"><span data-stu-id="3e290-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="3e290-172">No código a seguir, usaremos o recurso de [resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md) adicionado em EF6 para recuperar o serviço de pluralização que o EF teria usado e Pluralize nosso nome de tabela.</span><span class="sxs-lookup"><span data-stu-id="3e290-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="3e290-173">A versão genérica do GetService é um método de extensão no namespace System. Data. Entity. Infrastructure. DependencyResolution, você precisará adicionar uma instrução using ao seu contexto a fim de usá-la.</span><span class="sxs-lookup"><span data-stu-id="3e290-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="3e290-174">Por tabela e herança</span><span class="sxs-lookup"><span data-stu-id="3e290-174">ToTable and Inheritance</span></span>

<span data-ttu-id="3e290-175">Outro aspecto importante da paraTable é que, se você mapear explicitamente um tipo para uma determinada tabela, poderá alterar a estratégia de mapeamento que o EF usará.</span><span class="sxs-lookup"><span data-stu-id="3e290-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="3e290-176">Se você chamar natable para cada tipo em uma hierarquia de herança, passando o nome do tipo como o nome da tabela como fizemos acima, você alterará a estratégia de mapeamento padrão tabela por hierarquia (TPH) para tabela por tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="3e290-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="3e290-177">A melhor maneira de descrever isso é whith um exemplo concreto:</span><span class="sxs-lookup"><span data-stu-id="3e290-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="3e290-178">Por padrão, o funcionário e o gerente são mapeados para a mesma tabela (funcionários) no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3e290-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="3e290-179">A tabela conterá funcionários e gerentes com uma coluna discriminadora que informará qual tipo de instância está armazenado em cada linha.</span><span class="sxs-lookup"><span data-stu-id="3e290-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="3e290-180">Esse é o mapeamento TPH, pois há uma única tabela para a hierarquia.</span><span class="sxs-lookup"><span data-stu-id="3e290-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="3e290-181">No entanto, se você chamar outtable em ambas as classes, cada tipo será mapeado para sua própria tabela, também conhecida como TPT, já que cada tipo tem sua própria tabela.</span><span class="sxs-lookup"><span data-stu-id="3e290-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="3e290-182">O código acima será mapeado para uma estrutura de tabela parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="3e290-182">The code above will map to a table structure that looks like the following:</span></span>

![Exemplo de TPT](~/ef6/media/tptexample.jpg)

<span data-ttu-id="3e290-184">Você pode evitar isso e manter o mapeamento TPH padrão, de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="3e290-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="3e290-185">Chame natable com o mesmo nome de tabela para cada tipo na hierarquia.</span><span class="sxs-lookup"><span data-stu-id="3e290-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="3e290-186">Chame untable somente na classe base da hierarquia, em nosso exemplo que seria Employee.</span><span class="sxs-lookup"><span data-stu-id="3e290-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="3e290-187">Ordem de execução</span><span class="sxs-lookup"><span data-stu-id="3e290-187">Execution Order</span></span>

<span data-ttu-id="3e290-188">As convenções operam da última maneira com o WINS, o mesmo que a API fluente.</span><span class="sxs-lookup"><span data-stu-id="3e290-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="3e290-189">Isso significa que, se você escrever duas convenções que configuram a mesma opção da mesma propriedade, então a última para executar o WINS.</span><span class="sxs-lookup"><span data-stu-id="3e290-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="3e290-190">Por exemplo, no código abaixo, o comprimento máximo de todas as cadeias de caracteres é definido como 500, mas então configuramos todas as propriedades chamadas Name no modelo para ter um comprimento máximo de 250.</span><span class="sxs-lookup"><span data-stu-id="3e290-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="3e290-191">Como a Convenção para definir o comprimento máximo como 250 é após a que define todas as cadeias de caracteres como 500, todas as propriedades chamadas Name em nosso modelo terão um MaxLength de 250, enquanto quaisquer outras cadeias de caracteres, como descrições, seriam 500.</span><span class="sxs-lookup"><span data-stu-id="3e290-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="3e290-192">Usar convenções dessa maneira significa que você pode fornecer uma Convenção geral para tipos ou propriedades em seu modelo e, em seguida, substituir-los para subconjuntos que são diferentes.</span><span class="sxs-lookup"><span data-stu-id="3e290-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="3e290-193">A API fluente e as anotações de dados também podem ser usadas para substituir uma convenção em casos específicos.</span><span class="sxs-lookup"><span data-stu-id="3e290-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="3e290-194">Em nosso exemplo acima, se tivéssemos usado a API fluente para definir o comprimento máximo de uma propriedade, poderíamos colocá-la antes ou depois da Convenção, porque a API fluente mais específica ganhará sobre a Convenção de configuração mais geral.</span><span class="sxs-lookup"><span data-stu-id="3e290-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="3e290-195">Convenções internas</span><span class="sxs-lookup"><span data-stu-id="3e290-195">Built-in Conventions</span></span>

<span data-ttu-id="3e290-196">Como as convenções personalizadas podem ser afetadas pelas convenções de Code First padrão, pode ser útil adicionar convenções para execução antes ou depois de outra convenção.</span><span class="sxs-lookup"><span data-stu-id="3e290-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="3e290-197">Para fazer isso, você pode usar os métodos AddBefore e AddAfter da coleção de convenções em seu DbContext derivado.</span><span class="sxs-lookup"><span data-stu-id="3e290-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="3e290-198">O código a seguir adicionaria a classe de Convenção que criamos anteriormente para que ela seja executada antes da Convenção de descoberta de chave interna.</span><span class="sxs-lookup"><span data-stu-id="3e290-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="3e290-199">Esse será o mais usado ao adicionar convenções que precisam ser executadas antes ou depois das convenções internas, uma lista das convenções internas pode ser encontrada aqui: [System. Data. Entity. ModelConfiguration. Conventions namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e290-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="3e290-200">Você também pode remover as convenções que você não deseja aplicar ao seu modelo.</span><span class="sxs-lookup"><span data-stu-id="3e290-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="3e290-201">Para remover uma convenção, use o método remove.</span><span class="sxs-lookup"><span data-stu-id="3e290-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="3e290-202">Aqui está um exemplo de como remover o PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="3e290-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
