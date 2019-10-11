---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182438"
---
# <a name="model-first"></a>Model First
Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Model First usando Entity Framework. Model First permite que você crie um novo modelo usando o Entity Framework Designer e, em seguida, gere um esquema de banco de dados do modelo. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo e instruções passo a passo fornecem uma introdução ao desenvolvimento de Model First usando Entity Framework. Model First permite que você crie um novo modelo usando o Entity Framework Designer e, em seguida, gere um esquema de banco de dados do modelo. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes com as quais você interage em seu aplicativo são geradas automaticamente a partir do arquivo EDMX.

**Apresentado por**: [Rowan Miller](https://romiller.com/)

**Vídeo**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2010 ou o Visual Studio 2012 instalado para concluir este guia.

Se você estiver usando o Visual Studio 2010, também será necessário ter o [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. Criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o Model First para executar o acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo-&gt; Projeto New-&gt;...**
-   Selecione **Windows** no menu à esquerda e no **aplicativo de console**
-   Insira **ModelFirstSample** como o nome
-   Selecione **OK**

## <a name="2-create-model"></a>2. Criar modelo

Vamos usar Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto-&gt; adicionar novo item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **ADO.NET modelo de dados de entidade**
-   Insira **BloggingModel** como o nome e clique em **OK**, isso iniciará o assistente de modelo de dados de entidade
-   Selecione **modelo vazio** e clique em **concluir**

    ![Criar modelo vazio](~/ef6/media/createemptymodel.png)

O Entity Framework Designer é aberto com um modelo em branco. Agora, podemos começar a adicionar entidades, propriedades e associações ao modelo.

-   Clique com o botão direito do mouse na superfície de design e selecione **Propriedades**
-   No janela Propriedades alterar o **nome do contêiner de entidade** para **BloggingContext**
    *este é o nome do contexto derivado que será gerado para você, o contexto representa uma sessão com o banco de dados, permitindo que possamos consultar e salvar dados* do
-   Clique com o botão direito do mouse na superfície de design e selecione **Adicionar entidade New-&gt;...**
-   Insira o **blog** como o nome da entidade e o **blogid** como o nome da chave e clique em **OK**

    ![Adicionar entidade de blog](~/ef6/media/addblogentity.png)

-   Clique com o botão direito do mouse na nova entidade na superfície de design e selecione **Adicionar New-&gt; Propriedade escalar**, insira o **nome** como o nome da propriedade.
-   Repita esse processo para adicionar uma propriedade de **URL** .
-   Clique com o botão direito do mouse na propriedade **URL** na superfície de design e selecione **propriedades**, na janela Propriedades altere a configuração **Nullable** para **true**
    *isso nos permite salvar um blog no banco de dados sem atribuir a ele uma URL *
-   Usando as técnicas que você acabou de aprender, adicione uma entidade **post** com uma propriedade de chave **postid**
-   Adicionar propriedades escalares de **título** e **conteúdo** à entidade **post**

Agora que temos algumas entidades, é hora de adicionar uma associação (ou relação) entre elas.

-   Clique com o botão direito do mouse na superfície de design e selecione **Adicionar nova associação de &gt;...**
-   Faça com que uma extremidade da relação aponte para o **blog** com uma multiplicidade de **um** e o outro ponto de extremidade para **postar** com uma multiplicidade de **muitos**
    *isso significa que um blog tem muitas postagens e uma postagem pertence a um blog*
-   Verifique se a caixa de seleção **Adicionar propriedades de chave estrangeira à entidade ' Post '** está marcada e clique em **OK**

    ![Adicionar Associação MF](~/ef6/media/addassociationmf.png)

Agora temos um modelo simples para o qual podemos gerar um banco de dados e usá-lo para ler e gravar.

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

## <a name="3-generating-the-database"></a>3. Gerando o banco de dados

Considerando nosso modelo, Entity Framework pode calcular um esquema de banco de dados que nos permitirá armazenar e recuperar dados usando o modelo.

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, criará um banco de dados [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .

Vamos continuar e gerar o banco de dados.

-   Clique com o botão direito do mouse na superfície de design e selecione **gerar banco de dados do modelo...**
-   Clique em **nova conexão...** e especifique o LocalDB ou o SQL Express, dependendo da versão do Visual Studio que você está usando, insira **ModelFirst. blog** como o nome do banco de dados.

    ![Conexão LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexão do SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**
-   Selecione **Avançar** e o Entity Framework Designer calculará um script para criar o esquema de banco de dados
-   Depois que o script for exibido, clique em **concluir** e o script será adicionado ao seu projeto e aberto
-   Clique com o botão direito do mouse no script e selecione **executar**. você será solicitado a especificar o banco de dados ao qual se conectar, especificar LocalDB ou SQL Server Express, dependendo da versão do Visual Studio que você está usando

## <a name="4-reading--writing-data"></a>4. Lendo & gravando dados

Agora que temos um modelo, é hora de usá-lo para acessar alguns dados. As classes que usaremos para acessar os dados estão sendo geradas automaticamente para você com base no arquivo EDMX.

*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010, os arquivos BloggingModel.tt e BloggingModel.Context.tt estarão diretamente sob seu projeto, e não aninhados no arquivo EDMX.*

![Classes geradas](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Lidando com alterações de modelo

Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações, também precisamos atualizar o esquema do banco de dados.

Vamos começar adicionando uma nova entidade de usuário ao nosso modelo.

-   Adicione um novo nome de entidade de **usuário** com **username** como o nome da chave e a **cadeia de caracteres** como o tipo de propriedade para a chave

    ![Adicionar entidade de usuário](~/ef6/media/adduserentity.png)

-   Clique com o botão direito do mouse na propriedade **username** na superfície de design e selecione **Properties**, na janela Propriedades altere a configuração **MaxLength** para **50**
    *isso restringe os dados que podem ser armazenados em nome de usuário para 50 caracteres*
-   Adicionar uma propriedade escalar **DisplayName** à entidade do **usuário**

Agora temos um modelo atualizado e estamos prontos para atualizar o banco de dados para acomodar nosso novo tipo de entidade de usuário.

-   Clique com o botão direito do mouse na superfície de design e selecione **gerar banco de dados do modelo...** , Entity Framework calculará um script para recriar um esquema com base no modelo atualizado.
-   Clique em **concluir**
-   Você pode receber avisos sobre como substituir o script DDL existente e as partes de mapeamento e armazenamento do modelo, clicar em **Sim** para ambos os avisos
-   O script SQL atualizado para criar o banco de dados está aberto para você  
    o script *The gerado removerá todas as tabelas existentes e, em seguida, recriará o esquema do zero. Isso pode funcionar para o desenvolvimento local, mas não é viável para enviar alterações por push a um banco de dados que já foi implantado. Se você precisar publicar alterações em um banco de dados que já foi implantado, será necessário editar o script ou usar uma ferramenta de comparação de esquema para calcular um script de migração.*
-   Clique com o botão direito do mouse no script e selecione **executar**. você será solicitado a especificar o banco de dados ao qual se conectar, especificar LocalDB ou SQL Server Express, dependendo da versão do Visual Studio que você está usando

## <a name="summary"></a>Resumo

Neste tutorial, examinamos Model First desenvolvimento, que nos permitia criar um modelo no designer do EF e, em seguida, gerar um banco de dados a partir desse modelo. Em seguida, usamos o modelo para ler e gravar alguns dados do banco de dado. Por fim, atualizamos o modelo e recriamos o esquema de banco de dados para corresponder ao modelo.
