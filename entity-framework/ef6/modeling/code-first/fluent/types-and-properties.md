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
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API fluente – Configurando e mapeando Propriedades e tipos
Ao trabalhar com Entity Framework Code First o comportamento padrão é mapear suas classes POCO para tabelas usando um conjunto de convenções inclusas no EF. Às vezes, no entanto, você não pode ou não deseja seguir essas convenções e precisa mapear entidades para algo diferente do que as convenções ditam.  

Há duas maneiras principais de configurar o EF para usar algo diferente de convenções, ou seja, [anotações](~/ef6/modeling/code-first/data-annotations.md) ou a API fluente do EFS. As anotações abrangem apenas um subconjunto da funcionalidade da API fluente, portanto, há cenários de mapeamento que não podem ser obtidos usando anotações. Este artigo foi projetado para demonstrar como usar a API fluente para configurar propriedades.  

A API Fluent do Code First é mais comumente acessada pela substituição do método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) em seu [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)derivado. Os exemplos a seguir foram projetados para mostrar como realizar várias tarefas com a API Fluent e permitir que você copie o código e personalize-o para se adequar ao seu modelo, se você quiser ver o modelo que pode ser usado com o no estado em que ele é fornecido no final deste artigo.  

## <a name="model-wide-settings"></a>Configurações de todo o modelo  

### <a name="default-schema-ef6-onwards"></a>Esquema padrão (EF6 em diante)  

A partir do EF6, você pode usar o método HasDefaultSchema no DbModelBuilder para especificar o esquema de banco de dados a ser usado para todas as tabelas, procedimentos armazenados, etc. Essa configuração padrão será substituída para todos os objetos para os quais você configurar explicitamente um esquema diferente.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Convenções personalizadas (EF6 em diante)  

A partir do EF6, você pode criar suas próprias convenções para complementar aquelas incluídas no Code First. Para obter mais detalhes, consulte [convenções de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapeamento de propriedade  

O método [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) é usado para configurar atributos para cada propriedade que pertence a uma entidade ou tipo complexo. O método Property é usado para obter um objeto de configuração para uma determinada propriedade. As opções no objeto de configuração são específicas para o tipo que está sendo configurado; Isunicode está disponível somente em Propriedades de cadeia de caracteres, por exemplo.  

### <a name="configuring-a-primary-key"></a>Configurando uma chave primária  

A Convenção de Entity Framework para chaves primárias é:  

1. Sua classe define uma propriedade cujo nome é "ID" ou "ID"  
2. ou um nome de classe seguido por "ID" ou "ID"  

Para definir explicitamente uma propriedade como uma chave primária, você pode usar o método HasKey. No exemplo a seguir, o método HasKey é usado para configurar a chave primária do Instrutorid no tipo OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Configurando uma chave primária composta  

O exemplo a seguir configura as propriedades DepartmentID e Name como a chave primária composta do tipo de departamento.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Alternando identidade para chaves primárias numéricas  

O exemplo a seguir define a propriedade DepartmentID como System. ComponentModel. Annotations. DatabaseGeneratedOption. None para indicar que o valor não será gerado pelo banco de dados.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Especificando o comprimento máximo em uma propriedade  

No exemplo a seguir, a propriedade Name não deve ter mais de 50 caracteres. Se você tornar o valor maior que 50 caracteres, receberá uma exceção [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) . Se Code First criar um banco de dados desse modelo, ele também definirá o comprimento máximo da coluna de nome como 50 caracteres.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Configurando a propriedade a ser necessária  

No exemplo a seguir, a propriedade Name é necessária. Se você não especificar o nome, receberá uma exceção DbEntityValidationException. Se Code First criar um banco de dados desse modelo, a coluna usada para armazenar essa propriedade normalmente será não anulável.  

> [!NOTE]
> Em alguns casos, talvez não seja possível que a coluna no banco de dados seja não anulável, embora a propriedade seja necessária. Por exemplo, ao usar uma estratégia de herança TPH, os dados de vários tipos são armazenados em uma única tabela. Se um tipo derivado incluir uma propriedade obrigatória, a coluna não poderá se tornar não anulável, pois nem todos os tipos na hierarquia terão essa propriedade.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Configurando um índice em uma ou mais propriedades  

> [!NOTE]
> **Somente em versões do EF 6.1** – o atributo de índice foi introduzido no Entity Framework 6,1. Se você estiver usando uma versão anterior, as informações nesta seção não se aplicarão.  

A criação de índices não é suportada nativamente pela API Fluent, mas você pode fazer uso do suporte para **indexattribute** por meio da API fluente. Os atributos de índice são processados incluindo uma anotação de modelo no modelo que é então transformada em um índice no banco de dados posteriormente no pipeline. Você pode adicionar manualmente essas mesmas anotações usando a API fluente.  

A maneira mais fácil de fazer isso é criar uma instância de **indexattribute** que contenha todas as configurações para o novo índice. Em seguida, você pode criar uma instância de **IndexAnnotation** que é um tipo específico do EF que converterá as configurações de **indexattribute** em uma anotação de modelo que pode ser armazenada no modelo do EF. Eles podem então ser passados para o método **HasColumnAnnotation** na API Fluent, especificando o **índice** de nome para a anotação.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Para obter uma lista completa das configurações disponíveis no **indexattribute**, consulte a seção de *índice* de [Code First anotações de dados](~/ef6/modeling/code-first/data-annotations.md). Isso inclui a personalização do nome do índice, a criação de índices exclusivos e a criação de índices de várias colunas.  

Você pode especificar várias anotações de índice em uma única propriedade passando uma matriz de **indexattribute** para o construtor de **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Especificando para não mapear uma propriedade CLR para uma coluna no banco de dados  

O exemplo a seguir mostra como especificar que uma propriedade em um tipo CLR não está mapeada para uma coluna no banco de dados.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapeando uma propriedade CLR para uma coluna específica no banco de dados  

O exemplo a seguir mapeia a propriedade de nome CLR para a coluna Departmentname do banco de dados.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Renomeando uma chave estrangeira que não está definida no modelo  

Se você optar por não definir uma chave estrangeira em um tipo CLR, mas quiser especificar o nome que ele deve ter no banco de dados, faça o seguinte:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Configurando se uma propriedade de cadeia de caracteres dá suporte a conteúdo Unicode  

Por padrão, as cadeias de caracteres são Unicode (nvarchar em SQL Server). Você pode usar o método isunicode para especificar que uma cadeia de caracteres deve ser do tipo varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Configurando o tipo de dados de uma coluna de Database  

O método [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) habilita o mapeamento para diferentes representações do mesmo tipo básico. O uso desse método não permite que você execute qualquer conversão dos dados em tempo de execução. Observe que isunicode é a maneira preferida de definir colunas como varchar, pois ela é independente de banco de dados.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Configurando propriedades em um tipo complexo  

Há duas maneiras de configurar propriedades escalares em um tipo complexo.  

Você pode chamar a propriedade em ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Você também pode usar a notação de ponto para acessar uma propriedade de um tipo complexo.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Configurando uma propriedade a ser usada como um token de simultaneidade otimista  

Para especificar que uma propriedade em uma entidade representa um token de simultaneidade, você pode usar o atributo ConcurrencyCheck ou o método IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Você também pode usar o método IsRowVersion para configurar a propriedade para ser uma versão de linha no banco de dados. Definir a propriedade como uma versão de linha a configura automaticamente para ser um token de simultaneidade otimista.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapeamento de tipo  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Especificando que uma classe é um tipo complexo  

Por convenção, um tipo que não tem chave primária especificada é tratado como um tipo complexo. Há alguns cenários em que Code First não detectará um tipo complexo (por exemplo, se você tiver uma propriedade chamada ID, mas não quer que ela seja uma chave primária). Nesses casos, você usaria a API fluente para especificar explicitamente que um tipo é um tipo complexo.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Especificando para não mapear um tipo de entidade CLR para uma tabela no banco de dados  

O exemplo a seguir mostra como excluir um tipo CLR de ser mapeado para uma tabela no banco de dados.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapeando um tipo de entidade para uma tabela específica no banco de dados  

Todas as propriedades do departamento serão mapeadas para colunas em uma tabela chamada departamento t_.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Você também pode especificar o nome do esquema como este:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapeando a herança de tabela por hierarquia (TPH)  

No cenário de mapeamento TPH, todos os tipos em uma hierarquia de herança são mapeados para uma única tabela. Uma coluna discriminadora é usada para identificar o tipo de cada linha. Ao criar seu modelo com Code First, TPH é a estratégia padrão para os tipos que participam da hierarquia de herança. Por padrão, a coluna discriminador é adicionada à tabela com o nome "discriminador" e o nome do tipo CLR de cada tipo na hierarquia é usado para os valores discriminadores. Você pode modificar o comportamento padrão usando a API Fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapeando a herança de tabela por tipo (TPT)  

No cenário de mapeamento TPT, todos os tipos são mapeados para tabelas individuais. As propriedades que pertencem exclusivamente a um tipo base ou tipo derivado são armazenadas em uma tabela que é mapeada para esse tipo. Tabelas que são mapeadas para tipos derivados também armazenam uma chave estrangeira que une a tabela derivada com a tabela base.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapeando a herança da classe de tabela por concreto (TPC)  

No cenário de mapeamento do TPC, todos os tipos não abstratos na hierarquia são mapeados para tabelas individuais. As tabelas que são mapeadas para as classes derivadas não têm nenhuma relação com a tabela que é mapeada para a classe base no banco de dados. Todas as propriedades de uma classe, incluindo as propriedades herdadas, são mapeadas para colunas da tabela correspondente.  

Chame o método MapInheritedProperties para configurar cada tipo derivado. MapInheritedProperties remapeia todas as propriedades que foram herdadas da classe base para novas colunas na tabela para a classe derivada.  

> [!NOTE]
> Observe que, como as tabelas que participam da hierarquia de herança do TPC não compartilham uma chave primária, haverá chaves de entidade duplicadas ao inserir em tabelas mapeadas para subclasses se você tiver valores gerados pelo banco de dados com a mesma semente de identidade. Para resolver esse problema, você pode especificar um valor de semente inicial diferente para cada tabela ou desativar a identidade na propriedade de chave primária. Identity é o valor padrão para propriedades de chave de inteiro ao trabalhar com Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapeando Propriedades de um tipo de entidade para várias tabelas no banco de dados (divisão de entidade)  

A divisão de entidade permite que as propriedades de um tipo de entidade sejam distribuídas entre várias tabelas. No exemplo a seguir, a entidade Department é dividida em duas tabelas: Department e DepartmentDetails. A divisão de entidade usa várias chamadas para o método MAP para mapear um subconjunto de propriedades para uma tabela específica.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapear vários tipos de entidade para uma tabela no banco de dados (divisão de tabela)  

O exemplo a seguir mapeia dois tipos de entidade que compartilham uma chave primária para uma tabela.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapeando um tipo de entidade para inserir/atualizar/excluir procedimentos armazenados (EF6 em diante)  

A partir do EF6, você pode mapear uma entidade para usar procedimentos armazenados para Insert Update e Delete. Para obter mais detalhes, consulte [Code First inserir/atualizar/excluir procedimentos armazenados](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

## <a name="model-used-in-samples"></a>Modelo usado em exemplos  

O modelo de Code First a seguir é usado para os exemplos nesta página.  

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
