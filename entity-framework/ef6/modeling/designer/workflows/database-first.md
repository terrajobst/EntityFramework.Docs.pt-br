---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182456"
---
# <a name="database-first"></a>Database First
Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Database First usando Entity Framework. Database First permite que você reverta a engenharia de um modelo de um banco de dados existente. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo fornece uma introdução ao desenvolvimento de Database First usando Entity Framework. Database First permite que você reverta a engenharia de um modelo de um banco de dados existente. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.

**Apresentado por**: [Rowan Miller](https://romiller.com/)

**Vídeo**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter, pelo menos, o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir esta explicação.

Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

 

## <a name="1-create-an-existing-database"></a>1. Criar um banco de dados existente

Normalmente, quando você estiver direcionando um banco de dados existente, ele já será criado, mas para este passo a passos, precisamos criar um banco de dados a ser acessado.

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

 

Vamos continuar e gerar o banco de dados.

-   Abrir o Visual Studio
-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar Microsoft SQL Server como a fonte de dado

    ![Selecionar fonte de dados](~/ef6/media/selectdatasource.png)

-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou e insira **DatabaseFirst. blog** como o nome do banco de dados

    ![DF de conexão do SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![DF de conexão LocalDB](~/ef6/media/localdbconnectiondf.png)

-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**

    ![Caixa de diálogo Criar banco de dados](~/ef6/media/createdatabasedialog.png)

-   O novo banco de dados aparecerá agora na Gerenciador de Servidores, clique com o botão direito do mouse nele e selecione **nova consulta**
-   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**

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

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Database First para executar o acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo-&gt; Projeto New-&gt;...**
-   Selecione **Windows** no menu à esquerda e no **aplicativo de console**
-   Insira **DatabaseFirstSample** como o nome
-   Selecione **OK**

 

## <a name="3-reverse-engineer-model"></a>3. Modelo de engenharia reversa

Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto-&gt; adicionar novo item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**
-   Insira **BloggingModel** como o nome e clique em **OK**
-   Isso iniciará o **Assistente de modelo de dados de entidade**
-   Selecione **gerar do banco de dados** e clique em **Avançar**

    ![Etapa 1 do assistente](~/ef6/media/wizardstep1.png)

-   Selecione a conexão com o banco de dados que você criou na primeira seção, digite **BloggingContext** como o nome da cadeia de conexão e clique em **Avançar**

    ![Etapa 2 do assistente](~/ef6/media/wizardstep2.png)

-   Clique na caixa de seleção ao lado de ' tabelas ' para importar todas as tabelas e clique em ' Concluir '

    ![Etapa 3 do assistente](~/ef6/media/wizardstep3.png)

 

Depois que o processo de engenharia reversa for concluído, o novo modelo será adicionado ao seu projeto e aberto para que você exiba na Entity Framework Designer. Um arquivo app. config também foi adicionado ao seu projeto com os detalhes de conexão do banco de dados.

![Inicial do modelo](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Etapas adicionais no Visual Studio 2010

Se você estiver trabalhando no Visual Studio 2010, há algumas etapas adicionais que você precisa seguir para atualizar para a versão mais recente do Entity Framework. A atualização é importante porque fornece acesso a uma superfície de API aprimorada, que é muito mais fácil de usar, bem como as correções de bugs mais recentes.

Primeiro, precisamos obter a versão mais recente do Entity Framework do NuGet.

-   **Projeto – &gt; gerenciar pacotes NuGet...** 
    *se você não tiver a opção **gerenciar pacotes NuGet...** , você deve instalar a [versão mais recente do NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) *
-   Selecione a guia **online**
-   Selecionar o pacote do **EntityFramework**
-   Clique em **instalar**

Em seguida, precisamos trocar nosso modelo para gerar código que usa a API DbContext, que foi introduzida em versões posteriores do Entity Framework.

-   Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione **Adicionar item de geração de código...**
-   Selecione **Modelos online** no menu à esquerda e pesquise por **DbContext**
-   Selecione o gerador de DbContext do EF **5. x para C @ no__t-1**, digite **BloggingModel** como o nome e clique em **Adicionar**

    ![Modelo DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Lendo & gravando dados

Agora que temos um modelo, é hora de usá-lo para acessar alguns dados. As classes que usaremos para acessar os dados estão sendo geradas automaticamente para você com base no arquivo EDMX.

*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010, os arquivos BloggingModel.tt e BloggingModel.Context.tt estarão diretamente sob seu projeto, e não aninhados no arquivo EDMX.*

![Classes geradas DF](~/ef6/media/generatedclassesdf.png)

 

Implemente o método Main em Program.cs, conforme mostrado abaixo. Esse código cria uma nova instância do nosso contexto e, em seguida, a usa para inserir um novo blog. Em seguida, ele usa uma consulta LINQ para recuperar todos os Blogs do banco de dados ordenado alfabeticamente por título.

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

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. Lidando com alterações de banco de dados

Agora é hora de fazer algumas alterações no nosso esquema de banco de dados, quando fazemos essas alterações, também precisamos atualizar nosso modelo para refletir essas alterações.

A primeira etapa é fazer algumas alterações no esquema de banco de dados. Vamos adicionar uma tabela Users ao esquema.

-   Clique com o botão direito do mouse no banco de dados **DatabaseFirst. blog** em Gerenciador de servidores e selecione **nova consulta**
-   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Agora que o esquema está atualizado, é hora de atualizar o modelo com essas alterações.

-   Clique com o botão direito do mouse em um ponto vazio do modelo no designer do EF e selecione ' atualizar modelo do banco de dados... '. isso iniciará o assistente de atualização
-   Na guia adicionar do assistente de atualização, marque a caixa ao lado de tabelas, isso indica que queremos adicionar novas tabelas a partir do esquema.
    @no__t guia atualização de 0The mostra todas as tabelas existentes no modelo que serão verificadas quanto a alterações durante a atualização. As guias excluir mostram todas as tabelas que foram removidas do esquema e também serão removidas do modelo como parte da atualização. As informações nessas duas guias são detectadas automaticamente e são fornecidas apenas para fins informativos, você não pode alterar as configurações. *

    ![Assistente de atualização](~/ef6/media/refreshwizard.png)

-   Clique em concluir no assistente de atualização

 

O modelo agora é atualizado para incluir uma nova entidade de usuário que é mapeada para a tabela usuários que adicionamos ao banco de dados.

![Modelo atualizado](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Resumo

Neste tutorial, examinamos Database First desenvolvimento, que nos permitia criar um modelo no designer do EF com base em um banco de dados existente. Em seguida, usamos esse modelo para ler e gravar alguns dados do banco de dado. Por fim, atualizamos o modelo para refletir as alterações feitas no esquema de banco de dados.
