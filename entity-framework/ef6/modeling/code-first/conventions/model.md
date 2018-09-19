---
title: Convenções de baseado em modelo - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283701"
---
# <a name="model-based-conventions"></a><span data-ttu-id="eebaf-102">Convenções de modelo</span><span class="sxs-lookup"><span data-stu-id="eebaf-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="eebaf-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="eebaf-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="eebaf-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="eebaf-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="eebaf-105">Convenções de modelo com base são um método avançado de configuração do modelo baseado na convenção.</span><span class="sxs-lookup"><span data-stu-id="eebaf-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="eebaf-106">Na maioria dos cenários de [personalizado convenção de API do Code First no DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="eebaf-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="eebaf-107">Uma compreensão da API DbModelBuilder para convenções é recomendada antes de usar as convenções de modelo com base.</span><span class="sxs-lookup"><span data-stu-id="eebaf-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="eebaf-108">Convenções de modelo com base em permitem a criação de convenções que afetam as tabelas que não são configuráveis por meio de convenções de padrão e propriedades.</span><span class="sxs-lookup"><span data-stu-id="eebaf-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="eebaf-109">Exemplos disso são colunas de discriminador de tabela por modelos de hierarquia e as colunas de associação independente.</span><span class="sxs-lookup"><span data-stu-id="eebaf-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="eebaf-110">Criar uma convenção</span><span class="sxs-lookup"><span data-stu-id="eebaf-110">Creating a Convention</span></span>   

<span data-ttu-id="eebaf-111">A primeira etapa na criação de uma convenção de modelo com base é escolher quando o pipeline a convenção de precisa ser aplicado ao modelo.</span><span class="sxs-lookup"><span data-stu-id="eebaf-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="eebaf-112">Há dois tipos de convenções de modelo conceitual (C-Space) e Store (S-Space).</span><span class="sxs-lookup"><span data-stu-id="eebaf-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="eebaf-113">Uma convenção de C-Space é aplicada ao modelo de que o aplicativo for compilado, enquanto uma convenção de S-Space é aplicada para a versão do modelo que representa o banco de dados e controles de coisas, como colunas como automaticamente gerados são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="eebaf-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="eebaf-114">Uma convenção de modelo é uma classe que se estende do IConceptualModelConvention ou IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="eebaf-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="eebaf-115">Essas interfaces que ambos aceitar um tipo genérico que pode ser do tipo MetadataItem que é usado para filtrar o tipo de dados que se aplica a convenção a.</span><span class="sxs-lookup"><span data-stu-id="eebaf-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="eebaf-116">Adicionando uma convenção</span><span class="sxs-lookup"><span data-stu-id="eebaf-116">Adding a Convention</span></span>   

<span data-ttu-id="eebaf-117">Convenções de modelo são adicionadas da mesma forma como as classes de convenções regular.</span><span class="sxs-lookup"><span data-stu-id="eebaf-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="eebaf-118">No **OnModelCreating** método, adicione a convenção à lista de convenções para um modelo.</span><span class="sxs-lookup"><span data-stu-id="eebaf-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="eebaf-119">Também é possível adicionar uma convenção em relação à outra convenção usando o Conventions.AddBefore\< \> ou Conventions.AddAfter\< \> métodos.</span><span class="sxs-lookup"><span data-stu-id="eebaf-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="eebaf-120">Para obter mais informações sobre as convenções que aplica-se do Entity Framework, consulte a seção Observações.</span><span class="sxs-lookup"><span data-stu-id="eebaf-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="eebaf-121">Exemplo: Convenção de modelo do discriminador</span><span class="sxs-lookup"><span data-stu-id="eebaf-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="eebaf-122">Renomear colunas geradas pelo EF é um exemplo de algo que você não pode fazer com as outras convenções de APIs.</span><span class="sxs-lookup"><span data-stu-id="eebaf-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="eebaf-123">Essa é uma situação em que usar as convenções de modelo é sua única opção.</span><span class="sxs-lookup"><span data-stu-id="eebaf-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="eebaf-124">Um exemplo de como usar uma convenção de modelo com base em para configurar colunas geradas é Personalizando a maneira como colunas de discriminador são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="eebaf-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="eebaf-125">Abaixo está um exemplo de uma convenção de modelo simples com base em que renomeia todas as colunas no modelo denominada "Discriminadora" para "EntityType".</span><span class="sxs-lookup"><span data-stu-id="eebaf-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="eebaf-126">Isso inclui colunas que o desenvolvedor simplesmente denominada "Discriminadora".</span><span class="sxs-lookup"><span data-stu-id="eebaf-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="eebaf-127">Uma vez que a coluna "Discriminadora" é uma coluna gerada, que isso deve ser executado no espaço de S.</span><span class="sxs-lookup"><span data-stu-id="eebaf-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="eebaf-128">Exemplo: IA geral renomeando convenção</span><span class="sxs-lookup"><span data-stu-id="eebaf-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="eebaf-129">Outro exemplo mais complicado de convenções de modelo com base em ação é configurar o modo como os que associações independentes (IAs) são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="eebaf-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="eebaf-130">Essa é uma situação em que as convenções de modelo são aplicáveis porque IAs são gerados pelo EF e não estão presentes no modelo que pode acessar a API do DbModelBuilder.</span><span class="sxs-lookup"><span data-stu-id="eebaf-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="eebaf-131">Quando o EF gera um IA, ele cria uma coluna denominada EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="eebaf-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="eebaf-132">Por exemplo, para uma associação chamada Customer com uma coluna de chave chamado CustomerId isso geraria uma coluna denominada Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="eebaf-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="eebaf-133">As faixas de convenção a seguir o '\_' caractere fora do nome da coluna que é gerado para o IA.</span><span class="sxs-lookup"><span data-stu-id="eebaf-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="eebaf-134">Estendendo as convenções de existentes</span><span class="sxs-lookup"><span data-stu-id="eebaf-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="eebaf-135">Se você precisa escrever uma convenção que é semelhante a uma das convenções que o Entity Framework já se aplica ao seu modelo, que você pode estender sempre que a convenção para evitar a necessidade de reescrevê-lo a partir do zero.</span><span class="sxs-lookup"><span data-stu-id="eebaf-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="eebaf-136">Um exemplo disso é substituir a Id existente correspondente a convenção com uma personalizada.</span><span class="sxs-lookup"><span data-stu-id="eebaf-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="eebaf-137">Um benefício adicional para substituir a convenção de chave é que o método substituído será chamado apenas se não houver nenhuma chave já foi detectado ou configurado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="eebaf-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="eebaf-138">Uma lista de convenções que são usados pelo Entity Framework está disponível aqui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="eebaf-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="eebaf-139">Em seguida, precisamos adicionar nossa nova convenção antes da convenção de chave existente.</span><span class="sxs-lookup"><span data-stu-id="eebaf-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="eebaf-140">Depois de adicionarmos o CustomKeyDiscoveryConvention, podemos remover o IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="eebaf-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="eebaf-141">Se não removemos o IdKeyDiscoveryConvention existente essa convenção ainda terão precedência sobre a convenção de descoberta de Id, pois ele é executado pela primeira vez, mas no caso em que nenhuma propriedade de "chave" for encontrada, a convenção de "id" será executado.</span><span class="sxs-lookup"><span data-stu-id="eebaf-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="eebaf-142">Podemos ver esse comportamento porque cada convenção vê o modelo conforme atualizado pela convenção de anterior (em vez de operar de forma independente e todos os que estão sendo combinados) para que se, por exemplo, uma convenção anterior atualizado para corresponder a algo de um nome de coluna interesse sua convenção personalizada (quando antes que o nome não era de interesse), em seguida, ela será aplicada a essa coluna.</span><span class="sxs-lookup"><span data-stu-id="eebaf-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="eebaf-143">Observações</span><span class="sxs-lookup"><span data-stu-id="eebaf-143">Notes</span></span>  

<span data-ttu-id="eebaf-144">Uma lista de convenções que são aplicadas no momento pelo Entity Framework está disponível na documentação do MSDN aqui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="eebaf-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="eebaf-145">Essa lista é extraída diretamente do nosso código-fonte.</span><span class="sxs-lookup"><span data-stu-id="eebaf-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="eebaf-146">O código-fonte para o Entity Framework 6 está disponível no [GitHub](https://github.com/aspnet/entityframework6/) e muitas das convenções usadas pelo Entity Framework são bons pontos de partida para o modelo personalizado com base em convenções.</span><span class="sxs-lookup"><span data-stu-id="eebaf-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
