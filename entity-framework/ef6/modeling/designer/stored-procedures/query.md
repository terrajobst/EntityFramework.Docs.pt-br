---
title: Procedimentos armazenados de consulta do designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182479"
---
# <a name="designer-query-stored-procedures"></a>Procedimentos armazenados de consulta do designer
Esta explicação passo a passo mostra como usar o Entity Framework Designer (EF designer) para importar procedimentos armazenados em um modelo e, em seguida, chamar os procedimentos armazenados importados para recuperar resultados. 

Observe que Code First não dá suporte ao mapeamento para procedimentos armazenados ou funções. No entanto, você pode chamar procedimentos armazenados ou funções usando o método System. Data. Entity. DbSet. SQLQuery. Por exemplo:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo-&gt; Projeto New-&gt;**
-   No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console** .
-   Insira **EFwithSProcsSample** As o nome.
-   Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.
-   Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
-   Digite **EFwithSProcsModel. edmx** para o nome do arquivo e clique em **Adicionar**.
-   Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.
-   Clique em **nova conexão**.  
    Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(LocalDB) \\mssqllocaldb**), selecione o método de autenticação, digite **School** for o nome do banco de dados e clique em **OK**.  
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
-   Na caixa de diálogo escolher seu objeto de banco de dados, verifique as **tabelas** checkbox para selecionar todas as tabelas.  
    Além disso, selecione os seguintes procedimentos armazenados no nó **procedimentos armazenados e funções** : **GetStudentGrades** e **getdepartmentname**. 

    ![Importar](~/ef6/media/import.jpg)

    *Starting com o Visual Studio 2012, o designer do EF dá suporte à importação em massa de procedimentos armazenados. A **importação de procedimentos armazenados e funções selecionados para o modelo de entidade** é marcada por padrão.*
-   Clique em **concluir**.

Por padrão, a forma de resultado de cada procedimento armazenado importado ou função que retorna mais de uma coluna se tornará automaticamente um novo tipo complexo. Neste exemplo, queremos mapear os resultados da função **GetStudentGrades** para a entidade **StudentGrade** e os resultados de **getdepartmentname** para **None** (**None** é o valor padrão).

Para uma importação de função retornar um tipo de entidade, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder exatamente às propriedades escalares do tipo de entidade retornado. Uma importação de função também pode retornar coleções de tipos simples, tipos complexos ou nenhum valor.

-   Clique com o botão direito do mouse na superfície de design e selecione **navegador de modelos**.
-   No **navegador de modelos**, selecione **importações de função**e clique duas vezes na função **GetStudentGrades** .
-   Na caixa de diálogo Editar importação de função, selecione **entidades** And escolha **StudentGrade**.  
    @no__t-a caixa de seleção **importação de função 0The é combinável** na parte superior da caixa de diálogo **importações de função** permitirá que você mapeie para funções combináveis. Se você marcar essa caixa, somente as funções combináveis (funções com valor de tabela) aparecerão na lista suspensa **procedimento armazenado/nome da função** . Se você não marcar essa caixa, somente as funções não combináveis serão mostradas na lista. *

## <a name="use-the-model"></a>Usar o modelo

Abra o arquivo **Program.cs** em que o método **Main** está definido. Adicione o código a seguir à função main.

O código chama dois procedimentos armazenados: **GetStudentGrades** (retorna **StudentGrades** para a *StudentId*especificada) e **getdepartmentname** (retorna o nome do departamento no parâmetro de saída).  

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

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Parâmetros de saída
-----------------

Se os parâmetros de saída forem usados, seus valores não estarão disponíveis até que os resultados tenham sido completamente lidos. Isso ocorre devido ao comportamento subjacente de DbDataReader, consulte [recuperando dados usando um DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.
