---
title: Passo a passo de entidades - EF6 de autocontrole
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
caps.latest.revision: 3
ms.openlocfilehash: d07ae727ffba60a9296b34b9261acd99f7e8e3b6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119840"
---
# <a name="self-tracking-entities-walkthrough"></a>Passo a passo entidades de autocontrole
> [!IMPORTANT]
> Não recomendamos usar o modelo de entidades de rastreamento automático. Ele continuará disponível apenas para dar suporte aos aplicativos existentes. Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](http://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.

Este passo a passo demonstra o cenário no qual um serviço do Windows Communication Foundation (WCF) expõe uma operação que retorna um gráfico de entidade. Em seguida, um aplicativo cliente manipula esse gráfico e envia as modificações em uma operação de serviço que valida e salva as atualizações em um banco de dados usando o Entity Framework.

Antes de concluir este passo a passo, não deixe de ler o [entidades de autorrastreamento](index.md) página.

Este passo a passo conclui as seguintes ações:

-   Cria um banco de dados para acessar.
-   Cria uma biblioteca de classes que contém o modelo.
-   Trocas do modelo de rastreamento automático gerador de entidade.
-   Move as classes de entidade para um projeto separado.
-   Cria um serviço WCF que expõe as operações para consultar e salvar as entidades.
-   Cria aplicativos (Console e WPF) que consomem o serviço de cliente.

Vamos usar banco de dados primeiro neste passo a passo, mas as mesmas técnicas se aplicam igualmente ao Model First.

## <a name="pre-requisites"></a>Pré-requisitos

Para concluir este passo a passo, você precisará uma versão recente do Visual Studio.

## <a name="create-a-database"></a>Criar um banco de dados

O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2012, em seguida, você criará um banco de dados LocalDB.
-   Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.

Vamos prosseguir e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados
-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado
-   Insira **STESample** como o nome do banco de dados
-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**
-   O novo banco de dados agora aparecerão no Gerenciador de servidores
-   Se você estiver usando o Visual Studio 2012
    -   Clique com botão direito no banco de dados no Gerenciador de servidores e selecione **nova consulta**
    -   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**
-   Se você estiver usando o Visual Studio 2010
    -   Selecione **dados -&gt; Transact SQL Editor -&gt; nova Conexão de consulta...**
    -   Insira **.\\ SQLEXPRESS** como o nome do servidor e clique em **Okey**
    -   Selecione o **STESample** de banco de dados na lista suspensa na parte superior do editor de consultas
    -   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **executar SQL**

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

Primeiro, precisamos colocar o modelo em um projeto.

-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **biblioteca de classes**
-   Insira **STESample** como o nome e clique em **Okey**

Agora vamos criar um modelo simples no EF Designer para acessar nosso banco de dados:

-   **Projeto -&gt; Adicionar Novo Item...**
-   Selecione **dados** no painel esquerdo e, em seguida, **modelo de dados de entidade ADO.NET**
-   Insira **BloggingModel** como o nome e clique em **Okey**
-   Selecione **gerar a partir do banco de dados** e clique em **Avançar**
-   Insira as informações de conexão para o banco de dados que você criou na seção anterior
-   Insira **BloggingContext** como o nome para a cadeia de caracteres de conexão e clique em **Avançar**
-   A caixa de seleção ao lado **tabelas** e clique em **concluir**

## <a name="swap-to-ste-code-generation"></a>Alterne para a geração de código lar

Agora, é necessário desabilitar a geração de código padrão e a troca para entidades de autorrastreamento.

### <a name="if-you-are-using-visual-studio-2012"></a>Se você estiver usando o Visual Studio 2012

-   Expandir **BloggingModel.edmx** na **Gerenciador de soluções** e exclua os **BloggingModel.tt** e **BloggingModel.Context.tt** 
     *Isso desabilitará a geração de código padrão*
-   Clique com botão direito no EF Designer na superfície e selecione uma área vazia **Adicionar Item de geração de código...**
-   Selecione **Online** no painel esquerdo e procure para **gerador lar**
-   Selecione o **lar gerador para C\#**  modelo, insira **STETemplate** como o nome e clique em **adicionar**
-   O **STETemplate.tt** e **STETemplate.Context.tt** arquivos são adicionados aninhado sob o arquivo BloggingModel.edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Se você estiver usando o Visual Studio 2010

-   Clique com botão direito no EF Designer na superfície e selecione uma área vazia **Adicionar Item de geração de código...**
-   Selecione **código** no painel esquerdo e, em seguida, **gerador de entidade de rastreamento automático do ADO.NET**
-   Insira **STETemplate** como o nome e clique em **adicionar**
-   O **STETemplate.tt** e **STETemplate.Context.tt** arquivos são adicionados diretamente ao seu projeto

## <a name="move-entity-types-into-separate-project"></a>Mover os tipos de entidade em um projeto separado

Para usar entidades de autorrastreamento nosso aplicativo cliente precisa acessar as classes de entidade gerados a partir de nosso modelo. Porque não queremos expor o modelo inteiro para o aplicativo cliente, vamos para mover as classes de entidade em um projeto separado.

A primeira etapa é pare de gerar classes de entidade no projeto existente:

-   Clique duas vezes em **STETemplate.tt** na **Gerenciador de soluções** e selecione **propriedades**
-   No **propriedades** janela clara **TextTemplatingFileGenerator** do **CustomTool** propriedade
-   Expandir **STETemplate.tt** na **Gerenciador de soluções** e excluir todos os arquivos aninhados sob ela

Em seguida, vamos adicionar um novo projeto e gerar as classes de entidade nele

-   **Arquivo -&gt; Add -&gt; projeto...**
-   Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **biblioteca de classes**
-   Insira **STESample.Entities** como o nome e clique em **Okey**
-   **Projeto -&gt; Adicionar Item existente...**
-   Navegue até a **STESample** pasta do projeto
-   Selecione para exibir **todos os arquivos (\*.\*)**
-   Selecione o **STETemplate.tt** arquivo
-   Clique na seta suspensa ao lado de **Add** botão e selecione **adicionar como vínculo**

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

Também vamos para garantir que as classes de entidade obterem geradas no mesmo namespace que o contexto. Isso apenas reduz o número de instruções, precisamos adicionar em todo o nosso aplicativo de uso.

-   Clique duas vezes em vinculada **STETemplate.tt** na **Gerenciador de soluções** e selecione **propriedades**
-   No **propriedades** janela conjunto **Namespace de ferramenta personalizada** para **STESample**

O código gerado pelo modelo lar precisará de uma referência a **Serialization** fim de compilar. Essa biblioteca é necessária para o WCF **DataContract** e **DataMember** atributos que são usados nos tipos de entidade serializável.

-   Clique com botão direito do **STESample.Entities** project no **Gerenciador de soluções** e selecione **adicionar referência...**
    -   No Visual Studio 2012 – marque a caixa ao lado **Serialization** e clique em **Okey**
    -   No Visual Studio 2010 - selecione **Serialization** e clique em **Okey**

Por fim, o projeto com o nosso contexto em que ele precisa de uma referência para os tipos de entidade.

-   Clique com botão direito do **STESample** project no **Gerenciador de soluções** e selecione **adicionar referência...**
    -   No Visual Studio 2012 – selecione **Solution** no painel esquerdo, marque a caixa ao lado de **STESample.Entities** e clique em **Okey**
    -   No Visual Studio 2010 – selecione o **projetos** guia, selecione **STESample.Entities** e clique em **Okey**

>[!NOTE]
> Outra opção para mover os tipos de entidade para um projeto separado é mover o arquivo de modelo, em vez de vinculá-lo do local padrão. Se você fizer isso, você precisará atualizar o **arquivo_de_entrada** variável no modelo para fornecer o caminho relativo para o arquivo edmx (neste exemplo que seriam **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Criar um serviço WCF

Agora é hora de adicionar um serviço WCF para expor nossos dados, vamos começar pela criação do projeto.

-   **Arquivo -&gt; Add -&gt; projeto...**
-   Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo de serviço WCF**
-   Insira **STESample.Service** como o nome e clique em **Okey**
-   Adicione uma referência para o **Entity** assembly
-   Adicione uma referência para o **STESample** e **STESample.Entities** projetos

É preciso copiar a cadeia de caracteres de conexão do EF para este projeto para que ele é encontrado em tempo de execução.

-   Abra o **App. config** do arquivo para o * * STESample * * projeto e copie o **connectionStrings** elemento
-   Colar a **connectionStrings** como um elemento filho da **configuration** elemento do **Web. config** de arquivo no **STESample.Service** projeto

Agora é hora de implementar o serviço real.

-   Abra **IService1.cs** e substitua o conteúdo com o código a seguir

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

-   Abra **Service1.svc** e substitua o conteúdo com o código a seguir

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

## <a name="consume-the-service-from-a-console-application"></a>Consumir o serviço de um aplicativo de Console

Vamos criar um aplicativo de console que usa o nosso serviço.

-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo de Console**
-   Insira **STESample.ConsoleTest** como o nome e clique em **Okey**
-   Adicione uma referência para o **STESample.Entities** projeto

Precisamos de uma referência de serviço para nosso serviço WCF

-   Clique com botão direito do **STESample.ConsoleTest** project no **Gerenciador de soluções** e selecione **adicionar referência de serviço...**
-   Clique em **descobrir**
-   Insira **BloggingService** como o namespace e clique em **Okey**

Agora podemos escrever algum código para consumir o serviço.

-   Abra **Program.cs** e substitua o conteúdo com o código a seguir.

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

-   Clique com botão direito do **STESample.ConsoleTest** project no **Gerenciador de soluções** e selecione **Debug -&gt; iniciar nova instância**

Você verá a seguinte saída quando o aplicativo é executado.

```
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

## <a name="consume-the-service-from-a-wpf-application"></a>Consumir o serviço de um aplicativo WPF

Vamos criar um aplicativo WPF que usa o nosso serviço.

-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Visual C#\#**  no painel esquerdo e, em seguida, **aplicativo WPF**
-   Insira **STESample.WPFTest** como o nome e clique em **Okey**
-   Adicione uma referência para o **STESample.Entities** projeto

Precisamos de uma referência de serviço para nosso serviço WCF

-   Clique com botão direito do **STESample.WPFTest** project no **Gerenciador de soluções** e selecione **adicionar referência de serviço...**
-   Clique em **descobrir**
-   Insira **BloggingService** como o namespace e clique em **Okey**

Agora podemos escrever algum código para consumir o serviço.

-   Abra **MainWindow. XAML** e substitua o conteúdo com o código a seguir.

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

-   Abra o code-behind para MainWindow (**MainWindow.xaml.cs**) e substitua o conteúdo com o código a seguir

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

-   Clique com botão direito do **STESample.WPFTest** project no **Gerenciador de soluções** e selecione **Debug -&gt; iniciar nova instância**
-   Você pode manipular os dados usando a tela e salvá-lo por meio do serviço usando o **salvar** botão

![WPF](~/ef6/media/wpf.png)
