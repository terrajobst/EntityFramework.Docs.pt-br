---
title: Relações, as propriedades de navegação e chaves estrangeiras - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 46c2d11b5704ec7ae82a423ae042b87f5efe436f
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315653"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Relações, as propriedades de navegação e chaves estrangeiras
Este tópico fornece uma visão geral de como o Entity Framework gerencia os relacionamentos entre entidades. Ele também fornece algumas diretrizes sobre como mapear e manipular as relações.

## <a name="relationships-in-ef"></a>Relações no EF

Em bancos de dados relacionais (também chamadas de associações) de relações entre tabelas são definidas por meio de chaves estrangeiras. Uma chave estrangeira (FK) é uma coluna ou combinação de colunas que é usada para estabelecer e impor um link entre os dados nas duas tabelas. Normalmente, há três tipos de relações: um para um, um-para-muitos e muitos-para-muitos. Em uma relação um-para-muitos, a chave estrangeira é definida na tabela que representa o final de muitos da relação. A relação muitos-para-muitos envolve a definição de uma terceira tabela (chamada de uma tabela de junção ou de junção) cuja chave primária é composto de chaves estrangeiras de ambas as tabelas relacionadas. Em uma relação um para um, a chave primária atua Além disso, como uma chave estrangeira e não há nenhuma coluna de chave estrangeira separada para qualquer uma das tabelas.

A imagem a seguir mostra duas tabelas que participam no relacionamento um-para-muitos. O **curso** tabela é a tabela dependente, porque ela contém o **DepartmentID** coluna que vincula-o para o **departamento** tabela.

![Tabelas de departamento e do curso](~/ef6/media/database2.png)

No Entity Framework, uma entidade pode estar relacionada a outras entidades por meio de uma associação ou relação. Todo relacionamento contém duas extremidades que descrevem o tipo de entidade e a multiplicidade do tipo (um, zero-ou-um ou muitos) para as duas entidades nessa relação. A relação pode ser regida por uma restrição referencial, que descreve qual ponta da relação é uma função principal e o que é uma função dependente.

Propriedades de navegação fornecem uma maneira de navegar de uma associação entre dois tipos de entidade. Cada objeto pode ter uma propriedade de navegação para cada relação da qual ele participa. Propriedades de navegação permitem navegar e gerenciar relações em ambas as direções, retornando um objeto de referência (se a multiplicidade for deles, zero ou um ou mais) ou uma coleção (se a multiplicidade de muitos). Você também pode optar por ter unidirecional de navegação, caso em que você definir a propriedade de navegação em apenas um dos tipos que participa da relação e não em ambos.

É recomendável para incluir as propriedades no modelo que são mapeados para chaves estrangeiras no banco de dados. Com propriedades de chave estrangeira incluídas, você pode criar ou alterar uma relação, modificando o valor de chave estrangeira em um objeto dependente. Esse tipo de associação é chamado de uma associação de chave estrangeira. Usar chaves estrangeiras é ainda mais essencial ao trabalhar com entidades desconectadas. Observe que, quando trabalhar com 1 para 1 ou 1 para 0... relações 1, não há nenhuma coluna de chave estrangeira separada, a propriedade de chave primária atua como a chave estrangeira e é sempre incluída no modelo.

Quando colunas de chave estrangeira não são incluídas no modelo, as informações de associação são gerenciadas como um objeto independente. As relações são controladas por meio de referências de objeto, em vez de propriedades de chave estrangeira. Esse tipo de associação é chamado um *associação independente*. A maneira mais comum para modificar uma *associação independente* é modificar as propriedades de navegação que são geradas para cada entidade que participa da associação.

Você pode optar por usar um ou ambos os tipos de associações em seu modelo. No entanto, se você tiver uma relação muitos-para-muitos pura que está conectada por uma tabela de junção que contém apenas as chaves estrangeiras, o EF usará uma associação independente para gerenciar essa relação de muitos-para-muitos.   

A imagem a seguir mostra um modelo conceitual que foi criado com o Entity Framework Designer. O modelo contém duas entidades que participam da relação um-para-muitos. Ambas as entidades têm propriedades de navegação. **Curso** é a entidade depend e tem o **DepartmentID** propriedade de chave estrangeira definida.

![Tabelas de departamento e curso com propriedades de navegação](~/ef6/media/relationshipefdesigner.png)

O trecho de código a seguir mostra o mesmo modelo que foi criado com o Code First.

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
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Configurando ou relações de mapeamento

O restante desta página aborda como acessar e manipular dados de uso de relações. Para obter informações sobre como configurar as relações em seu modelo, consulte as páginas a seguir.

-   Para configurar as relações no Code First, consulte [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent – relações](~/ef6/modeling/code-first/fluent/relationships.md).
-   Para configurar as relações usando o Entity Framework Designer, consulte [relacionamentos com o EF Designer](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Criando e modificando relações

Em um *associação de chave estrangeira*, quando você altera a relação, o estado de um objeto dependente com um `EntityState.Unchanged` estado muda para `EntityState.Modified`. Em uma relação independente, alterar a relação não atualiza o estado do objeto dependente.

Os exemplos a seguir mostram como usar as propriedades de chave estrangeira e propriedades de navegação para associar os objetos relacionados. Com associações de chave estrangeiras, você pode usar qualquer um dos métodos para alterar, criar ou modificar relações. Com associações independentes, é possível usar a propriedade de chave estrangeira.

- Ao atribuir um novo valor a uma propriedade de chave estrangeira, como no exemplo a seguir.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- O código a seguir remove uma relação, definindo a chave estrangeira como **nulo**. Observe que a propriedade de chave estrangeira deve ser anulável.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Se a referência está no estado adicionado (no exemplo, o objeto de curso), a propriedade de navegação de referência não será sincronizada com os valores de chave de um novo objeto até que o SaveChanges é chamado. Sincronização não ocorrerá porque o contexto de objeto não contém chaves permanentes para objetos adicionados até que eles são salvos. Se você precisar ter novos objetos totalmente sincronizados, assim que você definir a relação, use um dos methods.* a seguir

- Ao atribuir um novo objeto a uma propriedade de navegação. O código a seguir cria uma relação entre um curso e um `department`. Se os objetos serão anexados ao contexto, o `course` também é adicionado ao `department.Courses` coleção e a chave estrangeira correspondente propriedade no `course` objeto é definido como o valor da propriedade de chave do departamento.  
  ``` csharp
  course.Department = department;
  ```

- Para excluir a relação, defina a propriedade de navegação como `null`. Se você estiver trabalhando com o Entity Framework é baseado no .NET 4.0, fim relacionado precisa ser carregado antes de você defini-lo como nulo. Por exemplo:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  Começando com o Entity Framework 5.0, que é baseado no .NET 4.5, você pode definir a relação como nulo sem carregar final relacionado. Você também pode definir o valor atual como nulo usando o método a seguir.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Excluindo ou adição de um objeto em uma coleção de entidades. Por exemplo, você pode adicionar um objeto do tipo `Course` para o `department.Courses` coleção. Esta operação cria uma relação entre um determinado **curso** e um determinado `department`. Se os objetos serão anexados ao contexto, a referência de departamento e a propriedade de chave estrangeira sobre a **curso** objeto será definido como apropriado `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Usando o `ChangeRelationshipState` método para alterar o estado da relação especificada entre dois objetos de entidade. Esse método é mais comumente usado ao trabalhar com aplicativos de N camadas e um *associação independente* (ele não pode ser usado com uma associação de chave estrangeira). Além disso, usar esse método você deve descartar para baixo até `ObjectContext`, conforme mostrado no exemplo a seguir.  
No exemplo a seguir, há uma relação muitos-para-muitos entre os cursos e instrutores. Chamar o `ChangeRelationshipState` método e passar a `EntityState.Added` parâmetro, permite que o `SchoolContext` sabe que foi adicionado um relacionamento entre os dois objetos:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Observe que, se você estiver atualizando (não apenas adicionando) uma relação, você deve excluir a relação antiga depois de adicionar um novo:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Sincronizando as alterações entre as propriedades de navegação e chaves estrangeiras

Quando você altera a relação dos objetos anexado ao contexto, usando um dos métodos descritos acima, o Entity Framework precisa manter as chaves estrangeiras, referências e coleções em sincronia. Entity Framework gerencia automaticamente essa sincronização (também conhecido como relação correção-up) para as entidades POCO com proxies. Para obter mais informações, consulte [trabalhar com Proxies](~/ef6/fundamentals/proxies.md).

Se você estiver usando as entidades POCO sem proxies, certifique-se de que o **DetectChanges** método é chamado para sincronizar os objetos relacionados no contexto. Observe que as seguintes APIs disparam automaticamente um **DetectChanges** chamar.

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
-   Executando um LINQ consulta em relação a um `DbSet`

## <a name="loading-related-objects"></a>Objetos relacionados ao carregamento

No Entity Framework, que você pode usar mais comumente use as propriedades de navegação para carregar entidades relacionadas à entidade retornada pela associação definida. Para obter mais informações, consulte [Carregando objetos relacionados](~/ef6/querying/related-data.md).

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

Em uma associação independente, a extremidade relacionada de um objeto dependente será consultada com base no valor de chave estrangeira que está atualmente no banco de dados. No entanto, se a relação foi modificada e a propriedade de referência sobre o objeto dependente aponta para um objeto de entidade diferente que é carregado no contexto de objeto, Entity Framework tentará criar uma relação como ele é definido no cliente.

## <a name="managing-concurrency"></a>Gerenciando a simultaneidade

Na chave estrangeira e associações independentes, verificações de simultaneidade baseiam-se sobre as chaves de entidade e outras propriedades da entidade que são definidas no modelo. Ao usar o EF Designer para criar um modelo, defina as `ConcurrencyMode` de atributo para **fixo** para especificar que a propriedade deve ser verificada para simultaneidade. Ao usar o Code First para definir um modelo, use o `ConcurrencyCheck` anotação em propriedades que você deseja a ser verificado para simultaneidade. Ao trabalhar com o Code First, também, você pode usar o `TimeStamp` anotação para especificar que a propriedade deve ser verificada para simultaneidade. Você pode ter apenas uma propriedade de carimbo de hora em uma determinada classe. Primeiro mapas de código dessa propriedade para um campo não anuláveis no banco de dados.

É recomendável que você sempre use a associação de chave estrangeira ao trabalhar com entidades que participam de verificação de simultaneidade e a resolução.

Para obter mais informações, consulte [tratamento de conflitos de simultaneidade](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Trabalhando com chaves sobrepostas

Chaves sobrepostas são chaves compostas em que algumas propriedades na chave também são parte de outra chave na entidade. Você não pode ter uma chave de sobreposição em uma associação independente. Para alterar uma associação de chave estrangeira que inclui chaves sobrepostas, é recomendável que você modifique os valores de chave estrangeiros em vez de usar as referências de objeto.
