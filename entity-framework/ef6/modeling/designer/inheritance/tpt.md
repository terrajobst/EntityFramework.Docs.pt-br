---
title: Herança TPT Designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489447"
---
# <a name="designer-tpt-inheritance"></a>Herança TPT Designer
Este passo a passo mostra como implementar a herança do tabela por tipo (TPT) em seu modelo usando o Entity Framework Designer (Designer de EF). Herança de tabela por tipo usa uma tabela separada no banco de dados para manter dados de propriedades não herdadas e as propriedades de chave para cada tipo na hierarquia de herança.

Este passo a passo, podemos mapeará os **curso** (tipo de base), **OnlineCourse** (deriva de curso), e **OnsiteCourse** (deriva **curso**) entidades para tabelas com os mesmos nomes. Vamos criar um modelo do banco de dados e, em seguida, alterar o modelo para implementar a herança TPT.

Você pode também iniciar com o modelo primeiro e, em seguida, gerar o banco de dados do modelo. O EF Designer usa a estratégia TPT por padrão e então qualquer herança no modelo será mapeada para tabelas separadas.

## <a name="other-inheritance-options"></a>Outras opções de herança

Tabela por hierarquia (TPH) é outro tipo de herança em qual banco de dados de uma tabela é usada para manter os dados para todos os tipos de entidade em uma hierarquia de herança.  Para obter informações sobre como mapear a herança de tabela por hierarquia com o Entity Designer, consulte [EF Designer TPH herança](~/ef6/modeling/designer/inheritance/tph.md). 

Observe que, o tabela por concreto tipo de herança TPC () e herança mista modelos são compatíveis com o tempo de execução do Entity Framework, mas não são compatíveis com o EF Designer. Se você quiser usar TPC ou herança mista, você tem duas opções: usar o Code First ou editar manualmente o arquivo EDMX. Se você optar por trabalhar com o arquivo EDMX, a janela de detalhes de mapeamento será colocada em "modo de segurança" e você não poderá usar o designer para alterar os mapeamentos.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo -&gt; New -&gt; projeto**
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.
-   Insira **TPTDBFirstSample** como o nome.
-   Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   Clique com botão direito no projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **TPTModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo caixa, selecione * * Gerar do banco de dados * * e, em seguida, clique em **próxima**.
-   Clique em **nova Conexão**.
    Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na caixa de diálogo Choose Your Database Objects, sob o nó de tabelas, selecione a **departamento**, **curso, OnlineCourse e OnsiteCourse** tabelas.
-   Clique em **Finalizar**.

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa de diálogo Choose Your Database Objects são adicionados ao modelo.

## <a name="implement-table-per-type-inheritance"></a>Implementar a herança de tabela por tipo

-   Na superfície de design, clique com botão direito do **OnlineCourse** tipo de entidade e selecione **propriedades**.
-   No **propriedades** janela, defina a propriedade de tipo de Base para **curso**.
-   Clique com botão direito do **OnsiteCourse** tipo de entidade e selecione **propriedades**.
-   No **propriedades** janela, defina a propriedade de tipo de Base para **curso**.
-   Clique com botão direito a associação (a linha) entre o **OnlineCourse** e **curso** tipos de entidade.
    Selecione **excluir do modelo**.
-   Clique com botão direito a associação entre o **OnsiteCourse** e **curso** tipos de entidade.
    Selecione **excluir do modelo**.

Agora vamos excluir o **CourseID** propriedade de **OnlineCourse** e **OnsiteCourse** porque herdam essas classes **CourseID** de o **curso** tipo base.

-   Com o botão direito do **CourseID** propriedade da **OnlineCourse** tipo de entidade e, em seguida, selecione **excluir do modelo**.
-   Com o botão direito do **CourseID** propriedade da **OnsiteCourse** tipo de entidade e, em seguida, selecione **excluir do modelo**
-   Herança de tabela por tipo agora é implementada.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Usar o modelo

Abra o **Program.cs** do arquivo onde o **Main** método é definido. Cole o seguinte código para o **Main** função. O código é executado três consultas. A primeira consulta traz de volta todos os **cursos** relacionadas ao departamento especificado. A segunda consulta usa a **OfType** método retorne **OnlineCourses** relacionadas ao departamento especificado. A terceira consulta retorna **OnsiteCourses**.

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
