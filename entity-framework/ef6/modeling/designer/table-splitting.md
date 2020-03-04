---
title: Divisão de tabela do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921779"
---
# <a name="designer-table-splitting"></a>Divisão de tabela do designer
Este tutorial mostra como mapear vários tipos de entidade para uma única tabela modificando um modelo com a Entity Framework Designer (Designer de EF).

Um motivo para usar a divisão de tabela é atrasar o carregamento de algumas propriedades ao usar o carregamento lento para carregar seus objetos. Você pode separar as propriedades que podem conter uma quantidade muito grande de dados em uma entidade separada e carregá-los apenas quando necessário.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prerequisites

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

Este tutorial está usando o Visual Studio 2012.

-   Abra o Visual Studio 2012.
-   No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.
-   No painel esquerdo, clique em Visual C\#e selecione o modelo aplicativo de console.
-   Insira **TableSplittingSample** como o nome do projeto e clique em **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Criar um modelo com base no banco de dados escolar

-   Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **TableSplittingModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar.**
-   Clique em Nova conexão. Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(\\LocalDB) mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
-   Na caixa de diálogo escolher seu objeto de banco de dados, desdobrar o nó **tabelas** e verificar a tabela **Person** . Isso adicionará a tabela especificada ao modelo **escolar** .
-   Clique em **concluir**.

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa de diálogo **escolher seus objetos** de banco de dados são adicionados ao modelo.

## <a name="map-two-entities-to-a-single-table"></a>Mapear duas entidades para uma única tabela

Nesta seção, você irá dividir a entidade **Person** em duas entidades e, em seguida, mapeá-las para uma única tabela.

> [!NOTE]
> A entidade **Person** não contém nenhuma propriedade que possa conter uma grande quantidade de dados; Ele é usado apenas como exemplo.

-   Clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e clique em **entidade**.
    A caixa de diálogo **nova entidade** é exibida.
-   Digite **HireInfo** para o **nome da entidade** e **PersonID** para o nome da **propriedade de chave** .
-   Clique em **OK**.
-   Um novo tipo de entidade é criado e exibido na superfície de design.
-   Selecione a ****  Propriedade HireDate do tipo de entidade **Person** e pressione as teclas **Ctrl + X** .
-   Selecione a entidade **HireInfo** e pressione **Ctrl + V** teclas.
-   Crie uma associação entre **Person** e **HireInfo**. Para fazer isso, clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e clique em **Associação**.
-   A caixa de diálogo **Adicionar Associação** é exibida. O nome do **PersonHireInfo** é fornecido por padrão.
-   Especifique a multiplicidade **1 (uma)** em ambas as extremidades da relação.
-   Pressione **OK**.

A próxima etapa requer a janela **detalhes** do mapeamento. Se você não puder ver essa janela, clique com o botão direito do mouse na superfície de design e selecione **detalhes de mapeamento**.

-   Selecione o tipo de entidade **HireInfo** e clique em **&lt;adicionar uma tabela&gt;ou exibição** na janela **detalhes** do mapeamento.
-   Selecione **pessoa** na lista suspensa  **&lt;adicionar uma tabela ou&gt;um**campo de exibição.  A lista contém tabelas ou exibições às quais a entidade selecionada pode ser mapeada.
    As propriedades apropriadas devem ser mapeadas por padrão.

    ![correlação](~/ef6/media/mapping.png)

-   Selecione a associação **PersonHireInfo** na superfície de design.
-   Clique com o botão direito do mouse na associação na superfície de design e selecione **Propriedades**.
-   Na janela **Propriedades** , selecione a propriedade **restrições referenciais** e clique no botão reticências.
-   Selecione **pessoa** na lista suspensa **principal** .
-   Pressione **OK**.

 

## <a name="use-the-model"></a>Usar o modelo

-   Cole o código a seguir no método Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Compile e execute o aplicativo.

As instruções T-SQL a seguir foram executadas no banco de dados **escolar** como resultado da execução deste aplicativo. 

-   A **inserção** a seguir foi executada como resultado da execução do contexto. SaveChanges () e combina dados das entidades **Person** e **HireInfo**

    ![Inserir](~/ef6/media/insert.png)

-   A **seleção** a seguir foi executada como resultado da execução do contexto. People. FirstOrDefault () e seleciona apenas as colunas mapeadas para **Person**

    ![Selecione 1](~/ef6/media/select1.png)

-   A **seleção** a seguir foi executada como resultado do acesso à propriedade de navegação ExistingPerson. instrutor e seleciona apenas as colunas mapeadas para **HireInfo**

    ![Selecionar 2](~/ef6/media/select2.png)
