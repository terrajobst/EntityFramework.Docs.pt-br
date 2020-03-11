---
title: Relações, propriedades de navegação e chaves estrangeiras-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 892e872e3cb11ea95084cf6d9ab43c8d500bc0de
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419349"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relações, propriedades de navegação e chaves estrangeiras

Este artigo fornece uma visão geral de como o Entity Framework gerencia relações entre entidades. Ele também fornece algumas diretrizes sobre como mapear e manipular relações.

## <a name="relationships-in-ef"></a>Relações no EF

Em bancos de dados relacionais, as relações (também chamadas de associações) entre tabelas são definidas por meio de chaves estrangeiras. Uma chave estrangeira (FK) é uma coluna ou combinação de colunas que é usada para estabelecer e impor um link entre os dados em duas tabelas. Em geral, há três tipos de relações: um-para-um, um-para-muitos e muitos para muitos. Em uma relação um-para-muitos, a chave estrangeira é definida na tabela que representa a extremidade de muitos da relação. A relação muitos para muitos envolve a definição de uma terceira tabela (chamada de tabela de junção ou junção), cuja chave primária é composta pelas chaves estrangeiras de ambas as tabelas relacionadas. Em uma relação um-para-um, a chave primária funciona além de uma chave estrangeira e não há nenhuma coluna de chave estrangeira separada para qualquer tabela.

A imagem a seguir mostra duas tabelas que participam de uma relação um-para-muitos. A tabela do **curso** é a tabela dependente porque contém a coluna **DepartmentID** que a vincula à tabela **Department** .

![Tabelas de departamento e curso](~/ef6/media/database2.png)

No Entity Framework, uma entidade pode estar relacionada a outras entidades por meio de uma associação ou relação. Cada relação contém duas extremidades que descrevem o tipo de entidade e a multiplicidade do tipo (um, zero-ou-um ou muitos) para as duas entidades nessa relação. A relação pode ser regida por uma restrição referencial, que descreve qual extremidade na relação é uma função principal e qual é uma função dependente.

As propriedades de navegação fornecem uma maneira de navegar por uma associação entre dois tipos de entidade. Cada objeto pode ter uma propriedade de navegação para cada relação na qual ele participa. As propriedades de navegação permitem que você navegue e gerencie relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for uma ou zero ou uma) ou uma coleção (se a multiplicidade for many). Você também pode optar por uma navegação unidirecional, caso em que você define a propriedade de navegação em apenas um dos tipos que participam da relação e não em ambos.

É recomendável incluir propriedades no modelo que são mapeadas para chaves estrangeiras no banco de dados. Com as propriedades de chave estrangeira incluídas, você pode criar ou alterar uma relação modificando o valor de chave estrangeira em um objeto dependente. Esse tipo de associação é chamado de associação de chave estrangeira. O uso de chaves estrangeiras é ainda mais essencial ao trabalhar com entidades desconectadas. Observe que, ao trabalhar com 1-para-1 ou 1 para 0.. 1 relações, não há nenhuma coluna de chave estrangeira separada, a propriedade de chave primária atua como a chave estrangeira e é sempre incluída no modelo.

Quando as colunas de chave estrangeira não são incluídas no modelo, as informações de associação são gerenciadas como um objeto independente. As relações são controladas por meio de referências de objeto em vez de propriedades de chave estrangeira. Esse tipo de associação é chamado de *associação independente*. A maneira mais comum de modificar uma *associação independente* é modificar as propriedades de navegação geradas para cada entidade que participa da associação.

Você pode optar por usar um ou ambos os tipos de associações em seu modelo. No entanto, se você tiver uma relação de muitos para muitos, que é conectada por uma tabela de junção que contém apenas chaves estrangeiras, o EF usará uma associação independente para gerenciar essa relação muitos para muitos.   

A imagem a seguir mostra um modelo conceitual que foi criado com o Entity Framework Designer. O modelo contém duas entidades que participam de uma relação um-para-muitos. Ambas as entidades têm propriedades de navegação. O **curso** é a entidade dependente e tem a propriedade de chave estrangeira **DepartmentID** definida.

![Tabelas de departamento e curso com propriedades de navegação](~/ef6/media/relationshipefdesigner.png)

O trecho de código a seguir mostra o mesmo modelo que foi criado com Code First.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Configurando ou mapeando relações

O restante desta página aborda como acessar e manipular dados usando relações. Para obter informações sobre como configurar relações em seu modelo, consulte as páginas a seguir.

-   Para configurar relações no Code First, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).
-   Para configurar relações usando o Entity Framework Designer, consulte [relações com o designer do EF](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Criando e modificando relações

Em uma *Associação de chave estrangeira*, quando você altera a relação, o estado de um objeto dependente com um `EntityState.Unchanged` estado é alterado para `EntityState.Modified`. Em uma relação independente, a alteração da relação não atualiza o estado do objeto dependente.

Os exemplos a seguir mostram como usar as propriedades de chave estrangeira e propriedades de navegação para associar os objetos relacionados. Com as associações de chave estrangeira, você pode usar qualquer um dos métodos para alterar, criar ou modificar relações. Com associações independentes, você não pode usar a propriedade Foreign Key.

- Atribuindo um novo valor a uma propriedade de chave estrangeira, como no exemplo a seguir.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- O código a seguir remove uma relação definindo a chave estrangeira como **NULL**. Observe que a propriedade Foreign Key deve ser anulável.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Se a referência estiver no estado adicionado (neste exemplo, o objeto curso), a propriedade de navegação de referência não será sincronizada com os valores de chave de um novo objeto até que SaveChanges seja chamado. A sincronização não ocorre porque o contexto do objeto não contém chaves permanentes para objetos adicionados até que eles sejam salvos. Se você precisar ter novos objetos totalmente sincronizados assim que definir a relação, use um dos métodos a seguir. *

- Atribuindo um novo objeto a uma propriedade de navegação. O código a seguir cria uma relação entre um curso e um `department`. Se os objetos forem anexados ao contexto, o `course` também será adicionado à coleção de `department.Courses` e a propriedade de chave estrangeira correspondente no objeto `course` será definida como o valor da propriedade de chave do departamento.  
  ``` csharp
  course.Department = department;
  ```

- Para excluir a relação, defina a propriedade de navegação como `null`. Se você estiver trabalhando com Entity Framework com base no .NET 4,0, a extremidade relacionada precisará ser carregada antes de você defini-la como NULL. Por exemplo:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  A partir do Entity Framework 5,0, que se baseia no .NET 4,5, você pode definir a relação como NULL sem carregar a extremidade relacionada. Você também pode definir o valor atual como nulo usando o método a seguir.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Excluindo ou adicionando um objeto em uma coleção de entidades. Por exemplo, você pode adicionar um objeto do tipo `Course` à coleção de `department.Courses`. Esta operação cria uma relação entre um **curso** específico e um `department`específico. Se os objetos forem anexados ao contexto, a referência de departamento e a propriedade Foreign Key no objeto **curso** serão definidas para o `department`apropriado.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Usando o método `ChangeRelationshipState` para alterar o estado da relação especificada entre dois objetos de entidade. Esse método é usado com mais frequência ao trabalhar com aplicativos de N camadas e uma *associação independente* (não pode ser usado com uma associação de chave estrangeira). Além disso, para usar esse método, você deve fazer a lista suspensa para `ObjectContext`, conforme mostrado no exemplo abaixo.  
No exemplo a seguir, há uma relação muitos-para-muitos entre instrutores e cursos. Chamar o método `ChangeRelationshipState` e passar o parâmetro `EntityState.Added`, permite que o `SchoolContext` saiba que uma relação foi adicionada entre os dois objetos:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Observe que, se você estiver atualizando (não apenas adicionando) uma relação, deverá excluir a relação antiga depois de adicionar a nova:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Sincronizando as alterações entre as chaves estrangeiras e as propriedades de navegação

Quando você altera a relação dos objetos anexados ao contexto usando um dos métodos descritos acima, Entity Framework precisa manter chaves estrangeiras, referências e coleções em sincronia. Entity Framework gerencia automaticamente essa sincronização (também conhecida como correção de relação) para as entidades POCO com proxies. Para obter mais informações, consulte [trabalhando com proxies](~/ef6/fundamentals/proxies.md).

Se você estiver usando entidades POCO sem proxies, deverá certificar-se de que o método **DetectChanges** seja chamado para sincronizar os objetos relacionados no contexto. Observe que as APIs a seguir disparam automaticamente uma chamada **DetectChanges** .

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   Executando uma consulta LINQ em um `DbSet`

## <a name="loading-related-objects"></a>Carregando objetos relacionados

Em Entity Framework você geralmente usa propriedades de navegação para carregar entidades relacionadas à entidade retornada pela associação definida. Para obter mais informações, consulte [carregando objetos relacionados](~/ef6/querying/related-data.md).

> [!NOTE]
> Em uma associação de chave estrangeira, quando você carrega uma extremidade relacionada de um objeto dependente, o objeto relacionado será carregado com base no valor de chave estrangeira do dependente que está atualmente na memória:

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

Em uma associação independente, a extremidade relacionada de um objeto dependente é consultada com base no valor de chave estrangeira que está atualmente no banco de dados. No entanto, se a relação tiver sido modificada e a propriedade de referência no objeto dependente apontar para um objeto principal diferente que é carregado no contexto do objeto, Entity Framework tentará criar uma relação como ela está definida no cliente.

## <a name="managing-concurrency"></a>Gerenciando simultaneidade

Na chave estrangeira e nas associações independentes, as verificações de simultaneidade se baseiam nas chaves de entidade e outras propriedades de entidade que são definidas no modelo. Ao usar o designer do EF para criar um modelo, defina o atributo `ConcurrencyMode` como **fixo** para especificar que a propriedade deve ser verificada quanto à simultaneidade. Ao usar Code First para definir um modelo, use a anotação `ConcurrencyCheck` nas propriedades que você deseja que sejam verificadas quanto à simultaneidade. Ao trabalhar com Code First você também pode usar a anotação `TimeStamp` para especificar que a propriedade deve ser verificada quanto à simultaneidade. Você pode ter apenas uma propriedade Timestamp em uma determinada classe. Code First mapeia essa propriedade para um campo não anulável no banco de dados.

É recomendável que você sempre use a associação de chave estrangeira ao trabalhar com entidades que participam da verificação e resolução de simultaneidade.

Para obter mais informações, consulte [lidando com conflitos de simultaneidade](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Trabalhando com chaves sobrepostas

As chaves que se sobrepõem são chaves compostas em que algumas propriedades na chave também fazem parte de outra chave na entidade. Você não pode ter uma chave sobreposta em uma associação independente. Para alterar uma associação de chave estrangeira que inclui chaves sobrepostas, recomendamos que você modifique os valores de chave estrangeira em vez de usar as referências de objeto.
