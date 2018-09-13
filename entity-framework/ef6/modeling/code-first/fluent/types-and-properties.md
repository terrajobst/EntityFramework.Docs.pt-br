---
title: API Fluent - Configurando e mapeamento de tipos e propriedades - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490164"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="dd877-102">API Fluent - Configurando e mapeamento de tipos e propriedades</span><span class="sxs-lookup"><span data-stu-id="dd877-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="dd877-103">Ao trabalhar com o Entity Framework Code First o comportamento padrão é mapear suas classes POCO para tabelas usando um conjunto de convenções integradas ao EF.</span><span class="sxs-lookup"><span data-stu-id="dd877-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="dd877-104">Às vezes, no entanto, você não pode ou não deseja siga essas convenções e precisa mapear entidades para algo que não seja o que ditam as convenções.</span><span class="sxs-lookup"><span data-stu-id="dd877-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="dd877-105">Há duas maneiras principais que você pode configurar para usar algo diferente de convenções, ou seja, o EF [anotações](~/ef6/modeling/code-first/data-annotations.md) ou a API fluente do EFs.</span><span class="sxs-lookup"><span data-stu-id="dd877-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="dd877-106">As anotações cobrem apenas um subconjunto da funcionalidade do API fluente, portanto, há cenários de mapeamento não podem ser obtidos usando anotações.</span><span class="sxs-lookup"><span data-stu-id="dd877-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="dd877-107">Este artigo foi projetado para demonstrar como usar a API fluente para configurar as propriedades.</span><span class="sxs-lookup"><span data-stu-id="dd877-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="dd877-108">A API fluente do code first é acessada com mais frequência, substituindo o [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) método no seu derivada [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd877-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="dd877-109">Os exemplos a seguir são projetados para mostrar como realizar várias tarefas com a api fluente e permitem que você copie o código e personalizar de acordo com seu modelo, se você quiser ver o modelo que pode ser usados com como-está, em seguida, ele é fornecido no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="dd877-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="dd877-110">Configurações de todo o modelo</span><span class="sxs-lookup"><span data-stu-id="dd877-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="dd877-111">Esquema padrão (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="dd877-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="dd877-112">Começando com o EF6, você pode usar o método HasDefaultSchema em DbModelBuilder para especificar o esquema de banco de dados a ser usado para todas as tabelas, procedimentos armazenados, etc. Essa configuração padrão será substituída para todos os objetos que você definir explicitamente um esquema diferente para.</span><span class="sxs-lookup"><span data-stu-id="dd877-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="dd877-113">Convenções personalizadas (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="dd877-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="dd877-114">Começando com o EF6, você pode criar suas próprias convenções para complementar aquelas incluídas no Code First.</span><span class="sxs-lookup"><span data-stu-id="dd877-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="dd877-115">Para obter mais detalhes, consulte [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="dd877-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="dd877-116">Mapeamento de propriedade</span><span class="sxs-lookup"><span data-stu-id="dd877-116">Property Mapping</span></span>  

<span data-ttu-id="dd877-117">O [propriedade](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) método é usado para configurar atributos para cada propriedade que pertence a uma entidade ou tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dd877-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="dd877-118">O método de propriedade é usado para obter um objeto de configuração para uma determinada propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd877-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="dd877-119">As opções no objeto de configuração são específicas para o tipo que está sendo configurado; IsUnicode está disponível somente nas propriedades de cadeia de caracteres por exemplo.</span><span class="sxs-lookup"><span data-stu-id="dd877-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="dd877-120">Configurando uma chave primária</span><span class="sxs-lookup"><span data-stu-id="dd877-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="dd877-121">A convenção do Entity Framework para chaves primárias é:</span><span class="sxs-lookup"><span data-stu-id="dd877-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="dd877-122">Sua classe define uma propriedade cujo nome é "ID" ou "Id"</span><span class="sxs-lookup"><span data-stu-id="dd877-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="dd877-123">ou um nome de classe seguido de "ID" ou "Id"</span><span class="sxs-lookup"><span data-stu-id="dd877-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="dd877-124">Para definir explicitamente uma propriedade a ser uma chave primária, você pode usar o método HasKey.</span><span class="sxs-lookup"><span data-stu-id="dd877-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="dd877-125">No exemplo a seguir, o método HasKey é usado para configurar a chave primária de InstructorID no tipo OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="dd877-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="dd877-126">Configurando uma chave primária composta</span><span class="sxs-lookup"><span data-stu-id="dd877-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="dd877-127">O exemplo a seguir configura as propriedades DepartmentID e o nome para ser a chave primária composta do tipo de departamento.</span><span class="sxs-lookup"><span data-stu-id="dd877-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="dd877-128">Desativando a identidade para chaves primárias numérico</span><span class="sxs-lookup"><span data-stu-id="dd877-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="dd877-129">O exemplo a seguir define a propriedade de ID do departamento para System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None para indicar que o valor não será gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="dd877-130">Especifica o comprimento máximo em uma propriedade</span><span class="sxs-lookup"><span data-stu-id="dd877-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="dd877-131">No exemplo a seguir, a propriedade Name deve ser não mais de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="dd877-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="dd877-132">Se você fizer o valor mais de 50 caracteres, você obterá um [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exceção.</span><span class="sxs-lookup"><span data-stu-id="dd877-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="dd877-133">Se o Code First cria um banco de dados por meio desse modelo também definirá o comprimento máximo da coluna de nome como 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="dd877-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="dd877-134">Configurando a propriedade como obrigatória</span><span class="sxs-lookup"><span data-stu-id="dd877-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="dd877-135">No exemplo a seguir, a propriedade de nome é necessária.</span><span class="sxs-lookup"><span data-stu-id="dd877-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="dd877-136">Se você não especificar o nome, você receberá uma exceção de DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="dd877-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="dd877-137">Se o Code First cria um banco de dados por meio desse modelo, em seguida, a coluna usada para armazenar essa propriedade geralmente será não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="dd877-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="dd877-138">Em alguns casos, pode não ser possível para a coluna no banco de dados a ser não anuláveis, mesmo que a propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="dd877-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="dd877-139">Por exemplo, quando usar uma data de estratégia de herança TPH para vários tipos é armazenado em uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="dd877-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="dd877-140">Se um tipo derivado inclui uma propriedade necessária que a coluna não pode se tornar não anulável, pois nem todos os tipos na hierarquia terá essa propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd877-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="dd877-141">Configurando um índice em uma ou mais propriedades</span><span class="sxs-lookup"><span data-stu-id="dd877-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="dd877-142">**EF6.1 em diante, somente** -atributo o índice foi introduzido no Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="dd877-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="dd877-143">Se você estiver usando uma versão anterior, as informações nesta seção não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="dd877-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="dd877-144">Criação de índices não é compatível nativamente com a API Fluent, mas você pode fazer uso do suporte para **IndexAttribute** por meio da API Fluent.</span><span class="sxs-lookup"><span data-stu-id="dd877-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="dd877-145">Atributos de índice são processados com a inclusão de uma anotação de modelo no modelo que, em seguida, é transformada em um índice no banco de dados posteriormente no pipeline.</span><span class="sxs-lookup"><span data-stu-id="dd877-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="dd877-146">Você pode adicionar manualmente esses mesmos anotações usando a API do Fluent.</span><span class="sxs-lookup"><span data-stu-id="dd877-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="dd877-147">A maneira mais fácil de fazer isso é criar uma instância do **IndexAttribute** que contém todas as configurações para o novo índice.</span><span class="sxs-lookup"><span data-stu-id="dd877-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="dd877-148">Em seguida, você pode criar uma instância do **IndexAnnotation** que é um tipo específico de EF que converterá o **IndexAttribute** configurações em uma anotação de modelo que podem ser armazenados no modelo de EF.</span><span class="sxs-lookup"><span data-stu-id="dd877-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="dd877-149">Elas podem ser passadas para o **HasColumnAnnotation** método na API Fluent, especificando o nome **índice** da anotação.</span><span class="sxs-lookup"><span data-stu-id="dd877-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="dd877-150">Para obter uma lista completa das configurações disponíveis no **IndexAttribute**, consulte o *índice* seção [anotações de dados Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="dd877-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="dd877-151">Isso inclui o nome do índice de personalização, Criando índices exclusivos e criar índices com múltiplas colunas.</span><span class="sxs-lookup"><span data-stu-id="dd877-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="dd877-152">Você pode especificar várias anotações de índice em uma única propriedade, passando uma matriz de **IndexAttribute** para o construtor da **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="dd877-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="dd877-153">Especificando para não mapear uma propriedade CLR para uma coluna no banco de dados</span><span class="sxs-lookup"><span data-stu-id="dd877-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="dd877-154">O exemplo a seguir mostra como especificar uma propriedade em um tipo CLR não está mapeada para uma coluna no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="dd877-155">Uma propriedade de CLR de mapeamento para uma coluna específica no banco de dados</span><span class="sxs-lookup"><span data-stu-id="dd877-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="dd877-156">O exemplo a seguir mapeia a propriedade CLR de nome para a coluna de banco de dados DepartmentName.</span><span class="sxs-lookup"><span data-stu-id="dd877-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="dd877-157">Renomear uma chave estrangeira que não está definida no modelo</span><span class="sxs-lookup"><span data-stu-id="dd877-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="dd877-158">Se você optar por não definir uma chave estrangeira em um tipo CLR, mas para especificar qual nome deve ter no banco de dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="dd877-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="dd877-159">Configurando se uma propriedade de cadeia de caracteres dá suporte a Unicode conteúdo</span><span class="sxs-lookup"><span data-stu-id="dd877-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="dd877-160">Por padrão, cadeias de caracteres são Unicode (nvarchar no SQL Server).</span><span class="sxs-lookup"><span data-stu-id="dd877-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="dd877-161">Você pode usar o método IsUnicode para especificar que uma cadeia de caracteres deve ser do tipo varchar.</span><span class="sxs-lookup"><span data-stu-id="dd877-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="dd877-162">Configurando o tipo de dados de uma coluna de banco de dados</span><span class="sxs-lookup"><span data-stu-id="dd877-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="dd877-163">O [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) método habilita o mapeamento para representações diferentes do mesmo tipo básico.</span><span class="sxs-lookup"><span data-stu-id="dd877-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="dd877-164">Usando esse método não permite que você executar qualquer conversão dos dados em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd877-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="dd877-165">Observe que o IsUnicode é a maneira preferencial de colunas de configuração para varchar, já que ele é independente de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="dd877-166">Configurando as propriedades em um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="dd877-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="dd877-167">Há duas maneiras de configurar propriedades escalares em um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dd877-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="dd877-168">Você pode chamar a propriedade em ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="dd877-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="dd877-169">Você também pode usar a notação de ponto para acessar uma propriedade de um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dd877-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="dd877-170">Configurando uma propriedade a ser usado como um Token de simultaneidade otimista</span><span class="sxs-lookup"><span data-stu-id="dd877-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="dd877-171">Para especificar que uma propriedade em uma entidade representa um token de simultaneidade, você pode usar o atributo ConcurrencyCheck ou o método IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="dd877-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="dd877-172">Você também pode usar o método IsRowVersion para configurar a propriedade para ser uma versão de linha no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="dd877-173">Definindo a propriedade seja que uma versão de linha automaticamente configura para ser um token de simultaneidade otimista.</span><span class="sxs-lookup"><span data-stu-id="dd877-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="dd877-174">Mapeamento de tipo</span><span class="sxs-lookup"><span data-stu-id="dd877-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="dd877-175">Especifica que uma classe é um tipo complexo</span><span class="sxs-lookup"><span data-stu-id="dd877-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="dd877-176">Por convenção, um tipo que não tem nenhuma chave primária especificada é tratado como um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dd877-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="dd877-177">Existem alguns cenários onde Code First não detectará um tipo complexo (por exemplo, se você tem uma propriedade chamada ID, mas você não quer dizer para que ele seja uma chave primária).</span><span class="sxs-lookup"><span data-stu-id="dd877-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="dd877-178">Nesses casos, você usaria a API fluente para especificar explicitamente um tipo é um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="dd877-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="dd877-179">Especificando para não mapear um tipo de entidade do CLR para uma tabela no banco de dados</span><span class="sxs-lookup"><span data-stu-id="dd877-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="dd877-180">O exemplo a seguir mostra como excluir um tipo CLR de que é mapeado para uma tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="dd877-181">Mapeando um tipo de entidade para uma tabela específica no banco de dados</span><span class="sxs-lookup"><span data-stu-id="dd877-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="dd877-182">Todas as propriedades de departamento serão ser mapeadas para colunas em uma tabela chamada t_ departamento.</span><span class="sxs-lookup"><span data-stu-id="dd877-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="dd877-183">Você também pode especificar o nome do esquema como este:</span><span class="sxs-lookup"><span data-stu-id="dd877-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="dd877-184">Mapeando a herança de tabela por hierarquia (TPH)</span><span class="sxs-lookup"><span data-stu-id="dd877-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="dd877-185">No cenário de mapeamento TPH, todos os tipos em uma hierarquia de herança são mapeados para uma única tabela.</span><span class="sxs-lookup"><span data-stu-id="dd877-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="dd877-186">Uma coluna de discriminador é usada para identificar o tipo de cada linha.</span><span class="sxs-lookup"><span data-stu-id="dd877-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="dd877-187">Ao criar seu modelo com o Code First, TPH é a estratégia padrão para os tipos que participam na hierarquia de herança.</span><span class="sxs-lookup"><span data-stu-id="dd877-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="dd877-188">Por padrão, a coluna de discriminador é adicionada à tabela com o nome "Discriminadora" e o nome do tipo CLR de cada tipo de hierarquia é usado para os valores de discriminador.</span><span class="sxs-lookup"><span data-stu-id="dd877-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="dd877-189">Você pode modificar o comportamento padrão usando a API fluente.</span><span class="sxs-lookup"><span data-stu-id="dd877-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="dd877-190">Mapeando a herança de tabela por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="dd877-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="dd877-191">No cenário de mapeamento TPT, todos os tipos são mapeados para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="dd877-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="dd877-192">Propriedades que pertencem apenas a um tipo base ou um tipo derivado são armazenadas em uma tabela que mapeia para esse tipo.</span><span class="sxs-lookup"><span data-stu-id="dd877-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="dd877-193">As tabelas que são mapeados para tipos derivados também armazenam uma chave estrangeira que une a tabela derivada com a tabela base.</span><span class="sxs-lookup"><span data-stu-id="dd877-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="dd877-194">Mapeando a herança de tabela-por classe concreta (TPC)</span><span class="sxs-lookup"><span data-stu-id="dd877-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="dd877-195">No cenário de mapeamento TPC, todos os tipos não abstratos na hierarquia são mapeados para tabelas individuais.</span><span class="sxs-lookup"><span data-stu-id="dd877-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="dd877-196">As tabelas que são mapeados para as classes derivadas não têm nenhuma relação à tabela que mapeia para a classe base no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dd877-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="dd877-197">Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd877-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="dd877-198">Chame o método MapInheritedProperties para configurar cada tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="dd877-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="dd877-199">MapInheritedProperties remapeia todas as propriedades que foram herdadas da classe base para novas colunas na tabela para a classe derivada.</span><span class="sxs-lookup"><span data-stu-id="dd877-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="dd877-200">Observe que como as tabelas que participam na hierarquia de herança TPC não compartilham uma chave primária existe será chaves de entidade duplicados ao inserir em tabelas que são mapeadas para subclasses se você tiver valores de banco de dados gerado com a mesma semente de identidade.</span><span class="sxs-lookup"><span data-stu-id="dd877-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="dd877-201">Para resolver esse problema, você pode especificar um valor de semente inicial diferente para cada tabela ou desativar a identidade na propriedade de chave primária.</span><span class="sxs-lookup"><span data-stu-id="dd877-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="dd877-202">Identidade é o valor padrão para propriedades de chave de inteiro ao trabalhar com o Code First.</span><span class="sxs-lookup"><span data-stu-id="dd877-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="dd877-203">Mapeamento de propriedades de um tipo de entidade para várias tabelas no banco de dados (separação da entidade)</span><span class="sxs-lookup"><span data-stu-id="dd877-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="dd877-204">Separação da entidade permite que as propriedades de um tipo de entidade sejam distribuídas em várias tabelas.</span><span class="sxs-lookup"><span data-stu-id="dd877-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="dd877-205">No exemplo a seguir, a entidade Department é dividida em duas tabelas: DepartmentDetails e departamento.</span><span class="sxs-lookup"><span data-stu-id="dd877-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="dd877-206">Separação da entidade usa várias chamadas para o método Map para mapear um subconjunto de propriedades para uma tabela específica.</span><span class="sxs-lookup"><span data-stu-id="dd877-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="dd877-207">Vários tipos de entidade de mapeamento para uma tabela no banco de dados (divisão de tabela)</span><span class="sxs-lookup"><span data-stu-id="dd877-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="dd877-208">O exemplo a seguir mapeia os dois tipos de entidade que compartilham uma chave primária para uma tabela.</span><span class="sxs-lookup"><span data-stu-id="dd877-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="dd877-209">Um tipo de entidade de mapeamento para procedimentos armazenados de Insert/Update/Delete (EF6 em diante)</span><span class="sxs-lookup"><span data-stu-id="dd877-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="dd877-210">Começando com o EF6, você pode mapear uma entidade para usar procedimentos armazenados para inserir atualizar e excluir.</span><span class="sxs-lookup"><span data-stu-id="dd877-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="dd877-211">Para obter mais detalhes, veja [código primeiro Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="dd877-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="dd877-212">Usado nos exemplos de modelo</span><span class="sxs-lookup"><span data-stu-id="dd877-212">Model Used in Samples</span></span>  

<span data-ttu-id="dd877-213">O seguinte modelo Code First é usado para os exemplos nesta página.</span><span class="sxs-lookup"><span data-stu-id="dd877-213">The following Code First model is used for the samples on this page.</span></span>  

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
