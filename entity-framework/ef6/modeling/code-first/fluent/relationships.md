---
title: API Fluent - relações - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490461"
---
# <a name="fluent-api---relationships"></a>API Fluent - relações
> [!NOTE]
> Esta página fornece informações sobre como configurar as relações em seu modelo Code First usando a API fluente. Para obter informações gerais sobre as relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).  

Ao trabalhar com o Code First, você define seu modelo definindo suas classes de domínio CLR. Por padrão, o Entity Framework usa as convenções Code First para mapear suas classes para o esquema de banco de dados. Se você usar as convenções de nomenclatura do Code First, na maioria dos casos, você pode confiar no Code First para definir as relações entre tabelas com base nas propriedades de navegação que você define nas classes e chaves estrangeiras. Se você não seguir as convenções ao definir suas classes, ou se você quiser alterar a maneira como as convenções de funcionam, você pode usar a API fluente ou anotações de dados para configurar suas classes, para que o Code First pode mapear as relações entre tabelas.  

## <a name="introduction"></a>Introdução  

Ao configurar uma relação com a API fluente, inicie com a instância de EntityTypeConfiguration e, em seguida, use o método HasRequired, HasOptional ou HasMany para especificar o tipo de relação que esta entidade participa. Os métodos HasRequired e HasOptional usam uma expressão lambda que representa uma propriedade de navegação de referência. O método HasMany usa uma expressão lambda que representa uma propriedade de navegação de coleção. Em seguida, você pode configurar uma propriedade de navegação inversa usando os métodos WithRequired, WithOptional e WithMany. Esses métodos têm sobrecargas que não usam argumentos e podem ser usadas para especificar a cardinalidade navegações unidirecionais.  

Em seguida, você pode configurar propriedades de chave estrangeira usando o método HasForeignKey. Esse método usa uma expressão lambda que representa a propriedade a ser usado como a chave estrangeira.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Configurando um relacionamento obrigatório-para-opcional (um-para-Zero-ou-um)  

O exemplo a seguir configura uma relação um-para-zero-ou-um. O OfficeAssignment tem a propriedade InstructorID que é uma chave primária e uma chave estrangeira, porque o nome da propriedade não segue a convenção, que o método HasKey é usado para configurar a chave primária.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Configurar uma relação em que ambas as extremidades são necessárias (individual)  

Na maioria dos casos do Entity Framework pode inferir o tipo é dependente e qual é a entidade de segurança em uma relação. No entanto, quando as extremidades da relação são necessárias ou ambos os lados são opcionais Entity Framework não pode identificar o dependente e principal. Quando ambas as extremidades da relação são necessárias, use WithRequiredPrincipal ou WithRequiredDependent após o método HasRequired. Quando ambas as extremidades da relação são opcionais, use WithOptionalPrincipal ou WithOptionalDependent após o método HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Configurar uma relação muitos-para-muitos  

O código a seguir configura uma relação muitos-para-muitos entre os tipos de curso e instrutor. No exemplo a seguir, as convenções de Code First padrão são usadas para criar uma tabela de junção. Como resultado, a tabela de CourseInstructor é criada com as colunas Course_CourseID e Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Se você quiser especificar o nome da tabela de junção e os nomes das colunas na tabela, você precisará fazer configurações adicionais por meio do método de mapa. O código a seguir gera uma tabela com colunas CourseID e InstructorID CourseInstructor.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Configurar uma relação com uma propriedade de navegação  

Um unidirecional (também chamado de unidirecional) relação é quando uma propriedade de navegação é definida em apenas uma das extremidades da relação e não em ambos. Por convenção, Code First sempre interpreta uma relação unidirecional como um-para-muitos. Por exemplo, se você quiser que uma relação um para um entre Instructor e OfficeAssignment, onde você tem uma propriedade de navegação apenas o tipo de instrutor, você precisa usar a API fluente para configurar essa relação.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Habilitando exclusão em cascata  

Você pode configurar a exclusão em cascata em uma relação usando o método WillCascadeOnDelete. Se uma chave estrangeira na entidade dependente não permite valor nulo, em seguida, Code First define exclusão em cascata na relação. Se uma chave estrangeira na entidade dependente é anulável, Code First não define a exclusão em cascata na relação e quando a entidade é excluída a chave estrangeira será definida como null.  

Você pode remover essas convenções de exclusão em cascata, usando:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

O código a seguir configura a relação para ser necessárias e, em seguida, desabilita a exclusão em cascata.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Configurando uma chave estrangeira composta  

Se a chave primária no tipo de departamento consistiu em Propriedades DepartmentID e Name, você configuraria a chave primária para o departamento e a chave estrangeira nos tipos de curso da seguinte maneira:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Renomear uma chave estrangeira que não está definida no modelo  

Se você optar por não definir uma chave estrangeira no tipo de CLR, mas para especificar qual nome deve ter no banco de dados, faça o seguinte:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Configurando um nome de chave estrangeiro que não segue a convenção de primeiro código  

Se a propriedade de chave estrangeira na classe curso foi chamada SomeDepartmentID em vez da ID do departamento, você precisa fazer o seguinte para especificar que você deseja SomeDepartmentID para ser a chave estrangeira:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Usado nos exemplos de modelo  

O seguinte modelo Code First é usado para os exemplos nesta página.  

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
