---
title: DataBinding com WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416592"
---
# <a name="databinding-with-wpf"></a>Associação de dados com o WPF
Este guia passo a passo mostra como associar tipos POCO a controles WPF em um formulário de "mestre-detalhes". O aplicativo usa as APIs de Entity Framework para preencher objetos com dados do banco de dado, controlar alterações e manter dados no banco de dado.

O modelo define dois tipos que participam de uma relação um-para-muitos: **Category** (principal\\mestre) e **Product** (dependente\\detalhe). Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo aos controles do WPF. A estrutura de associação de dados do WPF permite a navegação entre objetos relacionados: a seleção de linhas no modo de exibição mestre faz com que a exibição de detalhes seja atualizada com os dados filho correspondentes.

As capturas de tela e as listagens de código neste passo a passos são obtidas de Visual Studio 2013, mas você pode concluir este passo a passos com o Visual Studio 2012 ou o Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Usar a opção ' Object ' para criar fontes de dados do WPF

Com a versão anterior do Entity Framework, usamos para recomendar o uso da opção **Database** ao criar uma nova fonte de dados com base em um modelo criado com o designer do EF. Isso ocorreu porque o designer geraria um contexto derivado de ObjectContext e classes de entidade derivadas de EntityObject. Usar a opção de banco de dados ajudará você a escrever o melhor código para interagir com essa superfície de API.

Os designers do EF para Visual Studio 2012 e Visual Studio 2013 geram um contexto que deriva de DbContext junto com classes de entidade POCO simples. Com o Visual Studio 2010, recomendamos alternar para um modelo de geração de código que usa DbContext, conforme descrito mais adiante neste guia.

Ao usar a superfície da API DbContext, você deve usar a opção **Object** ao criar uma nova fonte de dados, conforme mostrado neste passo a passos.

Se necessário, você pode [reverter para a geração de código baseada em ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o designer do EF.

## <a name="pre-requisites"></a>Pré-requisitos

Você precisa ter Visual Studio 2013, o Visual Studio 2012 ou o Visual Studio 2010 instalado para concluir este passo a passos.

Se você estiver usando o Visual Studio 2010, também precisará instalar o NuGet. Para obter mais informações, consulte [instalando o NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Criar o aplicativo

-   Abra o Visual Studio
-   **Arquivo-&gt; novo&gt; projeto....**
-   Selecione do **Windows** no painel esquerdo e **WPFApplication** no painel direito
-   Insira **WPFwithEFSample** como o nome
-   Selecione **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalar o pacote NuGet Entity Framework

-   Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **WinFormswithEFSample**
-   Selecione **gerenciar pacotes NuGet...**
-   Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**
-   Clique em **Instalar**  
    >[!NOTE]
    > Além do assembly do EntityFramework, uma referência a System. ComponentModel. Annotations também é adicionada. Se o projeto tiver uma referência a System. Data. Entity, ele será removido quando o pacote do EntityFramework for instalado. O assembly System. Data. Entity não é mais usado para aplicativos Entity Framework 6.

## <a name="define-a-model"></a>Definir um modelo

Neste tutorial, você pode optar por implementar um modelo usando Code First ou o designer do EF. Conclua uma das duas seções a seguir.

### <a name="option-1-define-a-model-using-code-first"></a>Opção 1: definir um modelo usando Code First

Esta seção mostra como criar um modelo e seu banco de dados associado usando Code First. Pule para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF

Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).

-   Adicione uma nova classe ao **WPFwithEFSample:**
    -   Clique com o botão direito do mouse no nome do projeto
    -   Selecione **Adicionar**e **novo item**
    -   Selecione **classe** e insira do **produto** para o nome da classe
-   Substitua o **produto** definição de classe pelo seguinte código:

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

A propriedade **Products** na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação. Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.

Além de definir entidades, você precisa definir uma classe derivada de DbContext e expõe DbSet&lt;TEntity&gt; Propriedades. As propriedades DbSet&lt;TEntity&gt; permitem que o contexto saiba quais tipos você deseja incluir no modelo.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.

-   Adicione uma nova classe **ProductContext** ao projeto com a seguinte definição:

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

### <a name="option-2-define-a-model-using-database-first"></a>Opção 2: definir um modelo usando Database First

Esta seção mostra como usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF. Se você concluiu a seção anterior (**opção 1: definir um modelo usando Code First)** , ignore esta seção e vá direto para a seção **carregamento lento** .

#### <a name="create-an-existing-database"></a>Criar um banco de dados existente

Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Vamos continuar e gerar o banco de dados.

-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou e insira **produtos** como o nome do banco de dados

    ![Adicionar LocalDB de conexão](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar conexão Express](~/ef6/media/addconnectionexpress.png)

-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**
-   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**

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

Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto-&gt; adicionar novo item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**
-   Insira **ProductModel** como o nome e clique em **OK**
-   Isso iniciará o **Assistente de modelo de dados de entidade**
-   Selecione **gerar do banco de dados** e clique em **Avançar**

    ![Escolher conteúdo do modelo](~/ef6/media/choosemodelcontents.png)

-   Selecione a conexão com o banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de conexão e clique em **Avançar**

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer. Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.

#### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, será necessário atualizar o designer do EF para usar a geração de código EF6.

-   Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**
-   Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**
-   Selecione o **gerador de DbContext do EF 6. x para C\#,** insira **ProductsModel** como o nome e clique em Adicionar

#### <a name="updating-code-generation-for-data-binding"></a>Atualizando geração de código para associação de dados

O EF gera código do seu modelo usando modelos T4. Os modelos fornecidos com o Visual Studio ou baixados da galeria do Visual Studio são destinados para uso geral. Isso significa que as entidades geradas a partir desses modelos têm propriedades ICollection&lt;&gt; simples. No entanto, ao fazer a vinculação de dados usando o WPF, é desejável usar **ObservableCollection** para propriedades de coleção para que o WPF possa controlar as alterações feitas nas coleções. Para esse fim, iremos modificar os modelos para usar ObservableCollection.

-   Abra o **Gerenciador de soluções** e localize o arquivo **ProductModel. edmx**
-   Localize o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel. edmx

    ![Modelo de modelo de produto do WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio
-   Localizar e substituir as duas ocorrências de "**ICollection**" por "**ObservableCollection**". Eles estão localizados aproximadamente nas linhas 296 e 484.
-   Localizar e substituir a primeira ocorrência de "**HashSet**" por "**ObservableCollection**". Essa ocorrência está localizada aproximadamente na linha 50. **Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.
-   Localize e substitua a única ocorrência de "**System. Collections. Generic**" por "**System. Collections. ObjectModel**". Isso está localizado aproximadamente na linha 424.
-   Salve o arquivo ProductModel.tt. Isso deve fazer com que o código das entidades seja regenerado. Se o código não for regenerado automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "executar ferramenta personalizada".

Se agora você abrir o arquivo Category.cs (que está aninhado em ProductModel.tt), verá que a coleção Products tem o tipo **observablecollection&lt;&gt;do produto** .

Compile o projeto.

## <a name="lazy-loading"></a>Carregamento lento

A propriedade **Products** na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação. Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.

O EF oferece a você uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação. Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que na primeira vez que você acessar cada propriedade de navegação, uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.

Ao usar tipos de entidade POCO, o EF alcança o carregamento lento criando instâncias de tipos de proxy derivados durante o tempo de execução e, em seguida, substituindo as propriedades virtuais em suas classes para adicionar o gancho de carregamento. Para obter o carregamento lento de objetos relacionados, você deve declarar getters de propriedade de navegação como **público** e **virtual** (**substituível** em Visual Basic) e a classe não deve ser **selada** (não**substituível** em Visual Basic). Ao usar Database First Propriedades de navegação são automaticamente disponibilizadas para habilitar o carregamento lento. Na seção Code First, optamos por tornar as propriedades de navegação virtuais pela mesma razão

## <a name="bind-object-to-controls"></a>Associar objeto a controles

Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo WPF.

-   Clique duas vezes em **MainWindow. XAML** em Gerenciador de soluções para abrir o formulário principal
-   No menu principal, selecione **projeto-&gt; Adicionar nova fonte de dados...**
    (no Visual Studio 2010, você precisa selecionar **dados-&gt; Adicionar nova fonte de dados...** )
-   Em escolher uma fonte de dados Typewindow, selecione **objeto** e clique em **Avançar**
-   Na caixa de diálogo Selecionar os objetos de dados, desdobrar o **WPFwithEFSample** duas vezes e selecione **categoria**  
    *Não é necessário selecionar a fonte de dados do **produto** , pois vamos chegar a ela por meio da Propriedade do **produto**na fonte de dados **Category***  

    ![Selecionar objetos de dados](~/ef6/media/selectdataobjects.png)

-   Clique em **Concluir.**
-   A janela fontes de dados será aberta ao lado da janela MainWindow. XAML *se a janela fontes de dados não estiver aparecendo, selecione **exibir-&gt; outras fontes de dados de&gt; do Windows***
-   Pressione o ícone de pino para que a janela fontes de dados não seja ocultada automaticamente. Talvez seja necessário pressionar o botão atualizar se a janela já estava visível.

    ![Fontes de dados](~/ef6/media/datasources.png)

-   Selecione a fonte de dados **categoria** e arraste-a no formulário.

O seguinte ocorreu quando arrastamos esta fonte:

-   O recurso **categoryViewSource** e o controle **categoryDataGrid** foram adicionados ao XAML 
-   A Propriedade DataContext no elemento pai Grid foi definida como "{StaticResource **categoryViewSource** }". O recurso **categoryViewSource** serve como uma fonte de associação para o elemento de grade pai de\\externa. Os elementos da grade interna herdam o valor de DataContext da grade pai (a propriedade ItemsSource de categoryDataGrid é definida como "{Binding}")

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

## <a name="adding-a-details-grid"></a>Adicionando uma grade de detalhes

Agora que temos uma grade para exibir categorias, vamos adicionar uma grade de detalhes para exibir os produtos associados.

-   Selecione a propriedade **Products** na fonte de dados **Category** e arraste-a no formulário.
    -   O recurso **categoryProductsViewSource** e a grade **productDataGrid** são adicionados ao XAML
    -   O caminho de associação deste recurso está definido como produtos
    -   A estrutura de vinculação de dados do WPF garante que somente os produtos relacionados à categoria selecionada sejam exibidos no **productDataGrid**
-   Na caixa de ferramentas, arraste o **botão** para o formulário. Defina a propriedade **Name** como **buttonSave** e a propriedade **Content** para **salvar**.

O formulário deve ser semelhante a este:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Adicionar código que manipula a interação de dados

É hora de adicionar alguns manipuladores de eventos à janela principal.

-   Na janela XAML, clique no elemento de **janela&lt;** , isso seleciona a janela principal
-   Na janela **Propriedades** , escolha **eventos** no canto superior direito e clique duas vezes na caixa de texto à direita do rótulo **carregado**

    ![Propriedades da janela principal](~/ef6/media/mainwindowproperties.png)

-   Além disso, adicione o evento de **clique** para o botão **salvar** clicando duas vezes no botão salvar no designer. 

Isso o levará ao code-behind para o formulário. agora, vamos editar o código para usar o ProductContext para executar o acesso a dados. Atualize o código para MainWindow, conforme mostrado abaixo.

O código declara uma instância de execução longa do **ProductContext**. O objeto **ProductContext** é usado para consultar e salvar dados no banco de dado. Em seguida, **Dispose ()** na instância **ProductContext** é chamado do método **OnClosing** substituído. Os comentários de código fornecem detalhes sobre o que o código faz.

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

-   Compile e execute o aplicativo. Se você usou Code First, verá que um banco de dados **WPFwithEFSample. ProductContext** é criado para você.
-   Insira um nome de categoria na grade superior e os nomes de produtos na grade inferior *não insiram nada em colunas de ID, porque a chave primária é gerada pelo banco de dados*

    ![Janela principal com novas categorias e produtos](~/ef6/media/screen1.png)

-   Pressione o botão **salvar** para salvar os dados no banco de dado

Após a chamada para **SaveChanges ()** do DbContext, as IDs são populadas com os valores gerados pelo banco de dados. Como chamamos a **atualização ()** após **SaveChanges ()** , os controles **DataGrid** também são atualizados com os novos valores.

![Janela principal com IDs populadas](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Recursos adicionais

Para saber mais sobre a ligação de dados com coleções usando o WPF, consulte [Este tópico](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) na documentação do WPF.  
