---
title: Convenções baseadas em modelo-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656162"
---
# <a name="model-based-conventions"></a><span data-ttu-id="fa679-102">Convenções baseadas em modelo</span><span class="sxs-lookup"><span data-stu-id="fa679-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="fa679-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fa679-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fa679-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="fa679-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fa679-105">As convenções baseadas em modelo são um método avançado de configuração de modelo baseada em convenção.</span><span class="sxs-lookup"><span data-stu-id="fa679-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="fa679-106">Para a maioria dos cenários, a [API de Convenção de Code First personalizada em DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve ser usada.</span><span class="sxs-lookup"><span data-stu-id="fa679-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="fa679-107">É recomendável compreender a API DbModelBuilder para convenções antes de usar convenções baseadas em modelo.</span><span class="sxs-lookup"><span data-stu-id="fa679-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="fa679-108">As convenções baseadas em modelo permitem a criação de convenções que afetam as propriedades e as tabelas que não são configuráveis por meio de convenções padrão.</span><span class="sxs-lookup"><span data-stu-id="fa679-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="fa679-109">Exemplos deles são colunas discriminadoras em modelos de tabela por hierarquia e colunas de associação independentes.</span><span class="sxs-lookup"><span data-stu-id="fa679-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="fa679-110">Criando uma Convenção</span><span class="sxs-lookup"><span data-stu-id="fa679-110">Creating a Convention</span></span>   

<span data-ttu-id="fa679-111">A primeira etapa na criação de uma convenção baseada em modelo é escolher quando no pipeline a Convenção precisa ser aplicada ao modelo.</span><span class="sxs-lookup"><span data-stu-id="fa679-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="fa679-112">Há dois tipos de convenções de modelo, conceitual (C-Space) e loja (S-Space).</span><span class="sxs-lookup"><span data-stu-id="fa679-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="fa679-113">Uma Convenção de espaço C é aplicada ao modelo que o aplicativo cria, enquanto uma Convenção de S-Space é aplicada à versão do modelo que representa o banco de dados e controla itens como, por exemplo, como as colunas geradas automaticamente são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="fa679-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="fa679-114">Uma Convenção de modelo é uma classe que se estende de IConceptualModelConvention ou IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="fa679-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="fa679-115">Essas interfaces aceitam um tipo genérico que pode ser do tipo MetadataItem, que é usado para filtrar o tipo de dados ao qual a Convenção se aplica.</span><span class="sxs-lookup"><span data-stu-id="fa679-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="fa679-116">Adicionando uma Convenção</span><span class="sxs-lookup"><span data-stu-id="fa679-116">Adding a Convention</span></span>   

<span data-ttu-id="fa679-117">As convenções de modelo são adicionadas da mesma maneira que as classes de convenções regulares.</span><span class="sxs-lookup"><span data-stu-id="fa679-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="fa679-118">No método **OnModelCreating** , adicione a Convenção à lista de convenções para um modelo.</span><span class="sxs-lookup"><span data-stu-id="fa679-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="fa679-119">Uma convenção também pode ser adicionada em relação a outra convenção usando as convenções. addantes\<\> ou convenções. addapós\<métodos \>.</span><span class="sxs-lookup"><span data-stu-id="fa679-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="fa679-120">Para obter mais informações sobre as convenções que Entity Framework se aplica, consulte a seção observações.</span><span class="sxs-lookup"><span data-stu-id="fa679-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="fa679-121">Exemplo: Convenção de modelo discriminador</span><span class="sxs-lookup"><span data-stu-id="fa679-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="fa679-122">Renomear colunas geradas pelo EF é um exemplo de algo que você não pode fazer com as outras APIs de convenções.</span><span class="sxs-lookup"><span data-stu-id="fa679-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="fa679-123">Essa é uma situação em que o uso de convenções de modelo é sua única opção.</span><span class="sxs-lookup"><span data-stu-id="fa679-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="fa679-124">Um exemplo de como usar uma convenção baseada em modelo para configurar colunas geradas é personalizar a maneira como as colunas discriminadoras são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="fa679-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="fa679-125">Abaixo está um exemplo de uma convenção simples baseada em modelo que renomeia cada coluna no modelo chamado "discriminador" como "EntityType".</span><span class="sxs-lookup"><span data-stu-id="fa679-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="fa679-126">Isso inclui colunas que o desenvolvedor simplesmente chama "discriminador".</span><span class="sxs-lookup"><span data-stu-id="fa679-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="fa679-127">Como a coluna "discriminador" é uma coluna gerada, isso precisa ser executado em S-Space.</span><span class="sxs-lookup"><span data-stu-id="fa679-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="fa679-128">Exemplo: Convenção de renomeação de IA geral</span><span class="sxs-lookup"><span data-stu-id="fa679-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="fa679-129">Outro exemplo mais complicado de convenções baseadas em modelo em ação é configurar a maneira como as associações independentes (IAs) são nomeadas.</span><span class="sxs-lookup"><span data-stu-id="fa679-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="fa679-130">Essa é uma situação em que as convenções de modelo são aplicáveis porque o IAs é gerado pelo EF e não estão presentes no modelo que a API DbModelBuilder pode acessar.</span><span class="sxs-lookup"><span data-stu-id="fa679-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="fa679-131">Quando o EF gera um IA, ele cria uma coluna chamada EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="fa679-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="fa679-132">Por exemplo, para uma associação chamada Customer com uma coluna de chave chamada CustomerId, ela geraria uma coluna chamada Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="fa679-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="fa679-133">A Convenção a seguir remove o caractere '\_' do nome da coluna que é gerado para IA.</span><span class="sxs-lookup"><span data-stu-id="fa679-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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
        for (int i = 0; i < properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a><span data-ttu-id="fa679-134">Estendendo convenções existentes</span><span class="sxs-lookup"><span data-stu-id="fa679-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="fa679-135">Se você precisar escrever uma convenção semelhante a uma das convenções que Entity Framework já se aplica ao modelo, você sempre poderá estender essa Convenção para evitar ter que reescrevê-la do zero.</span><span class="sxs-lookup"><span data-stu-id="fa679-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="fa679-136">Um exemplo disso é substituir a Convenção de correspondência de ID existente por uma personalizada.</span><span class="sxs-lookup"><span data-stu-id="fa679-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="fa679-137">Um benefício adicional para substituir a Convenção de chave é que o método substituído será chamado somente se não houver nenhuma chave já detectada ou configurada explicitamente.</span><span class="sxs-lookup"><span data-stu-id="fa679-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="fa679-138">Uma lista de convenções que são usadas pelo Entity Framework está disponível aqui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa679-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="fa679-139">Em seguida, precisamos adicionar nossa nova Convenção antes da Convenção de chave existente.</span><span class="sxs-lookup"><span data-stu-id="fa679-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="fa679-140">Depois de adicionarmos o CustomKeyDiscoveryConvention, podemos remover o IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="fa679-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="fa679-141">Se não removermos o IdKeyDiscoveryConvention existente, essa Convenção ainda terá precedência sobre a Convenção de descoberta de ID, pois ela é executada primeiro, mas, no caso em que nenhuma propriedade "Key" for encontrada, a Convenção "ID" será executada.</span><span class="sxs-lookup"><span data-stu-id="fa679-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="fa679-142">Vemos esse comportamento porque cada convenção vê o modelo como atualizado pela Convenção anterior (em vez de operar nele de forma independente e tudo sendo combinado) para que, por exemplo, uma Convenção anterior tenha atualizado um nome de coluna para corresponder a algo de de interesse para sua Convenção personalizada (quando antes de o nome não ser interessante), ela será aplicada a essa coluna.</span><span class="sxs-lookup"><span data-stu-id="fa679-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="fa679-143">Anotações</span><span class="sxs-lookup"><span data-stu-id="fa679-143">Notes</span></span>  

<span data-ttu-id="fa679-144">Uma lista de convenções atualmente aplicadas pelo Entity Framework está disponível na documentação do MSDN aqui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="fa679-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="fa679-145">Essa lista é extraída diretamente do nosso código-fonte.</span><span class="sxs-lookup"><span data-stu-id="fa679-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="fa679-146">O código-fonte para o Entity Framework 6 está disponível no [GitHub](https://github.com/aspnet/entityframework6/) e muitas das convenções usadas pelo Entity Framework são bons pontos de partida para as convenções personalizadas baseadas em modelos.</span><span class="sxs-lookup"><span data-stu-id="fa679-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
