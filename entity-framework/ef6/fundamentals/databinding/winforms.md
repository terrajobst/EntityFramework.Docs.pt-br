---
title: DataBinding com WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 3c7c58f5ded29c136bbdca1d81c64b07c53ce583
ms.sourcegitcommit: 7391cc31193c1216ec9ed485709042ad0c2106cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985470"
---
# <a name="databinding-with-winforms"></a>Associação de dados com WinForms
Este guia passo a passo mostra como associar tipos POCO a controles Window Forms (WinForms) em um formulário "Master-Detail". O aplicativo usa Entity Framework para preencher objetos com dados do banco de dados, controlar alterações e manter dados no banco de dado.

O modelo define dois tipos que participam de uma relação um-para-muitos: Categoria (mestre\\principal) e produto (detalhes\\dependentes). Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo aos controles WinForms. A estrutura de associação de dados do WinForms permite a navegação entre objetos relacionados: a seleção de linhas no modo de exibição mestre faz com que a exibição de detalhes seja atualizada com os dados filho correspondentes.

As capturas de tela e as listagens de código neste passo a passos são obtidas de Visual Studio 2013, mas você pode concluir este passo a passos com o Visual Studio 2012 ou o Visual Studio 2010.

## <a name="pre-requisites"></a>Pré-requisitos

Você precisa ter Visual Studio 2013, o Visual Studio 2012 ou o Visual Studio 2010 instalado para concluir este passo a passos.

Se você estiver usando o Visual Studio 2010, também precisará instalar o NuGet. Para obter mais informações, consulte Instalando o [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Criar o aplicativo

-   Abrir o Visual Studio
-   **Arquivo-&gt; novo-&gt; projeto...**
-   Selecione **Windows** no painel esquerdo e **FormsApplication do Windows** no painel direito
-   Insira **WinFormswithEFSample** como o nome
-   Selecione **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instalar o pacote NuGet Entity Framework

-   Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **WinFormswithEFSample**
-   Selecione **gerenciar pacotes NuGet...**
-   Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**
-   Clique em **instalar**  
    > [!NOTE]
    > Além do assembly do EntityFramework, uma referência a System. ComponentModel. Annotations também é adicionada. Se o projeto tiver uma referência a System. Data. Entity, ele será removido quando o pacote do EntityFramework for instalado. O assembly System. Data. Entity não é mais usado para aplicativos Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementando IListSource para coleções

As propriedades de coleção devem implementar a interface IListSource para habilitar a vinculação de dados bidirecional com classificação ao usar Windows Forms. Para fazer isso, Vamos estender ObservableCollection para adicionar a funcionalidade IListSource.

-   Adicione uma classe **ObservableListSource** ao projeto:
    -   Clique com o botão direito do mouse no nome do projeto
    -   Selecione **Adicionar –&gt; novo item**
    -   Selecione **classe** e insira **ObservableListSource** para o nome da classe
-   Substitua o código gerado por padrão pelo seguinte código:

*Essa classe habilita a vinculação de dados bidirecional, bem como a classificação. A classe deriva de ObservableCollection&lt;T&gt; e adiciona uma implementação explícita de IListSource. O método GetList () de IListSource é implementado para retornar uma implementação IBindingList que permanece em sincronia com a ObservableCollection. A implementação IBindingList gerada por tobindlist dá suporte à classificação. O método de extensão tobindlist é definido no assembly do EntityFramework.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definir um modelo

Neste tutorial, você pode optar por implementar um modelo usando Code First ou o designer do EF. Conclua uma das duas seções a seguir.

### <a name="option-1-define-a-model-using-code-first"></a>Opção 1: Definir um modelo usando Code First

Esta seção mostra como criar um modelo e seu banco de dados associado usando Code First. Pule para a próxima seção (**opção 2: Definir um modelo usando Database First)** se você preferir usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF

Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio).

-   Adicionar uma nova classe de **produto** ao projeto
-   Substitua o código gerado por padrão pelo seguinte código:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Adicione uma classe de **categoria** ao projeto.
-   Substitua o código gerado por padrão pelo seguinte código:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Além de definir as entidades, você precisa definir uma classe derivada de **DbContext** e expõe as propriedades de **DbSet&lt;TEntity&gt;**  . As propriedades **DbSet** permitem que o contexto saiba quais tipos você deseja incluir no modelo. Os tipos **DbContext** e **DbSet** são definidos no assembly do EntityFramework.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.

-   Adicione uma nova classe **ProductContext** ao projeto.
-   Substitua o código gerado por padrão pelo seguinte código:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

Esta seção mostra como usar Database First para fazer a engenharia reversa de seu modelo de um banco de dados usando o designer do EF. Se você concluiu a seção anterior **(opção 1: Defina um modelo usando Code First)** , em seguida, ignore esta seção e vá direto para a seção **carregamento lento** .

#### <a name="create-an-existing-database"></a>Criar um banco de dados existente

Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Vamos continuar e gerar o banco de dados.

-   **Exibir-&gt; Gerenciador de servidores**
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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Selecione a conexão com o banco de dados que você criou na primeira seção, digite **ProductContext** como o nome da cadeia de conexão e clique em **Avançar**

    ![Escolha sua conexão](~/ef6/media/chooseyourconnection.png)

-   Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '

    ![Escolha seus objetos](~/ef6/media/chooseyourobjects.png)

Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer. Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.

#### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, será necessário atualizar o designer do EF para usar a geração de código EF6.

-   Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**
-   Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**
-   Selecione o **gerador de DbContext do EF 6. x\#para C,** insira **ProductsModel** como o nome e clique em Adicionar

#### <a name="updating-code-generation-for-data-binding"></a>Atualizando geração de código para associação de dados

O EF gera código do seu modelo usando modelos T4. Os modelos fornecidos com o Visual Studio ou baixados da galeria do Visual Studio são destinados para uso geral. Isso significa que as entidades geradas a partir desses modelos têm&lt;propriedades&gt; ICollection T simples. No entanto, ao fazer a ligação de dados, é desejável ter propriedades de coleção que implementam IListSource. É por isso que criamos a classe ObservableListSource acima e agora vamos modificar os modelos para usar essa classe.

-   Abra o **Gerenciador de soluções** e localize o arquivo **ProductModel. edmx**
-   Localize o arquivo **ProductModel.tt** que será aninhado no arquivo ProductModel. edmx

    ![Modelo de modelo de produto](~/ef6/media/productmodeltemplate.png)

-   Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio
-   Localizar e substituir as duas ocorrências de "**ICollection**" por "**ObservableListSource**". Elas estão localizadas em aproximadamente as linhas 296 e 484.
-   Localize e substitua a primeira ocorrência de "**HashSet**" por "**ObservableListSource**". Essa ocorrência está localizada em aproximadamente a linha 50. **Não** substitua a segunda ocorrência de HashSet encontrada posteriormente no código.
-   Salve o arquivo ProductModel.tt. Isso deve fazer com que o código das entidades seja regenerado. Se o código não for regenerado automaticamente, clique com o botão direito do mouse em ProductModel.tt e escolha "executar ferramenta personalizada".

Se agora você abrir o arquivo Category.cs (que está aninhado em ProductModel.TT), verá que a coleção Products tem o tipo **ObservableListSource&lt;Product&gt;** .

Compile o projeto.

## <a name="lazy-loading"></a>Carregamento lento

A Propriedade Products na classe **Category** e a propriedade **Category** na classe **Product** são propriedades de navegação. Em Entity Framework, as propriedades de navegação fornecem uma maneira de navegar em uma relação entre dois tipos de entidade.

O EF oferece a você uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação. Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que na primeira vez que você acessar cada propriedade de navegação, uma consulta separada será executada no banco de dados se o conteúdo ainda não estiver no contexto.

Ao usar tipos de entidade POCO, o EF alcança o carregamento lento criando instâncias de tipos de proxy derivados durante o tempo de execução e, em seguida, substituindo as propriedades virtuais em suas classes para adicionar o gancho de carregamento. Para obter o carregamento lento de objetos relacionados, você deve declarar getters de propriedade de navegação como **público** e **virtual** (**substituível** em Visual Basic) e a classe não deve ser **selada** (não**substituível** em Visual Basic). Ao usar Database First Propriedades de navegação são automaticamente disponibilizadas para habilitar o carregamento lento. Na seção Code First, optamos por tornar as propriedades de navegação virtuais pela mesma razão

## <a name="bind-object-to-controls"></a>Associar objeto a controles

Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo WinForms.

-   No menu principal, selecione **projeto-Adicionar&gt; nova fonte de dados...**
    (no Visual Studio 2010, você precisa selecionar **dados-adicionar&gt; nova fonte de dados...** )
-   Na janela escolher um tipo de fonte de dados, selecione **objeto** e clique em **Avançar**
-   Na caixa de diálogo Selecionar os objetos de dados, desdobrar o **WinFormswithEFSample** duas vezes e selecione **categoria** não há necessidade de selecionar a fonte de dados do produto, pois vamos chegar a ela por meio da Propriedade do produto na fonte de dados Category.

    ![fonte de dados](~/ef6/media/datasource.png)

-   Clique em **concluir.**
    Se a janela fontes de dados não estiver aparecendo, selecione **Exibir&gt; -outras fontes&gt; de dados do Windows**
-   Pressione o ícone de pino para que a janela fontes de dados não seja ocultada automaticamente. Talvez seja necessário pressionar o botão atualizar se a janela já estava visível.

    ![Fonte de dados 2](~/ef6/media/datasource2.png)

-   Em Gerenciador de Soluções, clique duas vezes no arquivo **Form1.cs** para abrir o formulário principal no designer.
-   Selecione a fonte de dados **categoria** e arraste-a no formulário. Por padrão, um novo DataGridView (**categoryDataGridView**) e controles da barra de ferramentas de navegação são adicionados ao designer. Esses controles são vinculados aos componentes BindingSource (**categoryBindingSource**) e**categoryBindingNavigator**(navegador de associação) que também são criados.
-   Edite as colunas no **categoryDataGridView**. Queremos definir a coluna **CategoryID** como somente leitura. O valor para a propriedade **CategoryID** é gerado pelo banco de dados depois que os mesmos são salvos.
    -   Clique com o botão direito do mouse no controle DataGridView e selecione Editar colunas...
    -   Selecione a coluna CategoryId e defina ReadOnly como true
    -   Pressione OK
-   Selecione produtos na fonte de dados categoria e arraste-o no formulário. O productDataGridView e o productBindingSource são adicionados ao formulário.
-   Edite as colunas no productDataGridView. Queremos ocultar as colunas CategoryId e Category e definir ProductId como somente leitura. O valor para a propriedade ProductId é gerado pelo banco de dados depois que os mesmos são salvos.
    -   Clique com o botão direito do mouse no controle DataGridView e selecione **Editar colunas...** .
    -   Selecione a coluna **ProductID** e defina **ReadOnly** como **true**.
    -   Selecione a coluna **CategoryID** e pressione o botão **remover** . Faça o mesmo com a coluna **categoria** .
    -   Pressione **OK**.

    Até agora, nós associomos nossos controles DataGridView a componentes BindingSource no designer. Na próxima seção, adicionaremos o código ao code-behind para definir categoryBindingSource. DataSource como a coleção de entidades que são rastreadas atualmente por DbContext. Quando arrastamos e descartamos produtos sob a categoria, os WinForms cuidam da configuração da propriedade productsBindingSource. DataSource para a propriedade categoryBindingSource e productsBindingSource. DataMember para Products. Devido a essa associação, somente os produtos que pertencem à categoria atualmente selecionada serão exibidos no productDataGridView.
-   Habilite o botão **salvar** na barra de ferramentas de navegação clicando com o botão direito do mouse e selecionando **habilitado**.

    ![Designer de formulário 1](~/ef6/media/form1-designer.png)

-   Adicione o manipulador de eventos ao botão salvar clicando duas vezes no botão. Isso adicionará o manipulador de eventos e o levará ao code-behind para o formulário. O código para o manipulador de eventos de **clique categoryBindingNavigatorSaveItem\_** será adicionado na próxima seção.

## <a name="add-the-code-that-handles-data-interaction"></a>Adicionar o código que manipula a interação de dados

Agora, vamos adicionar o código para usar o ProductContext para executar o acesso a dados. Atualize o código para a janela de formulário principal, conforme mostrado abaixo.

O código declara uma instância de execução longa do ProductContext. O objeto ProductContext é usado para consultar e salvar dados no banco de dado. O método Dispose () na instância ProductContext é então chamado a partir do método OnClosing substituído. Os comentários de código fornecem detalhes sobre o que o código faz.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Testar o aplicativo Windows Forms

-   Compile e execute o aplicativo e você pode testar a funcionalidade.

    ![Formulário 1 antes de salvar](~/ef6/media/form1beforesave.png)

-   Depois de salvar as chaves geradas pelo repositório, elas são mostradas na tela.

    ![Formulário 1 após salvar](~/ef6/media/form1aftersave.png)

-   Se você usou Code First, verá também que um banco de dados **WinFormswithEFSample. ProductContext** é criado para você.

    ![Pesquisador de objetos do servidor](~/ef6/media/serverobjexplorer.png)
