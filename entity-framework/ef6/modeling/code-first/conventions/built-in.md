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
# <a name="code-first-conventions"></a><span data-ttu-id="cb31b-102">Convenções de Code First</span><span class="sxs-lookup"><span data-stu-id="cb31b-102">Code First Conventions</span></span>
<span data-ttu-id="cb31b-103">Code First permite descrever um modelo usando C# ou Visual Basic classes .net.</span><span class="sxs-lookup"><span data-stu-id="cb31b-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="cb31b-104">A forma básica do modelo é detectada usando convenções.</span><span class="sxs-lookup"><span data-stu-id="cb31b-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="cb31b-105">As convenções são conjuntos de regras que são usadas para configurar automaticamente um modelo conceitual com base em definições de classe ao trabalhar com Code First.</span><span class="sxs-lookup"><span data-stu-id="cb31b-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="cb31b-106">As convenções são definidas no namespace System. Data. Entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="cb31b-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="cb31b-107">Você pode configurar ainda mais seu modelo usando as anotações de dados ou a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cb31b-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="cb31b-108">A precedência é dada à configuração por meio da API fluente seguida pelas anotações de dados e pelas convenções.</span><span class="sxs-lookup"><span data-stu-id="cb31b-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="cb31b-109">Para obter mais informações, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [relações de API fluentes](~/ef6/modeling/code-first/fluent/relationships.md), [tipos de API fluentes & propriedades](~/ef6/modeling/code-first/fluent/types-and-properties.md) e [API fluente com vb.net](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="cb31b-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="cb31b-110">Uma lista detalhada de convenções de Code First está disponível na [documentação da API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="cb31b-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="cb31b-111">Este tópico fornece uma visão geral das convenções usadas pelo Code First.</span><span class="sxs-lookup"><span data-stu-id="cb31b-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="cb31b-112">Descoberta de tipo</span><span class="sxs-lookup"><span data-stu-id="cb31b-112">Type Discovery</span></span>  

<span data-ttu-id="cb31b-113">Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).</span><span class="sxs-lookup"><span data-stu-id="cb31b-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="cb31b-114">Além de definir as classes, você também precisa permitir que o **DbContext** saiba quais tipos você deseja incluir no modelo.</span><span class="sxs-lookup"><span data-stu-id="cb31b-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="cb31b-115">Para fazer isso, você define uma classe de contexto que deriva de **DbContext** e expõe propriedades **DbSet** para os tipos que você deseja que façam parte do modelo.</span><span class="sxs-lookup"><span data-stu-id="cb31b-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="cb31b-116">Code First incluirá esses tipos e também fará o pull de qualquer tipo referenciado, mesmo que os tipos referenciados sejam definidos em um assembly diferente.</span><span class="sxs-lookup"><span data-stu-id="cb31b-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="cb31b-117">Se seus tipos participarem de uma hierarquia de herança, será suficiente definir uma propriedade **DbSet** para a classe base e os tipos derivados serão incluídos automaticamente, se estiverem no mesmo assembly que a classe base.</span><span class="sxs-lookup"><span data-stu-id="cb31b-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="cb31b-118">No exemplo a seguir, há apenas uma propriedade **DbSet** definida na classe **SchoolEntities** (**departamentos**).</span><span class="sxs-lookup"><span data-stu-id="cb31b-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="cb31b-119">Code First usa essa propriedade para descobrir e efetuar pull de qualquer tipo referenciado.</span><span class="sxs-lookup"><span data-stu-id="cb31b-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="cb31b-120">Se você quiser excluir um tipo do modelo, use o atributo não **mapeado** ou a API Fluent **DbModelBuilder. ignore** .</span><span class="sxs-lookup"><span data-stu-id="cb31b-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="cb31b-121">Convenção de chave primária</span><span class="sxs-lookup"><span data-stu-id="cb31b-121">Primary Key Convention</span></span>  

<span data-ttu-id="cb31b-122">Code First infere que uma propriedade é uma chave primária se uma propriedade em uma classe for denominada "ID" (não diferencia maiúsculas de minúsculas) ou o nome da classe seguido por "ID".</span><span class="sxs-lookup"><span data-stu-id="cb31b-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="cb31b-123">Se o tipo da propriedade de chave primária for numeric ou GUID, ela será configurada como uma coluna de identidade.</span><span class="sxs-lookup"><span data-stu-id="cb31b-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="cb31b-124">Convenção de relacionamento</span><span class="sxs-lookup"><span data-stu-id="cb31b-124">Relationship Convention</span></span>  

<span data-ttu-id="cb31b-125">Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="cb31b-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="cb31b-126">Cada objeto pode ter uma propriedade de navegação para cada relação na qual ele participa.</span><span class="sxs-lookup"><span data-stu-id="cb31b-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="cb31b-127">As propriedades de navegação permitem que você navegue e gerencie relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for uma ou zero ou uma) ou uma coleção (se a multiplicidade for many).</span><span class="sxs-lookup"><span data-stu-id="cb31b-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="cb31b-128">Code First infere relações com base nas propriedades de navegação definidas em seus tipos.</span><span class="sxs-lookup"><span data-stu-id="cb31b-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="cb31b-129">Além das propriedades de navegação, recomendamos que você inclua as propriedades de chave estrangeira nos tipos que representam objetos dependentes.</span><span class="sxs-lookup"><span data-stu-id="cb31b-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="cb31b-130">Qualquer propriedade com o mesmo tipo de dados que a propriedade Primary Key principal e com um nome que segue um dos seguintes formatos representa uma chave estrangeira para a relação: '\<nome da propriedade de navegação\>\<nome da propriedade da chave primária principal\>', '\<nome da classe principal\>\<nome da propriedade da chave primária\>' ou '\<nome da propriedade da chave primária da entidade\></span><span class="sxs-lookup"><span data-stu-id="cb31b-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="cb31b-131">Se várias correspondências forem encontradas, a precedência será fornecida na ordem listada acima.</span><span class="sxs-lookup"><span data-stu-id="cb31b-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="cb31b-132">A detecção de chave estrangeira não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="cb31b-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="cb31b-133">Quando uma propriedade de chave estrangeira é detectada, o Code First infere a multiplicidade da relação com base na nulidade da chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="cb31b-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="cb31b-134">Se a propriedade for anulável, a relação será registrada como opcional; caso contrário, a relação será registrada conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="cb31b-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="cb31b-135">Se uma chave estrangeira na entidade dependente não for anulável, Code First definirá a exclusão em cascata na relação.</span><span class="sxs-lookup"><span data-stu-id="cb31b-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="cb31b-136">Se uma chave estrangeira na entidade dependente for anulável, Code First não definirá a exclusão em cascata na relação e, quando a entidade de segurança for excluída, a chave estrangeira será definida como NULL.</span><span class="sxs-lookup"><span data-stu-id="cb31b-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="cb31b-137">O comportamento de exclusão em cascata e multiplicidade detectado por convenção pode ser substituído usando a API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cb31b-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="cb31b-138">No exemplo a seguir, as propriedades de navegação e uma chave estrangeira são usadas para definir a relação entre as classes Department e Course.</span><span class="sxs-lookup"><span data-stu-id="cb31b-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="cb31b-139">Se você tiver várias relações entre os mesmos tipos (por exemplo, suponha que você defina as classes **Person** e **Book** , em que a classe **Person** contém as propriedades de navegação **ReviewedBooks** e **AuthoredBooks** e a classe **Book** contiver as propriedades de navegação **Author** e **revisor** ), você precisará configurar manualmente as relações usando as anotações de dados ou a API fluente.</span><span class="sxs-lookup"><span data-stu-id="cb31b-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="cb31b-140">Para obter mais informações, consulte [Data Annotations-Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API-Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="cb31b-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="cb31b-141">Convenção de tipos complexos</span><span class="sxs-lookup"><span data-stu-id="cb31b-141">Complex Types Convention</span></span>  

<span data-ttu-id="cb31b-142">Quando Code First descobre uma definição de classe em que uma chave primária não pode ser inferida e nenhuma chave primária é registrada por meio de anotações de dados ou da API fluente, o tipo é registrado automaticamente como um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="cb31b-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="cb31b-143">A detecção de tipo complexo também requer que o tipo não tenha propriedades que referenciem tipos de entidade e não seja referenciada de uma propriedade de coleção em outro tipo.</span><span class="sxs-lookup"><span data-stu-id="cb31b-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="cb31b-144">Considerando as seguintes definições de classe Code First inferiria que os **detalhes** são um tipo complexo porque não tem chave primária.</span><span class="sxs-lookup"><span data-stu-id="cb31b-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="cb31b-145">Convenção de cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="cb31b-145">Connection String Convention</span></span>  

<span data-ttu-id="cb31b-146">Para saber mais sobre as convenções que o DbContext usa para descobrir a conexão a ser usada [, consulte conexões e modelos](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="cb31b-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="cb31b-147">Removendo convenções</span><span class="sxs-lookup"><span data-stu-id="cb31b-147">Removing Conventions</span></span>  

<span data-ttu-id="cb31b-148">Você pode remover qualquer uma das convenções definidas no namespace System. Data. Entity. ModelConfiguration. Conventions.</span><span class="sxs-lookup"><span data-stu-id="cb31b-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="cb31b-149">O exemplo a seguir remove **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="cb31b-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="cb31b-150">Convenções personalizadas</span><span class="sxs-lookup"><span data-stu-id="cb31b-150">Custom Conventions</span></span>  

<span data-ttu-id="cb31b-151">As convenções personalizadas têm suporte no EF6 em diante.</span><span class="sxs-lookup"><span data-stu-id="cb31b-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="cb31b-152">Para obter mais informações, consulte [convenções de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="cb31b-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
