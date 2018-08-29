---
title: Funções com valor de tabela (TVFs) - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993241"
---
# <a name="table-valued-functions-tvfs"></a>Funções com valor de tabela (TVFs)
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O passo a passo e vídeo passo a passo mostra como mapear funções avaliadas por tabela (TVFs) usando o Entity Framework Designer. Ele também demonstra como chamar uma TVF de uma consulta LINQ.

TVFs estão atualmente só tem suporte no banco de dados primeiro fluxo de trabalho.

Suporte TVF foi introduzido no Entity Framework versão 5. Observe que, para usar os novos recursos, como funções com valor de tabela, enumerações e tipos espaciais, você deve ter como destino .NET Framework 4.5. Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.

TVFs são muito semelhantes aos procedimentos armazenados com uma diferença importante: o resultado de uma TVF é passível de composição. Isso significa que os resultados de uma TVF podem ser usados em uma consulta LINQ, enquanto os resultados de um procedimento armazenado não é possível.

## <a name="watch-the-video"></a>Assista ao vídeo

**Apresentado por**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Para concluir este passo a passo, você precisa:

- Instalar o [banco de dados School](~/ef6/resources/school-database.md).

- Ter uma versão recente do Visual Studio

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio
2.  Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**
3.  No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo
4.  Insira **TVF** como o nome do projeto e clique em **Okey**

## <a name="add-a-tvf-to-the-database"></a>Adicionar uma TVF ao banco de dados

-   Selecione **exibição -&gt; Pesquisador de objetos do SQL Server**
-   Se o LocalDB não está na lista de servidores: clique duas vezes em **do SQL Server** e selecione **adicionar SQL Server** Use o padrão **autenticação do Windows** para se conectar ao servidor do LocalDB
-   Expanda o nó do LocalDB
-   Sob o nó bancos de dados, clique com botão direito no nó do banco de dados de escola e selecione **nova consulta...**
-   No Editor T-SQL, cole a seguinte definição de TVF

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   Clique no botão direito do mouse no editor T-SQL e selecione **Execute**
-   A função GetStudentGradesForCourse é adicionada ao banco de dados School

 

## <a name="create-a-model"></a>Criar um modelo

1.  Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**
2.  Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no **modelos** painel
3.  Insira **TVFModel.edmx** para o nome de arquivo e clique **adicionar**
4.  Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **Avançar**
5.  Clique em **nova Conexão** Enter **(localdb)\\mssqllocaldb** no texto de nome de servidor caixa Enter **School** para o banco de dados nome clique **Okey**
6.  Na escolha seu objetos de banco de dados caixa de diálogo do **tabelas** nó, selecione o **pessoa**, **StudentGrade**, e **curso** tabelas
7.  Selecione o **GetStudentGradesForCourse** função localizado sob a **procedimentos armazenados e funções** nó nota, começando com o Visual Studio 2012, o Entity Designer permite a importação de lote seus procedimentos armazenados e funções
8.  Clique em **concluir**
9.  O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na **Choose Your Database Objects** caixa de diálogo são adicionados ao modelo.
10. Por padrão, a forma de resultado de cada importados de procedimento armazenado ou função tornará automaticamente um novo tipo complexo em seu modelo de entidade. Mas queremos mapear os resultados da função GetStudentGradesForCourse à entidade StudentGrade: a superfície de design e selecione com o botão direito **Model Browser** no Pesquisador de modelos, selecione **Function Imports**e, em seguida, clique duas vezes o **GetStudentGradesForCourse** caixa de diálogo no the Editar função importar função, selecione **entidades** e escolha **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo onde o método Main é definido. Adicione o seguinte código na função principal.

O código a seguir demonstra como criar uma consulta que usa uma função com valor de tabela. A consulta projeta os resultados em um tipo anônimo que contém o título do curso relacionado e os alunos relacionados com um nível maior ou igual a 3.5.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Compile e execute o aplicativo. O programa produz a seguinte saída:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Resumo

Neste passo a passo analisamos como mapear funções avaliadas por tabela (TVFs) usando o Entity Framework Designer. Ele também demonstrou como chamar uma TVF de uma consulta LINQ.
