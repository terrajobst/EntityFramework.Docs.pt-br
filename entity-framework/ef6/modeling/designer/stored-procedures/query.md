---
title: Designer de consulta armazenados procedimentos - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 29b7745c2229ce4a38ad81e11406474424adfa24
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994966"
---
# <a name="designer-query-stored-procedures"></a>Designer de consulta procedimentos armazenados
Este passo a passo mostram como usar o Entity Framework Designer (Designer de EF) para importar os procedimentos armazenados em um modelo e, em seguida, chamar procedimentos armazenados para recuperar resultados importados. 

Observe que o Code First não suporta o mapeamento para procedimentos armazenados ou funções. No entanto, você pode chamar procedimentos armazenados ou funções por meio do método System.Data.Entity.DbSet.SqlQuery. Por exemplo:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo -&gt; New -&gt; projeto**
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.
-   Insira **EFwithSProcsSample** como o nome.
-   Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   Clique com botão direito no projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **EFwithSProcsModel.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.
-   Clique em **nova Conexão**.  
    Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.  
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na caixa de diálogo Choose Your Database Objects, verifique as **tabelas** caixa de seleção para selecionar todas as tabelas.  
    Além disso, selecione os seguintes procedimentos armazenados sob a **procedimentos armazenados e funções** nó: **GetStudentGrades** e **GetDepartmentName**. 

    ![Importar](~/ef6/media/import.jpg)

    *Começando com o Visual Studio 2012, o EF Designer oferece suporte a importação em massa de procedimentos armazenados. O **importação selecionado procedimentos armazenados e funções no modelo theentity** é marcada por padrão.*
-   Clique em **Finalizar**.

Por padrão, a forma de resultado de cada importados de procedimento armazenado ou função que retorna mais de uma coluna tornará automaticamente um novo tipo complexo. Neste exemplo desejamos mapear os resultados do **GetStudentGrades** funcionar para o **StudentGrade** entidade e os resultados do **GetDepartmentName** para **none** (**none** é o valor padrão).

Para uma importação de função retornar um tipo de entidade, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder exatamente as propriedades escalares do tipo de entidade retornada. Uma importação de função também pode retornar coleções de tipos simples, tipos complexos ou nenhum valor.

-   Clique com botão direito a superfície de design e selecione **Model Browser**.
-   Na **Model Browser**, selecione **Function Imports**e, em seguida, clique duas vezes o **GetStudentGrades** função.
-   Na caixa de diálogo Editar importação de função, selecione **entidades** e escolha **StudentGrade**.  
    *O **importação de função é passível de composição** na parte superior da caixa de seleção a **Function Imports** diálogo permitirá que você mapear para funções combináveis. Se você marcar esta caixa, apenas funções combináveis (funções com valor de tabela) aparecerá na **procedimento armazenado / nome da função** lista suspensa. Se você não marcar essa caixa, apenas as funções não combináveis serão mostradas na lista.*

## <a name="use-the-model"></a>Usar o modelo

Abra o **Program.cs** do arquivo onde o **Main** método é definido. Adicione o seguinte código na função principal.

O código chama dois procedimentos armazenados: **GetStudentGrades** (retorna **StudentGrades** especificado *StudentId*) e **GetDepartmentName** (retorna o nome do departamento no parâmetro de saída).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Compile e execute o aplicativo. O programa produz a seguinte saída:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parâmetros de saída
-----------------

Se forem usados parâmetros de saída, seus valores não estará disponíveis até que os resultados foram lidas completamente. Isso é devido ao comportamento subjacente do DbDataReader, consulte [recuperando dados usando um DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.
