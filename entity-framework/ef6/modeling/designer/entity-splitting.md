---
title: Divisão de entidade do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418458"
---
# <a name="designer-entity-splitting"></a>Divisão de entidade do designer
Este tutorial mostra como mapear um tipo de entidade para duas tabelas modificando um modelo com o Entity Framework Designer (EF designer). Você pode mapear uma entidade para várias tabelas quando as tabelas compartilham uma chave comum. Os conceitos que se aplicam ao mapeamento de um tipo de entidade para duas tabelas são facilmente estendidos para mapear um tipo de entidade para mais de duas tabelas.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prerequisites

Visual Studio 2012 ou Visual Studio 2010, Ultimate, Premium, Professional ou Web Express Edition.

## <a name="create-the-database"></a>Criar o banco de dados

O servidor de banco de dados instalado com o Visual Studio é diferente dependendo da versão do Visual Studio instalada:

-   Se você estiver usando o Visual Studio 2012, criará um banco de dados LocalDB.
-   Se você estiver usando o Visual Studio 2010, criará um banco de dados do SQL Express.

Primeiro, criaremos um banco de dados com duas tabelas que vamos combinar em uma única entidade.

-   Abra o Visual Studio
-   **Exibir-&gt; Gerenciador de Servidores**
-   Clique com o botão direito em **conexões de dados-&gt; Adicionar conexão...**
-   Se você ainda não se conectou a um banco de dados do Gerenciador de Servidores antes de precisar selecionar **Microsoft SQL Server** como a fonte de dado
-   Conecte-se ao LocalDB ou ao SQL Express, dependendo de qual deles você instalou
-   Insira **EntitySplitting** como o nome do banco de dados
-   Selecione **OK** e você será perguntado se deseja criar um novo banco de dados, selecione **Sim**
-   O novo banco de dados aparecerá agora no Gerenciador de Servidores
-   Se você estiver usando o Visual Studio 2012
    -   Clique com o botão direito do mouse no banco de dados em Gerenciador de Servidores e selecione **nova consulta**
    -   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **executar**
-   Se você estiver usando o Visual Studio 2010
    -   Selecione **Data-&gt; Editor Transact-SQL-&gt; nova conexão de consulta...**
    -   Insira **.\\SQLExpress** como o nome do servidor e clique em **OK**
    -   Selecione o banco de dados **EntitySplitting** na lista suspensa na parte superior do editor de consultas
    -   Copie o SQL a seguir na nova consulta, clique com o botão direito do mouse na consulta e selecione **Executar SQL**

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

-   No menu **Arquivo** , aponte para **Novo**e clique em **Projeto**.
-   No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo **aplicativo de console** .
-   Insira **MapEntityToTablesSample** como o nome do projeto e clique em **OK**.
-   Clique em **não** se for solicitado a salvar a consulta SQL criada na primeira seção.

## <a name="create-a-model-based-on-the-database"></a>Criar um modelo com base no banco de dados

-   Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **MapEntityToTablesModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar.**
-   Selecione a conexão **EntitySplitting** na lista suspensa e clique em **Avançar**.
-   Na caixa de diálogo escolher seu objeto de banco de dados, marque a caixa ao lado do nó **tabelas** .
    Isso adicionará todas as tabelas do banco de dados **EntitySplitting** ao modelo.
-   Clique em **concluir**.

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.

## <a name="map-an-entity-to-two-tables"></a>Mapear uma entidade para duas tabelas

Nesta etapa, atualizaremos o tipo de entidade **Person** para combinar dados das tabelas **Person** e **personinfo** .

-   Selecione as propriedades de **email** e **telefone** da entidade **personinfo **e pressione **Ctrl + X** .
-   Selecione a entidade **Person **e pressione **Ctrl + V** teclas.
-   Na superfície de design, selecione a entidade **PersonInfo** e pressione o botão **excluir** no teclado.
-   Clique em **não** quando for perguntado se você deseja remover a tabela **personinfo** do modelo, estamos prestes a mapeá-la para a entidade **Person** .

    ![Excluir tabelas](~/ef6/media/deletetables.png)

As próximas etapas exigem a janela **detalhes do mapeamento** . Se você não puder ver essa janela, clique com o botão direito do mouse na superfície de design e selecione **detalhes de mapeamento**.

-   Selecione o tipo de entidade **pessoa** e clique em **&lt;adicionar uma tabela ou exibição&gt;**  na janela  **detalhes do mapeamento** .
-   Selecione **PersonInfo ** na lista suspensa.
    A janela de **detalhes de mapeamento** é atualizada com mapeamentos de coluna padrão, mas isso é bom para nosso cenário.

A **pessoa** tipo de entidade agora está mapeada para a **pessoa** e as tabelas de de **personinfo** .

![Mapeamento 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Usar o modelo

-   Cole o código a seguir no método Main.

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

As instruções T-SQL a seguir foram executadas no banco de dados como resultado da execução deste aplicativo. 

-   As duas instruções **Insert** a seguir foram executadas como resultado da execução do contexto. SaveChanges (). Eles pegam os dados da entidade **Person** e os dividem entre as tabelas **Person** e **personinfo** .

    ![Inserir 1](~/ef6/media/insert1.png)

    ![Inserir 2](~/ef6/media/insert2.png)
-   A **seleção** a seguir foi executada como resultado da enumeração das pessoas no banco de dados. Ele combina os dados da tabela **Person** e **personinfo** .

    ![Selecionar](~/ef6/media/select.png)
