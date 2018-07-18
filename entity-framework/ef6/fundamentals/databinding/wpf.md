---
title: Associação de dados com o WPF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119829"
---
# <a name="databinding-with-wpf"></a>Associação de dados com o WPF
Este passo a passo mostra como associar controles WPF em um formulário de "master-detail" tipos de POCO. O aplicativo usa as APIs do Entity Framework para preencher os objetos com dados do banco de dados, controlar as alterações e manter os dados no banco de dados.

O modelo define dois tipos que participam no relacionamento um-para-muitos: **categoria** (entidade de segurança\\mestre) e **produto** (dependentes\\detalhes). Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo para os controles do WPF. A estrutura de vinculação de dados do WPF permite a navegação entre objetos relacionados: selecionar linhas no modo de exibição mestre faz com que a exibição de detalhes atualizar com os dados filho correspondentes.

As listagens de código neste passo a passo e capturas de tela são obtidas do Visual Studio 2013, mas você pode concluir este passo a passo com o Visual Studio 2012 ou Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Use a opção de 'Object' para a criação de fontes de dados do WPF

Com a versão anterior do Entity Framework usamos recomendável usar o **banco de dados** opção ao criar uma nova fonte de dados baseado em um modelo criado com o EF Designer. Isso acontecia porque o designer geraria um contexto que é derivada de ObjectContext e classes de entidade derivam de EntityObject. Usando a opção de banco de dados seria ajudá-lo a escrever o código de práticas para interagir com essa superfície de API.

Os Designers do EF para Visual Studio 2012 e Visual Studio 2013 gerar um contexto que é derivada de DbContext, junto com as classes de entidade POCO simples. Com o Visual Studio 2010, é recomendável alternar para um modelo de geração de código que usa DbContext, conforme descrito posteriormente neste passo a passo.

Ao usar a superfície da API DbContext que você deve usar o **objeto** opção ao criar uma nova fonte de dados, conforme mostrado neste passo a passo.

Se necessário, você pode [reverter para a geração de código de ObjectContext com base em](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o EF Designer.

## <a name="pre-requisites"></a>Pré-requisitos

Você precisa ter o Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 instalado para concluir este passo a passo.

Se você estiver usando o Visual Studio 2010, você também precisará instalar o NuGet. Para obter mais informações, consulte [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Criar o aplicativo

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no painel esquerdo e **WPFApplication** no painel à direita
-   Insira **WPFwithEFSample** como o nome
-   Selecione **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instale o pacote NuGet do Entity Framework

-   No Gerenciador de soluções, clique com botão direito no **WinFormswithEFSample** projeto
-   Selecione **gerenciar pacotes NuGet...**
-   Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote
-   Clique em **instalar**  
    >[!NOTE]
    > Uma referência ao DataAnnotations também é adicionada além do assembly EntityFramework. Se o projeto tem uma referência a System, em seguida, ele será removido quando o pacote EntityFramework é instalado. O assembly System não é mais usado para aplicativos do Entity Framework 6.

## <a name="define-a-model"></a>Definir um modelo

Neste passo a passo que você pode optar por implementar um modelo usando o EF Designer ou o Code First. Conclua uma das duas seções a seguir.

### <a name="option-1-define-a-model-using-code-first"></a>Opção 1: Definir um modelo usando o Code First

Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First. Vá para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para reverter a engenharia de seu modelo de banco de dados usando o designer EF

Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio).

-   Adicione uma nova classe para o **WPFwithEFSample:**
    -   Clique com botão direito no nome do projeto
    -   Selecione **adicione**, em seguida, **Novo Item**
    -   Selecione **classe** e insira **produto** para o nome de classe
-   Substitua os **produto** definição com o código a seguir da classe:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação. No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.

Além de definir entidades, você precisa definir uma classe que deriva de DbContext e expõe um DbSet&lt;TEntity&gt; propriedades. O DbSet&lt;TEntity&gt; propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.

-   Adicione um novo **ProductContext** classe ao projeto com a seguinte definição:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Compile o projeto.

### <a name="option-2-define-a-model-using-database-first"></a>Opção 2: Definir um modelo usando Database First

Esta seção mostra como usar o primeiro banco de dados para fazer engenharia reversa seu modelo de banco de dados usando o EF designer. Se você concluiu a seção anterior (**opção 1: definir um modelo usando o Code First)**, ignore esta seção e ir direto para o **o carregamento lento** seção.

#### <a name="create-an-existing-database"></a>Criar um banco de dados existente

Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.

O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) banco de dados.

Vamos prosseguir e gerar o banco de dados.

-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **produtos** como o nome do banco de dados

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**
-   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Modelo de engenharia reversa

Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto -&gt; Adicionar Novo Item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**
-   Insira **ProductModel** como o nome e clique em **Okey**
-   Isso inicia o **Assistente de modelo de dados de entidade**
-   Selecione **gerar a partir do banco de dados** e clique em **Avançar**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Selecione a conexão ao banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de caracteres de conexão e clique **Avançar**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer. Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.

#### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, em seguida, você precisará atualizar o EF designer para usar a geração de código do EF6.

-   Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**
-   Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**
-   Selecione o **EF 6.x gerador DbContext para C\#,** insira **ProductsModel** como o nome e clique em Adicionar

#### <a name="updating-code-generation-for-data-binding"></a>Atualizando a geração de código para a associação de dados

O EF gera o código de seu modelo usando os modelos T4. Os modelos fornecidos com o Visual Studio ou baixado na Galeria do Visual Studio são destinados a uso de finalidade geral. Isso significa que as entidades geradas a partir desses modelos tem simple ICollection&lt;T&gt; propriedades. No entanto, ao fazer a dados de associação usando o WPF é desejável usar **ObservableCollection** para propriedades de coleção para que o WPF pode manter controle das alterações feitas às coleções. Para esse fim, vamos modificar os modelos para usar ObservableCollection.

-   Abra o **Gerenciador de soluções** e localize **ProductModel.edmx** arquivo
-   Localizar o **ProductModel.tt** arquivo que será aninhado sob o arquivo ProductModel.edmx

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio
-   Localizar e substituir as duas ocorrências de "**ICollection**"com"**ObservableCollection**". Eles estão localizados aproximadamente em linhas 296 e 484.
-   Localizar e substituir a primeira ocorrência de "**HashSet**"com"**ObservableCollection**". Essa ocorrência está localizada aproximadamente na linha 50. **Não** substituir a segunda ocorrência da HashSet encontrada posteriormente no código.
-   Localizar e substituir a ocorrência única de "**Collections**"com"**ObjectModel**". Isso está localizado aproximadamente na linha 424.
-   Salve o arquivo ProductModel.tt. Isso deve fazer com que o código para entidades sejam regenerados. Se o código não regenerar automaticamente, em seguida, clique com botão direito em ProductModel.tt e escolha "Executar ferramenta personalizada".

Se você agora, abra o arquivo de Category.cs (que é aninhado sob ProductModel.tt) e em seguida, você deverá ver a coleção de produtos tem o tipo **ObservableCollection&lt;produto&gt;**.

Compile o projeto.

## <a name="lazy-loading"></a>Carregamento lento

O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação. No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.

O EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação. Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que a primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo não ainda estiverem no contexto.

Ao usar tipos de entidade POCO, o EF atinge o carregamento lento criando instâncias de tipos de proxy derivado durante o tempo de execução e, em seguida, substituindo propriedades virtuais nas suas classes para adicionar o gancho do carregamento. Para obter o carregamento lento de objetos relacionados, você deve declarar navegação getters de propriedade como **pública** e **virtual** (**Overridable** no Visual Basic), e você classe não deve ser **lacrado** (**NotOverridable** no Visual Basic). Quando o banco de dados usando as propriedades de navegação primeiro são feitas automaticamente virtuais para habilitar o carregamento lento. Na seção Code First que escolhemos tornar as propriedades de navegação virtual pelo mesmo motivo

## <a name="bind-object-to-controls"></a>Associar o objeto a controles

Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo do WPF.

-   Clique duas vezes em **MainWindow. XAML** no Gerenciador de soluções para abrir o formulário principal
-   No menu principal, selecione **projeto -&gt; adicionar nova fonte de dados...**
    (no Visual Studio 2010, você precisa selecionar **dados -&gt; adicionar nova fonte de dados...** )
-   Na escolha um Typewindow de fonte de dados, selecione **objeto** e clique em **Avançar**
-   Na caixa de diálogo de objetos de dados selecione, desdobrar o **WPFwithEFSample** duas vezes e selecione **categoria**  
    *Não é necessário selecionar o **produto** fonte de dados, porque obteremos a ele por meio o **produto**da propriedade no **categoria** fonte de dados*  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   Clique em **concluir.**
-   A janela fontes de dados é aberta ao lado da janela de MainWindow. XAML *se a janela fontes de dados não está aparecendo, selecione **exibição -&gt; outras Windows -&gt; fontes de dados***
-   Pressione o ícone de pino, portanto, a janela fontes de dados não auto ocultar. Você talvez precise clicar no botão de atualização se a janela estiver visível.

    ![Fontes de dados](~/ef6/media/datasources.png)

-   Selecione o * * categoria * * fonte de dados e arraste-o no formulário.

O seguinte aconteceu quando foi arrastado essa fonte:

-   O **categoryViewSource** recursos e o * * categoryDataGrid * * o controle foram adicionados ao XAML. Para obter mais informações sobre DataViewSources, consulte http://bea.stollnitz.com/blog/?p=387.
-   A propriedade DataContext no elemento de grade pai foi definida como "{StaticResource **categoryViewSource** }".  O **categoryViewSource** recurso serve como uma origem da associação para o exterior\\elemento de grade pai. Os elementos internos de grade, em seguida, herdam o valor de DataContext do pai (a propriedade de ItemsSource do categoryDataGrid é definida como "{Binding}") de grade. 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Adicionar uma grade de detalhes

Agora que temos uma grade para exibir categorias vamos adicionar uma grade de detalhes para exibir os produtos associados.

-   Selecione o * * produtos * * propriedade no * * categoria * * fonte de dados e arraste-o no formulário.
    -   O **categoryProductsViewSource** recursos e **productDataGrid** grade são adicionados ao XAML
    -   O caminho de associação para este recurso é definido como produtos
    -   Estrutura de vinculação de dados do WPF garante que somente os produtos relacionados à categoria selecionada aparecem no **productDataGrid**
-   Na caixa de ferramentas, arraste **botão** para o formulário. Definir a **nome** propriedade a ser **buttonSave** e o **conteúdo** propriedade **salvar**.

O formulário deve ser semelhante a este:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Adicione o código que trata a interação de dados

É hora de adicionar alguns manipuladores de eventos para a janela principal.

-   Na janela do XAML, clique no  **&lt;janela** elemento, isso seleciona a janela principal
-   No **propriedades** janela escolher **eventos** no canto superior direito, clique duas vezes para a direita da caixa de texto a **Loaded** rótulo

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   Também adicione a **clique em** evento para o **salvar** botão clicando duas vezes no botão Salvar no designer. 

Isso leva você ao code-behind para o formulário, agora vamos editar o código para usar o ProductContext para executar o acesso a dados. Atualize o código para a MainWindow, conforme mostrado abaixo.

O código declara uma instância de execução longa da **ProductContext**. O **ProductContext** objeto é usado para consultar e salvar dados no banco de dados. O **Dispose**() na **ProductContext** instância é chamada de substituído **OnClosing** método. Os comentários do código fornecem detalhes sobre o que o código faz.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Testar o aplicativo do WPF

-   Compile e execute o aplicativo. Se você usou o Code First e, em seguida, você verá que um **WPFwithEFSample.ProductContext** banco de dados é criado para você.
-   Insira um nome de categoria nos nomes de produto e de grade principais na grade inferior *não inserir nada nas colunas de ID, como a chave primária é gerada pelo banco de dados*

    ![Screen1](~/ef6/media/screen1.png)

-   Pressione a **salvar** botão para salvar os dados no banco de dados

Após a chamada para do DbContext **SaveChanges**(), as IDs são preenchidas com os valores de banco de dados gerado. Pois chamamos **Refresh**() após **SaveChanges**() a **DataGrid** controles são atualizados com os novos valores.

![Screen2](~/ef6/media/screen2.png)
