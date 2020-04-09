---
title: Vinculação de dados com WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639137"
---
> [!IMPORTANT]
> **Este documento é válido apenas para WPF no Quadro .NET**
>
> Este documento descreve a vinculação de dados para WPF no Quadro .NET. Para novos projetos .NET Core, recomendamos que você use [o EF Core](/ef/core) em vez do Entity Framework 6. A documentação para vinculação de dados no EF Core é rastreada no [#778 de Problemas](https://github.com/dotnet/EntityFramework.Docs/issues/778).

# <a name="databinding-with-wpf"></a>Associação de dados com WPF
Este passo a passo mostra como vincular os tipos POCO aos controles WPF em um formulário "master-detail". O aplicativo usa as APIs do Framework da entidade para preencher objetos com dados do banco de dados, rastrear alterações e persistir dados no banco de dados.

O modelo define dois tipos que participam de uma relação\\de um a muitos: **Categoria** (master principal) e **Produto** (detalhe dependente).\\ Em seguida, as ferramentas do Visual Studio são usadas para vincular os tipos definidos no modelo aos controles WPF. A estrutura de vinculação de dados do WPF permite a navegação entre objetos relacionados: selecionar linhas na exibição mestre faz com que a visualização de detalhes seja atualizada com os dados de filho correspondentes.

As capturas de tela e as listas de código neste passo a passo são tiradas do Visual Studio 2013, mas você pode completar este passo a passo com o Visual Studio 2012 ou visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Use a opção 'Objeto' para criar fontes de dados WPF

Com a versão anterior do Entity Framework, costumávamos recomendar o uso da opção **Banco de Dados** ao criar uma nova Fonte de Dados baseada em um modelo criado com o EF Designer. Isso porque o designer geraria um contexto derivado do ObjectContext e das classes de entidade susobtidas derivadas do EntityObject. Usar a opção Banco de Dados ajudaria a escrever o melhor código para interagir com essa superfície de API.

Os EF Designers for Visual Studio 2012 e Visual Studio 2013 geram um contexto que deriva do DbContext juntamente com classes simples de entidadePOCO. Com o Visual Studio 2010 recomendamos a troca por um modelo de geração de código que usa o DbContext como descrito mais tarde neste passo a passo.

Ao usar a superfície da API dbContext, você deve usar a opção **Objeto** ao criar uma nova Fonte de Dados, como mostrado neste passo a passo.

Se necessário, você pode [reverter para a geração de código baseado em ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para modelos criados com o EF Designer.

## <a name="pre-requisites"></a>Pré-requisitos

Você precisa ter o Visual Studio 2013, visual studio 2012 ou Visual Studio 2010 instalado para completar este passo a passo.

Se você estiver usando o Visual Studio 2010, você também tem que instalar o NuGet. Para obter mais informações, consulte [Instalando NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Criar o aplicativo

-   Abra o Visual Studio
-   **Arquivo&gt; -&gt; Novo - Projeto....**
-   Selecione **Windows** no painel esquerdo e **WPFApplication** no painel direito
-   Digite **WPFwithEFSample** como o nome
-   Selecione **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instale o pacote NuGet do Framework da entidade

-   No Solution Explorer, clique com o botão direito do mouse no projeto **WinFormswithEFSample**
-   Selecione **Gerenciar pacotes NuGet...**
-   Na caixa de diálogo Gerenciar pacotes NuGet, selecione a guia **On-line** e escolha o pacote **EntityFramework**
-   Clique **em Instalar**  
    >[!NOTE]
    > Além do conjunto EntityFramework, também é adicionada uma referência ao System.ComponentModel.DataAnnotações. Se o projeto tiver uma referência ao System.Data.Entity, ele será removido quando o pacote EntityFramework estiver instalado. O conjunto System.Data.Entity não é mais usado para aplicações do Quadro de Entidades 6.

## <a name="define-a-model"></a>Definir um modelo

Neste passo a passo você pode optar por implementar um modelo usando o Code First ou o EF Designer. Complete uma das duas seções seguintes.

### <a name="option-1-define-a-model-using-code-first"></a>Opção 1: Defina um modelo usando o Código Primeiro

Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First. Pule para a próxima seção **(Opção 2: Defina um modelo usando o banco de dados primeiro)** se você preferir usar o Banco de Dados Primeiro para fazer engenharia reversa do seu modelo a partir de um banco de dados usando o designer Da EF

Ao usar o desenvolvimento do Code First, você geralmente começa escrevendo classes .NET Framework que definem seu modelo conceitual (domínio).

-   Adicionar uma nova classe ao **WPFwithEFSample:**
    -   Clique com o botão direito do mouse no nome do projeto
    -   Selecione **Adicionar,** em seguida, **novo item**
    -   Selecione **Classe** e digite **Produto** para o nome da classe
-   Substitua a definição de classe **de produto** pelo seguinte código:

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

A **Products** propriedade Produtos **na** **categoria** classe e categoria de propriedade na classe **Produto** são propriedades de navegação. No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar uma relação entre dois tipos de entidades.

Além de definir entidades, você precisa definir uma classe que deriva&lt;do&gt; DbContext e expõe as propriedades do DbSet TEntity. As propriedades&lt;DbSet TEntity&gt; indizem o contexto para saber quais tipos você deseja incluir no modelo.

Uma instância do tipo derivado do DbContext gerencia os objetos da entidade durante o tempo de execução, o que inclui preencher objetos com dados de um banco de dados, alterar o rastreamento e persistir dados no banco de dados.

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

### <a name="option-2-define-a-model-using-database-first"></a>Opção 2: Defina um modelo usando o banco de dados primeiro

Esta seção mostra como usar o Banco de Dados Primeiro para fazer engenharia reversa do seu modelo a partir de um banco de dados usando o designer EF. Se você completou a seção anterior **(Opção 1: Defina um modelo usando o Código Primeiro)**, então pule esta seção e vá direto para a seção **Carga Preguiçosa.**

#### <a name="create-an-existing-database"></a>Criar um banco de dados existente

Normalmente, quando você está mirando em um banco de dados existente ele já será criado, mas para este passo a passo precisamos criar um banco de dados para acessar.

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2010, você estará criando um banco de dados SQL Express.
-   Se você estiver usando o Visual Studio 2012, então você estará criando um banco de dados [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Vamos em frente e gerar o banco de dados.

-   **Ver&gt; - Explorador do servidor**
-   Clique com o botão direito do mouse em **Conexões de Dados -&gt; Adicionar Conexão...**
-   Se você não tiver conectado a um banco de dados do Server Explorer antes de precisar selecionar o Microsoft SQL Server como fonte de dados

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual você instalou e **insira Produtos** como o nome do banco de dados

    ![Adicionar conexão LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar expresso de conexão](~/ef6/media/addconnectionexpress.png)

-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

-   O novo banco de dados agora aparecerá no Server Explorer, clique com o botão direito do mouse nele e selecione **Nova consulta**
-   Copie o SQL a seguir na nova consulta e clique com o botão direito do mouse na consulta e selecione **Executar**

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

#### <a name="reverse-engineer-model"></a>Modelo de Engenheiro Reverso

Vamos usar o Entity Framework Designer, que está incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto&gt; - Adicionar Novo Item...**
-   Selecione **Dados** no menu esquerdo e, em seguida, **ADO.NET Modelo de Dados da Entidade**
-   Digite **ProductModel** como o nome e clique em **OK**
-   Isso lança o **Assistente de Modelo de Dados de Entidade**
-   Selecione **Gerar no banco de dados** e clique em **Avançar**

    ![Escolher conteúdo do modelo](~/ef6/media/choosemodelcontents.png)

-   Selecione a conexão com o banco de dados criado na primeira seção, digite **ProductContext** como o nome da seqüência de conexões e clique em **Avançar**

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

Uma vez que o processo de engenharia reversa é concluído, o novo modelo é adicionado ao seu projeto e aberto para você visualizar no Entity Framework Designer. Um arquivo App.config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.

#### <a name="additional-steps-in-visual-studio-2010"></a>Passos adicionais no Visual Studio 2010

Se você está trabalhando no Visual Studio 2010, então você precisará atualizar o designer Da EF para usar a geração de código EF6.

-   Clique com o botão direito do mouse em um local vazio do seu modelo no EF Designer e selecione **Adicionar item de geração de código...**
-   Selecione **Modelos Online** no menu esquerdo e pesquise por **DbContext**
-   Selecione o **Gerador EF\#6.x DbContext para C,** insira **ProductsModel** como nome e clique em Adicionar

#### <a name="updating-code-generation-for-data-binding"></a>Atualização da geração de código para vinculação de dados

EF gera código do seu modelo usando modelos T4. Os modelos enviados com o Visual Studio ou baixados da galeria Visual Studio são destinados para uso geral. Isso significa que as entidades geradas a&lt;&gt; partir desses modelos possuem propriedades simples de ICollection T. No entanto, ao fazer a vinculação de dados usando o WPF é desejável usar **oObservávelCollection** para propriedades de coleta para que o WPF possa acompanhar as alterações feitas nas coleções. Para isso, modificaremos os modelos para usar o ObservávelCollection.

-   Abra o **Solution Explorer** e encontre o arquivo **ProductModel.edmx**
-   Encontre o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel.edmx

    ![Modelo de modelo de produto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio
-   Encontre e substitua as duas ocorrências de "**ICollection**" por "**ObservávelCollection**". Estes estão localizados aproximadamente nas linhas 296 e 484.
-   Encontre e substitua a primeira ocorrência de "**HashSet**" com "**ObservávelCollection**". Esta ocorrência está localizada aproximadamente na linha 50. **Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.
-   Encontre e substitua a única ocorrência de "**System.Collections.Generic**" com "**System.Collections.ObjectModel**". Isto está localizado aproximadamente na linha 424.
-   Salve o arquivo ProductModel.tt. Isso deve fazer com que o código para entidades seja regenerado. Se o código não se regenerar automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "Executar ferramenta personalizada".

Se você abrir agora o arquivo Category.cs (que está aninhado em ProductModel.tt) então você deve ver que a coleção Produtos tem o tipo **Produto&lt;&gt;de Coleta Observável**.

Compile o projeto.

## <a name="lazy-loading"></a>Carregamento preguiçoso

A **Products** propriedade Produtos **na** **categoria** classe e categoria de propriedade na classe **Produto** são propriedades de navegação. No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar uma relação entre dois tipos de entidades.

A EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação. Com esse tipo de carregamento (chamado de carregamento preguiçoso), saiba que na primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.

Ao usar os tipos de entidade POCO, o EF consegue um carregamento preguiçoso criando instâncias de tipos proxy derivados durante o tempo de execução e, em seguida, substituindo propriedades virtuais em suas classes para adicionar o gancho de carregamento. Para obter o carregamento preguiçoso de objetos relacionados, você deve declarar os getters de propriedade de navegação como **públicos** e **virtuais** (**Overridable** in Visual Basic), e você classe não deve ser **lacrado** **(NotOverridable** in Visual Basic). Ao usar o Banco de Dados, as propriedades de navegação First são automaticamente feitas virtuais para permitir o carregamento preguiçoso. Na seção Código Primeiro, optamos por tornar as propriedades de navegação virtuais pelo mesmo motivo

## <a name="bind-object-to-controls"></a>Vincular objeto a controles

Adicione as classes definidas no modelo como fontes de dados para este aplicativo WPF.

-   Clique duas vezes em **MainWindow.xaml** no Solution Explorer para abrir o formulário principal
-   No menu principal, selecione **Projeto -&gt; Adicionar Nova Fonte de Dados ...**
    (no Visual Studio 2010, você precisa selecionar **Dados&gt; - Adicionar Nova Fonte de Dados...**)
-   Na janela Escolher uma janela de tipo de origem de dados, selecione **Objeto** e clique **em Seguir**
-   Na marca Select the Data Objects (Seleção objetos de dados) desdobre o **WPFwithEFSample** duas vezes e selecione **Categoria**  
    *Não há necessidade de selecionar a fonte de dados do **Produto,** porque chegaremos a ela através da propriedade do **Produto**na fonte de dados **da Categoria***  

    ![Selecionar objetos de dados](~/ef6/media/selectdataobjects.png)

-   Clique em **Concluir.**
-   A janela Fontes de dados é aberta ao lado da janela MainWindow.xaml *Se a janela Fontes de dados não estiver aparecendo, selecione Exibir - **&gt; Outras fontes de&gt; dados do Windows** *
-   Pressione o ícone do pino, para que a janela Fontes de dados não se esconda automaticamente. Talvez seja necessário apertar o botão de atualização se a janela já estiver visível.

    ![Fontes de dados](~/ef6/media/datasources.png)

-   Selecione a fonte de dados **da categoria** e arraste-a no formulário.

O seguinte aconteceu quando arrastamos esta fonte:

-   O recurso **categoryViewSource** e a **categoriaControleDataGrid** foram adicionados ao XAML 
-   A propriedade DataContext no elemento Grade pai foi definida como "{Categoria **StaticResourceViewSource** }".O recurso **categoryViewSource** serve como uma\\fonte de vinculação para o elemento de grade pai externo. Os elementos internos da Grade herdam o valor DataContext da grade pai (a propriedade ItemsSource da categoriaDataGrid está definida como "{Binding}")

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

Agora que temos uma grade para exibir Categorias, vamos adicionar uma grade de detalhes para exibir os Produtos associados.

-   Selecione a propriedade **Produtos** na fonte de dados **da Categoria** e arraste-a no formulário.
    -   A **categoriaProductsViewSource** recurso e **produtoDataGrid** grade são adicionados ao XAML
    -   O caminho de vinculação para este recurso é definido como Produtos
    -   A estrutura de vinculação de dados do WPF garante que apenas produtos relacionados à categoria selecionada apareçam no **produtoDataGrid**
-   Na caixa de ferramentas, arraste **Button** para o formulário. Defina a propriedade **Nome** para **botãoSalvar** e a propriedade **Conteúdo** para **Salvar**.

O formulário deve ser semelhante a isso:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Adicionar código que lida com a interação de dados

É hora de adicionar alguns manipuladores de eventos à janela principal.

-   Na janela XAML, clique ** &lt;** no elemento Janela, isso seleciona a janela principal
-   Na janela **Propriedades,** escolha **Eventos** no canto superior direito e clique duas vezes na caixa de texto para a direita do rótulo **Carregado**

    ![Propriedades da janela principal](~/ef6/media/mainwindowproperties.png)

-   Adicione também o evento **Clique** para salvar o botão **Salvar** clicando duas vezes no botão Salvar no designer. 

Isso o leva ao código para trás para o formulário, agora vamos editar o código para usar o ProductContext para realizar o acesso aos dados. Atualize o código para o MainWindow como mostrado abaixo.

O código declara uma instância de longo prazo do **ProductContext**. O objeto **ProductContext** é usado para consultar e salvar dados no banco de dados. **A ocorrência Descarte()** na instância **ProductContext** é então chamada do método **OnClosing** substituído.Os comentários do código fornecem detalhes sobre o que o código faz.

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

## <a name="test-the-wpf-application"></a>Teste o aplicativo WPF

-   Compile e execute o aplicativo. Se você usou o Code First, verá que um banco de dados **WPFwithEFSample.ProductContext** foi criado para você.
-   Digite um nome de categoria na grade superior e nomes de produtos na grade inferior *Não digite nada em colunas de ID, pois a chave principal é gerada pelo banco de dados*

    ![Janela Principal com novas categorias e produtos](~/ef6/media/screen1.png)

-   Pressione o botão **Salvar** para salvar os dados no banco de dados

Após a chamada para **SaveChanges()** do DbContext, os IDs são preenchidos com os valores gerados pelo banco de dados. Como chamamos **de Atualização()** após **saveChanges()** os controles **do DataGrid** também são atualizados com os novos valores.

![Janela principal com IDs preenchidos](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Recursos adicionais

Para saber mais sobre a vinculação de dados às coletas usando o WPF, consulte [este tópico](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) na documentação do WPF.  
