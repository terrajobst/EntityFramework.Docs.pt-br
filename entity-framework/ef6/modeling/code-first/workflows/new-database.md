---
title: Code First para um novo banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 50c6a4710bc50879304f64e781a46c4836f86882
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152472"
---
# <a name="code-first-to-a-new-database"></a>Code First para um novo banco de dados
Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Code First, direcionando um novo banco de dados. Este cenário inclui o direcionamento de um banco de dados que não existe e Code First criará ou um banco de dados vazio que Code First será adicionar novas tabelas. Código primeiro permite que você defina seu modelo usando o C\# ou classes VB.Net. Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo fornece uma introdução ao desenvolvimento Code First, direcionando um novo banco de dados. Este cenário inclui o direcionamento de um banco de dados que não existe e Code First criará ou um banco de dados vazio que Code First será adicionar novas tabelas. Código primeiro permite que você defina seu modelo usando classes c# ou VB.Net. Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.

**Apresentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter pelo menos o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.

Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. Criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Code First para executar o acesso a dados.

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e **aplicativo de Console**
-   Insira **CodeFirstNewDatabaseSample** como o nome
-   Selecione **OK**

## <a name="2-create-the-model"></a>2. Criar o modelo

Vamos definir um modelo muito simples usando classes. Estamos apenas definindo-os no arquivo Program.cs, mas em um aplicativo do mundo real que você seria dividir suas classes out em arquivos separados e, potencialmente, um projeto separado.

Abaixo da definição de classe do programa em Program.cs, adicione as seguintes classes.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Você observará que estamos fazendo as duas propriedades de navegação (Blog.Posts e Post.Blog) virtual. Isso permite que o recurso de carregamento lento do Entity Framework. Carregamento lento significa que o conteúdo dessas propriedades serão automaticamente carregado do banco de dados quando você tentar acessá-los.

## <a name="3-create-a-context"></a>3. Criar um contexto

Agora é hora de definir um contexto derivado que representa uma sessão com o banco de dados, que nos permite consultar e salvar dados. Definimos um contexto que deriva de System.Data.Entity.DbContext e expõe um DbSet tipado&lt;TEntity&gt; para cada classe em nosso modelo.

Agora estamos começando a usar tipos do Entity Framework, portanto, precisamos adicionar o pacote EntityFramework NuGet.

-   **Projeto –&gt; gerenciar pacotes NuGet...**
    Observação: Se você não tiver o **gerenciar pacotes NuGet...** opção, você deve instalar o [versão mais recente do NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Selecione o **Online** guia
-   Selecione o **EntityFramework** pacote
-   Clique em **instalar**

Adicionar uma instrução de Entity na parte superior de Program.cs.

``` csharp
using System.Data.Entity;
```

Abaixo a classe de postagem em Program.cs, adicione o seguinte contexto derivado.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Aqui está uma listagem completa dos quais Program.cs agora deve conter.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

Isso é todo o código que é preciso começar a armazenar e recuperar dados. É claro que há bem pouco acontecendo nos bastidores e vamos dar uma olhada no que em breve, mas primeiro vamos ver isso em ação.

## <a name="4-reading--writing-data"></a>4. Ler e gravar dados

Implemente o método Main em Program.cs, conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir um novo Blog. Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados classificado em ordem alfabética por título.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Agora você pode executar o aplicativo e testá-lo.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Onde estão meus dados?

Por convenção, o DbContext criou um banco de dados para você.

-   Se uma instância local do SQL Express está disponível (instalado por padrão com o Visual Studio 2010), em seguida, Code First criou o banco de dados nessa instância
-   Se o SQL Express não está disponível, Code First será tente e use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado por padrão com o Visual Studio 2012)
-   O banco de dados é nomeado após o nome totalmente qualificado do contexto derivado, em nosso caso, que é **CodeFirstNewDatabaseSample.BloggingContext**

Esses são apenas as convenções padrão e há várias maneiras de alterar o banco de dados que usa o Code First, mais informações estão disponíveis na **como o DbContext descobre o modelo e a Conexão de banco de dados** tópico.
Você pode se conectar ao banco de dados usando o Gerenciador de servidores no Visual Studio

-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados** e selecione **Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado

Agora podemos pode inspecionar o esquema que o código primeiro criou.

![SchemaInitial](~/ef6/media/schemainitial.png)

DbContext solucionaram quais classes devem ser incluídas no modelo, observando as propriedades DbSet que definimos. Em seguida, ele usa o conjunto padrão de convenções Code First para determinar os nomes de tabela e coluna, determinar os tipos de dados, localize as chaves primárias, etc. Posteriormente neste passo a passo, examinaremos como você pode substituir essas convenções.

## <a name="5-dealing-with-model-changes"></a>5. Lidando com alterações no modelo

Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações que também precisamos atualizar o esquema de banco de dados. Para fazer isso, vamos usar um recurso chamado migrações do Code First ou migrações de forma abreviada.

As migrações nos permite ter um conjunto ordenado de etapas que descrevem como atualizar (e fazer downgrade) nosso esquema de banco de dados. Cada uma dessas etapas, conhecidas como uma migração, contém um código que descreve as alterações sejam aplicadas. 

A primeira etapa é habilitar migrações do Code First para nosso BloggingContext.

-   **Ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**
-   Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes
-   Uma nova pasta migrações foi adicionada ao nosso projeto que contém dois itens:
    -   **Configuration.CS** – esse arquivo contém as configurações que usarão as migrações para a migração de BloggingContext. Não precisamos alterar nada para este passo a passo, mas aqui é onde você pode especificar dados de semente, provedores de registro para outros bancos de dados, altera o namespace que as migrações são geradas no etc.
    -   **&lt;carimbo de hora&gt;\_InitialCreate.cs** – é sua primeira migração, ele representa as alterações que já foram aplicadas ao banco de dados para colocá-lo de ser um banco de dados vazio para um que inclua as tabelas de Blogs e postagens . Embora nós deixava o Code First criar automaticamente essas tabelas para nós, agora que estamos tiver aceitado migrações foram convertidos em uma migração. Código pela primeira vez também registrou em nosso banco de dados local se essa migração já foi aplicada. O carimbo de hora em que o nome do arquivo é usado para fins de ordenação.

    Agora vamos fazer uma alteração ao nosso modelo, adicione uma propriedade de Url para a classe de Blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Execute o **Add-Migration AddUrl** comando no Console do Gerenciador de pacotes.
    O comando Add-Migration procura as alterações desde a última migração e aplica Scaffold em uma nova migração com todas as alterações que são encontrados. Pode dar a migrações de um nome; Nesse caso, chamamos a migração 'AddUrl'.
    O código gerado por scaffolding está dizendo que precisamos adicionar uma coluna de Url, que pode conter dados de cadeia de caracteres, para o dbo. Tabela de blogs. Se necessário, podemos pode editar o código gerado automaticamente, mas isso não é necessário neste caso.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Execute o **Update-Database** comando no Console do Gerenciador de pacotes. Esse comando irá aplicar quaisquer migrações pendentes no banco de dados. Nossa migração InitialCreate já foi aplicada para que migrações apenas seja aplicada a nossa nova migração AddUrl.
    Dica: Você pode usar o **– Verbose** ao chamar Update-Database para ver o que está sendo executado no banco de dados SQL.

A nova coluna de Url agora é adicionada à tabela Blogs no banco de dados:

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Anotações de dados

Até agora estamos apenas deixei o EF descobrir o modelo usando suas convenções de padrão, mas haverá momentos quando nossas classes não seguem as convenções e é preciso ser capaz de executar a configuração. Há duas opções para isso. Vamos examinar as anotações de dados nesta seção e, em seguida, a API fluente na próxima seção.

-   Vamos adicionar uma classe de usuário com o nosso modelo

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Também precisamos adicionar um conjunto para o nosso contexto derivado

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Se tentar adicionar uma migração, teríamos um erro dizendo que "*EntityType 'User' não tem nenhuma chave definida. Definir a chave para esse EntityType."* porque o EF não tem como saber que o nome de usuário deve ser a chave primária para o usuário.
-   Vamos usar anotações de dados agora, portanto, precisamos adicionar uma instrução na parte superior de Program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Agora, anote a propriedade de nome de usuário para identificar o que é a chave primária

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Use o **Add-Migration AddUser** comando para criar o scaffolding de uma migração para aplicar essas alterações no banco de dados
-   Execute o **Update-Database** comando para aplicar a nova migração para o banco de dados

Agora, a nova tabela é adicionada ao banco de dados:

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

A lista completa de anotações com suporte pelo EF é:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. API fluente

Na seção anterior, vimos usando Data Annotations para suplementar ou substituir o que foi detectado pela convenção. A outra maneira para configurar o modelo é por meio da API fluente Code First.

A maioria das configuração de modelo pode ser feita usando anotações de dados simples. A API fluente é uma maneira mais avançada de especificar a configuração de modelo que aborda tudo o que as anotações de dados podem fazer Além disso, alguns não são possíveis com anotações de dados de configuração mais avançado. Anotações de dados e a API fluente podem ser usados juntos.

Para acessar a API fluente é substituir o método OnModelCreating no DbContext. Digamos que queríamos renomear a coluna armazenados em User. DisplayName para exibir\_nome.

-   Substitua o método OnModelCreating no BloggingContext com o código a seguir

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Use o **Add-Migration ChangeDisplayName** comando para criar o scaffolding de uma migração para aplicar essas alterações no banco de dados.
-   Execute o **Update-Database** comando para aplicar a nova migração para o banco de dados.

A coluna DisplayName agora foi renomeada para exibir\_nome:

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Resumo

Neste passo a passo analisamos desenvolvimento Code First usando um novo banco de dados. Estamos definido um modelo usando classes e usado esse modelo para criar um banco de dados e armazenar e recuperar dados. Depois que o banco de dados foi criado, nós usamos migrações do Code First para alterar o esquema como nosso modelo evoluído. Também vimos como configurar um modelo usando Data Annotations e a API Fluent.
