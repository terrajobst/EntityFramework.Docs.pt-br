---
title: Walkthrough-EF6 de entidades de rastreamento automático
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181712"
---
# <a name="self-tracking-entities-walkthrough"></a>Instruções de auto-acompanhamento de entidades
> [!IMPORTANT]
> Não recomendamos usar o modelo de entidades de rastreamento automático. Ele continuará disponível apenas para dar suporte aos aplicativos existentes. Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](https://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.

Este tutorial demonstra o cenário em que um serviço Windows Communication Foundation (WCF) expõe uma operação que retorna um grafo de entidade. Em seguida, um aplicativo cliente manipula esse grafo e envia as modificações a uma operação de serviço que valida e salva as atualizações em um banco de dados usando Entity Framework.

Antes de concluir este passo a passos, certifique-se de ler a página de [entidades de autoacompanhamento](index.md) .

Este passo a passos conclui as seguintes ações:

-   Cria um banco de dados a ser acessado.
-   Cria uma biblioteca de classes que contém o modelo.
-   Alterna para o modelo de gerador de entidade de rastreamento automático.
-   Move as classes de entidade para um projeto separado.
-   Cria um serviço WCF que expõe operações para consultar e salvar entidades.
-   Cria aplicativos cliente (console e WPF) que consomem o serviço.

Usaremos Database First neste passo a passos, mas as mesmas técnicas se aplicam igualmente a Model First.

## <a name="pre-requisites"></a>Pré-requisitos

Para concluir este passo a passos, você precisará de uma versão recente do Visual Studio.

## <a name="create-a-database"></a>Criar um banco de dados

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2012, criará um banco de dados LocalDB.
-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.

Vamos continuar e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar **Microsoft SQL Server** como a fonte de dado
-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou
-   Insira **STESample** como o nome do banco de dados
-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**
-   O novo banco de dados aparecerá agora no Gerenciador de Servidores
-   Se você estiver usando o Visual Studio 2012
    -   Clique com o botão direito do mouse no banco de dados em Gerenciador de Servidores e selecione **nova consulta**
    -   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**
-   Se você estiver usando o Visual Studio 2010
    -   Selecione **Data-&gt; Editor Transact-SQL-&gt; nova conexão de consulta...**
    -   Insira **.\\SQLExpress** como o nome do servidor e clique em **OK**
    -   Selecione o banco de dados **STESample** na lista suspensa na parte superior do editor de consultas
    -   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **Executar SQL**

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Criar o modelo

Primeiro, precisamos de um projeto no qual colocar o modelo.

-   **Arquivo-&gt; novo&gt; projeto...**
-   Selecione o **\#do Visual C** no painel esquerdo e, em seguida, **biblioteca de classes**
-   Insira **STESample** como o nome e clique em **OK**

Agora, vamos criar um modelo simples no designer do EF para acessar nosso banco de dados:

-   **Projeto-&gt; adicionar novo item...**
-   Selecione **dados** no painel esquerdo e, em seguida, **ADO.NET modelo de dados de entidade**
-   Insira **BloggingModel** como o nome e clique em **OK**
-   Selecione **gerar do banco de dados** e clique em **Avançar**
-   Insira as informações de conexão do banco de dados que você criou na seção anterior
-   Insira **BloggingContext** como o nome da cadeia de conexão e clique em **Avançar**
-   Marque a caixa ao lado de **tabelas** e clique em **concluir**

## <a name="swap-to-ste-code-generation"></a>Alternar para geração de código lar

Agora, precisamos desabilitar a geração de código padrão e alternar para entidades de autocontrole.

### <a name="if-you-are-using-visual-studio-2012"></a>Se você estiver usando o Visual Studio 2012

-   Expanda **BloggingModel. edmx** em **Gerenciador de soluções** e exclua o **BloggingModel.tt** e o **BloggingModel.Context.tt**
    *isso desabilitará a geração de código padrão*
-   Clique com o botão direito do mouse em uma área vazia na superfície do designer do EF e selecione **Adicionar item de geração de código...**
-   Selecione **online** no painel esquerdo e procure por **gerador de lar**
-   Selecione o **lar generator for C\#** template, digite **STETemplate** como o nome e clique em **Adicionar**
-   Os arquivos **STETemplate.tt** e **STETemplate.Context.tt** são adicionados aninhados no arquivo BloggingModel. edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Se você estiver usando o Visual Studio 2010

-   Clique com o botão direito do mouse em uma área vazia na superfície do designer do EF e selecione **Adicionar item de geração de código...**
-   Selecione o **código** no painel esquerdo e, em seguida, **ADO.net gerador de entidade de rastreamento automático**
-   Insira **STETemplate** como o nome e clique em **Adicionar**
-   Os arquivos **STETemplate.tt** e **STETemplate.Context.tt** são adicionados diretamente ao seu projeto

## <a name="move-entity-types-into-separate-project"></a>Mover tipos de entidade para projeto separado

Para usar entidades de autoacompanhamento, nosso aplicativo cliente precisa acessar as classes de entidade geradas de nosso modelo. Como não queremos expor todo o modelo ao aplicativo cliente, vamos mover as classes de entidade para um projeto separado.

A primeira etapa é parar a geração de classes de entidade no projeto existente:

-   Clique com o botão direito do mouse em **STETemplate.tt** em **Gerenciador de soluções** e selecione **Propriedades**
-   Na janela **Propriedades** , limpe **TextTemplatingFileGenerator** da propriedade **CustomTool**
-   Expanda **STETemplate.tt** em **Gerenciador de soluções** e exclua todos os arquivos aninhados nele

Em seguida, vamos adicionar um novo projeto e gerar as classes de entidade nele

-   **&gt;&gt; projeto de adição de arquivo...**
-   Selecione o **\#do Visual C** no painel esquerdo e, em seguida, **biblioteca de classes**
-   Insira **STESample. Entities** como o nome e clique em **OK**
-   **Projeto-&gt; Adicionar item existente...**
-   Navegue até a pasta do projeto **STESample**
-   Selecione para exibir **todos os arquivos (\*.\*)**
-   Selecione o arquivo **STETemplate.tt**
-   Clique na seta suspensa ao lado do botão **Adicionar** e selecione **Adicionar como link**

    ![Adicionar modelo vinculado](~/ef6/media/addlinkedtemplate.png)

Também vamos garantir que as classes de entidade sejam geradas no mesmo namespace que o contexto. Isso apenas reduz o número de instruções de uso que precisamos adicionar em todo o nosso aplicativo.

-   Clique com o botão direito do mouse no **STETemplate.tt** vinculado em **Gerenciador de soluções** e selecione **Propriedades**
-   Na janela **Propriedades** , defina **namespace da ferramenta personalizada** como **STESample**

O código gerado pelo modelo lar precisará de uma referência a **System. Runtime. Serialization** para compilar. Essa biblioteca é necessária para os atributos **DataContract** e **DataMember** do WCF que são usados nos tipos de entidade serializáveis.

-   Clique com o botão direito do mouse no projeto **STESample. Entities** em **Gerenciador de soluções** e selecione **Adicionar referência..** .
    -   No Visual Studio 2012-marque a caixa ao lado de **System. Runtime. Serialization** e clique em **OK**
    -   No Visual Studio 2010-selecione **System. Runtime. Serialization** e clique em **OK**

Por fim, o projeto com nosso contexto será necessário uma referência aos tipos de entidade.

-   Clique com o botão direito do mouse no projeto **STESample** em **Gerenciador de soluções** e selecione **Adicionar referência..** .
    -   No Visual Studio 2012 – selecione **solução** no painel esquerdo, marque a caixa ao lado de **STESample. Entities** e clique em **OK**
    -   No Visual Studio 2010-selecione a guia **projetos** , selecione **STESample. Entities** e clique em **OK**

>[!NOTE]
> Outra opção para mover os tipos de entidade para um projeto separado é mover o arquivo de modelo, em vez de vinculá-lo de seu local padrão. Se você fizer isso, será necessário atualizar a variável de **InputFile** no modelo para fornecer o caminho relativo para o arquivo EDMX (neste exemplo, que seria **..\\BloggingModel. edmx**).

## <a name="create-a-wcf-service"></a>Criar um serviço WCF

Agora é hora de adicionar um serviço WCF para expor nossos dados, vamos começar criando o projeto.

-   **&gt;&gt; projeto de adição de arquivo...**
-   Selecione o **Visual C\#** no painel esquerdo e, em seguida, **aplicativo de serviço WCF**
-   Insira **STESample. Service** como o nome e clique em **OK**
-   Adicionar uma referência ao assembly **System. Data. Entity**
-   Adicione uma referência aos projetos **STESample** e **STESample. Entities**

Precisamos copiar a cadeia de conexão do EF para este projeto para que ele seja encontrado em tempo de execução.

-   Abra o arquivo **app. config** para o projeto **STESample **e copie o elemento **connectionStrings**
-   Cole o elemento **connectionStrings** como um elemento filho do elemento **Configuration** do arquivo **Web. config** no projeto **STESample. Service**

Agora é hora de implementar o serviço real.

-   Abra **IService1.cs** e substitua o conteúdo pelo código a seguir

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Abra **Service1. svc** e substitua o conteúdo pelo código a seguir

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Consumir o serviço de um aplicativo de console

Vamos criar um aplicativo de console que usa nosso serviço.

-   **Arquivo-&gt; novo&gt; projeto...**
-   Selecione o **Visual C\#** no painel esquerdo e, em seguida, **aplicativo de console**
-   Insira **STESample. ConsoleTest** como o nome e clique em **OK**
-   Adicionar uma referência ao projeto **STESample. Entities**

Precisamos de uma referência de serviço para nosso serviço WCF

-   Clique com o botão direito do mouse no projeto **STESample. ConsoleTest** em **Gerenciador de Soluções** e selecione **Adicionar referência de serviço...**
-   Clique em **descobrir**
-   Insira **BloggingService** como o namespace e clique em **OK**

Agora, podemos escrever um código para consumir o serviço.

-   Abra **Program.cs** e substitua o conteúdo pelo código a seguir.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Agora, você pode executar o aplicativo para vê-lo em ação.

-   Clique com o botão direito do mouse no projeto **STESample. ConsoleTest** no **Gerenciador de soluções** e selecione **debug-&gt; iniciar nova instância**

Você verá a saída a seguir quando o aplicativo for executado.

```console
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Consumir o serviço de um aplicativo do WPF

Vamos criar um aplicativo WPF que usa nosso serviço.

-   **Arquivo-&gt; novo&gt; projeto...**
-   Selecione o **Visual C\#** no painel esquerdo e, em seguida, **aplicativo WPF**
-   Insira **STESample. WPFTest** como o nome e clique em **OK**
-   Adicionar uma referência ao projeto **STESample. Entities**

Precisamos de uma referência de serviço para nosso serviço WCF

-   Clique com o botão direito do mouse no projeto **STESample. WPFTest** em **Gerenciador de Soluções** e selecione **Adicionar referência de serviço...**
-   Clique em **descobrir**
-   Insira **BloggingService** como o namespace e clique em **OK**

Agora, podemos escrever um código para consumir o serviço.

-   Abra **MainWindow. XAML** e substitua o conteúdo pelo código a seguir.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Abra o code-behind para MainWindow (**MainWindow.XAML.cs**) e substitua o conteúdo pelo código a seguir

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Agora, você pode executar o aplicativo para vê-lo em ação.

-   Clique com o botão direito do mouse no projeto **STESample. WPFTest** no **Gerenciador de soluções** e selecione **debug-&gt; iniciar nova instância**
-   Você pode manipular os dados usando a tela e salvá-los por meio do serviço usando o botão **salvar**

![Janela principal do WPF](~/ef6/media/wpf.png)
