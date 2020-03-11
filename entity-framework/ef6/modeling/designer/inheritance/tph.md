---
title: Herança de TPH do designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418423"
---
# <a name="designer-tph-inheritance"></a>Herança de TPH do designer
Este guia passo a passo mostra como implementar a herança de tabela por hierarquia (TPH) em seu modelo conceitual com o Entity Framework Designer (EF designer). A herança de TPH usa uma tabela de banco de dados para manter todos os tipos de entidade em uma hierarquia de herança.

Neste tutorial, mapearemos a tabela Person para três tipos de entidade: Person (o tipo base), Student (derivada de Person) e instrutor (derivado de Person). Vamos criar um modelo conceitual do banco de dados (Database First) e, em seguida, alterar o modelo para implementar a herança TPH usando o designer do EF.

É possível mapear para uma herança TPH usando Model First, mas você teria que escrever seu próprio fluxo de trabalho de geração de banco de dados, que é complexo. Em seguida, você atribuiria esse fluxo de trabalho à propriedade de **fluxo de trabalho de geração de banco de dados** no designer do EF. Uma alternativa mais fácil é usar Code First.

## <a name="other-inheritance-options"></a>Outras opções de herança

A tabela por tipo (TPT) é outro tipo de herança no qual as tabelas separadas no banco de dados são mapeadas para entidades que participam da herança.  Para obter informações sobre como mapear a herança de tabela por tipo com o designer do EF, consulte [herança do TPT do EF designer](~/ef6/modeling/designer/inheritance/tpt.md).

A herança de tipo de tabela por concreto (TPC) e os modelos de herança misto têm suporte pelo tempo de execução Entity Framework, mas não têm suporte do designer do EF. Se você quiser usar o TPC ou a herança mista, terá duas opções: Use Code First ou edite manualmente o arquivo EDMX. Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.

## <a name="prerequisites"></a>Prerequisites

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo-&gt; projeto de novo&gt;**
-   No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console** .
-   Insira **TPHDBFirstSample** como o nome.
-   Selecione  **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **TPHModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.
-   Clique em **nova conexão**.
    Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
-   Na caixa de diálogo escolher seu objeto de banco de dados, no nó tabelas, selecione a tabela **Person** .
-   Clique em **concluir**.

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa de diálogo escolher seus objetos de banco de dados são adicionados ao modelo.

É assim que a tabela **Person** procura no banco de dados.

![Tabela Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementar a herança de tabela por hierarquia

A tabela **Person** tem a coluna **discriminadora** , que pode ter um dos dois valores: "Student" e "instrutor". Dependendo do valor, a tabela **Person** será mapeada para a entidade **Student** ou o **instrutor** . A tabela **Person** também tem duas colunas, **contratou** e **EnrollmentDate**, que devem ser **anuláveis** porque uma pessoa não pode ser um aluno e um instrutor ao mesmo tempo (pelo menos não neste passo-a).

### <a name="add-new-entities"></a>Adicionar novas entidades

-   Adicione uma nova entidade.
    Para fazer isso, clique com o botão direito do mouse em um espaço vazio da superfície de design do Entity Framework Designer e selecione **entidade Add-&gt;** .
-   Digite do **instrutor** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo base**.
-   Clique em **OK**.
-   Adicione outra nova entidade. Digite **Student** para o **nome da entidade** e selecione **Person** na lista suspensa para o **tipo base**.

Dois novos tipos de entidade foram adicionados à superfície de design. Uma seta aponta dos novos tipos de entidade para a **pessoa** tipo de entidade; Isso indica que a **pessoa** é o tipo base para os novos tipos de entidade.

-   Clique com o botão direito do mouse na propriedade **Contratada** da entidade **Person** . Selecione **recortar** (ou use a tecla CTRL-X).
-   Clique com o botão direito do mouse na entidade do **instrutor** e selecione **colar** (ou use a tecla Ctrl-V).
-   Clique com o botão direito do mouse na propriedade **Contratada** e selecione **Propriedades**.
-   Na janela de **Propriedades** , defina a propriedade **Nullable** como **false**.
-   Clique com o botão direito do mouse na propriedade **EnrollmentDate** da entidade **Person** . Selecione **recortar** (ou use a tecla CTRL-X).
-   Clique com o botão direito do mouse na entidade **Student** e selecione **colar (ou use a tecla Ctrl-V).**
-   Selecione a propriedade de  **EnrollmentDate** e defina a propriedade **Nullable** como **false**.
-   Selecione o tipo de entidade  **pessoa** . Na janela de **Propriedades** , defina sua propriedade **abstrata** como **true**.
-   Exclua a propriedade **discriminadora** de **Person**. O motivo pelo qual ele deve ser excluído é explicado na seção a seguir.

### <a name="map-the-entities"></a>Mapear as entidades

-   Clique com o botão direito do mouse no **instrutor** e selecione **mapeamento de tabela.**
    A entidade do instrutor é selecionada na janela detalhes do mapeamento.
-   Clique **&lt;adicionar uma tabela ou exibição&gt;**  na janela **detalhes do mapeamento** .
    O **&lt;adicionar uma tabela ou exibição&gt;** campo se torna uma lista suspensa de tabelas ou exibições para as quais a entidade selecionada pode ser mapeada.
-   Selecione **pessoa** na lista suspensa.
-   A janela **detalhes do mapeamento** é atualizada com mapeamentos de coluna padrão e uma opção para adicionar uma condição.
-   Clique em **&lt;adicionar uma condição&gt;** .
    O **&lt;adicionar uma condição&gt;** campo se torna uma lista suspensa de colunas para as quais as condições podem ser definidas.
-   Selecione  **discriminador** na lista suspensa.
-   Na coluna **operador** da janela **detalhes do mapeamento** , selecione = na lista suspensa.
-   Na coluna **valor/Propriedade** , digite **instrutor**. O resultado final deve ser assim:

    ![Detalhes do mapeamento](~/ef6/media/mappingdetails2.png)

-   Repita essas etapas para o tipo de entidade **student** , mas torne a condição igual ao valor **Student** .  
    *O motivo pelo qual queríamos remover a propriedade **discriminador** é porque você não pode mapear uma coluna de tabela mais de uma vez. Esta coluna será usada para mapeamento condicional e, portanto, não poderá ser usada para mapeamento de propriedade também. A única maneira de poder ser usada para ambos, se uma condição usar uma  **nula** ou **não for nula** comparação.*

A herança de tabela por hierarquia agora está implementada.

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Usar o modelo

Abra o arquivo **Program.cs** em que o método **Main** está definido. Cole o código a seguir na função **Main** . O código executa três consultas. A primeira consulta traz de volta todos os objetos **Person** . A segunda consulta usa o método **OfType** para retornar objetos do **instrutor** . A terceira consulta usa o método **OfType** para retornar objetos **Student** .

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
