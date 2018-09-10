---
title: Herança TPH Designer – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250836"
---
# <a name="designer-tph-inheritance"></a>Herança TPH Designer
Este passo a passo mostra como implementar a herança do tabela por hierarquia (TPH) em seu modelo conceitual com o Entity Framework Designer (Designer de EF). Herança TPH usa uma tabela de banco de dados para manter os dados para todos os tipos de entidade em uma hierarquia de herança.

Neste passo a passo, podemos mapeará a tabela Person para três tipos de entidade: pessoa (o tipo base), (deriva de pessoa) de aluno e instrutor (deriva de pessoa). Vamos criar um modelo conceitual do banco de dados (banco de dados primeiro) e, em seguida, alterar o modelo para implementar a herança TPH usando o EF Designer.

É possível mapear para uma herança TPH usando Model First, mas você precisaria escrever seu próprio fluxo de trabalho de geração de banco de dados que é complexo. Em seguida, atribua esse fluxo de trabalho para o **fluxo de trabalho de geração de banco de dados** propriedade no EF Designer. Uma alternativa mais fácil é usar o Code First.

## <a name="other-inheritance-options"></a>Outras opções de herança

Tabela por tipo (TPT) é outro tipo de herança em que tabelas separadas no banco de dados são mapeadas para entidades que participam de herança.  Para obter informações sobre como mapear a herança de tabela por tipo com o EF Designer, consulte [EF Designer TPT herança](~/ef6/modeling/designer/inheritance/tpt.md).

Herança de tipo de tabela por concreto (TPC) e modelos de herança misto são compatíveis com o tempo de execução do Entity Framework, mas não são compatíveis com o EF Designer. Se você quiser usar TPC ou herança mista, você tem duas opções: usar o Code First ou editar manualmente o arquivo EDMX. Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo -&gt; New -&gt; projeto**
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.
-   Insira **TPHDBFirstSample** como o nome.
-   Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   O nome do projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **TPHModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.
-   Clique em **nova Conexão**.
    Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na caixa de diálogo Choose Your Database Objects, sob o nó de tabelas, selecione a **pessoa** tabela.
-   Clique em **Finalizar**.

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa de diálogo Choose Your Database Objects são adicionados ao modelo.

Isto é como o **pessoa** tabela fica no banco de dados.

![Tabela Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementar a herança de tabela por hierarquia

O **pessoa** a tabela tem o **discriminador** coluna, que pode ter um dos dois valores: "Aluno" e "Instrutor". Dependendo do valor de **pessoa** tabela será mapeada para o **aluno** entidade ou o **instrutor** entidade. O **pessoa** tabela também tem duas colunas, **HireDate** e **EnrollmentDate**, que deve ser **anulável** porque uma pessoa não pode ser um aluno e instrutor ao mesmo tempo (pelo menos não neste passo a passo).

### <a name="add-new-entities"></a>Adicionar novas entidades

-   Adicione uma nova entidade.
    Para fazer isso, clique com botão direito em uma área vazia da superfície de design do Entity Framework Designer e selecione **Add -&gt;entidade**.
-   Tipo de **instrutor** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo Base**.
-   Clique em **OK**.
-   Adicione outra nova entidade. Tipo de **aluno** para o **nome da entidade** e selecione **pessoa** na lista suspensa para o **tipo Base**.

Dois novos tipos de entidade foram adicionados à superfície de design. Uma seta aponta de novos tipos de entidade para o **pessoa** tipo de entidade; isso indica que **pessoa** é o tipo base para os novos tipos de entidade.

-   Clique com botão direito do **HireDate** propriedade do **pessoa** entidade. Selecione **Recortar** (ou use a tecla Ctrl-X).
-   Clique com botão direito do **instrutor** entidade e selecione **colar** (ou use a tecla Ctrl + V).
-   Clique com botão direito do **HireDate** propriedade e selecione **propriedades**.
-   No **propriedades** janela, defina as **Nullable** propriedade a ser **false**.
-   Clique com botão direito do **EnrollmentDate** propriedade do **pessoa** entidade. Selecione **Recortar** (ou use a tecla Ctrl-X).
-   Clique com botão direito do **aluno** entidade e selecione **Colar (ou chave de uso de Ctrl + V).**
-   Selecione o **EnrollmentDate** propriedade e defina o **Nullable** propriedade a ser **false**.
-   Selecione o **pessoa** tipo de entidade. No **propriedades** janela, defina seu **abstrata** propriedade a ser **verdadeiro**.
-   Excluir o **discriminador** propriedade de **pessoa**. O motivo pelo qual que ele deve ser excluído é explicado na seção a seguir.

### <a name="map-the-entities"></a>As entidades do mapa

-   Clique com botão direito do **instrutor** e selecione **mapeamento de tabela.**
    A entidade Instructor é selecionada na janela de detalhes de mapeamento.
-   Clique em **&lt;adicionar uma tabela ou exibição&gt;** no **Mapping Details** janela.
    O **&lt;adicionar uma tabela ou exibição&gt;** campo se torna uma lista suspensa de tabelas ou exibições para o qual a entidade selecionada pode ser mapeada.
-   Selecione **pessoa** na lista suspensa.
-   O **Mapping Details** janela é atualizada com mapeamentos de coluna padrão e uma opção para adicionar uma condição.
-   Clique em  **&lt;adicionar uma condição&gt;**.
    O **&lt;adicionar uma condição&gt;** campo se torna uma lista suspensa de colunas para o qual as condições podem ser definidas.
-   Selecione **discriminador** na lista suspensa.
-   No **operador** coluna o **Mapping Details** janela, selecione = na lista suspensa.
-   No **propriedade/valor** coluna, digite **instrutor**. O resultado final deve ter esta aparência:

    ![Detalhes de mapeamento](~/ef6/media/mappingdetails2.png)

-   Repita essas etapas para o **aluno** tipo de entidade, mas verifique a condição igual a **aluno** valor.  
    *O motivo pelo qual desejamos remover os **discriminador** propriedade, é porque você não pode mapear uma coluna de tabela mais de uma vez. Esta coluna será usada para mapeamento condicional, então ele não pode ser usado para mapeamento de propriedade também. A única maneira que ele pode ser usado para ambos, se uma condição usa um **é nulo** ou **Is Not Null** comparação.*

Herança de tabela por hierarquia agora é implementada.

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Usar o modelo

Abra o **Program.cs** do arquivo onde o **Main** método é definido. Cole o seguinte código para o **Main** função. O código é executado três consultas. A primeira consulta traz de volta todos os **pessoa** objetos. A segunda consulta usa a **OfType** método retorne **instrutor** objetos. A terceira consulta usa a **OfType** método retorne **aluno** objetos.

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
