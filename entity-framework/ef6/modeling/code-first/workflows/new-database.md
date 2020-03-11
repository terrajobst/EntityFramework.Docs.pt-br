---
title: Code First a um novo banco de dados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418808"
---
# <a name="code-first-to-a-new-database"></a>Code First a um novo banco de dados
Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Code First visando um novo banco de dados. Esse cenário inclui o direcionamento de um banco de dados que não existe e Code First será criado, ou um banco de dados vazio ao qual Code First adicionará novas tabelas. Code First permite que você defina seu modelo usando as classes C\# ou VB.Net. A configuração adicional pode, opcionalmente, ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo fornece uma introdução ao desenvolvimento de Code First destinado a um novo banco de dados. Esse cenário inclui o direcionamento de um banco de dados que não existe e Code First será criado, ou um banco de dados vazio ao qual Code First adicionará novas tabelas. Code First permite que você defina seu modelo usando C# as classes ou VB.net. A configuração adicional pode, opcionalmente, ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.

**Apresentado por**: [Rowan Miller](https://romiller.com/)

**Vídeo**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (zip)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter, pelo menos, o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir esta explicação.

Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa Code First para executar o acesso a dados.

-   Abra o Visual Studio
-   **Arquivo-&gt; novo&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e no **aplicativo de console**
-   Insira **CodeFirstNewDatabaseSample** como o nome
-   Selecione **OK**

## <a name="2-create-the-model"></a>2. criar o modelo

Vamos definir um modelo muito simples usando classes. Estamos apenas definindo-as no arquivo Program.cs, mas em um aplicativo real, você dividiria suas classes em arquivos separados e potencialmente um projeto separado.

Abaixo da definição de classe do programa em Program.cs, adicione as duas classes a seguir.

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

Você observará que estamos fazendo as duas propriedades de navegação (blog. postes e post. blog) virtuais. Isso habilita o recurso de carregamento lento do Entity Framework. O carregamento lento significa que o conteúdo dessas propriedades será carregado automaticamente do banco de dados quando você tentar acessá-los.

## <a name="3-create-a-context"></a>3. criar um contexto

Agora, é hora de definir um contexto derivado, que representa uma sessão com o banco de dados, permitindo que possamos consultar e salvar. Definimos um contexto que deriva de System. Data. Entity. DbContext e expõe um DbSet tipado&lt;TEntity&gt; para cada classe em nosso modelo.

Agora estamos começando a usar os tipos do Entity Framework, portanto, precisamos adicionar o pacote NuGet do EntityFramework.

-   **Projeto –&gt; gerenciar pacotes NuGet...**
    Observação: se você não tiver o **Manage NuGet Packages...** opção você deve instalar a [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Selecione a guia **online**
-   Selecionar o pacote do **EntityFramework**
-   Clique em **Instalar**

Adicione uma instrução using para System. Data. Entity na parte superior de Program.cs.

``` csharp
using System.Data.Entity;
```

Abaixo da classe post em Program.cs, adicione o seguinte contexto derivado.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Aqui está uma lista completa do que o Program.cs deve conter agora.

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

Esse é todo o código de que precisamos para iniciar o armazenamento e a recuperação de dados. Obviamente, há um pouco sobre os bastidores e vamos dar uma olhada nele em um momento, mas primeiro vamos vê-lo em ação.

## <a name="4-reading--writing-data"></a>4. lendo & gravando dados

Implemente o método Main em Program.cs, conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo blog. Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados ordenado alfabeticamente por título.

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

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Onde estão meus dados?

Por convenção DbContext criou um banco de dados para você.

-   Se uma instância local do SQL Express estiver disponível (instalada por padrão com o Visual Studio 2010), Code First criou o banco de dados nessa instância
-   Se o SQL Express não estiver disponível, Code First tentará usar o [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado por padrão com o Visual Studio 2012)
-   O banco de dados é nomeado após o nome totalmente qualificado do contexto derivado, em nosso caso, é **CodeFirstNewDatabaseSample. BloggingContext**

Essas são apenas as convenções padrão e há várias maneiras de alterar o banco de dados que Code First usa, mais informações estão disponíveis no tópico **como o DbContext descobre o modelo e a conexão de banco de dados** .
Você pode se conectar a este banco de dados usando Gerenciador de Servidores no Visual Studio

-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados** e selecione **Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou

Agora podemos inspecionar o esquema que Code First criado.

![Inicial do esquema](~/ef6/media/schemainitial.png)

O DbContext deu as classes a serem incluídas no modelo examinando as propriedades DbSet que definimos. Em seguida, ele usa o conjunto padrão de convenções de Code First para determinar nomes de tabela e coluna, determinar tipos de dados, localizar chaves primárias, etc. Mais adiante neste tutorial, veremos como você pode substituir essas convenções.

## <a name="5-dealing-with-model-changes"></a>5. lidando com alterações de modelo

Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações, também precisamos atualizar o esquema do banco de dados. Para isso, vamos usar um recurso chamado Migrações do Code First ou migrações de forma abreviada.

As migrações nos permitem ter um conjunto ordenado de etapas que descrevem como atualizar (e fazer downgrade) nosso esquema de banco de dados. Cada uma dessas etapas, conhecida como migração, contém algum código que descreve as alterações a serem aplicadas. 

A primeira etapa é habilitar Migrações do Code First para nosso BloggingContext.

-   **Ferramentas-&gt; Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**
-   Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes
-   Uma nova pasta de migrações foi adicionada ao nosso projeto que contém dois itens:
    -   **Configuration.cs** – esse arquivo contém as configurações que serão usadas pelas migrações para a migração de BloggingContext. Não precisamos alterar nada para este passo a passos, mas aqui é onde você pode especificar dados de semente, registrar provedores para outros bancos de dado, altera o namespace em que as migrações são geradas etc.
    -   **&lt;carimbo de data/hora&gt;\_InitialCreate.cs** – esta é a sua primeira migração, mas representa as alterações que já foram aplicadas ao banco de dados para levá-lo de ser um banco de dados vazio para um que inclua as tabelas Blogs e postagens. Embora permitamos que Code First criem essas tabelas automaticamente para nós, agora que optamos por migrações que foram convertidas em uma migração. Code First também foi registrado em nosso banco de dados local que essa migração já foi aplicada. O carimbo de data/hora no nome de arquivo é usado para fins de ordenação.

    Agora, vamos fazer uma alteração em nosso modelo, adicionar uma propriedade URL à classe blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Execute o comando **Add-Migration addurl** no console do Gerenciador de pacotes.
    O comando Add-Migration verifica as alterações desde sua última migração e aplica Scaffold uma nova migração com as alterações encontradas. Podemos dar um nome às migrações; Nesse caso, estamos chamando a migração ' AddUrl '.
    O código com Scaffold está dizendo que precisamos adicionar uma coluna de URL, que pode conter dados de cadeia de caracteres, ao dbo. Tabela de Blogs. Se necessário, poderíamos editar o código com Scaffold, mas isso não é necessário nesse caso.

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

-   Execute o comando **Update-Database** no console do Gerenciador de pacotes. Esse comando aplicará todas as migrações pendentes ao banco de dados. Nossa migração de InitialCreate já foi aplicada, portanto, as migrações só aplicarão nossa nova migração AddUrl.
    Dica: você pode usar a opção **– Verbose** ao chamar Update-Database para ver o SQL que está sendo executado no banco de dados.

A nova coluna URL agora é adicionada à tabela Blogs no banco de dados:

![Esquema com URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. anotações de dados

Até agora, deixamos que o EF descubra o modelo usando suas convenções padrão, mas haverá momentos em que nossas classes não seguem as convenções e precisamos ser capazes de executar outras configurações. Há duas opções para isso; Veremos as anotações de dados nesta seção e, em seguida, a API Fluent na próxima seção.

-   Vamos adicionar uma classe de usuário ao nosso modelo

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Também precisamos adicionar um conjunto ao nosso contexto derivado

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Se tentamos adicionar uma migração, obtemos um erro dizendo "*EntityType ' user ' não tem nenhuma chave definida. Defina a chave para este EntityType. "* Porque o EF não tem como saber que username deve ser a chave primária para o usuário.
-   Vamos usar as anotações de dados agora para que seja necessário adicionar uma instrução using na parte superior de Program.cs

```csharp
using System.ComponentModel.DataAnnotations;
```

-   Agora, anote a propriedade UserName para identificar que é a chave primária

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Use o comando **Add-migrar adduser** para Scaffold uma migração para aplicar essas alterações ao banco de dados
-   Execute o comando **Update-Database** para aplicar a nova migração para o banco de dados

A nova tabela agora é adicionada ao banco de dados:

![Esquema com usuários](~/ef6/media/schemawithusers.png)

A lista completa de anotações com suporte no EF é:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [Carimbo de data/hora](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [Não Mapeadoattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. API fluente

Na seção anterior, examinamos o uso de anotações de dados para complementar ou substituir o que foi detectado pela Convenção. A outra maneira de configurar o modelo é por meio da API Fluent Code First.

A maioria das configurações de modelo pode ser feita usando anotações de dados simples. A API fluente é uma maneira mais avançada de especificar a configuração do modelo que abrange tudo o que as anotações de dados podem fazer além de algumas configurações mais avançadas não possíveis com as anotações de dados. As anotações de dados e a API fluente podem ser usadas em conjunto.

Para acessar a API fluente, você substitui o método OnModelCreating em DbContext. Digamos que desejamos renomear a coluna que User. DisplayName está armazenada para exibir\_nome.

-   Substitua o método OnModelCreating em BloggingContext pelo código a seguir

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

-   Use o comando **Add-Migration ChangeDisplayName** para Scaffold uma migração para aplicar essas alterações ao banco de dados.
-   Execute o comando **Update-Database** para aplicar a nova migração para o banco de dados.

A coluna DisplayName agora é renomeada para exibir\_nome:

![Esquema com nome para exibição renomeado](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Resumo

Neste tutorial, examinamos Code First desenvolvimento usando um novo banco de dados. Definimos um modelo usando classes e, em seguida, usamos esse modelo para criar um banco de dados e armazenar e recuperar os mesmos. Depois que o banco de dados foi criado, usamos Migrações do Code First para alterar o esquema à medida que nosso modelo evoluiu. Também vimos como configurar um modelo usando as anotações de dados e a API fluente.
