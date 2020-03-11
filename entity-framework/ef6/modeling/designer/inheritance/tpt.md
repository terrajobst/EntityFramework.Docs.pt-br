---
title: Herança de TPT do designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418206"
---
# <a name="designer-tpt-inheritance"></a>Herança de TPT do designer
Este guia passo a passo mostra como implementar a herança de tabela por tipo (TPT) em seu modelo usando o Entity Framework Designer (Designer de EF). A herança de tabela por tipo usa uma tabela separada no banco de dados para manter o dado para propriedades não herdadas e propriedades de chave para cada tipo na hierarquia de herança.

Neste tutorial, mapearemos o **curso** (tipo base), **OnlineCourse** (deriva de curso) e **OnsiteCourse** (deriva de entidades do **curso**) para tabelas com os mesmos nomes. Vamos criar um modelo do banco de dados e, em seguida, alterar o modelo para implementar a herança TPT.

Você também pode começar com a Model First e, em seguida, gerar o banco de dados do modelo. O designer do EF usa a estratégia TPT por padrão e, portanto, qualquer herança no modelo será mapeada para tabelas separadas.

## <a name="other-inheritance-options"></a>Outras opções de herança

A tabela por hierarquia (TPH) é outro tipo de herança em que uma tabela de banco de dados é usada para manter todos os tipos de entidade em uma hierarquia de herança.  Para obter informações sobre como mapear a herança de tabela por hierarquia com o Entity Designer, consulte [herança do EF designer TPH](~/ef6/modeling/designer/inheritance/tph.md). 

Observe que a herança de tipo de tabela por concreto (TPC) e os modelos de herança misto têm suporte pelo tempo de execução Entity Framework, mas não têm suporte do designer do EF. Se você quiser usar o TPC ou a herança mista, terá duas opções: Use Code First ou edite manualmente o arquivo EDMX. Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.

## <a name="prerequisites"></a>Prerequisites

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo-&gt; projeto de novo&gt;**
-   No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console** .
-   Insira **TPTDBFirstSample** como o nome.
-   Selecione  **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   Clique com o botão direito do mouse no projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **TPTModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione ** gerar do banco de dados**e clique em **Avançar**.
-   Clique em **nova conexão**.
    Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
-   Na caixa de diálogo escolher seu objeto de banco de dados, no nó tabelas, selecione as tabelas **Departamento**, **curso, OnlineCourse e OnsiteCourse** .
-   Clique em **concluir**.

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa de diálogo escolher seus objetos de banco de dados são adicionados ao modelo.

## <a name="implement-table-per-type-inheritance"></a>Implementar a herança de tabela por tipo

-   Na superfície de design, clique com o botão direito do mouse no tipo de entidade **OnlineCourse** e selecione **Propriedades**.
-   Na janela **Propriedades** , defina a propriedade tipo base como **curso**.
-   Clique com o botão direito do mouse no tipo de entidade **OnsiteCourse** e selecione **Propriedades**.
-   Na janela **Propriedades** , defina a propriedade tipo base como **curso**.
-   Clique com o botão direito do mouse na associação (a linha) entre os tipos de entidade **OnlineCourse** e **curso** .
    Selecione **excluir do modelo**.
-   Clique com o botão direito do mouse na associação entre os tipos de entidade **OnsiteCourse** e **curso** .
    Selecione **excluir do modelo**.

Agora, excluiremos a propriedade **cursoid** de **OnlineCourse** e **OnsiteCourse** porque essas classes herdam o **cursoid** do tipo base do **curso** .

-   Clique com o botão direito do mouse na propriedade **cursoid** do tipo de entidade **OnlineCourse** e selecione **excluir do modelo**.
-   Clique com o botão direito do mouse na propriedade **cursoid** do tipo de entidade **OnsiteCourse** e selecione **excluir do modelo**
-   A herança de tabela por tipo agora está implementada.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Usar o modelo

Abra o arquivo **Program.cs** em que o método **Main** está definido. Cole o código a seguir na função **Main** . O código executa três consultas. A primeira consulta traz de volta todos os **cursos** relacionados ao departamento especificado. A segunda consulta usa o método **OfType** para retornar **OnlineCourses** relacionados ao departamento especificado. A terceira consulta retorna **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
