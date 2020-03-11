---
title: API fluente-relações-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419069"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="53327-102">API fluente-relações</span><span class="sxs-lookup"><span data-stu-id="53327-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="53327-103">Esta página fornece informações sobre como configurar relações em seu modelo de Code First usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="53327-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="53327-104">Para obter informações gerais sobre relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="53327-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="53327-105">Ao trabalhar com Code First, você define seu modelo definindo suas classes CLR de domínio.</span><span class="sxs-lookup"><span data-stu-id="53327-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="53327-106">Por padrão, Entity Framework usa as convenções de Code First para mapear suas classes para o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="53327-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="53327-107">Se você usar as convenções de nomenclatura de Code First, na maioria dos casos, você pode contar com Code First para configurar relações entre as tabelas com base nas chaves estrangeiras e propriedades de navegação definidas nas classes.</span><span class="sxs-lookup"><span data-stu-id="53327-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="53327-108">Se você não seguir as convenções ao definir suas classes, ou se quiser alterar a forma como as convenções funcionam, poderá usar a API fluente ou as anotações de dados para configurar suas classes para que Code First possa mapear as relações entre as tabelas.</span><span class="sxs-lookup"><span data-stu-id="53327-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="53327-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="53327-109">Introduction</span></span>  

<span data-ttu-id="53327-110">Ao configurar uma relação com a API Fluent, você começa com a instância EntityTypeConfiguration e, em seguida, usa o método HasRequired, HasOptional ou HasMany para especificar o tipo de relação em que essa entidade participa.</span><span class="sxs-lookup"><span data-stu-id="53327-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="53327-111">Os métodos HasRequired e HasOptional usam uma expressão lambda que representa uma propriedade de navegação de referência.</span><span class="sxs-lookup"><span data-stu-id="53327-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="53327-112">O método HasMany usa uma expressão lambda que representa uma propriedade de navegação de coleção.</span><span class="sxs-lookup"><span data-stu-id="53327-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="53327-113">Em seguida, você pode configurar uma propriedade de navegação inversa usando os métodos WithRequired, WithOptional e WithMany.</span><span class="sxs-lookup"><span data-stu-id="53327-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="53327-114">Esses métodos têm sobrecargas que não usam argumentos e podem ser usados para especificar a cardinalidade com navegações unidirecionais.</span><span class="sxs-lookup"><span data-stu-id="53327-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="53327-115">Em seguida, você pode configurar as propriedades de chave estrangeira usando o método HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="53327-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="53327-116">Esse método usa uma expressão lambda que representa a propriedade a ser usada como chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="53327-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="53327-117">Configurando uma relação obrigatória para opcional (um-para-zero-ou-um)</span><span class="sxs-lookup"><span data-stu-id="53327-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="53327-118">O exemplo a seguir configura uma relação um-para-zero-ou-um.</span><span class="sxs-lookup"><span data-stu-id="53327-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="53327-119">O OfficeAssignment tem a propriedade Instrutorid que é uma chave primária e uma chave estrangeira, porque o nome da propriedade não segue a Convenção em que o método HasKey é usado para configurar a chave primária.</span><span class="sxs-lookup"><span data-stu-id="53327-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="53327-120">Configurando uma relação em que ambas as extremidades são necessárias (um-para-um)</span><span class="sxs-lookup"><span data-stu-id="53327-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="53327-121">Na maioria dos casos Entity Framework pode inferir qual tipo é o dependente e qual é a entidade de segurança em uma relação.</span><span class="sxs-lookup"><span data-stu-id="53327-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="53327-122">No entanto, quando ambas as extremidades da relação são necessárias ou ambos os lados são opcionais Entity Framework não podem identificar o dependente e a entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="53327-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="53327-123">Quando ambas as extremidades da relação forem necessárias, use WithRequiredPrincipal ou WithRequiredDependent após o método HasRequired.</span><span class="sxs-lookup"><span data-stu-id="53327-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="53327-124">Quando ambas as extremidades da relação forem opcionais, use WithOptionalPrincipal ou WithOptionalDependent após o método HasOptional.</span><span class="sxs-lookup"><span data-stu-id="53327-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="53327-125">Configurando uma relação muitos-para-muitos</span><span class="sxs-lookup"><span data-stu-id="53327-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="53327-126">O código a seguir configura uma relação muitos-para-muitos entre os tipos de curso e instrutor.</span><span class="sxs-lookup"><span data-stu-id="53327-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="53327-127">No exemplo a seguir, as convenções de Code First padrão são usadas para criar uma tabela de junção.</span><span class="sxs-lookup"><span data-stu-id="53327-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="53327-128">Como resultado, a tabela CourseInstructor é criada com Course_CourseID e Instructor_InstructorID colunas.</span><span class="sxs-lookup"><span data-stu-id="53327-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="53327-129">Se você quiser especificar o nome da tabela de junção e os nomes das colunas na tabela, precisará fazer configurações adicionais usando o método Map.</span><span class="sxs-lookup"><span data-stu-id="53327-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="53327-130">O código a seguir gera a tabela CourseInstructor com as colunas Cursoid e Instrutorid.</span><span class="sxs-lookup"><span data-stu-id="53327-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="53327-131">Configurando uma relação com uma propriedade de navegação</span><span class="sxs-lookup"><span data-stu-id="53327-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="53327-132">Uma relação unidirecional (também chamada unidirecional) é quando uma propriedade de navegação é definida em apenas um dos lados da relação e não em ambos.</span><span class="sxs-lookup"><span data-stu-id="53327-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="53327-133">Por convenção, Code First sempre interpreta uma relação unidirecional como um-para-muitos.</span><span class="sxs-lookup"><span data-stu-id="53327-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="53327-134">Por exemplo, se você quiser uma relação um-para-um entre instrutor e OfficeAssignment, em que você tem uma propriedade de navegação somente no tipo de instrutor, precisará usar a API fluente para configurar essa relação.</span><span class="sxs-lookup"><span data-stu-id="53327-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="53327-135">Habilitando exclusão em cascata</span><span class="sxs-lookup"><span data-stu-id="53327-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="53327-136">Você pode configurar a exclusão em cascata em uma relação usando o método WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="53327-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="53327-137">Se uma chave estrangeira na entidade dependente não for anulável, Code First definirá a exclusão em cascata na relação.</span><span class="sxs-lookup"><span data-stu-id="53327-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="53327-138">Se uma chave estrangeira na entidade dependente for anulável, Code First não definirá a exclusão em cascata na relação e, quando a entidade de segurança for excluída, a chave estrangeira será definida como NULL.</span><span class="sxs-lookup"><span data-stu-id="53327-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="53327-139">Você pode remover essas convenções de exclusão em cascata usando:</span><span class="sxs-lookup"><span data-stu-id="53327-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="53327-140">modelBuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="53327-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="53327-141">modelBuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="53327-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="53327-142">O código a seguir configura a relação como necessária e, em seguida, desabilita a exclusão em cascata.</span><span class="sxs-lookup"><span data-stu-id="53327-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="53327-143">Configurando uma chave estrangeira composta</span><span class="sxs-lookup"><span data-stu-id="53327-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="53327-144">Se a chave primária no tipo de departamento consistir nas propriedades DepartmentID e Name, você configurará a chave primária para o departamento e a chave estrangeira nos tipos de curso da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="53327-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="53327-145">Renomeando uma chave estrangeira que não está definida no modelo</span><span class="sxs-lookup"><span data-stu-id="53327-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="53327-146">Se você optar por não definir uma chave estrangeira no tipo CLR, mas quiser especificar o nome que ele deve ter no banco de dados, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="53327-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="53327-147">Configurando um nome de chave estrangeira que não segue a Convenção de Code First</span><span class="sxs-lookup"><span data-stu-id="53327-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="53327-148">Se a propriedade Foreign Key na classe Course for chamada SomeDepartmentID em vez de DepartmentID, você precisará fazer o seguinte para especificar que deseja que SomeDepartmentID seja a chave estrangeira:</span><span class="sxs-lookup"><span data-stu-id="53327-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="53327-149">Modelo usado em exemplos</span><span class="sxs-lookup"><span data-stu-id="53327-149">Model Used in Samples</span></span>  

<span data-ttu-id="53327-150">O modelo de Code First a seguir é usado para os exemplos nesta página.</span><span class="sxs-lookup"><span data-stu-id="53327-150">The following Code First model is used for the samples on this page.</span></span>  

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
