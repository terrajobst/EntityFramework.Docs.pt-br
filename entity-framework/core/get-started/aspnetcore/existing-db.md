---
title: Introdução ao ASP.NET Core – Banco de dados existente – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: c231a456abd4c110aba0326821799d6e9d567b3c
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614315"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introdução ao EF Core no ASP.NET Core com um banco de dados existente

Neste tutorial, você compila um aplicativo MVC do ASP.NET Core que realiza acesso básico a dados usando o Entity Framework Core. Realize a engenharia reversa de um banco de dados existente para criar um modelo do Entity Framework.

[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Pré-requisitos

Instale o software a seguir:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:
  * **Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)
  * **Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)
* [SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Criar banco de dados de blog

Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente. Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.

* Abrir o Visual Studio
* **Ferramentas > Conectar ao Banco de Dados...**
* Selecione **Microsoft SQL Server** e clique em **Continuar**
* Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**
* Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**
* O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**
* Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**
* Copie o script listado abaixo para o editor de consultas
* Clique com o botão direito do mouse no editor de consultas e selecione **Executar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abra o Visual Studio 2017
* **Arquivo > Novo > Projeto...**
* No menu à esquerda, selecione **Instalado > Visual C# > Web**
* Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**
* Insira **EFGetStarted.AspNetCore.ExistingDb** como o nome e clique em **OK**
* Aguarde a caixa de diálogo **Novo Aplicativo Web ASP.NET Core** aparecer
* Verifique se a lista suspensa da estrutura de destino está definida como **.NET Core** e a lista suspensa da versão definida como **ASP.NET Core 2.1**
* Selecione o modelo **Aplicativo Web (Model-View-Controller)**
* Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**
* Clique em **OK**

## <a name="install-entity-framework-core"></a>Instalar o Entity Framework Core

Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md). 

Para este tutorial, não é necessário instalar um pacote de provedor, porque o tutorial usa o SQL Server. O pacote de provedor do SQL Server está incluído no [metapacote Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

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

 As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar. Veja as classes de entidade `Blog` e `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Para habilitar o carregamento lento, crie propriedades de navegação `virtual` (Blog.Post e Post.Blog).

 O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrar e configurar o contexto em Startup.cs

Para disponibilizar `BloggingContext` para os controladores do MVC, registre-o como um serviço.

* Abra **Startup.cs**
* Adicione as declarações `using` a seguir ao início do método

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Agora é possível usar o método `AddDbContext(...)` para registrá-lo como um serviço.
* Localize o método `ConfigureServices(...)`
* Adicionar o seguinte código realçado para registrar o contexto como um serviço

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> Em um aplicativo real, normalmente você colocaria a cadeia de conexão em um arquivo de configuração ou variável de ambiente. Para simplificar, este tutorial define isso no código. Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Criar um controlador e exibições

* Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador...**
* Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**
* Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**
* Clique em **Adicionar**

## <a name="run-the-application"></a>Executar o aplicativo

Agora, você pode executar o aplicativo para vê-lo em ação.

* **Depurar > Iniciar Sem Depuração**
* O aplicativo é compilado e aberto em um navegador da Web
* Navegue até `/Blogs`
* Clique em **Criar Novo**
* Insira uma **Url** para o novo blog e clique em **Criar**

![imagem](_static/create.png)

![imagem](_static/index-existing-db.png)
