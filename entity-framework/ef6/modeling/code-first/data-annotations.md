---
title: Code First de anotações de dados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419181"
---
# <a name="code-first-data-annotations"></a>Code First anotações de dados
> [!NOTE]
> **Somente EF 4.1 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 4,1. Se você estiver usando uma versão anterior, algumas ou todas essas informações não se aplicarão.

O conteúdo desta página é adaptado de um artigo escrito originalmente por Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First permite que você use suas próprias classes de domínio para representar o modelo que o EF depende para executar consultas, controle de alterações e atualizações de funções. Code First aproveita um padrão de programação conhecido como ' Convenção sobre configuração '. Code First presumirá que suas classes seguem as convenções de Entity Framework e, nesse caso, irão automaticamente resolver como executar seu trabalho. No entanto, se suas classes não seguirem essas convenções, você terá a capacidade de adicionar configurações às suas classes para fornecer ao EF as informações necessárias.

Code First oferece duas maneiras de adicionar essas configurações às suas classes. Uma delas é usar atributos simples, chamados Annotations, e a segunda é usar a API fluente do Code First, que fornece uma maneira de descrever as configurações imperativamente, no código.

Este artigo se concentrará no uso de Annotations (no namespace System. ComponentModel. Annotations) para configurar suas classes – destacando as configurações mais comumente necessárias. Annotations também são compreendidos por vários aplicativos .net, como o ASP.NET MVC, que permite que esses aplicativos aproveitem as mesmas anotações para validações do lado do cliente.


## <a name="the-model"></a>O modelo

Demonstrarei Code First Annotations com um par simples de classes: blog e post.

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

Como estão, as classes blog e post seguem convenientemente a Convenção do código e não exigem ajustes para habilitar a compatibilidade do EF. No entanto, você também pode usar as anotações para fornecer mais informações ao EF sobre as classes e o banco de dados no qual eles são mapeados.

 

## <a name="key"></a>Chave

Entity Framework se baseia em cada entidade com um valor de chave que é usado para acompanhamento de entidade. Uma Convenção de Code First é Propriedades de chave implícita; Code First procurará uma propriedade chamada "ID" ou uma combinação de nome de classe e "ID", como "blogid". Essa propriedade será mapeada para uma coluna de chave primária no banco de dados.

As classes blog e post seguem essa convenção. E se eles não estivessem? E se o blog usava o nome *PrimaryTrackingKey* , ou até *foo*? Se o Code First não encontrar uma propriedade que corresponda a essa convenção, ele lançará uma exceção devido ao requisito de Entity Framework que você deve ter uma propriedade de chave. Você pode usar a anotação de chave para especificar qual propriedade deve ser usada como EntityKey.

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

Se você estiver usando o recurso de geração de banco de dados de primeiro código, a tabela de blog terá uma coluna de chave primária denominada PrimaryTrackingKey, que também é definida como identidade por padrão.

![Tabela de blog com chave primária](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Chaves compostas

Entity Framework dá suporte a chaves compostas-chaves primárias que consistem em mais de uma propriedade. Por exemplo, você poderia ter uma classe do Passport cuja chave primária é uma combinação de PassportNumber e IssuingCountry.

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

A tentativa de usar a classe acima em seu modelo de EF resultaria em um `InvalidOperationException`:

*Não é possível determinar a ordenação de chave primária composta para o tipo ' Passport '. Use o método ColumnAttribute ou HasKey para especificar uma ordem para chaves primárias compostas.*

Para usar chaves compostas, Entity Framework exige que você defina uma ordem para as propriedades de chave. Você pode fazer isso usando a anotação de coluna para especificar um pedido.

>[!NOTE]
> O valor do pedido é relativo (em vez de baseado em índice) para que qualquer valor possa ser usado. Por exemplo, 100 e 200 seriam aceitáveis no lugar de 1 e 2.

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

Se você tiver entidades com chaves estrangeiras compostas, deverá especificar a mesma ordenação de coluna usada para as propriedades de chave primária correspondentes.

Somente a ordenação relativa dentro das propriedades de chave estrangeira precisa ser a mesma, os valores exatos atribuídos à **ordem** não precisam corresponder. Por exemplo, na classe a seguir, 3 e 4 podem ser usados no lugar de 1 e 2.

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

## <a name="required"></a>Obrigatório

A anotação necessária informa ao EF que uma determinada propriedade é necessária.

Adicionar necessário à propriedade Title forçará o EF (e MVC) a garantir que a propriedade tenha dados nele.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sem alterações de código ou marcação adicionais no aplicativo, um aplicativo MVC executará a validação do lado do cliente, até mesmo criando dinamicamente uma mensagem usando os nomes da propriedade e da anotação.

![Criar página com título é um erro obrigatório](~/ef6/media/jj591583-figure02.png)

O atributo Required também afetará o banco de dados gerado, tornando a propriedade mapeada não anulável. Observe que o campo título foi alterado para "não nulo".

>[!NOTE]
> Em alguns casos, talvez não seja possível que a coluna no banco de dados seja não anulável, embora a propriedade seja necessária. Por exemplo, ao usar uma estratégia de herança TPH, os dados de vários tipos são armazenados em uma única tabela. Se um tipo derivado incluir uma propriedade obrigatória, a coluna não poderá se tornar não anulável, pois nem todos os tipos na hierarquia terão essa propriedade.

 

![Tabela de Blogs](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength e MinLength

Os atributos MaxLength e MinLength permitem especificar validações de propriedade adicionais, exatamente como você fez com required.

Aqui está o Bloggername com requisitos de comprimento. O exemplo também demonstra como combinar atributos.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

A anotação MaxLength afetará o banco de dados definindo o comprimento da propriedade como 10.

![Tabela de Blogs mostrando o comprimento máximo na coluna Bloggername](~/ef6/media/jj591583-figure04.png)

A anotação do lado do cliente do MVC e a anotação do lado do servidor do EF 4,1 respeitarão essa validação, novamente criando uma mensagem de erro de forma dinâmica: "o campo Bloggername deve ser uma cadeia de caracteres ou um tipo de matriz com um comprimento máximo de ' 10 '." Essa mensagem é um pouco longa. Muitas anotações permitem que você especifique uma mensagem de erro com o atributo ErrorMessage.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Você também pode especificar ErrorMessage na anotação necessária.

![Criar página com mensagem de erro personalizada](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

A Convenção do código First determina que todas as propriedades de um tipo de dados com suporte são representadas no Database. Mas esse nem sempre é o caso em seus aplicativos. Por exemplo, você pode ter uma propriedade na classe blog que cria um código com base nos campos title e Bloggername. Essa propriedade pode ser criada dinamicamente e não precisa ser armazenada. Você pode marcar todas as propriedades que não são mapeadas para o banco de dados com a anotação não mapeada, como essa propriedade BlogCode.

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

Não é incomum descrever suas entidades de domínio em um conjunto de classes e, em seguida, camadar essas classes para descrever uma entidade completa. Por exemplo, você pode adicionar uma classe chamada BlogDetails ao seu modelo.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Observe que BlogDetails não tem nenhum tipo de propriedade de chave. No design controlado por domínio, BlogDetails é chamado de objeto value. Entity Framework se refere a objetos de valor como tipos complexos.  Tipos complexos não podem ser rastreados por conta própria.

No entanto, como uma propriedade na classe blog, BlogDetails ele será acompanhado como parte de um objeto de blog. Para que o Code First reconheça isso, você deve marcar a classe BlogDetails como um complexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Agora você pode adicionar uma propriedade na classe blog para representar o BlogDetails para esse blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

No banco de dados, a tabela de blog conterá todas as propriedades do blog, incluindo as propriedades contidas em sua propriedade BlogDetail. Por padrão, cada um é precedido do nome do tipo complexo, BlogDetail.

![Tabela de blog com tipo complexo](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

A anotação ConcurrencyCheck permite sinalizar uma ou mais propriedades a serem usadas para verificação de simultaneidade no banco de dados quando um usuário edita ou exclui uma entidade. Se você trabalha com o designer do EF, isso se alinha com a definição do ConcurrencyMode de uma propriedade como Fixed.

Vamos ver como o ConcurrencyCheck funciona adicionando-o à propriedade Bloggername.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Quando SaveChanges é chamado, devido à anotação ConcurrencyCheck no campo Bloggername, o valor original dessa propriedade será usado na atualização. O comando tentará localizar a linha correta filtrando não apenas o valor da chave, mas também o valor original de Bloggername.  Aqui estão as partes críticas do comando UPDATE enviadas ao banco de dados, onde você pode ver que o comando atualizará a linha que tem um PrimaryTrackingKey é 1 e um Bloggername de "Julie", que era o valor original quando o blog foi recuperado do banco de dados.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Se alguém tiver alterado o nome do Blogger para esse blog enquanto isso, essa atualização falhará e você receberá um DbUpdateConcurrencyException que você precisará manipular.

 

## <a name="timestamp"></a>TimeStamp

É mais comum usar os campos de IsVersion ou timestamp para verificação de simultaneidade. Mas, em vez de usar a anotação ConcurrencyCheck, você pode usar a anotação de carimbo de data/hora mais específica, contanto que o tipo da propriedade seja matriz de bytes. O Code First tratará as propriedades de carimbo de data/hora da mesma forma que as propriedades ConcurrencyCheck, mas também garantirá que o campo de banco de dados que o Code First gera seja não anulável. Você só pode ter uma propriedade Timestamp em uma determinada classe.

Adicionando a seguinte propriedade à classe blog:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

resulta em um código primeiro criando uma coluna de carimbo de data/hora não anulável na tabela de banco de dados.

![Tabela de Blogs com coluna de carimbo de data/hora](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabela e coluna

Se você estiver permitindo que Code First crie o banco de dados, convém alterar o nome das tabelas e das colunas que ele está criando. Você também pode usar Code First com um banco de dados existente. Mas nem sempre é o caso de os nomes das classes e das propriedades em seu domínio corresponderem aos nomes das tabelas e colunas no banco de dados.

Minha classe é chamada blog e, por convenção, o Code First pressupõe que isso será mapeado para uma tabela chamada Blogs. Se esse não for o caso, você poderá especificar o nome da tabela com o atributo Table. Aqui, por exemplo, a anotação está especificando que o nome da tabela é InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

A anotação de coluna é mais adepta na especificação dos atributos de uma coluna mapeada. Você pode estipular um nome, um tipo de dados ou até mesmo a ordem na qual uma coluna aparece na tabela. Aqui está um exemplo do atributo Column.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Não confunda o atributo TypeName da coluna com a DataAnnotation DataType. DataType é uma anotação usada para a interface do usuário e é ignorada por Code First.

Aqui está a tabela após sua regeneração. O nome da tabela foi alterado para InternalBlogs e a coluna de descrição do tipo complexo agora é BlogDescription. Como o nome foi especificado na anotação, o Code First não usará a Convenção de iniciar o nome da coluna com o nome do tipo complexo.

![Tabela de Blogs e coluna renomeada](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Um importante recurso de banco de dados é a capacidade de ter propriedades computadas. Se você estiver mapeando suas classes de Code First para tabelas que contêm colunas computadas, você não quer que Entity Framework tente atualizar essas colunas. Mas você deseja que o EF retorne esses valores do banco de dados depois de inserir ou atualizar os dados. Você pode usar a anotação DatabaseGenerated para sinalizar essas propriedades em sua classe junto com a enumeração computada. Outros enums são None e Identity.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Você pode usar o banco de dados gerado em colunas de byte ou carimbo de data/hora em que o Code First está gerando o banco de dados, caso contrário, você deve usá-lo apenas ao apontar para bancos de dados existentes, pois o Code First não será capaz de determinar a fórmula para a coluna computada.

Você leu acima que, por padrão, uma propriedade de chave que é um inteiro se tornará uma chave de identidade no banco de dados. Isso seria o mesmo que definir DatabaseGenerated como DatabaseGeneratedOption. Identity. Se você não quiser que ela seja uma chave de identidade, poderá definir o valor como DatabaseGeneratedOption. None.

 

## <a name="index"></a>Índice

> [!NOTE]
> **Somente em versões do EF 6.1** – o atributo de índice foi introduzido no Entity Framework 6,1. Se você estiver usando uma versão anterior, as informações nesta seção não se aplicarão.

Você pode criar um índice em uma ou mais colunas usando o **indexattribute**. Adicionar o atributo a uma ou mais propriedades fará com que o EF crie o índice correspondente no banco de dados quando ele criar o banco de dados ou Scaffold as chamadas **CreateIndex** correspondentes se você estiver usando migrações do Code First.

Por exemplo, o código a seguir resultará em um índice sendo criado na coluna **classificação** da tabela de **postagens** no banco de dados.

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

Por padrão, o índice será nomeado **IX\_&lt;nome da propriedade&gt;** (IX\_classificação no exemplo acima). No entanto, você também pode especificar um nome para o índice. O exemplo a seguir especifica que o índice deve ser nomeado **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Por padrão, os índices são não exclusivos, mas você pode usar o parâmetro **IsUnique** nomeado para especificar que um índice deve ser exclusivo. O exemplo a seguir apresenta um índice exclusivo no nome de logon de um **usuário**.

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

Os índices que abrangem várias colunas são especificados usando o mesmo nome em várias anotações de índice para uma determinada tabela. Ao criar índices de várias colunas, você precisa especificar uma ordem para as colunas no índice. Por exemplo, o código a seguir cria um índice de várias colunas em **classificação** e **blogid** chamado **IX\_BlogIdAndRating**. O **blogid** é a primeira coluna no índice e a **classificação** é a segunda.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Atributos de relação: inversoproperty e ForeignKey

> [!NOTE]
> Esta página fornece informações sobre como configurar relações em seu modelo de Code First usando anotações de dados. Para obter informações gerais sobre relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md). *

A Convenção do código First cuidará das relações mais comuns em seu modelo, mas há alguns casos em que precisa de ajuda.

A alteração do nome da propriedade de chave na classe de blog criou um problema com sua relação com a postagem. 

Ao gerar o banco de dados, o Code First vê a propriedade blogid na classe post e a reconhece, pela Convenção de que ela corresponde a um nome de classe, mais "ID", como uma chave estrangeira para a classe de blog. Mas não há nenhuma propriedade blogid na classe blog. A solução para isso é criar uma propriedade de navegação na postagem e usar a anotação externa para ajudar o Code a entender primeiro como criar a relação entre as duas classes — usando a propriedade post. blogid — e também como especificar restrições no banco.

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

A restrição no banco de dados mostra uma relação entre InternalBlogs. PrimaryTrackingKey e postes. blogid. 

![relação entre InternalBlogs. PrimaryTrackingKey e postes. blogid](~/ef6/media/jj591583-figure09.png)

O inversoproperty é usado quando você tem várias relações entre classes.

Na classe post, convém acompanhar quem escreveu uma postagem de blog, bem como quem a editou. Aqui estão duas novas propriedades de navegação para a classe post.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Você também precisará adicionar a classe Person referenciada por essas propriedades. A classe Person tem propriedades de navegação de volta para a postagem, uma para todas as postagens escritas pela pessoa e uma para todas as postagens atualizadas por essa pessoa.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

O Code First não é capaz de corresponder as propriedades nas duas classes por conta própria. A tabela de banco de dados para postagens deve ter uma chave estrangeira para a pessoa CreatedBy e outra para a pessoa UpdatedBy, mas o Code First criará quatro propriedades de chave estrangeira: Person\_ID, Person\_Id1, CreatedBy\_ID e UpdatedBy\_ID.

![Lança a tabela com chaves estrangeiras extras](~/ef6/media/jj591583-figure10.png)

Para corrigir esses problemas, você pode usar a anotação inversoproperty para especificar o alinhamento das propriedades.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Como a propriedade PostsWritten em Person sabe que isso se refere ao tipo post, ela criará a relação para post. CreatedBy. Da mesma forma, PostsUpdated será conectado a post. UpdatedBy. E o Code First não criará as chaves estrangeiras extras.

![Lança a tabela sem chaves estrangeiras extras](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Resumo

Annotations não só permitem que você descreva a validação do cliente e do servidor nas classes Code First, mas elas também permitem aprimorar e até mesmo corrigir as suposições que o Code First fará sobre suas classes com base em suas convenções. Com as anotações de dados, você não só pode impulsionar a geração do esquema de banco, mas também pode mapear suas primeiras classes code para um banco de dados pré-existente.

Embora eles sejam muito flexíveis, lembre-se de que Annotations fornecem apenas as alterações de configuração mais comumente necessárias que você pode fazer em suas classes de código primeiro. Para configurar suas classes para alguns dos casos de borda, você deve procurar o mecanismo de configuração alternativo, a API Fluent do Code First.
