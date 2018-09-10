---
title: Code First para um banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: fedfb921919582e2cdb5f3bc497f11889b972ad6
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251070"
---
# <a name="code-first-to-an-existing-database"></a>Code First para um banco de dados existente
Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Code First, direcionando um banco de dados existente. Código primeiro permite que você defina seu modelo usando o C\# ou classes VB.Net. Configuração adicional, opcionalmente, pode ser executada usando atributos em classes e propriedades ou usando uma API fluente.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo [agora está disponível no Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter **Visual Studio 2012** ou **Visual Studio 2013** instalado para concluir este passo a passo.

Você também precisará versão **6.1** (ou posterior) da **Entity Framework Tools para Visual Studio** instalado. Ver [obter o Entity Framework](~/ef6/fundamentals/install.md) para obter informações sobre como instalar a versão mais recente das ferramentas do Entity Framework.

## <a name="1-create-an-existing-database"></a>1. Criar um banco de dados existente

Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.

Vamos prosseguir e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do **Gerenciador de servidores** antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   Conectar-se à instância do LocalDB e insira **blogs** como o nome do banco de dados

    ![Conexão do LocalDB](~/ef6/media/localdbconnection.png)

-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**

    ![Criar caixa de diálogo banco de dados](~/ef6/media/createdatabasedialog.png)

-   O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**
-   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Code First para executar o acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e **aplicativo de Console**
-   Insira **CodeFirstExistingDatabaseSample** como o nome
-   Selecione **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Modelo de engenharia reversa

Vamos usar o Entity Framework Tools para Visual Studio para nos ajudar a gerar um código inicial para mapear para o banco de dados. Essas ferramentas são apenas gerar código que você também pode digitar manualmente se preferir.

-   **Projeto -&gt; Adicionar Novo Item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**
-   Insira **BloggingContext** como o nome e clique em **Okey**
-   Isso inicia o **Assistente de modelo de dados de entidade**
-   Selecione **Code First do banco de dados** e clique em **Avançar**

    ![CFE de um assistente](~/ef6/media/wizardonecfe.png)

-   Selecione a conexão ao banco de dados que você criou na primeira seção e clique em **Avançar**

    ![CFE dois do Assistente](~/ef6/media/wizardtwocfe.png)

-   Clique na caixa de seleção ao lado **tabelas** para importar todas as tabelas e clique em **concluir**

    ![Assistente CFE três](~/ef6/media/wizardthreecfe.png)

Depois que o processo de engenharia reversa de um número de itens é concluído terão sido adicionadas ao projeto, vamos dar uma olhada no que foi adicionado.

### <a name="configuration-file"></a>arquivo de configuração

Um arquivo App. config foi adicionado ao projeto, esse arquivo contém a cadeia de caracteres de conexão ao banco de dados existente.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Você observará algumas outras configurações no arquivo de configuração muito, essas são as configurações padrão do EF que informam ao Code First onde criar bancos de dados. Uma vez que estamos estiver mapeando para um banco de dados existente, essas configurações serão ignoradas em nosso aplicativo.*

### <a name="derived-context"></a>Contexto derivado

Um **BloggingContext** classe foi adicionada ao projeto. O contexto representa uma sessão com o banco de dados, que nos permite consultar e salvar dados.
Expõe o contexto de um **DbSet&lt;TEntity&gt;**  para cada tipo em nosso modelo. Você também observará que o construtor padrão chama um construtor base usando o **nome =** sintaxe. Isso informa ao Code First que a cadeia de caracteres de conexão a ser usado para este contexto deve ser carregada do arquivo de configuração.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Você sempre deve usar o **nome =** sintaxe quando você estiver usando uma cadeia de caracteres de conexão no arquivo de configuração. Isso garante que se a cadeia de caracteres de conexão não estiver presente, em seguida, Entity Framework gerará em vez de criar um novo banco de dados por convenção.*

### <a name="model-classes"></a>Classes de modelo

Por fim, uma **Blog** e **Post** classe também foram adicionados ao projeto. Essas são as classes de domínio que compõem o nosso modelo. Você verá as anotações de dados aplicados às classes para especificar a configuração em que as convenções Code First não seriam alinha com a estrutura do banco de dados existente. Por exemplo, você verá a **StringLength** anotação no **Blog.Name** e **Blog.Url** , pois eles têm um comprimento máximo de **200** no banco de dados (o padrão de Code First é usar o comprimento máximo com suporte pelo provedor de banco de dados - **nvarchar (max)** no SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Ler e gravar dados

Agora que temos um modelo que é hora de usá-lo para acessar alguns dados. Implemente a **principal** método na **Program.cs** conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir uma nova **Blog**. Em seguida, ele usa uma consulta LINQ para recuperar todos os **Blogs** do banco de dados classificado em ordem alfabética por **título**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>E se meu banco de dados é alterado?

O Code First para o Assistente de banco de dados foi projetado para gerar um conjunto de ponto de partida de classes que você pode ajustar e modificar. Se seu esquema de banco de dados for alterado você pode manualmente editar as classes ou executar outra engenharia reversa para substituir as classes.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Usando as migrações do Code First para um banco de dados existente

Se você quiser usar migrações do Code First com um banco de dados existente, consulte [migrações do Code First para um banco de dados](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Resumo

Neste passo a passo analisamos desenvolvimento Code First usando um banco de dados existente. Usamos o Entity Framework Tools para Visual Studio para um conjunto de classes que são mapeados para o banco de dados e pode ser usado para armazenar e recuperar dados de engenharia reversa.
