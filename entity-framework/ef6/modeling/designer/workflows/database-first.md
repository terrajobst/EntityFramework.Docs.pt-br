---
title: Primeiro banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: 93ae5729e487ed9be3972ac78d599dbea19ed458
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251083"
---
# <a name="database-first"></a>Primeiro banco de dados
Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento de banco de dados primeiro usando o Entity Framework. Banco de dados pela primeira vez permite que você faz engenharia reversa de um modelo de banco de dados existente. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo fornece uma introdução ao desenvolvimento de banco de dados primeiro usando o Entity Framework. Banco de dados pela primeira vez permite que você faz engenharia reversa de um modelo de banco de dados existente. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.

**Apresentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter pelo menos o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.

Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

 

## <a name="1-create-an-existing-database"></a>1. Criar um banco de dados existente

Normalmente quando você estiver direcionando um banco de dados existente que já será criada, mas para este passo a passo, é necessário criar um banco de dados para acessar.

O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) banco de dados.

 

Vamos prosseguir e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar o Microsoft SQL Server como a fonte de dados

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado e insira **DatabaseFirst.Blogging** como o nome do banco de dados

    ![Conexão do SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Conexão LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**

    ![Criar caixa de diálogo banco de dados](~/ef6/media/createdatabasedialog.png)

-   O novo banco de dados agora aparecerão no Gerenciador de servidores, clique com botão direito nele e selecione **nova consulta**
-   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**

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
```

## <a name="2-create-the-application"></a>2. Criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o primeiro banco de dados para executar o acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e **aplicativo de Console**
-   Insira **DatabaseFirstSample** como o nome
-   Selecione **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Modelo de engenharia reversa

Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto -&gt; Adicionar Novo Item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**
-   Insira **BloggingModel** como o nome e clique em **Okey**
-   Isso inicia o **Assistente de modelo de dados de entidade**
-   Selecione **gerar a partir do banco de dados** e clique em **Avançar**

    ![Etapa 1 do Assistente](~/ef6/media/wizardstep1.png)

-   Selecione a conexão ao banco de dados que você criou na primeira seção, digite **BloggingContext** como o nome da cadeia de caracteres de conexão e clique **Avançar**

    ![Etapa 2 do Assistente](~/ef6/media/wizardstep2.png)

-   Clique na caixa de seleção ao lado de 'Tabelas' para importar todas as tabelas e clique em 'Concluir'

    ![Etapa 3 do Assistente](~/ef6/media/wizardstep3.png)

 

Depois de concluir o processo de engenharia reversa o novo modelo é adicionado ao seu projeto e aberto para visualização no Entity Framework Designer. Um arquivo App. config também foi adicionado ao seu projeto com os detalhes de conexão para o banco de dados.

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework. A atualização é importante porque ele fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bug mais recentes.

Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.

-   **Projeto –&gt; gerenciar pacotes NuGet... ** 
     *Se você não tiver o **gerenciar pacotes NuGet... ** opção, você deve instalar o [versão mais recente do NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Selecione o **Online** guia
-   Selecione o **EntityFramework** pacote
-   Clique em **instalar**

Em seguida, precisamos alternar nosso modelo para gerar o código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.

-   Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione **Adicionar Item de geração de código...**
-   Selecione **modelos Online** no menu esquerdo e pesquise **DbContext**
-   Selecione o EF **5.x gerador DbContext para C\#**, insira **BloggingModel** como o nome e clique em **adicionar**

    ![Modelo de DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Ler e gravar dados

Agora que temos um modelo que é hora de usá-lo para acessar alguns dados. As classes que vamos usar para acessar os dados são gerados automaticamente para você com base no arquivo EDMX.

*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010 a BloggingModel.tt e BloggingModel.Context.tt arquivos diretamente em seu projeto, em vez de aninhado sob o arquivo EDMX.*

![Classes geradas DF](~/ef6/media/generatedclassesdf.png)

 

Implemente o método Main em Program.cs, conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, usa-o para inserir um novo Blog. Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados classificado em ordem alfabética por título.

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

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Lidando com alterações de banco de dados

Agora é hora de fazer algumas alterações em nosso esquema de banco de dados, quando fazemos essas alterações que também precisamos atualizar nosso modelo para refletir essas alterações.

A primeira etapa é fazer algumas alterações no esquema de banco de dados. Vamos adicionar uma tabela de usuários no esquema.

-   Clique com botão direito no **DatabaseFirst.Blogging** do banco de dados no Gerenciador de servidores e selecione **nova consulta**
-   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Agora que o esquema é atualizado, é hora de atualizar o modelo com essas alterações.

-   Clique duas vezes em uma área vazia do seu modelo no EF Designer e selecione 'modelo de atualização do banco de dados...', isso iniciará o Assistente de atualização
-   Na guia Add da verificação de Assistente de atualização, a caixa ao lado de tabelas, isso indica que desejamos adicionar quaisquer novas tabelas do esquema.
    *A guia de atualização mostra todas as tabelas existentes no modelo que será verificado para que as alterações durante a atualização. As marcas de exclusão mostram todas as tabelas que foram removidas do esquema e também serão removidas do modelo como parte da atualização. As informações nessas duas guias são detectadas automaticamente e são fornecidas apenas para fins informativos, é possível alterar as configurações.*

    ![Assistente de atualização](~/ef6/media/refreshwizard.png)

-   Clique em Finish no Assistente de atualização

 

O modelo foi atualizado para incluir uma nova entidade de usuário que mapeia para a tabela de usuários que adicionamos ao banco de dados.

![Modelo atualizado](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Resumo

Neste passo a passo que vimos o desenvolvimento de banco de dados primeiro, que nos permitiu criar um modelo no Designer de EF com base em um banco de dados existente. Em seguida, usamos esse modelo para ler e gravar alguns dados do banco de dados. Por fim, atualizamos o modelo para refletir as alterações feitas no esquema de banco de dados.
