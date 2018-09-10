---
title: Associação de dados com WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 071172810f7dac45f42aca0efa7f329bac31e9cd
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251187"
---
# <a name="databinding-with-winforms"></a>Associação de dados com WinForms
Este passo a passo mostra como associar os tipos de POCO para controles de Windows Forms (WinForms) em um formulário de "master-detail". O aplicativo usa o Entity Framework para preencher os objetos com dados do banco de dados, controlar as alterações e manter os dados no banco de dados.

O modelo define dois tipos que participam no relacionamento um-para-muitos: categoria (entidade de segurança\\mestre) e o produto (dependentes\\detalhes). Em seguida, as ferramentas do Visual Studio são usadas para associar os tipos definidos no modelo para os controles do WinForms. A estrutura de vinculação de dados WinForms permite a navegação entre objetos relacionados: selecionar linhas no modo de exibição mestre faz com que a exibição de detalhes atualizar com os dados filho correspondentes.

As listagens de código neste passo a passo e capturas de tela são obtidas do Visual Studio 2013, mas você pode concluir este passo a passo com o Visual Studio 2012 ou Visual Studio 2010.

## <a name="pre-requisites"></a>Pré-requisitos

Você precisa ter o Visual Studio 2013, Visual Studio 2012 ou Visual Studio 2010 instalado para concluir este passo a passo.

Se você estiver usando o Visual Studio 2010, você também precisará instalar o NuGet. Para obter mais informações, consulte [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Criar o aplicativo

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no painel esquerdo e **FormsApplication Windows** no painel à direita
-   Insira **WinFormswithEFSample** como o nome
-   Selecione **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Instale o pacote NuGet do Entity Framework

-   No Gerenciador de soluções, clique com botão direito no **WinFormswithEFSample** projeto
-   Selecione **gerenciar pacotes NuGet...**
-   Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote
-   Clique em **instalar**  
    > [!NOTE]
    > Uma referência ao DataAnnotations também é adicionada além do assembly EntityFramework. Se o projeto tem uma referência a System, em seguida, ele será removido quando o pacote EntityFramework é instalado. O assembly System não é mais usado para aplicativos do Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementação de IListSource para coleções

Propriedades de coleção devem implementar a interface IListSource para habilitar a associação de dados bidirecional com a classificação ao usar o Windows Forms. Para fazer isso, vamos estender ObservableCollection para adicionar a funcionalidade de IListSource.

-   Adicionar um **ObservableListSource** classe ao projeto:
    -   Clique com botão direito no nome do projeto
    -   Selecione **Add -&gt; Novo Item**
    -   Selecione **classe** e insira **ObservableListSource** para o nome de classe
-   Substitua o código gerado por padrão com o código a seguir:

*Essa classe permite que a associação, bem como classificação de dados bidirecional. A classe derive de ObservableCollection&lt;T&gt; e adiciona uma implementação explícita de IListSource. O método GetList() de IListSource é implementado para retornar uma implementação de IBindingList permaneça em sincronia com a ObservableCollection. A implementação de IBindingList gerada pelo ToBindingList dá suporte à classificação. O método de extensão ToBindingList é definido no assembly EntityFramework.*

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
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definir um modelo

Neste passo a passo que você pode optar por implementar um modelo usando o EF Designer ou o Code First. Conclua uma das duas seções a seguir.

### <a name="option-1-define-a-model-using-code-first"></a>Opção 1: Definir um modelo usando o Code First

Esta seção mostra como criar um modelo e seu banco de dados associado usando o Code First. Vá para a próxima seção (**opção 2: definir um modelo usando Database First)** se você preferir usar Database First para reverter a engenharia de seu modelo de banco de dados usando o designer EF

Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio).

-   Adicione um novo **produto** classe ao projeto
-   Substitua o código gerado por padrão com o código a seguir:

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

-   Adicionar um **categoria** classe ao projeto.
-   Substitua o código gerado por padrão com o código a seguir:

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

Além de definir entidades, você precisa definir uma classe que deriva de **DbContext** e expõe **DbSet&lt;TEntity&gt;**  propriedades. O **DbSet** propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo. O **DbContext** e **DbSet** tipos são definidos no assembly EntityFramework.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.

-   Adicione um novo **ProductContext** classe ao projeto.
-   Substitua o código gerado por padrão com o código a seguir:

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

    ![Alterar fonte de dados](~/ef6/media/changedatasource.png)

-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **produtos** como o nome do banco de dados

    ![Adicionar Conexão LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Adicionar Conexão Express](~/ef6/media/addconnectionexpress.png)

-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**

    ![Criar banco de dados](~/ef6/media/createdatabase.png)

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

    ![Escolha sua Conexão](~/ef6/media/chooseyourconnection.png)

-   Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'

    ![Escolher objetos do](~/ef6/media/chooseyourobjects.png)

Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer. Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.

#### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, em seguida, você precisará atualizar o EF designer para usar a geração de código do EF6.

-   Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**
-   Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**
-   Selecione o **EF 6.x gerador DbContext para C\#,** insira **ProductsModel** como o nome e clique em Adicionar

#### <a name="updating-code-generation-for-data-binding"></a>Atualizando a geração de código para a associação de dados

O EF gera o código de seu modelo usando os modelos T4. Os modelos fornecidos com o Visual Studio ou baixado na Galeria do Visual Studio são destinados a uso de finalidade geral. Isso significa que as entidades geradas a partir desses modelos tem simple ICollection&lt;T&gt; propriedades. No entanto, ao fazer a ligação de dados é interessante ter propriedades de coleção que implementam IListSource. Isso é por isso que criamos a classe ObservableListSource acima e agora vamos modificar os modelos para tornar usar essa classe.

-   Abra o **Gerenciador de soluções** e localize **ProductModel.edmx** arquivo
-   Localizar o **ProductModel.tt** arquivo que será aninhado sob o arquivo ProductModel.edmx

    ![Modelo de produto](~/ef6/media/productmodeltemplate.png)

-   Clique duas vezes no arquivo ProductModel.tt para abri-lo no editor do Visual Studio
-   Localizar e substituir as duas ocorrências de "**ICollection**"com"**ObservableListSource**". Eles estão localizados em aproximadamente linhas 296 e 484.
-   Localizar e substituir a primeira ocorrência de "**HashSet**"com"**ObservableListSource**". Essa ocorrência está localizada em linha aproximadamente 50. **Não** substituir a segunda ocorrência da HashSet encontrada posteriormente no código.
-   Salve o arquivo ProductModel.tt. Isso deve fazer com que o código para entidades sejam regenerados. Se o código não regenerar automaticamente, em seguida, clique com botão direito em ProductModel.tt e escolha "Executar ferramenta personalizada".

Se você agora, abra o arquivo de Category.cs (que é aninhado sob ProductModel.tt) e em seguida, você deverá ver a coleção de produtos tem o tipo **ObservableListSource&lt;produto&gt;**.

Compile o projeto.

## <a name="lazy-loading"></a>Carregamento lento

O **produtos** propriedade no **categoria** classe e **categoria** propriedade o **produto** classe são propriedades de navegação. No Entity Framework, as propriedades de navegação fornecem uma maneira de navegar um relacionamento entre dois tipos de entidade.

O EF oferece uma opção de carregar entidades relacionadas do banco de dados automaticamente na primeira vez que você acessa a propriedade de navegação. Com esse tipo de carregamento (chamado de carregamento lento), lembre-se de que a primeira vez que você acessar cada propriedade de navegação uma consulta separada será executada no banco de dados se o conteúdo não ainda estiverem no contexto.

Ao usar tipos de entidade POCO, o EF atinge o carregamento lento criando instâncias de tipos de proxy derivado durante o tempo de execução e, em seguida, substituindo propriedades virtuais nas suas classes para adicionar o gancho do carregamento. Para obter o carregamento lento de objetos relacionados, você deve declarar navegação getters de propriedade como **pública** e **virtual** (**Overridable** no Visual Basic), e você classe não deve ser **lacrado** (**NotOverridable** no Visual Basic). Quando o banco de dados usando as propriedades de navegação primeiro são feitas automaticamente virtuais para habilitar o carregamento lento. Na seção Code First que escolhemos tornar as propriedades de navegação virtual pelo mesmo motivo

## <a name="bind-object-to-controls"></a>Associar o objeto a controles

Adicione as classes que são definidas no modelo como fontes de dados para este aplicativo de WinForms.

-   No menu principal, selecione **projeto -&gt; adicionar nova fonte de dados...**
    (no Visual Studio 2010, você precisa selecionar **dados -&gt; adicionar nova fonte de dados...** )
-   Na escolha de uma janela do tipo de fonte de dados, selecione **objeto** e clique em **Avançar**
-   Na caixa de diálogo de objetos de dados selecione, desdobrar o **WinFormswithEFSample** duas vezes e selecione **categoria** não é necessário selecionar a fonte de dados do produto, porque obteremos a ele por meio do produto propriedade na fonte de dados de categoria.

    ![fonte de dados](~/ef6/media/datasource.png)

-   Clique em **concluir.** 
     *Se a janela fontes de dados não está aparecendo, selecione * * * Exibir -&gt; outras Windows -&gt; fontes de dados**
-   Pressione o ícone de pino, portanto, a janela fontes de dados não auto ocultar. Você talvez precise clicar no botão de atualização se a janela estiver visível.

    ![Fonte de dados 2](~/ef6/media/datasource2.png)

-   No Gerenciador de soluções, clique duas vezes o **Form1.cs** arquivo para abrir o formulário principal no designer.
-   Selecione o **categoria** fonte de dados e arraste-o no formulário. Por padrão, um novo DataGridView (**categoryDataGridView**) e controles de barra de ferramentas de navegação são adicionados ao designer. Esses controles estão vinculados ao BindingSource (**categoryBindingSource**) e associação Navigator (**categoryBindingNavigator**) componentes também são criados.
-   Editar as colunas sobre o **categoryDataGridView**. Queremos definir a **CategoryId** coluna como somente leitura. O valor para o **CategoryId** propriedade é gerada pelo banco de dados após salvar os dados.
    -   O controle DataGridView com o botão direito e selecione Edit Columns...
    -   Selecione a coluna CategoryId e definir ReadOnly para True
    -   Pressione Okey
-   Selecione os produtos de fonte de dados de categoria e arraste-o no formulário. O productDataGridView e productBindingSource são adicionados ao formulário.
-   Edite as colunas no productDataGridView. Queremos ocultar as colunas CategoryId e categoria e defina o ProductId como somente leitura. O valor da propriedade ProductId é gerado pelo banco de dados após salvar os dados.
    -   O controle DataGridView com o botão direito e selecione **Editar colunas...** .
    -   Selecione o **ProductId** coluna e o conjunto **ReadOnly** para **verdadeiro**.
    -   Selecione o **CategoryId** coluna e pressione a **remover** botão. Fazer o mesmo com o **categoria** coluna.
    -   Pressione **Okey**.

    Até agora, podemos associados aos nossos controles DataGridView BindingSource componentes no designer. A próxima seção, adicionaremos o código para code-behind definir categoryBindingSource.DataSource para a coleção de entidades que estão atualmente acompanhados pelo DbContext. Quando estamos arrasta e solta os produtos de categoria, o WinForms cuidou de configurar a propriedade productsBindingSource.DataSource à propriedade categoryBindingSource e productsBindingSource.DataMember para produtos. Devido a essa associação, somente os produtos que pertencem à categoria selecionada no momento serão exibidos no productDataGridView.
-   Habilitar o **salve** botão na barra de navegação clicando o botão direito do mouse e selecionando **habilitado**.

    ![Designer de formulário 1](~/ef6/media/form1-designer.png)

-   Adicione o manipulador de eventos para o salvamento botão clicando duas vezes no botão. Isso adicionará o manipulador de eventos e levam você para o code-behind para o formulário. O código para o **categoryBindingNavigatorSaveItem\_clique em** manipulador de eventos será adicionado na próxima seção.

## <a name="add-the-code-that-handles-data-interaction"></a>Adicione o código que trata a interação de dados

Agora, adicionaremos o código para usar o ProductContext para executar o acesso a dados. Atualize o código para a janela do formulário principal, conforme mostrado abaixo.

O código declara uma instância de execução longa da ProductContext. O objeto ProductContext é usado para consultar e salvar dados no banco de dados. O método Dispose () na instância ProductContext, em seguida, é chamado de método OnClosing substituído. Os comentários do código fornecem detalhes sobre o que o código faz.

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

## <a name="test-the-windows-forms-application"></a>Testar o aplicativo do Windows Forms

-   Compile e execute o aplicativo e você pode testar a funcionalidade.

    ![Formulário 1 antes de salvar](~/ef6/media/form1beforesave.png)

-   Depois de salvar as chaves geradas pelo repositório são mostradas na tela.

    ![Formulário 1 após salvar](~/ef6/media/form1aftersave.png)

-   Se você usou o Code First e, em seguida, você também verá que um **WinFormswithEFSample.ProductContext** banco de dados é criado para você.

    ![Pesquisador de objetos do servidor](~/ef6/media/serverobjexplorer.png)
