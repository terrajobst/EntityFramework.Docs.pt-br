---
title: Convenções de Code First - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119721"
---
# <a name="code-first-conventions"></a>Convenções de Code First
Código primeiro permite que você descrevem um modelo usando classes c# ou Visual Basic .NET. A forma básica do modelo é detectada por meio de convenções. Convenções são conjuntos de regras que são usados para configurar automaticamente um modelo conceitual com base em definições de classe ao trabalhar com o Code First. As convenções são definidas no namespace System.Data.Entity.ModelConfiguration.Conventions.  

Além disso, você pode configurar seu modelo usando anotações de dados ou a API fluente. É dada precedência à configuração por meio da API fluente, seguida por convenções e, em seguida, as anotações de dados. Para obter mais informações, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relações](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - tipos de & propriedades](~/ef6/modeling/code-first/fluent/types-and-properties.md) e [API Fluent com VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Uma lista detalhada das convenções Code First está disponível na [documentação da API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Este tópico fornece uma visão geral das convenções usadas pelo Code First.  

## <a name="type-discovery"></a>Descoberta do tipo  

Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio). Além de definir as classes, você também precisará permitem **DbContext** saber quais tipos que você deseja incluir no modelo. Para fazer isso, você define uma classe de contexto deriva **DbContext** e expõe **DbSet** propriedades para os tipos que você deseja fazer parte do modelo. Código pela primeira vez, incluirá esses tipos e também irá receber quaisquer tipos referenciados, mesmo se os tipos referenciados são definidos em um assembly diferente.  

Se seus tipos de participarem de uma hierarquia de herança, é suficiente para definir um **DbSet** propriedade para a classe base e os tipos derivados será incluída automaticamente, se eles estiverem no mesmo assembly como a classe base.  

No exemplo a seguir, há apenas um **DbSet** propriedade definida na **SchoolEntities** classe (**departamentos**). Primeiro, o código usa essa propriedade para descobrir e efetuar pull em quaisquer tipos referenciados.  

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

Se você quiser excluir um tipo de modelo, use o **NotMapped** atributo ou o **DbModelBuilder.Ignore** API fluente.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Convenção de chave primária  

Código primeiro infere que uma propriedade é uma chave primária, se uma propriedade em uma classe é chamada de "ID" (não diferencia maiusculas de minúsculas) ou o nome da classe seguido de "ID". Se o tipo de propriedade de chave primária for numérico ou GUID será configurado como uma coluna de identidade.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Convenção de relação  

No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade. Cada objeto pode ter uma propriedade de navegação para cada relação da qual ele participa. Propriedades de navegação permitem navegar e gerenciar relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for deles, zero ou um ou mais) ou uma coleção (se a multiplicidade de muitos). Código primeiro infere baseadas nas propriedades de navegação definidas em seus tipos de relações.  

Além das propriedades de navegação, recomendamos que você inclua propriedades de chave estrangeira sobre os tipos que representam os objetos dependentes. Qualquer propriedade com o mesmo tipo de dados como a principal propriedade de chave primária e com um nome que segue um dos formatos a seguir representa uma chave estrangeira da relação: '\<nome da propriedade de navegação\>\<principal nome de propriedade de chave primária\>','\<nome da classe principal\>\<nome da propriedade de chave primária\>', ou '\<nome da entidade de propriedade de chave primária\>'. Se várias correspondências forem encontradas, em seguida, é dada precedência na ordem listada acima. Detecção de chave estrangeira não diferencia maiusculas de minúsculas. Quando uma propriedade de chave estrangeira for detectada, o Code First infere a multiplicidade da relação com base na possibilidade de nulidade da chave estrangeira. Se a propriedade for nula, em seguida, a relação é registrada como opcionais. Caso contrário, a relação é registrada conforme necessário.  

Se uma chave estrangeira na entidade dependente não permite valor nulo, em seguida, Code First define exclusão em cascata na relação. Se uma chave estrangeira na entidade dependente é anulável, Code First não define a exclusão em cascata na relação e quando a entidade é excluída a chave estrangeira será definida como null. A multiplicidade e cascade excluir comportamento detectado pela convenção pode ser substituída usando a API fluente.  

No exemplo a seguir, as propriedades de navegação e uma chave estrangeira são usados para definir a relação entre as classes de departamento e curso.  

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
> Se você tiver várias relações entre os mesmos tipos (por exemplo, suponha que você defina a **pessoa** e **livro** classes, em que o **pessoa** classe contém a  **ReviewedBooks** e **AuthoredBooks** propriedades de navegação e o **livro** classe contém o **autor** e  **Revisor** propriedades de navegação) você precisa configurar manualmente as relações usando Data Annotations ou a API fluente. Para obter mais informações, consulte [anotações de dados - relações](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent - relações](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Convenção de tipos complexos  

Quando o Code First detecta uma definição de classe em que uma chave primária não pode ser inferida, e nenhuma chave primária é registrada por meio de anotações de dados ou a API fluente, o tipo é registrado automaticamente como um tipo complexo. Detecção de tipo complexo também requer que o tipo não tem propriedades que fazem referência a tipos de entidade e não são referenciadas em uma propriedade de coleção em outro tipo. Considerando as seguintes definições de classe Code First seria inferir que **detalhes** é um tipo complexo, porque ele não tem nenhuma chave primária.  

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

## <a name="connection-string-convention"></a>Convenção de cadeia de caracteres de Conexão  

Para saber mais sobre as convenções que usa o DbContext para descobrir a conexão usar, consulte [conexões e modelos](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Removendo as convenções  

Você pode remover qualquer uma das convenções definidas no namespace System.Data.Entity.ModelConfiguration.Conventions. O exemplo a seguir remove **PluralizingTableNameConvention**.  

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

Convenções personalizadas têm suporte no EF6 em diante. Para obter mais informações, consulte [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).
