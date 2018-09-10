---
title: Separação da entidade Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250588"
---
# <a name="designer-entity-splitting"></a>Separação da entidade Designer
Este passo a passo mostra como mapear um tipo de entidade para duas tabelas, modificando um modelo com o Entity Framework Designer (Designer de EF). Você pode mapear uma entidade para várias tabelas quando as tabelas compartilham uma chave comum. Os conceitos que se aplicam a um tipo de entidade de mapeamento com duas tabelas são facilmente estendidos para mapear um tipo de entidade para mais de duas tabelas.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Pré-requisitos

O Visual Studio 2012 ou Visual Studio 2010, Ultimate, Premium, Professional ou Web Express edition.

## <a name="create-the-database"></a>Criar o banco de dados

O servidor de banco de dados que é instalado com o Visual Studio é diferente dependendo da versão do Visual Studio que você instalou:

-   Se você estiver usando o Visual Studio 2012, em seguida, você criará um banco de dados LocalDB.
-   Se você estiver usando o Visual Studio 2010 você criará um banco de dados do SQL Express.

Primeiro, criaremos um banco de dados com duas tabelas, vamos combinar em uma única entidade.

-   Abrir o Visual Studio
-   **Exibição -&gt; Gerenciador de servidores**
-   Clique com botão direito **conexões de dados -&gt; Adicionar Conexão...**
-   Se você ainda não tiver conectado a um banco de dados do Gerenciador de servidores antes de você precisará selecionar **Microsoft SQL Server** como fonte de dados
-   Conectar-se ao LocalDB ou Express do SQL, dependendo de qual deles você ter instalado
-   Insira **EntitySplitting** como o nome do banco de dados
-   Selecione **Okey** e você será solicitado se você deseja criar um novo banco de dados, selecione **Sim**
-   O novo banco de dados agora aparecerão no Gerenciador de servidores
-   Se você estiver usando o Visual Studio 2012
    -   Clique com botão direito no banco de dados no Gerenciador de servidores e selecione **nova consulta**
    -   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **Execute**
-   Se você estiver usando o Visual Studio 2010
    -   Selecione **dados -&gt; Transact SQL Editor -&gt; nova Conexão de consulta...**
    -   Insira **.\\ SQLEXPRESS** como o nome do servidor e clique em **Okey**
    -   Selecione o **EntitySplitting** de banco de dados na lista suspensa na parte superior do editor de consultas
    -   Copie o SQL a seguir para a nova consulta e, em seguida, clique com botão direito na consulta e selecione **executar SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Criar o projeto

-   No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **aplicativo de Console** modelo.
-   Insira **MapEntityToTablesSample** como o nome do projeto e clique **Okey**.
-   Clique em **não** se solicitado a salvar a consulta SQL criada na primeira seção.

## <a name="create-a-model-based-on-the-database"></a>Criar um modelo baseado no banco de dados

-   Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **MapEntityToTablesModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar.**
-   Selecione o **EntitySplitting** conexão na lista suspensa e clique em **próxima**.
-   Na caixa de diálogo Choose Your Database Objects, marque a caixa ao lado de **tabelas** nó.
    Isso adicionará todas as tabelas do **EntitySplitting** banco de dados para o modelo.
-   Clique em **Finalizar**.

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.

## <a name="map-an-entity-to-two-tables"></a>Mapear uma entidade para duas tabelas

Nesta etapa, atualizaremos o **pessoa** tipo de entidade para combinar dados do **pessoa** e **PersonInfo** tabelas.

-   Selecione o **Email** e **telefone** propriedades do * * PersonInfo * * entity e pressione **Ctrl + X** chaves.
-   Selecione o * * pessoa * * entity e pressione **Ctrl + V** chaves.
-   Na superfície de design, selecione o **PersonInfo** entidade e pressione **excluir** no teclado.
-   Clique em **nenhuma** quando perguntado se você deseja remover o **PersonInfo** tabela do modelo, estamos prestes a mapeá-lo para o **pessoa** entidade.

    ![Excluir tabelas](~/ef6/media/deletetables.png)

As próximas etapas exigem o **Mapping Details** janela. Se você não vir essa janela, clique com botão direito a superfície de design e selecione **Mapping Details**.

-   Selecione o **pessoa** tipo de entidade e clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.
-   Selecione * * PersonInfo * * na lista suspensa.
    O **Mapping Details** janela é atualizada com mapeamentos de coluna padrão, essas são boas para nosso cenário.

O **pessoa** tipo de entidade agora está mapeado para o **pessoa** e **PersonInfo** tabelas.

![Mapeamento de 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Usar o modelo

-   Cole o seguinte código no método Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Compile e execute o aplicativo.

Instruções T-SQL a seguir foram executadas no banco de dados como resultado da execução desse aplicativo. 

-   Os dois seguintes **inserir** instruções foram executadas como resultado da execução de contexto. SaveChanges (). Eles levam os dados a partir o **pessoa** entidade e dividi-la entre a **pessoa** e **PersonInfo** tabelas.

    ![Insira 1](~/ef6/media/insert1.png)

    ![Insira 2](~/ef6/media/insert2.png)
-   O seguinte **selecionar** foi executado como resultado de enumerar as pessoas no banco de dados. Ele combina os dados a partir de **pessoa** e **PersonInfo** tabela.

    ![Selecionar](~/ef6/media/select.png)
