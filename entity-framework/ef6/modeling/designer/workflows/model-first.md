---
title: Model First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 3dd0eba29619f09995d7009dd29462c14bde98c4
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251135"
---
# <a name="model-first"></a>Model First
Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Model First usando o Entity Framework. Modelo primeiro permite que você criar um novo modelo usando o Entity Framework Designer e, em seguida, gerar um esquema de banco de dados do modelo. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.

## <a name="watch-the-video"></a>Assista ao vídeo
Este passo a passo e vídeo passo a passo fornecem uma introdução ao desenvolvimento Model First usando o Entity Framework. Modelo primeiro permite que você criar um novo modelo usando o Entity Framework Designer e, em seguida, gerar um esquema de banco de dados do modelo. O modelo é armazenado em um arquivo EDMX (extensão. edmx) e pode ser exibido e editado no Entity Framework Designer. As classes que interagem com em seu aplicativo são geradas automaticamente do arquivo EDMX.

**Apresentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2010 ou Visual Studio 2012 instalado para concluir este passo a passo.

Se você estiver usando o Visual Studio 2010, você também precisará ter [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. Criar o aplicativo

Para manter as coisas simples, vamos criar um aplicativo de console básico que usa o modelo primeiro para executar acesso a dados:

-   Abrir o Visual Studio
-   **Arquivo -&gt; New -&gt; projeto...**
-   Selecione **Windows** no menu à esquerda e **aplicativo de Console**
-   Insira **ModelFirstSample** como o nome
-   Selecione **OK**

## <a name="2-create-model"></a>2. Criar modelo

Vamos fazer uso do Entity Framework Designer, que é incluído como parte do Visual Studio, para criar nosso modelo.

-   **Projeto -&gt; Adicionar Novo Item...**
-   Selecione **dados** no menu à esquerda e, em seguida, **modelo de dados de entidade ADO.NET**
-   Insira **BloggingModel** como o nome e clique em **Okey**, isso inicia o Assistente de modelo de dados de entidade
-   Selecione **modelo vazio** e clique em **concluir**

    ![Criar um modelo vazio](~/ef6/media/createemptymodel.png)

O Entity Framework Designer é aberto com um modelo em branco. Agora podemos começar a adicionar entidades, propriedades e associações no modelo.

-   Clique com botão direito na superfície do design e selecione **propriedades**
-   Em que a alteração da janela de propriedades de **nome do contêiner de entidade** para **BloggingContext**
    *esse é o nome do contexto derivado que será gerado para você, o contexto representa uma sessão com o banco de dados, que nos permite consultar e salvar dados*
-   Clique com botão direito na superfície do design e selecione **adicionar novo -&gt; Entity...**
-   Insira **Blog** como o nome da entidade e **BlogId** como o nome da chave e clique em **Okey**

    ![Adicionar entidade de Blog](~/ef6/media/addblogentity.png)

-   Clique com botão direito na nova entidade na superfície do design e selecione **adicionar novo -&gt; propriedade escalar**, insira **nome** como o nome da propriedade.
-   Repita esse processo para adicionar um **Url** propriedade.
-   Com o botão direito no **Url** propriedade na superfície do design e selecione **propriedades**, em que a alteração da janela de propriedades a **Nullable** definindo como **True** 
     *Isso nos permite economizar um Blog no banco de dados sem atribuí-lo uma Url*
-   Usando as técnicas que você acabou de adquiridos, adicione uma **Post** entidade com um **PostId** propriedade de chave
-   Adicione **Title** e **conteúdo** propriedades escalares para o **Post** entidade

Agora que temos duas entidades, é hora de adicionar uma associação (ou relação) entre eles.

-   Clique com botão direito na superfície do design e selecione **adicionar novo -&gt; associação...**
-   Fazer uma extremidade da relação aponte para **Blog** com uma multiplicidade de **um** e o outro ponto de extremidade para **Post** com uma multiplicidade de **muitos** 
     *Isso significa que um Blog tem muitas postagens e uma postagem pertencer a um Blog*
-   Verifique se o **adicionar propriedades de chave estrangeira para 'Post' Entity** caixa está marcada e clique em **Okey**

    ![Adicionar associação MF](~/ef6/media/addassociationmf.png)

Agora temos um modelo simples que podemos gerar um banco de dados e usar para ler e gravar dados.

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

## <a name="3-generating-the-database"></a>3. Gerando o banco de dados

Considerando nosso modelo, o Entity Framework pode calcular um esquema de banco de dados que nos permitirá armazenar e recuperar dados usando o modelo.

O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.
-   Se você estiver usando o Visual Studio 2012, você criará um [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) banco de dados.

Vamos prosseguir e gerar o banco de dados.

-   Clique com botão direito na superfície do design e selecione **gerar banco de dados do modelo...**
-   Clique em **nova Conexão...** e especifique o LocalDB ou Express do SQL, dependendo de qual versão do Visual Studio, você está usando, insira **ModelFirst.Blogging** como o nome do banco de dados.

    ![Conexão LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexão do SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**
-   Selecione **próxima** e o Entity Framework Designer calculará um script para criar o esquema de banco de dados
-   Depois que o script é exibido, clique em **concluir** e o script será adicionado ao seu projeto e aberto
-   Clique duas vezes em script e selecione **Execute**, você será solicitado a especificar o banco de dados para se conectar ao, especifique o LocalDB ou Express do SQL Server, dependendo de qual versão do Visual Studio que você está usando

## <a name="4-reading--writing-data"></a>4. Ler e gravar dados

Agora que temos um modelo que é hora de usá-lo para acessar alguns dados. As classes que vamos usar para acessar os dados são gerados automaticamente para você com base no arquivo EDMX.

*Esta captura de tela é do Visual Studio 2012, se você estiver usando o Visual Studio 2010 a BloggingModel.tt e BloggingModel.Context.tt arquivos diretamente em seu projeto, em vez de aninhado sob o arquivo EDMX.*

![Classes geradas](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Lidando com alterações no modelo

Agora é hora de fazer algumas alterações em nosso modelo, quando fazemos essas alterações que também precisamos atualizar o esquema de banco de dados.

Vamos começar adicionando uma nova entidade de usuário ao nosso modelo.

-   Adicione um novo **usuário** com o nome da entidade **nome de usuário** como o nome da chave e **cadeia de caracteres** como o tipo de propriedade da chave

    ![Adicionar entidade de usuário](~/ef6/media/adduserentity.png)

-   Com o botão direito no **nome de usuário** propriedade na superfície do design e selecione **propriedades**, alteração de janela em Propriedades de **MaxLength** definindo como **50 ** 
     *Isso restringe os dados que podem ser armazenados em nome de usuário a 50 caracteres*
-   Adicionar um **DisplayName** propriedade escalar para o **usuário** entidade

Agora temos um modelo atualizado e você está pronto para atualizar o banco de dados para acomodar o nosso novo tipo de entidade do usuário.

-   Clique com botão direito na superfície do design e selecione **gerar banco de dados do modelo...** , Entity Framework irá calcular um script para recriar um esquema com base no modelo atualizado.
-   Clique em **concluir**
-   Você pode receber avisos sobre a substituição de script DDL existente e as partes de armazenamento e mapeamento do modelo, clique em **Sim** para ambos os esses avisos
-   O script SQL atualizado para criar o banco de dados é aberto para você  
    *O script gerado será descartar todas as tabelas existentes e, em seguida, recriar o esquema do zero. Isso pode funcionar para o desenvolvimento local, mas não é viável para enviar alterações por push para um banco de dados que já foi implantado. Se você precisar publicar alterações em um banco de dados que já tenha sido implantado, você precisará editar o script ou usar uma ferramenta de comparação de esquema para calcular um script de migração.*
-   Clique duas vezes em script e selecione **Execute**, você será solicitado a especificar o banco de dados para se conectar ao, especifique o LocalDB ou Express do SQL Server, dependendo de qual versão do Visual Studio que você está usando

## <a name="summary"></a>Resumo

Neste passo a passo que vimos desenvolvimento Model First, que nos permitiu criar um modelo no Designer de EF e, em seguida, gerar um banco de dados de modelo. Em seguida, usamos o modelo para ler e gravar alguns dados do banco de dados. Finalmente, atualizado o modelo e, em seguida, recriado o esquema de banco de dados para corresponder ao modelo.
