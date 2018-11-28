---
title: API Fluent - Configurando e mapeamento de tipos e propriedades - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: 75f8a179ac9a70ad390fc7ab2a6c5e714e701b8b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339797"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API Fluent - Configurando e mapeamento de tipos e propriedades
Ao trabalhar com o Entity Framework Code First o comportamento padrão é mapear suas classes POCO para tabelas usando um conjunto de convenções integradas ao EF. Às vezes, no entanto, você não pode ou não deseja siga essas convenções e precisa mapear entidades para algo que não seja o que ditam as convenções.  

Há duas maneiras principais que você pode configurar para usar algo diferente de convenções, ou seja, o EF [anotações](~/ef6/modeling/code-first/data-annotations.md) ou a API fluente do EFs. As anotações cobrem apenas um subconjunto da funcionalidade do API fluente, portanto, há cenários de mapeamento não podem ser obtidos usando anotações. Este artigo foi projetado para demonstrar como usar a API fluente para configurar as propriedades.  

A API fluente do code first é acessada com mais frequência, substituindo o [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) método no seu derivada [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Os exemplos a seguir são projetados para mostrar como realizar várias tarefas com a api fluente e permitem que você copie o código e personalizar de acordo com seu modelo, se você quiser ver o modelo que pode ser usados com como-está, em seguida, ele é fornecido no final deste artigo.  

## <a name="model-wide-settings"></a>Configurações de todo o modelo  

### <a name="default-schema-ef6-onwards"></a>Esquema padrão (EF6 em diante)  

Começando com o EF6, você pode usar o método HasDefaultSchema em DbModelBuilder para especificar o esquema de banco de dados a ser usado para todas as tabelas, procedimentos armazenados, etc. Essa configuração padrão será substituída para todos os objetos que você definir explicitamente um esquema diferente para.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Convenções personalizadas (EF6 em diante)  

Começando com o EF6, você pode criar suas próprias convenções para complementar aquelas incluídas no Code First. Para obter mais detalhes, consulte [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapeamento de propriedade  

O [propriedade](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) método é usado para configurar atributos para cada propriedade que pertence a uma entidade ou tipo complexo. O método de propriedade é usado para obter um objeto de configuração para uma determinada propriedade. As opções no objeto de configuração são específicas para o tipo que está sendo configurado; IsUnicode está disponível somente nas propriedades de cadeia de caracteres por exemplo.  

### <a name="configuring-a-primary-key"></a>Configurando uma chave primária  

A convenção do Entity Framework para chaves primárias é:  

1. Sua classe define uma propriedade cujo nome é "ID" ou "Id"  
2. ou um nome de classe seguido de "ID" ou "Id"  

Para definir explicitamente uma propriedade a ser uma chave primária, você pode usar o método HasKey. No exemplo a seguir, o método HasKey é usado para configurar a chave primária de InstructorID no tipo OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Configurando uma chave primária composta  

O exemplo a seguir configura as propriedades DepartmentID e o nome para ser a chave primária composta do tipo de departamento.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Desativando a identidade para chaves primárias numérico  

O exemplo a seguir define a propriedade de ID do departamento para System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None para indicar que o valor não será gerado pelo banco de dados.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Especifica o comprimento máximo em uma propriedade  

No exemplo a seguir, a propriedade Name deve ser não mais de 50 caracteres. Se você fizer o valor mais de 50 caracteres, você obterá um [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exceção. Se o Code First cria um banco de dados por meio desse modelo também definirá o comprimento máximo da coluna de nome como 50 caracteres.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Configurando a propriedade como obrigatória  

No exemplo a seguir, a propriedade de nome é necessária. Se você não especificar o nome, você receberá uma exceção de DbEntityValidationException. Se o Code First cria um banco de dados por meio desse modelo, em seguida, a coluna usada para armazenar essa propriedade geralmente será não anuláveis.  

> [!NOTE]
> Em alguns casos, pode não ser possível para a coluna no banco de dados a ser não anuláveis, mesmo que a propriedade é necessária. Por exemplo, quando usar uma data de estratégia de herança TPH para vários tipos é armazenado em uma única tabela. Se um tipo derivado inclui uma propriedade necessária que a coluna não pode se tornar não anulável, pois nem todos os tipos na hierarquia terá essa propriedade.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Configurando um índice em uma ou mais propriedades  

> [!NOTE]
> **EF6.1 em diante, somente** -atributo o índice foi introduzido no Entity Framework 6.1. Se você estiver usando uma versão anterior, as informações nesta seção não é aplicável.  

Criação de índices não é compatível nativamente com a API Fluent, mas você pode fazer uso do suporte para **IndexAttribute** por meio da API Fluent. Atributos de índice são processados com a inclusão de uma anotação de modelo no modelo que, em seguida, é transformada em um índice no banco de dados posteriormente no pipeline. Você pode adicionar manualmente esses mesmos anotações usando a API do Fluent.  

A maneira mais fácil de fazer isso é criar uma instância do **IndexAttribute** que contém todas as configurações para o novo índice. Em seguida, você pode criar uma instância do **IndexAnnotation** que é um tipo específico de EF que converterá o **IndexAttribute** configurações em uma anotação de modelo que podem ser armazenados no modelo de EF. Elas podem ser passadas para o **HasColumnAnnotation** método na API Fluent, especificando o nome **índice** da anotação.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Para obter uma lista completa das configurações disponíveis no **IndexAttribute**, consulte o *índice* seção [anotações de dados Code First](~/ef6/modeling/code-first/data-annotations.md). Isso inclui o nome do índice de personalização, Criando índices exclusivos e criar índices com múltiplas colunas.  

Você pode especificar várias anotações de índice em uma única propriedade, passando uma matriz de **IndexAttribute** para o construtor da **IndexAnnotation**.  

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

O exemplo a seguir mostra como especificar uma propriedade em um tipo CLR não está mapeada para uma coluna no banco de dados.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Uma propriedade de CLR de mapeamento para uma coluna específica no banco de dados  

O exemplo a seguir mapeia a propriedade CLR de nome para a coluna de banco de dados DepartmentName.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Renomear uma chave estrangeira que não está definida no modelo  

Se você optar por não definir uma chave estrangeira em um tipo CLR, mas para especificar qual nome deve ter no banco de dados, faça o seguinte:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Configurando se uma propriedade de cadeia de caracteres dá suporte a Unicode conteúdo  

Por padrão, cadeias de caracteres são Unicode (nvarchar no SQL Server). Você pode usar o método IsUnicode para especificar que uma cadeia de caracteres deve ser do tipo varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Configurando o tipo de dados de uma coluna de banco de dados  

O [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) método habilita o mapeamento para representações diferentes do mesmo tipo básico. Usando esse método não permite que você executar qualquer conversão dos dados em tempo de execução. Observe que o IsUnicode é a maneira preferencial de colunas de configuração para varchar, já que ele é independente de banco de dados.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Configurando as propriedades em um tipo complexo  

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

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Configurando uma propriedade a ser usado como um Token de simultaneidade otimista  

Para especificar que uma propriedade em uma entidade representa um token de simultaneidade, você pode usar o atributo ConcurrencyCheck ou o método IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Você também pode usar o método IsRowVersion para configurar a propriedade para ser uma versão de linha no banco de dados. Definindo a propriedade seja que uma versão de linha automaticamente configura para ser um token de simultaneidade otimista.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapeamento de tipo  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Especifica que uma classe é um tipo complexo  

Por convenção, um tipo que não tem nenhuma chave primária especificada é tratado como um tipo complexo. Existem alguns cenários onde Code First não detectará um tipo complexo (por exemplo, se você tem uma propriedade chamada ID, mas você não quer dizer para que ele seja uma chave primária). Nesses casos, você usaria a API fluente para especificar explicitamente um tipo é um tipo complexo.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Especificando para não mapear um tipo de entidade do CLR para uma tabela no banco de dados  

O exemplo a seguir mostra como excluir um tipo CLR de que é mapeado para uma tabela no banco de dados.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapeando um tipo de entidade para uma tabela específica no banco de dados  

Todas as propriedades de departamento serão ser mapeadas para colunas em uma tabela chamada t_ departamento.  

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

No cenário de mapeamento TPH, todos os tipos em uma hierarquia de herança são mapeados para uma única tabela. Uma coluna de discriminador é usada para identificar o tipo de cada linha. Ao criar seu modelo com o Code First, TPH é a estratégia padrão para os tipos que participam na hierarquia de herança. Por padrão, a coluna de discriminador é adicionada à tabela com o nome "Discriminadora" e o nome do tipo CLR de cada tipo de hierarquia é usado para os valores de discriminador. Você pode modificar o comportamento padrão usando a API fluente.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapeando a herança de tabela por tipo (TPT)  

No cenário de mapeamento TPT, todos os tipos são mapeados para tabelas individuais. Propriedades que pertencem apenas a um tipo base ou um tipo derivado são armazenadas em uma tabela que mapeia para esse tipo. As tabelas que são mapeados para tipos derivados também armazenam uma chave estrangeira que une a tabela derivada com a tabela base.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapeando a herança de tabela-por classe concreta (TPC)  

No cenário de mapeamento TPC, todos os tipos não abstratos na hierarquia são mapeados para tabelas individuais. As tabelas que são mapeados para as classes derivadas não têm nenhuma relação à tabela que mapeia para a classe base no banco de dados. Todas as propriedades de uma classe, incluindo propriedades herdadas, são mapeadas para colunas da tabela correspondente.  

Chame o método MapInheritedProperties para configurar cada tipo derivado. MapInheritedProperties remapeia todas as propriedades que foram herdadas da classe base para novas colunas na tabela para a classe derivada.  

> [!NOTE]
> Observe que como as tabelas que participam na hierarquia de herança TPC não compartilham uma chave primária existe será chaves de entidade duplicados ao inserir em tabelas que são mapeadas para subclasses se você tiver valores de banco de dados gerado com a mesma semente de identidade. Para resolver esse problema, você pode especificar um valor de semente inicial diferente para cada tabela ou desativar a identidade na propriedade de chave primária. Identidade é o valor padrão para propriedades de chave de inteiro ao trabalhar com o Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapeamento de propriedades de um tipo de entidade para várias tabelas no banco de dados (separação da entidade)  

Separação da entidade permite que as propriedades de um tipo de entidade sejam distribuídas em várias tabelas. No exemplo a seguir, a entidade Department é dividida em duas tabelas: DepartmentDetails e departamento. Separação da entidade usa várias chamadas para o método Map para mapear um subconjunto de propriedades para uma tabela específica.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Vários tipos de entidade de mapeamento para uma tabela no banco de dados (divisão de tabela)  

O exemplo a seguir mapeia os dois tipos de entidade que compartilham uma chave primária para uma tabela.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Um tipo de entidade de mapeamento para procedimentos armazenados de Insert/Update/Delete (EF6 em diante)  

Começando com o EF6, você pode mapear uma entidade para usar procedimentos armazenados para inserir atualizar e excluir. Para obter mais detalhes, veja [código primeiro Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
