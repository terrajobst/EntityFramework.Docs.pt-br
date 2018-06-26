---
title: Introdução ao ASP.NET Core – Banco de dados existente – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151008"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introdução ao EF Core no ASP.NET Core com um banco de dados existente

Neste passo a passo, você compilará um aplicativo MVC do ASP.NET Core que executa acesso básico aos dados usando o Entity Framework. Você usará engenharia reversa para criar um modelo do Entity Framework com base em um banco de dados existente.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:
  * **Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)
  * **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)
* [SDK do .NET Core 2.0](https://www.microsoft.com/net/download/core).
* [Banco de dados de blog](#blogging-database)

### <a name="blogging-database"></a>Banco de dados de blog

Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.

> [!TIP]  
> Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.

* Abrir o Visual Studio
* **Ferramentas > Conectar ao Banco de Dados...**
* Selecione **Microsoft SQL Server** e clique em **Continuar**
* Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**
* Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**
* O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**
* Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**
* Copie o script, listado abaixo, no editor de consultas
* Clique com o botão direito do mouse no editor de consultas e selecione **Executar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abra o Visual Studio 2017
* **Arquivo > Novo > Projeto...**
* No menu à esquerda, selecione **Instalado > Modelos > Visual C# > Web**
* Selecione o modelo de projeto **Aplicativo Web ASP.NET Core (.NET Core)**
* Insira **EFGetStarted.AspNetCore.ExistingDb** como o nome e clique em **OK**
* Aguarde a caixa de diálogo **Novo Aplicativo Web ASP.NET Core** aparecer
* Em **ASP.NET Core Templates 2.0** selecione o **Aplicativo Web (Model-View-Controller)**
* Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**
* Clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Usaremos Entity Framework Tools para criar um modelo do banco de dados. Portanto, também instalaremos o pacote de ferramentas:

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

Usaremos algumas ferramentas de scaffolding do ASP.NET Core para criar controladores e exibições posteriormente. Portanto, também instalaremos o pacote de design:

* Execute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Fazer engenharia reversa em seu modelo

Agora é hora de criar o modelo EF com base em seu banco de dados existente.

* **Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**
* Execute o seguinte comando para criar um modelo do banco de dados existente:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Se você receber um erro indicando `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.

> [!TIP]  
> Especifique para quais tabelas você deseja gerar entidades adicionando o argumento `-Tables` para o comando acima. Por exemplo, `-Tables Blog,Post`.

O processo de engenharia reversa criou classes de entidade (`Blog.cs` & `Post.cs`) e um contexto derivado (`BloggingContext.cs`) com base no esquema do banco de dados existente.

 As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Registre seu contexto com injeção de dependência

O conceito de injeção de dependência é central ao ASP.NET Core. Serviços (como `BloggingContext`) são registrados com injeção de dependência durante a inicialização do aplicativo. Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor. Para saber mais sobre injeção de dependência, veja o artigo [Injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) no site do ASP.NET.

### <a name="remove-inline-context-configuration"></a>Remover configuração de contexto embutida

No ASP.NET Core, a configuração geralmente é executada em **Startup.cs**. Para atender a esse padrão, mudaremos a configuração do provedor de banco de dados para **Startup.cs**.

* Abrir `Models\BloggingContext.cs`
* Exclua o método `OnConfiguring(...)`

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Adicione o seguinte construtor, que permitirá que a configuração seja passada para o contexto pela injeção de dependência

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrar e configurar o contexto em Startup.cs

Para que os controladores MVC usem o `BloggingContext`, vamos registrá-lo como um serviço.

* Abra **Startup.cs**
* Adicione as declarações `using` a seguir ao início do método

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Agora podemos usar o método `AddDbContext(...)` para registrá-lo como um serviço.
* Localize o método `ConfigureServices(...)`
* Adicione o seguinte código para registrar o contexto como um serviço

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> Em um aplicativo real, normalmente você colocaria a cadeia de conexão em um arquivo de configuração. Para simplificar, definimos isso no código. Para saber mais, confira [Strings de conexão](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Criar um controlador

Em seguida, habilitaremos o scaffolding em nosso projeto.

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**
* Selecione **Dependências Completas** e clique em **Adicionar**
* Você pode ignorar as instruções no arquivo `ScaffoldingReadMe.txt` aberto

Agora que o scaffolding está habilitado, é possível realizar o scaffolding de um controlador para a entidade `Blog`.

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**
* Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**
* Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**
* Clique em **Adicionar**

## <a name="run-the-application"></a>Executar o aplicativo

Agora, você pode executar o aplicativo para vê-lo em ação.

* **Depurar > Iniciar Sem Depuração**
* O aplicativo será compilado e será aberto em um navegador da Web
* Navegue até `/Blogs`
* Clique em **Criar Novo**
* Insira uma **Url** para o novo blog e clique em **Criar**

![imagem](_static/create.png)

![imagem](_static/index-existing-db.png)
