---
title: Convenções de Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419286"
---
# <a name="code-first-conventions"></a>Convenções de Code First
Code First permite descrever um modelo usando C# ou Visual Basic classes .net. A forma básica do modelo é detectada usando convenções. As convenções são conjuntos de regras que são usadas para configurar automaticamente um modelo conceitual com base em definições de classe ao trabalhar com Code First. As convenções são definidas no namespace System. Data. Entity. ModelConfiguration. Conventions.  

Você pode configurar ainda mais seu modelo usando as anotações de dados ou a API Fluent. A precedência é dada à configuração por meio da API fluente seguida pelas anotações de dados e pelas convenções. Para obter mais informações, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [relações de API fluentes](~/ef6/modeling/code-first/fluent/relationships.md), [tipos de API fluentes & propriedades](~/ef6/modeling/code-first/fluent/types-and-properties.md) e [API fluente com vb.net](~/ef6/modeling/code-first/fluent/vb.md).  

Uma lista detalhada de convenções de Code First está disponível na [documentação da API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Este tópico fornece uma visão geral das convenções usadas pelo Code First.  

## <a name="type-discovery"></a>Descoberta de tipo  

Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio). Além de definir as classes, você também precisa permitir que o **DbContext** saiba quais tipos você deseja incluir no modelo. Para fazer isso, você define uma classe de contexto que deriva de **DbContext** e expõe propriedades **DbSet** para os tipos que você deseja que façam parte do modelo. Code First incluirá esses tipos e também fará o pull de qualquer tipo referenciado, mesmo que os tipos referenciados sejam definidos em um assembly diferente.  

Se seus tipos participarem de uma hierarquia de herança, será suficiente definir uma propriedade **DbSet** para a classe base e os tipos derivados serão incluídos automaticamente, se estiverem no mesmo assembly que a classe base.  

No exemplo a seguir, há apenas uma propriedade **DbSet** definida na classe **SchoolEntities** (**departamentos**). Code First usa essa propriedade para descobrir e efetuar pull de qualquer tipo referenciado.  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

Se você quiser excluir um tipo do modelo, use o atributo não **mapeado** ou a API Fluent **DbModelBuilder. ignore** .  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Convenção de chave primária  

Code First infere que uma propriedade é uma chave primária se uma propriedade em uma classe for denominada "ID" (não diferencia maiúsculas de minúsculas) ou o nome da classe seguido por "ID". Se o tipo da propriedade de chave primária for numeric ou GUID, ela será configurada como uma coluna de identidade.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Convenção de relacionamento  

Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade. Cada objeto pode ter uma propriedade de navegação para cada relação na qual ele participa. As propriedades de navegação permitem que você navegue e gerencie relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for uma ou zero ou uma) ou uma coleção (se a multiplicidade for many). Code First infere relações com base nas propriedades de navegação definidas em seus tipos.  

Além das propriedades de navegação, recomendamos que você inclua as propriedades de chave estrangeira nos tipos que representam objetos dependentes. Qualquer propriedade com o mesmo tipo de dados que a propriedade Primary Key principal e com um nome que segue um dos seguintes formatos representa uma chave estrangeira para a relação: '\<nome da propriedade de navegação\>\<nome da propriedade da chave primária principal\>', '\<nome da classe principal\>\<nome da propriedade da chave primária\>' ou '\<nome da propriedade da chave primária da entidade\> Se várias correspondências forem encontradas, a precedência será fornecida na ordem listada acima. A detecção de chave estrangeira não diferencia maiúsculas de minúsculas. Quando uma propriedade de chave estrangeira é detectada, o Code First infere a multiplicidade da relação com base na nulidade da chave estrangeira. Se a propriedade for anulável, a relação será registrada como opcional; caso contrário, a relação será registrada conforme necessário.  

Se uma chave estrangeira na entidade dependente não for anulável, Code First definirá a exclusão em cascata na relação. Se uma chave estrangeira na entidade dependente for anulável, Code First não definirá a exclusão em cascata na relação e, quando a entidade de segurança for excluída, a chave estrangeira será definida como NULL. O comportamento de exclusão em cascata e multiplicidade detectado por convenção pode ser substituído usando a API Fluent.  

No exemplo a seguir, as propriedades de navegação e uma chave estrangeira são usadas para definir a relação entre as classes Department e Course.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> Se você tiver várias relações entre os mesmos tipos (por exemplo, suponha que você defina as classes **Person** e **Book** , em que a classe **Person** contém as propriedades de navegação **ReviewedBooks** e **AuthoredBooks** e a classe **Book** contiver as propriedades de navegação **Author** e **revisor** ), você precisará configurar manualmente as relações usando as anotações de dados ou a API fluente. Para obter mais informações, consulte [Data Annotations-Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Convenção de tipos complexos  

Quando Code First descobre uma definição de classe em que uma chave primária não pode ser inferida e nenhuma chave primária é registrada por meio de anotações de dados ou da API fluente, o tipo é registrado automaticamente como um tipo complexo. A detecção de tipo complexo também requer que o tipo não tenha propriedades que referenciem tipos de entidade e não seja referenciada de uma propriedade de coleção em outro tipo. Considerando as seguintes definições de classe Code First inferiria que os **detalhes** são um tipo complexo porque não tem chave primária.  

``` csharp
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
```  

## <a name="connection-string-convention"></a>Convenção de cadeia de conexão  

Para saber mais sobre as convenções que o DbContext usa para descobrir a conexão a ser usada [, consulte conexões e modelos](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Removendo convenções  

Você pode remover qualquer uma das convenções definidas no namespace System. Data. Entity. ModelConfiguration. Conventions. O exemplo a seguir remove **PluralizingTableNameConvention**.  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a>Convenções personalizadas  

As convenções personalizadas têm suporte no EF6 em diante. Para obter mais informações, consulte [convenções de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).
