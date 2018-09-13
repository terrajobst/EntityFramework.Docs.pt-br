---
title: A divisão de tabela Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 8b0ca6778a06ed43b1365d2e5969ff15948f8004
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490664"
---
# <a name="designer-table-splitting"></a>Tabela do Designer de divisão
Este passo a passo mostra como mapear vários tipos de entidade para uma única tabela, modificando um modelo com o Entity Framework Designer (Designer de EF).

Um motivo que você talvez queira usar a divisão de tabela está atrasando o carregamento de algumas propriedades ao usar o carregamento para carregar os objetos lento. Você pode separar as propriedades que podem conter uma quantidade muito grande de dados em uma entidade separada e carregar apenas quando necessário.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

Este passo a passo é usando o Visual Studio 2012.

-   Abra o Visual Studio 2012.
-   No menu **Arquivo**, aponte para **Novo** e clique em **Projeto**.
-   No painel esquerdo, clique em Visual C\#e, em seguida, selecione o modelo de aplicativo de Console.
-   Insira **TableSplittingSample** como o nome do projeto e clique **Okey**.

## <a name="create-a-model-based-on-the-school-database"></a>Criar um modelo com base no banco de dados School

-   Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **TableSplittingModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar.**
-   Clique em nova Conexão. Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na caixa de diálogo Choose Your Database Objects, desdobrar o **tabelas** nó e verifique se o **pessoa** tabela. Isso adicionará a tabela especificada para o **School** modelo.
-   Clique em **Finalizar**.

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na **Choose Your Database Objects** caixa de diálogo são adicionados ao modelo.

## <a name="map-two-entities-to-a-single-table"></a>Mapear duas entidades para uma única tabela

Nesta seção, você dividirá a **pessoa** entidade em duas entidades e, em seguida, mapeá-los para uma única tabela.

> [!NOTE]
> O **pessoa** entidade não contém todas as propriedades que podem conter grandes quantidades de dados; ele é usado apenas como um exemplo.

-   Clique em uma área vazia da superfície de design, aponte para **adicionar novo**e clique em **entidade**.
    O **nova entidade** caixa de diálogo é exibida.
-   Tipo de **HireInfo** para o **nome da entidade** e **PersonID** para o **propriedade Key** nome.
-   Clique em **OK**.
-   Um novo tipo de entidade é criado e exibido na superfície de design.
-   Selecione o **HireDate** propriedade da **pessoa** tipo de entidade e pressione **Ctrl + X** chaves.
-   Selecione o **HireInfo** entidade e pressione **Ctrl + V** chaves.
-   Criar uma associação entre **pessoa** e **HireInfo**. Para fazer isso, clique em uma área vazia da superfície de design, aponte para **adicionar novo**e clique em **associação**.
-   O **Adicionar associação** caixa de diálogo é exibida. O **PersonHireInfo** nome for fornecido por padrão.
-   Especificar multiplicidade **1(One)** em ambas as extremidades da relação.
-   Pressione **Okey**.

A próxima etapa exige a **Mapping Details** janela. Se você não vir essa janela, clique com botão direito a superfície de design e selecione **Mapping Details**.

-   Selecione o **HireInfo** tipo de entidade e clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.
-   Selecione **pessoa** da **&lt;adicionar uma tabela ou exibição&gt;** campo na lista suspensa. A lista contém tabelas ou exibições para o qual a entidade selecionada pode ser mapeada.
    As propriedades adequadas devem ser mapeadas por padrão.

    ![Mapeamento](~/ef6/media/mapping.png)

-   Selecione o **PersonHireInfo** associação na superfície de design.
-   Clique com botão direito a associação na superfície do design e selecione **propriedades**.
-   No **propriedades** janela, selecione a **restrições referenciais** propriedade e clique no botão de reticências.
-   Selecione **pessoa** da **Principal** lista suspensa.
-   Pressione **Okey**.

 

## <a name="use-the-model"></a>Usar o modelo

-   Cole o seguinte código no método Main.

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

Instruções T-SQL a seguir foram executadas em relação a **School** banco de dados como resultado da execução desse aplicativo. 

-   O seguinte **inserir** foi executado como resultado da execução de contexto. Dados de SaveChanges () e combina o **pessoa** e **HireInfo** entidades

    ![Inserir](~/ef6/media/insert.png)

-   O seguinte **selecionar** foi executado como resultado da execução de contexto. People.FirstOrDefault() e seleciona apenas as colunas mapeadas para **pessoa**

    ![Selecione 1](~/ef6/media/select1.png)

-   O seguinte **selecionar** foi executado como resultado de acessar o existingPerson.Instructor de propriedade de navegação e seleciona apenas as colunas mapeadas para **HireInfo**

    ![Selecione 2](~/ef6/media/select2.png)
