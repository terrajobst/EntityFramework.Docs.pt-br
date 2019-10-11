---
title: Funções com valor de tabela (TVFs) – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182530"
---
# <a name="table-valued-functions-tvfs"></a>Funções com valor de tabela (TVFs)
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O vídeo e instruções passo a passo mostram como mapear TVFs (funções com valor de tabela) usando o Entity Framework Designer. Ele também demonstra como chamar um TVF de uma consulta LINQ.

Atualmente, os TVFs só têm suporte no fluxo de trabalho Database First.

O suporte a TVF foi introduzido na versão 5 do Entity Framework. Observe que para usar os novos recursos, como funções com valor de tabela, enums e tipos espaciais, você deve direcionar .NET Framework 4,5. O Visual Studio 2012 visa o .NET 4,5 por padrão.

TVFs são muito semelhantes aos procedimentos armazenados com uma diferença importante: o resultado de um TVF é combinável. Isso significa que os resultados de um TVF podem ser usados em uma consulta LINQ, enquanto os resultados de um procedimento armazenado não podem.

## <a name="watch-the-video"></a>Assista ao vídeo

**Apresentado por**: Julia Kornich

[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Para concluir este passo a passos, você precisa:

- Instale o [banco de dados escolar](~/ef6/resources/school-database.md).

- Ter uma versão recente do Visual Studio

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio
2.  No menu **arquivo** , aponte para **novo**e clique em **projeto**
3.  No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console**
4.  Insira **TVF** como o nome do projeto e clique em **OK**

## <a name="add-a-tvf-to-the-database"></a>Adicionar um TVF ao banco de dados

-   Selecione **Exibir-&gt; pesquisador de objetos do SQL Server**
-   Se o LocalDB não estiver na lista de servidores: Clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar ao servidor LocalDB
-   Expandir o nó LocalDB
-   No nó bancos de dados, clique com o botão direito do mouse no nó escolar e selecione **nova consulta...**
-   No Editor T-SQL, Cole a seguinte definição de TVF

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

-   Clique com o botão direito do mouse no Editor T-SQL e selecione **executar**
-   A função GetStudentGradesForCourse é adicionada ao banco de dados School

 

## <a name="create-a-model"></a>Criar um modelo

1.  Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**
2.  Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel **modelos**
3.  Digite **TVFModel. edmx** para o nome do arquivo e clique em **Adicionar**
4.  Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**
5.  Clique em **nova conexão** inserir **(LocalDB) \\mssqllocaldb** na caixa de texto nome do servidor, digite **School** for o nome do banco de dados clique em **OK**
6.  Na caixa de diálogo escolher seu objeto de banco de dados, nas **tabelas** node, selecione **pessoa**, **StudentGrade**e **curso** tables
7.  Selecione a função **GetStudentGradesForCourse** localizada em **procedimentos armazenados e funções** node Observe que, começando com o Visual Studio 2012, o Entity designer permite que você importe em lote seus procedimentos armazenados e funções
8.  Clique em **concluir**
9.  O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido. Todos os objetos que você selecionou na caixa **escolher seus objetos de banco de dados** dialog são adicionados ao modelo.
10. Por padrão, a forma de resultado de cada procedimento armazenado ou função importada se tornará automaticamente um novo tipo complexo em seu modelo de entidade. Mas queremos mapear os resultados da função GetStudentGradesForCourse para a entidade StudentGrade: Clique com o botão direito do mouse na superfície de design e selecione **navegador de modelos** no navegador de modelos, selecione **importações de função**e clique duas vezes na função **GetStudentGradesForCourse** na caixa de diálogo Editar importação de função, selecione **entidades**  and escolher **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo em que o método Main está definido. Adicione o código a seguir à função main.

O código a seguir demonstra como criar uma consulta que usa uma função com valor de tabela. A consulta projeta os resultados em um tipo anônimo que contém o título do curso relacionado e os alunos relacionados com uma taxa maior ou igual a 3,5.

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

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Resumo

Neste tutorial, vimos como mapear TVFs (funções com valor de tabela) usando o Entity Framework Designer. Ele também demonstrou como chamar um TVF de uma consulta LINQ.
