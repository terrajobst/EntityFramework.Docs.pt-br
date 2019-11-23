---
title: Code First a um banco de dados existente-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182628"
---
# <a name="code-first-to-an-existing-database"></a>Code First a um banco de dados existente
Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Code First direcionamento a um banco de dados existente. Code First permite que você defina seu modelo usando as classes C\# ou VB.Net. Opcionalmente, a configuração adicional pode ser executada usando atributos em suas classes e propriedades ou usando uma API fluente.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo [agora está disponível no Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o **Visual Studio 2012** ou o **Visual Studio 2013** instalado para concluir este guia.

Você também precisará da versão **6,1** (ou posterior) do **Entity Framework Tools para o Visual Studio** instalado. Consulte [obter Entity Framework](~/ef6/fundamentals/install.md) para obter informações sobre como instalar a versão mais recente do Entity Framework Tools.

## <a name="1-create-an-existing-database"></a>1. criar um banco de dados existente

Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.

Vamos continuar e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do **Gerenciador de servidores** antes de precisar selecionar **Microsoft SQL Server** como a fonte de dado

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   Conecte-se à sua instância do LocalDB e insira **Blogs** como o nome do banco de dados

    ![Conexão LocalDB](~/ef6/media/localdbconnection.png)

-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**

    ![Caixa de diálogo Criar banco de dados](~/ef6/media/createdatabasedialog.png)

-   O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**
-   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**

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

## <a name="2-create-the-application"></a>2. criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa Code First para executar o acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo-&gt; novo&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e no **aplicativo de console**
-   Insira **CodeFirstExistingDatabaseSample** como o nome
-   Selecione **OK**

 

## <a name="3-reverse-engineer-model"></a>3. modelo de engenharia reversa

Vamos usar o Entity Framework Tools para que o Visual Studio nos ajude a gerar um código inicial para mapear para o banco de dados. Essas ferramentas estão apenas gerando código que você também pode digitar manualmente, se preferir.

-   **Projeto-&gt; adicionar novo item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**
-   Insira **BloggingContext** como o nome e clique em **OK**
-   Isso iniciará o **Assistente de modelo de dados de entidade**
-   Selecione **Code First do banco de dados** e clique em **Avançar**

    ![Assistente One CFE](~/ef6/media/wizardonecfe.png)

-   Selecione a conexão com o banco de dados que você criou na primeira seção e clique em **Avançar**

    ![Assistente dois CFE](~/ef6/media/wizardtwocfe.png)

-   Clique na caixa de seleção ao lado de **tabelas** para importar todas as tabelas e clique em **concluir**

    ![Assistente de três CFE](~/ef6/media/wizardthreecfe.png)

Depois que o processo de engenharia reversa for concluído, vários itens serão adicionados ao projeto, vamos dar uma olhada no que foi adicionado.

### <a name="configuration-file"></a>arquivo de configuração

Um arquivo app. config foi adicionado ao projeto, esse arquivo contém a cadeia de conexão para o banco de dados existente.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Você também observará algumas outras configurações no arquivo de configuração, essas são configurações padrão do EF que informam Code First onde criar bancos de dados. Como estamos mapeando para um banco de dados existente, essas configurações serão ignoradas em nosso aplicativo.*

### <a name="derived-context"></a>Contexto derivado

Uma classe **BloggingContext** foi adicionada ao projeto. O contexto representa uma sessão com o banco de dados, o que nos permite consultar e salvar os dados.
O contexto expõe um **DbSet&lt;TEntity&gt;** para cada tipo em nosso modelo. Você também observará que o construtor padrão chama um construtor base usando a sintaxe **Name =** . Isso informa Code First que a cadeia de conexão a ser usada para esse contexto deve ser carregada a partir do arquivo de configuração.

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

*Você sempre deve usar a sintaxe **Name =** quando estiver usando uma cadeia de conexão no arquivo de configuração. Isso garante que, se a cadeia de conexão não estiver presente, Entity Framework será lançada em vez de criar um novo banco de dados por convenção.*

### <a name="model-classes"></a>Classes de modelo

Por fim, um **blog** e uma classe **post** também foram adicionados ao projeto. Essas são as classes de domínio que compõem nosso modelo. Você verá as anotações de dados aplicadas às classes para especificar a configuração em que as convenções de Code First não se alinhariam com a estrutura do banco de dados existente. Por exemplo, você verá a anotação **StringLength** em **blog.Name** e **blog. URL** , pois eles têm um comprimento máximo de **200** no banco de dados (o padrão de Code First é usar o comprimento de máximo suportado pelo provedor de banco de dados – **nvarchar (max)** em SQL Server).

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

## <a name="4-reading--writing-data"></a>4. lendo & gravando dados

Agora que temos um modelo, é hora de usá-lo para acessar alguns dados. Implemente o método **Main** em **Program.cs** , conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo **blog**. Em seguida, ele usa uma consulta LINQ para recuperar todos os **Blogs** do banco de dados ordenado alfabeticamente por **título**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>E se meu banco de dados mudar?

O assistente de Code First para banco de dados foi criado para gerar um conjunto de classes de ponto de partida que você pode ajustar e modificar. Se o esquema do banco de dados for alterado, você poderá editar manualmente as classes ou executar outra engenharia reversa para substituir as classes.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Usando Migrações do Code First para um banco de dados existente

Se você quiser usar Migrações do Code First com um banco de dados existente, consulte [migrações do Code First para um banco de dados existente](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Resumo

Neste tutorial, examinamos Code First desenvolvimento usando um banco de dados existente. Usamos o Entity Framework Tools para que o Visual Studio reverta a engenharia de um conjunto de classes mapeadas para o banco de dados e pudesse ser usado para armazenar e recuperar os dados.
