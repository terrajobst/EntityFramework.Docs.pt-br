---
title: Primeira Data Annotations - EF6 de código
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 8d85ef85f56a23d9b3b526554417dc9dd360e139
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980035"
---
# <a name="code-first-data-annotations"></a>Anotações de dados Code First
> [!NOTE]
> **EF4.1 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 4.1. Se você estiver usando uma versão anterior, algumas ou todas essas informações não se aplica.

O conteúdo desta página é adaptado de um artigo escrito originalmente de Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo de EF depende para executar a consulta, altere de controle e a atualização de funções. Código primeiro aproveita um padrão de programação conhecido como 'convenção em detrimento da configuração'. Código primeiro pressupor que suas classes de seguem as convenções do Entity Framework e nesse caso, funcionarão automaticamente como realizar seu trabalho. No entanto, se suas classes não seguir essas convenções, você tem a capacidade de adicionar as configurações para suas classes para fornecer o EF com as informações necessárias.

Código primeiro oferece duas maneiras de adicionar essas configurações para suas classes. Uma é usando atributos simples chamados DataAnnotations, e o segundo é usando a API do Fluent do Code First, que fornece uma maneira de descrever as configurações de modo imperativo, no código.

Este artigo se concentra no uso de DataAnnotations (no namespace DataAnnotations) para configurar classes – realçando as configurações mais necessárias. DataAnnotations também são compreendidas por um número de aplicativos .NET, como o ASP.NET MVC, que permite que esses aplicativos para aproveitar as mesmas anotações para validações do lado do cliente.


## <a name="the-model"></a>O modelo

Vou demonstrar DataAnnotations de código primeiro com um par simple de classes: Blog e Post.

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

Como estão, as classes de postagem de Blog convenientemente seguem a convenção de código primeiro e exigem sem ajustes para habilitar a compatibilidade do EF. No entanto, você também pode usar as anotações para fornecer mais informações para o EF sobre as classes e o banco de dados ao qual eles são mapeados.

 

## <a name="key"></a>Chave

Entity Framework se baseia em cada entidade que tem um valor de chave que é usado para acompanhamento de entidade. Uma convenção do Code First é implícitas propriedades-chave; Primeiro, código procurará uma propriedade chamada "Id" ou uma combinação de nome de classe e "Id", como "BlogId". Essa propriedade será mapeada para uma coluna de chave primária no banco de dados.

As classes no Blog e Post seguem Esta convenção. E se eles não? E se o Blog usou o nome *PrimaryTrackingKey* em vez disso, ou até mesmo *foo*? Se o código primeiro não encontrar uma propriedade que corresponde a essa convenção, ele lançará uma exceção devido ao requisito do Entity Framework que você deve ter uma propriedade de chave. Você pode usar a anotação de chave para especificar qual propriedade deve ser usado como EntityKey.

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

Se você estiver usando o código é o recurso de geração de banco de dados, a tabela Blog terá uma coluna de chave primária denominada PrimaryTrackingKey, que também é definido como identidade, por padrão.

![Blog de tabela com a chave primária](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Chaves compostas

Entity Framework oferece suporte a chaves compostas - chaves primárias que consistem em mais de uma propriedade. Por exemplo, você poderia ter uma classe de Passport cuja chave primária é uma combinação de PassportNumber e IssuingCountry.

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

A tentativa de usar a classe acima em seu modelo EF resultaria em um `InvalidOperationException`:

*Não é possível determinar a composição primário ordenação de chave para o tipo 'Passport'. Use o ColumnAttribute ou o método HasKey para especificar uma ordem para chaves primárias compostas.*

Para usar chaves compostas, Entity Framework exige que você pode definir uma ordem para as propriedades de chave. Você pode fazer isso usando a anotação de coluna para especificar uma ordem.

>[!NOTE]
> O valor de ordem é relativo (em vez de índice com base) para que todos os valores possam ser usados. Por exemplo, 100 e 200 seria aceitável no lugar de 1 e 2.

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Se você tiver entidades com chaves estrangeiras compostas, você deve especificar a mesma coluna de classificação que você usou para as propriedades de chave primárias correspondentes.

Somente a ordenação relativa dentro das propriedades de chave estrangeiras deve ser o mesmo, os valores exatos atribuídos a **ordem** não precisa corresponder. Por exemplo, a seguinte classe, 3 e 4 poderiam ser usados no lugar de 1 e 2.

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a>Necessária

A anotação necessária informa ao EF que uma propriedade específica é necessária.

Adicionando necessárias para a propriedade Title forçará o EF (e MVC) para garantir que a propriedade tem dados nele.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sem nenhum adicional nenhum código ou marcação alterações no aplicativo, um aplicativo MVC executará validação do lado do cliente, até mesmo dinamicamente a criação de uma mensagem usando os nomes de propriedade e a anotação.

![Criar página com o título é obrigatório de erro](~/ef6/media/jj591583-figure02.png)

O atributo Required também afetará o banco de dados gerado, tornando a propriedade mapeada não anuláveis. Observe que o campo de título foi alterado para "not null".

>[!NOTE]
> Em alguns casos, pode não ser possível para a coluna no banco de dados a ser não anuláveis, mesmo que a propriedade é necessária. Por exemplo, quando usar uma data de estratégia de herança TPH para vários tipos é armazenado em uma única tabela. Se um tipo derivado inclui uma propriedade necessária que a coluna não pode se tornar não anulável, pois nem todos os tipos na hierarquia terá essa propriedade.

 

![Tabela de blogs](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength e MinLength

Os atributos MaxLength e MinLength permitem que você especifique validações de propriedade adicional, assim como você fez com necessários.

Aqui está o BloggerName com requisitos de comprimento. O exemplo também demonstra como combinar atributos.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

A anotação de MaxLength terá impacto sobre o banco de dados, definindo o comprimento da propriedade como 10.

![Tabela de blogs que mostra o tamanho máximo na coluna BloggerName](~/ef6/media/jj591583-figure04.png)

Anotação do lado do cliente MVC e anotação do lado do servidor de EF 4.1 ambos respeitará essa validação novamente dinamicamente a criação de uma mensagem de erro: "o campo BloggerName deve ser um tipo de cadeia de caracteres ou matriz com um comprimento máximo de '10'". Essa mensagem é um pouco longa. Muitas anotações permitem que você especifique uma mensagem de erro com o atributo de ErrorMessage.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Você também pode especificar ErrorMessage na anotação necessária.

![Criar página com a mensagem de erro personalizada](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Convenção de primeiro código dita que cada propriedade que é de um tipo de dados com suporte é representada no banco de dados. Mas isso nem sempre é o caso em seus aplicativos. Por exemplo, você pode ter uma propriedade na classe de Blog que cria um código com base nos campos de título e BloggerName. Essa propriedade pode ser criada dinamicamente e não precisam ser armazenados. Você pode marcar qualquer propriedade que não é mapeados para o banco de dados com a anotação NotMapped como essa propriedade BlogCode.

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a>ComplexType

Não é incomum para descrever as entidades de domínio em um conjunto de classes e, em seguida, essas classes para descrever uma entidade completa de camada. Por exemplo, você pode adicionar uma classe chamada BlogDetails ao seu modelo.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Observe que BlogDetails não tem qualquer tipo de propriedade de chave. No design controlado por domínio, BlogDetails é conhecido como um objeto de valor. Entity Framework se refere a objetos de valor como tipos complexos.  Tipos complexos não podem ser controlados por conta própria.

No entanto como uma propriedade na classe de Blog, BlogDetails serão rastreada como parte de um objeto de Blog. Em ordem para o code first para reconhecer isso, você deve marcar a classe BlogDetails como um ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Agora você pode adicionar uma propriedade na classe para representar o BlogDetails para esse blog Blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

No banco de dados, a tabela Blog conterá todas as propriedades do blog, incluindo as propriedades contidas na sua propriedade BlogDetail. Por padrão, cada uma delas é precedida pelo nome do tipo complexo, BlogDetail.

![Tabela de blog com tipo complexo](~/ef6/media/jj591583-figure06.png)

Outra observação interessante é que, embora a propriedade DateCreated foi definida como uma data e hora não anuláveis na classe, o campo de banco de dados relevante é anulável. Você deve usar a anotação necessária se você quiser afetar o esquema de banco de dados.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

A anotação ConcurrencyCheck permite sinalizar uma ou mais propriedades a serem usados para verificação no banco de dados quando um usuário edita ou exclui uma entidade de concorrência. Se você já trabalhou com o EF Designer, isso se alinha com a configuração ConcurrencyMode da propriedade como Fixed.

Vamos ver como ConcurrencyCheck funciona ao adicioná-lo à propriedade BloggerName.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Quando SaveChanges for chamado, devido à anotação ConcurrencyCheck no campo BloggerName, o valor original da propriedade será usado na atualização. O comando tentará localizar a linha correta por meio da filtragem não apenas no valor da chave, mas também no valor original de BloggerName.  Aqui estão as partes críticas do comando UPDATE enviados para o banco de dados, onde você pode ver o comando atualizará a linha que tem um PrimaryTrackingKey é 1 e um BloggerName de "Julie", que era o valor original quando esse blog foi recuperado do banco de dados.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Se alguém alterou o nome do blogger para esse blog enquanto isso, essa atualização falhará e você obterá um DbUpdateConcurrencyException que você precisará para administrar.

 

## <a name="timestamp"></a>Carimbo de hora

É mais comum usar campos de rowversion ou carimbo de hora para verificação de simultaneidade. Mas, em vez de usar a anotação ConcurrencyCheck, você pode usar a anotação de carimbo de hora mais específica como o tipo da propriedade é a matriz de bytes. Código primeiro tratarão propriedades de carimbo de hora da mesma como propriedades ConcurrencyCheck, mas ele garantirá que o campo de banco de dados que o código gera primeiro é não anulável. Você só pode ter uma propriedade de carimbo de hora em uma determinada classe.

Adicionando a seguinte propriedade à classe de Blog:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

resultados no código criando primeiro uma coluna de carimbo de hora não anulável na tabela de banco de dados.

![Blogs de tabela com coluna de carimbo de data / hora](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabela e coluna

Se você está permitindo que o Code First cria o banco de dados, você talvez queira alterar o nome das tabelas e colunas que ele está criando. Você também pode usar Code First com um banco de dados existente. Mas nem sempre é o caso que os nomes das classes e propriedades em seu domínio de corresponderem aos nomes das tabelas e colunas no banco de dados.

Minha classe é chamado Blog e por convenção, código primeiro supõe que isso será mapeado para uma tabela chamada Blogs. Se não for o caso, você pode especificar o nome da tabela com o atributo de tabela. Aqui, por exemplo, a anotação está especificando que o nome da tabela é InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

A anotação de coluna é um mais adeptos em especificando os atributos de uma coluna mapeada. Você pode estipular um nome, tipo de dados ou até mesmo a ordem na qual uma coluna aparece na tabela. Aqui está um exemplo de atributo de coluna.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Não confunda o atributo de TypeName da coluna com o DataType DataAnnotation. Tipo de dados é uma anotação, usada para a interface do usuário e é ignorado pelo Code First.

Aqui está a tabela depois que ele é foi regenerado. O nome da tabela foi alterado para InternalBlogs e a coluna de descrição de tipo complexo é agora BlogDescription. Como o nome foi especificado na anotação, código pela primeira vez não usará a convenção de iniciar o nome da coluna com o nome do tipo complexo.

![Blogs de tabela e coluna renomeada](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Recursos de um banco de dados importante é a capacidade de ter propriedades computadas. Se estiver mapeando suas classes de Code First para tabelas que contêm as colunas computadas, você não deseja o Entity Framework ao tentar atualizar as colunas. Mas se você desejar EF para retornar esses valores do banco de dados depois de inserido ou dados atualizados. Você pode usar a anotação DatabaseGenerated para sinalizar essas propriedades em sua classe, juntamente com o enum computado. Outras enumerações são None e identidade.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Você pode usar o banco de dados gerado em colunas timestamp ou byte ao código pela primeira vez é gerar o banco de dados, caso contrário, você deve usar isso apenas quando apontando para bancos de dados existentes, porque o código primeiro não será capaz de determinar a fórmula para a coluna computada.

Ler acima, por padrão, uma propriedade de chave que é um número inteiro se torna uma chave de identidade no banco de dados. Que seria o mesmo que definir DatabaseGenerated como DatabaseGeneratedOption.Identity. Se você não quiser que ele seja uma chave de identidade, você pode definir o valor para DatabaseGeneratedOption.None.

 

## <a name="index"></a>Índice

> [!NOTE]
> **EF6.1 em diante, somente** -atributo o índice foi introduzido no Entity Framework 6.1. Se você estiver usando uma versão anterior, as informações nesta seção não é aplicável.

Você pode criar um índice em uma ou mais colunas usando o **IndexAttribute**. Adicionando o atributo a uma ou mais propriedades será fazer com que o EF criar o índice correspondente no banco de dados quando ele cria o banco de dados ou criar o scaffolding correspondente **CreateIndex** chama se você estiver usando as migrações Code First.

Por exemplo, o código a seguir resultará em um índice que está sendo criado o **Rating** coluna da **postagens** tabela no banco de dados.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

Por padrão, o índice será nomeado **IX\_&lt;nome da propriedade&gt;**  (IX\_de classificação no exemplo acima). Você também pode especificar um nome para o índice no entanto. O exemplo a seguir especifica que o índice deve ser nomeado **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Por padrão, os índices não forem exclusivas, mas você pode usar o **IsUnique** chamado parâmetro para especificar que um índice deve ser exclusivo. O exemplo a seguir apresenta um índice exclusivo em uma **usuário**do nome de logon.

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a>Índices de várias colunas

Os índices que abrangem várias colunas são especificados usando o mesmo nome em várias anotações de índice para uma determinada tabela. Ao criar índices com múltiplas colunas, você precisa especificar uma ordem para as colunas no índice. Por exemplo, o código a seguir cria um índice de várias colunas em **Rating** e **BlogId** chamado **IX\_BlogAndRating**. **BlogId** é a primeira coluna no índice e **classificação** é o segundo.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Atributos de relação: InverseProperty e ForeignKey

> [!NOTE]
> Esta página fornece informações sobre como configurar as relações em seu modelo Code First usando anotações de dados. Para obter informações gerais sobre as relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md). *

Convenção de primeiro código se encarregará das relações mais comuns em seu modelo, mas há alguns casos em que ele precisa de Ajuda.

Alterando o nome da propriedade chave na classe de Blog que criou um problema com sua relação com o Post. 

Ao gerar o banco de dados, o código primeiro vê a propriedade BlogId na classe Post e reconheça, pela convenção de que ele corresponda a um nome de classe além da "Id", como uma chave estrangeira para a classe de Blog. Mas não há nenhuma propriedade BlogId na classe blog. A solução para isso é criar uma propriedade de navegação na postagem e usar o DataAnnotation externa para ajudar o código primeiro entender como criar a relação entre as duas classes — usando a propriedade Post.BlogId — bem como especificar as restrições no banco de dados.

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

A restrição no banco de dados mostra uma relação entre InternalBlogs.PrimaryTrackingKey e Posts.BlogId. 

![relação entre InternalBlogs.PrimaryTrackingKey e Posts.BlogId](~/ef6/media/jj591583-figure09.png)

O InverseProperty é usado quando você tiver várias relações entre classes.

Na classe Post, você talvez queira manter o controle de quem o escreveu uma postagem de blog, bem como quem editou. Aqui estão duas novas propriedades de navegação para a classe de postagem.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Você também precisará adicionar a classe Person referenciada por essas propriedades. A classe Person tem as propriedades de navegação de volta para a postagem, uma para todas as postagens escritas por pessoa e outro para todas as postagens atualizadas por aquela pessoa.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Code first não é capaz de corresponder as propriedades nas duas classes por conta própria. A tabela de banco de dados para postagens deve ter uma chave estrangeira para a pessoa CreatedBy e outro para a pessoa UpdatedBy mas o código primeiro criar propriedades de chave estrangeira quatro do será: pessoa\_Id, a pessoa\_Id1, CreatedBy\_Id e UpdatedBy\_ID.

![Tabela de postagens com chaves estrangeiras extra](~/ef6/media/jj591583-figure10.png)

Para corrigir esses problemas, você pode usar a anotação InverseProperty para especificar o alinhamento das propriedades.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Como a propriedade PostsWritten pessoalmente sabe que isso se refere ao tipo de postagem, ele criará a relação para Post.CreatedBy. Da mesma forma, PostsUpdated será conectado ao Post.UpdatedBy. E o código primeiro não criará as chaves estrangeiras extra.

![Tabela de postagens sem chaves estrangeiras extra](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Resumo

DataAnnotations não só permitem que você descreva a validação do lado do cliente e servidor nas suas classes de primeiro código, mas eles também permitem que você melhore e até mesmo corrigir as suposições que código fará primeiro sobre suas classes com base nas suas convenções. Com DataAnnotations é possível não apenas conduzir geração de esquema de banco de dados, mas você também pode mapear suas classes de primeiro código para um banco de dados já existente.

Enquanto eles são muito flexíveis, tenha em mente que DataAnnotations fornecem apenas mais comumente necessárias alterações de configuração, que você pode fazer em suas classes de código primeiro. Para configurar suas classes de alguns dos casos de borda, você deve procurar para o mecanismo de configuração alternativa, a API Fluent do Code First.
