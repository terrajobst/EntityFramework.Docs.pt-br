---
title: API fluente – Configurando e mapeando Propriedades e tipos-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419062"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="82695-102">API fluente – Configurando e mapeando Propriedades e tipos</span><span class="sxs-lookup"><span data-stu-id="82695-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="82695-103">Ao trabalhar com Entity Framework Code First o comportamento padrão é mapear suas classes POCO para tabelas usando um conjunto de convenções inclusas no EF.</span><span class="sxs-lookup"><span data-stu-id="82695-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="82695-104">Às vezes, no entanto, você não pode ou não deseja seguir essas convenções e precisa mapear entidades para algo diferente do que as convenções ditam.</span><span class="sxs-lookup"><span data-stu-id="82695-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="82695-105">Há duas maneiras principais de configurar o EF para usar algo diferente de convenções, ou seja, [anotações](~/ef6/modeling/code-first/data-annotations.md) ou a API fluente do EFS.</span><span class="sxs-lookup"><span data-stu-id="82695-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="82695-106">As anotações abrangem apenas um subconjunto da funcionalidade da API fluente, portanto, há cenários de mapeamento que não podem ser obtidos usando anotações.</span><span class="sxs-lookup"><span data-stu-id="82695-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="82695-107">Este artigo foi projetado para demonstrar como usar a API fluente para configurar propriedades.</span><span class="sxs-lookup"><span data-stu-id="82695-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="82695-108">A API Fluent do Code First é mais comumente acessada pela substituição do método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) em seu [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)derivado.</span><span class="sxs-lookup"><span data-stu-id="82695-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="82695-109">Os exemplos a seguir foram projetados para mostrar como realizar várias tarefas com a API Fluent e permitir que você copie o código e personalize-o para se adequar ao seu modelo, se você quiser ver o modelo que pode ser usado com o no estado em que ele é fornecido no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="82695-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="82695-110">Configurações de todo o modelo</span><span class="sxs-lookup"><span data-stu-id="82695-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="82695-111">Esquema padrão (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="82695-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="82695-112">A partir do EF6, você pode usar o método HasDefaultSchema no DbModelBuilder para especificar o esquema de banco de dados a ser usado para todas as tabelas, procedimentos armazenados, etc. Essa configuração padrão será substituída para todos os objetos para os quais você configurar explicitamente um esquema diferente.</span><span class="sxs-lookup"><span data-stu-id="82695-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="82695-113">Convenções personalizadas (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="82695-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="82695-114">A partir do EF6, você pode criar suas próprias convenções para complementar aquelas incluídas no Code First.</span><span class="sxs-lookup"><span data-stu-id="82695-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="82695-115">Para obter mais detalhes, consulte [convenções de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="82695-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="82695-116">Mapeamento de propriedade</span><span class="sxs-lookup"><span data-stu-id="82695-116">Property Mapping</span></span>  

<span data-ttu-id="82695-117">O método [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) é usado para configurar atributos para cada propriedade que pertence a uma entidade ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="82695-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="82695-118">O método Property é usado para obter um objeto de configuração para uma determinada propriedade.</span><span class="sxs-lookup"><span data-stu-id="82695-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="82695-119">As opções no objeto de configuração são específicas para o tipo que está sendo configurado; Isunicode está disponível somente em Propriedades de cadeia de caracteres, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="82695-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="82695-120">Configurando uma chave primária</span><span class="sxs-lookup"><span data-stu-id="82695-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="82695-121">A Convenção de Entity Framework para chaves primárias é:</span><span class="sxs-lookup"><span data-stu-id="82695-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="82695-122">Sua classe define uma propriedade cujo nome é "ID" ou "ID"</span><span class="sxs-lookup"><span data-stu-id="82695-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="82695-123">ou um nome de classe seguido por "ID" ou "ID"</span><span class="sxs-lookup"><span data-stu-id="82695-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="82695-124">Para definir explicitamente uma propriedade como uma chave primária, você pode usar o método HasKey.</span><span class="sxs-lookup"><span data-stu-id="82695-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="82695-125">No exemplo a seguir, o método HasKey é usado para configurar a chave primária do Instrutorid no tipo OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="82695-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="82695-126">Configurando uma chave primária composta</span><span class="sxs-lookup"><span data-stu-id="82695-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="82695-127">O exemplo a seguir configura as propriedades DepartmentID e Name como a chave primária composta do tipo de departamento.</span><span class="sxs-lookup"><span data-stu-id="82695-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="82695-128">Alternando identidade para chaves primárias numéricas</span><span class="sxs-lookup"><span data-stu-id="82695-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="82695-129">O exemplo a seguir define a propriedade DepartmentID como System. ComponentModel. Annotations. DatabaseGeneratedOption. None para indicar que o valor não será gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="82695-130">Especificando o comprimento máximo em uma propriedade</span><span class="sxs-lookup"><span data-stu-id="82695-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="82695-131">No exemplo a seguir, a propriedade Name não deve ter mais de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="82695-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="82695-132">Se você tornar o valor maior que 50 caracteres, receberá uma exceção [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="82695-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="82695-133">Se Code First criar um banco de dados desse modelo, ele também definirá o comprimento máximo da coluna de nome como 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="82695-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="82695-134">Configurando a propriedade a ser necessária</span><span class="sxs-lookup"><span data-stu-id="82695-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="82695-135">No exemplo a seguir, a propriedade Name é necessária.</span><span class="sxs-lookup"><span data-stu-id="82695-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="82695-136">Se você não especificar o nome, receberá uma exceção DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="82695-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="82695-137">Se Code First criar um banco de dados desse modelo, a coluna usada para armazenar essa propriedade normalmente será não anulável.</span><span class="sxs-lookup"><span data-stu-id="82695-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="82695-138">Em alguns casos, talvez não seja possível que a coluna no banco de dados seja não anulável, embora a propriedade seja necessária.</span><span class="sxs-lookup"><span data-stu-id="82695-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="82695-139">Por exemplo, ao usar uma estratégia de herança TPH, os dados de vários tipos são armazenados em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="82695-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="82695-140">Se um tipo derivado incluir uma propriedade obrigatória, a coluna não poderá se tornar não anulável, pois nem todos os tipos na hierarquia terão essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="82695-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="82695-141">Configurando um índice em uma ou mais propriedades</span><span class="sxs-lookup"><span data-stu-id="82695-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="82695-142">**Somente em versões do EF 6.1** – o atributo de índice foi introduzido no Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="82695-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="82695-143">Se você estiver usando uma versão anterior, as informações nesta seção não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="82695-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="82695-144">A criação de índices não é suportada nativamente pela API Fluent, mas você pode fazer uso do suporte para **indexattribute** por meio da API fluente.</span><span class="sxs-lookup"><span data-stu-id="82695-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="82695-145">Os atributos de índice são processados incluindo uma anotação de modelo no modelo que é então transformada em um índice no banco de dados posteriormente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="82695-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="82695-146">Você pode adicionar manualmente essas mesmas anotações usando a API fluente.</span><span class="sxs-lookup"><span data-stu-id="82695-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="82695-147">A maneira mais fácil de fazer isso é criar uma instância de **indexattribute** que contenha todas as configurações para o novo índice.</span><span class="sxs-lookup"><span data-stu-id="82695-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="82695-148">Em seguida, você pode criar uma instância de **IndexAnnotation** que é um tipo específico do EF que converterá as configurações de **indexattribute** em uma anotação de modelo que pode ser armazenada no modelo do EF.</span><span class="sxs-lookup"><span data-stu-id="82695-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="82695-149">Eles podem então ser passados para o método **HasColumnAnnotation** na API Fluent, especificando o **índice** de nome para a anotação.</span><span class="sxs-lookup"><span data-stu-id="82695-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="82695-150">Para obter uma lista completa das configurações disponíveis no **indexattribute**, consulte a seção de *índice* de [Code First anotações de dados](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="82695-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="82695-151">Isso inclui a personalização do nome do índice, a criação de índices exclusivos e a criação de índices de várias colunas.</span><span class="sxs-lookup"><span data-stu-id="82695-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="82695-152">Você pode especificar várias anotações de índice em uma única propriedade passando uma matriz de **indexattribute** para o construtor de **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="82695-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="82695-153">Especificando para não mapear uma propriedade CLR para uma coluna no banco de dados</span><span class="sxs-lookup"><span data-stu-id="82695-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="82695-154">O exemplo a seguir mostra como especificar que uma propriedade em um tipo CLR não está mapeada para uma coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="82695-155">Mapeando uma propriedade CLR para uma coluna específica no banco de dados</span><span class="sxs-lookup"><span data-stu-id="82695-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="82695-156">O exemplo a seguir mapeia a propriedade de nome CLR para a coluna Departmentname do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="82695-157">Renomeando uma chave estrangeira que não está definida no modelo</span><span class="sxs-lookup"><span data-stu-id="82695-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="82695-158">Se você optar por não definir uma chave estrangeira em um tipo CLR, mas quiser especificar o nome que ele deve ter no banco de dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="82695-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="82695-159">Configurando se uma propriedade de cadeia de caracteres dá suporte a conteúdo Unicode</span><span class="sxs-lookup"><span data-stu-id="82695-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="82695-160">Por padrão, as cadeias de caracteres são Unicode (nvarchar em SQL Server).</span><span class="sxs-lookup"><span data-stu-id="82695-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="82695-161">Você pode usar o método isunicode para especificar que uma cadeia de caracteres deve ser do tipo varchar.</span><span class="sxs-lookup"><span data-stu-id="82695-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="82695-162">Configurando o tipo de dados de uma coluna de Database</span><span class="sxs-lookup"><span data-stu-id="82695-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="82695-163">O método [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) habilita o mapeamento para diferentes representações do mesmo tipo básico.</span><span class="sxs-lookup"><span data-stu-id="82695-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="82695-164">O uso desse método não permite que você execute qualquer conversão dos dados em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="82695-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="82695-165">Observe que isunicode é a maneira preferida de definir colunas como varchar, pois ela é independente de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="82695-166">Configurando propriedades em um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="82695-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="82695-167">Há duas maneiras de configurar propriedades escalares em um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="82695-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="82695-168">Você pode chamar a propriedade em ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="82695-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="82695-169">Você também pode usar a notação de ponto para acessar uma propriedade de um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="82695-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="82695-170">Configurando uma propriedade a ser usada como um token de simultaneidade otimista</span><span class="sxs-lookup"><span data-stu-id="82695-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="82695-171">Para especificar que uma propriedade em uma entidade representa um token de simultaneidade, você pode usar o atributo ConcurrencyCheck ou o método IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="82695-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="82695-172">Você também pode usar o método IsRowVersion para configurar a propriedade para ser uma versão de linha no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="82695-173">Definir a propriedade como uma versão de linha a configura automaticamente para ser um token de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="82695-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="82695-174">Mapeamento de tipo</span><span class="sxs-lookup"><span data-stu-id="82695-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="82695-175">Especificando que uma classe é um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="82695-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="82695-176">Por convenção, um tipo que não tem chave primária especificada é tratado como um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="82695-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="82695-177">Há alguns cenários em que Code First não detectará um tipo complexo (por exemplo, se você tiver uma propriedade chamada ID, mas não quer que ela seja uma chave primária).</span><span class="sxs-lookup"><span data-stu-id="82695-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="82695-178">Nesses casos, você usaria a API fluente para especificar explicitamente que um tipo é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="82695-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="82695-179">Especificando para não mapear um tipo de entidade CLR para uma tabela no banco de dados</span><span class="sxs-lookup"><span data-stu-id="82695-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="82695-180">O exemplo a seguir mostra como excluir um tipo CLR de ser mapeado para uma tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="82695-181">Mapeando um tipo de entidade para uma tabela específica no banco de dados</span><span class="sxs-lookup"><span data-stu-id="82695-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="82695-182">Todas as propriedades do departamento serão mapeadas para colunas em uma tabela chamada departamento t_.</span><span class="sxs-lookup"><span data-stu-id="82695-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="82695-183">Você também pode especificar o nome do esquema como este:</span><span class="sxs-lookup"><span data-stu-id="82695-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="82695-184">Mapeando a herança de tabela por hierarquia (TPH)</span><span class="sxs-lookup"><span data-stu-id="82695-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="82695-185">No cenário de mapeamento TPH, todos os tipos em uma hierarquia de herança são mapeados para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="82695-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="82695-186">Uma coluna discriminadora é usada para identificar o tipo de cada linha.</span><span class="sxs-lookup"><span data-stu-id="82695-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="82695-187">Ao criar seu modelo com Code First, TPH é a estratégia padrão para os tipos que participam da hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="82695-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="82695-188">Por padrão, a coluna discriminador é adicionada à tabela com o nome "discriminador" e o nome do tipo CLR de cada tipo na hierarquia é usado para os valores discriminadores.</span><span class="sxs-lookup"><span data-stu-id="82695-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="82695-189">Você pode modificar o comportamento padrão usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="82695-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="82695-190">Mapeando a herança de tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="82695-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="82695-191">No cenário de mapeamento TPT, todos os tipos são mapeados para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="82695-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="82695-192">As propriedades que pertencem exclusivamente a um tipo base ou tipo derivado são armazenadas em uma tabela que é mapeada para esse tipo.</span><span class="sxs-lookup"><span data-stu-id="82695-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="82695-193">Tabelas que são mapeadas para tipos derivados também armazenam uma chave estrangeira que une a tabela derivada com a tabela base.</span><span class="sxs-lookup"><span data-stu-id="82695-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="82695-194">Mapeando a herança da classe de tabela por concreto (TPC)</span><span class="sxs-lookup"><span data-stu-id="82695-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="82695-195">No cenário de mapeamento do TPC, todos os tipos não abstratos na hierarquia são mapeados para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="82695-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="82695-196">As tabelas que são mapeadas para as classes derivadas não têm nenhuma relação com a tabela que é mapeada para a classe base no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="82695-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="82695-197">Todas as propriedades de uma classe, incluindo as propriedades herdadas, são mapeadas para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="82695-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="82695-198">Chame o método MapInheritedProperties para configurar cada tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="82695-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="82695-199">MapInheritedProperties remapeia todas as propriedades que foram herdadas da classe base para novas colunas na tabela para a classe derivada.</span><span class="sxs-lookup"><span data-stu-id="82695-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="82695-200">Observe que, como as tabelas que participam da hierarquia de herança do TPC não compartilham uma chave primária, haverá chaves de entidade duplicadas ao inserir em tabelas mapeadas para subclasses se você tiver valores gerados pelo banco de dados com a mesma semente de identidade.</span><span class="sxs-lookup"><span data-stu-id="82695-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="82695-201">Para resolver esse problema, você pode especificar um valor de semente inicial diferente para cada tabela ou desativar a identidade na propriedade de chave primária.</span><span class="sxs-lookup"><span data-stu-id="82695-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="82695-202">Identity é o valor padrão para propriedades de chave de inteiro ao trabalhar com Code First.</span><span class="sxs-lookup"><span data-stu-id="82695-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="82695-203">Mapeando Propriedades de um tipo de entidade para várias tabelas no banco de dados (divisão de entidade)</span><span class="sxs-lookup"><span data-stu-id="82695-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="82695-204">A divisão de entidade permite que as propriedades de um tipo de entidade sejam distribuídas entre várias tabelas.</span><span class="sxs-lookup"><span data-stu-id="82695-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="82695-205">No exemplo a seguir, a entidade Department é dividida em duas tabelas: Department e DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="82695-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="82695-206">A divisão de entidade usa várias chamadas para o método MAP para mapear um subconjunto de propriedades para uma tabela específica.</span><span class="sxs-lookup"><span data-stu-id="82695-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="82695-207">Mapear vários tipos de entidade para uma tabela no banco de dados (divisão de tabela)</span><span class="sxs-lookup"><span data-stu-id="82695-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="82695-208">O exemplo a seguir mapeia dois tipos de entidade que compartilham uma chave primária para uma tabela.</span><span class="sxs-lookup"><span data-stu-id="82695-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="82695-209">Mapeando um tipo de entidade para inserir/atualizar/excluir procedimentos armazenados (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="82695-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="82695-210">A partir do EF6, você pode mapear uma entidade para usar procedimentos armazenados para Insert Update e Delete.</span><span class="sxs-lookup"><span data-stu-id="82695-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="82695-211">Para obter mais detalhes, consulte [Code First inserir/atualizar/excluir procedimentos armazenados](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="82695-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="82695-212">Modelo usado em exemplos</span><span class="sxs-lookup"><span data-stu-id="82695-212">Model Used in Samples</span></span>  

<span data-ttu-id="82695-213">O modelo de Code First a seguir é usado para os exemplos nesta página.</span><span class="sxs-lookup"><span data-stu-id="82695-213">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
