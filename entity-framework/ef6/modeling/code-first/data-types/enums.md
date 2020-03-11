---
title: Suporte a enum-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419104"
---
# <a name="enum-support---code-first"></a>Suporte a enum-Code First
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este vídeo e instruções passo a passo mostram como usar tipos de enumeração com Entity Framework Code First. Ele também demonstra como usar enums em uma consulta LINQ.

Este passo a passos usará Code First para criar um novo banco de dados, mas você também pode usar [Code First para mapear para um banco de dados existente](~/ef6/modeling/code-first/workflows/existing-database.md).

O suporte a enum foi introduzido no Entity Framework 5. Para usar os novos recursos como enums, tipos de dados espaciais e funções com valor de tabela, você deve direcionar .NET Framework 4,5. O Visual Studio 2012 visa o .NET 4,5 por padrão.

No Entity Framework, uma enumeração pode ter os seguintes tipos subjacentes: **byte**, **Int16**, **Int32**, **Int64** ou **SByte**.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como usar tipos de enumeração com Entity Framework Code First. Ele também demonstra como usar enums em uma consulta LINQ.

**Apresentado por**: Julia Kornich

**Vídeo**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.

 

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio 2012
2.  No menu **arquivo** , aponte para **novo**e clique em **projeto**
3.  No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**
4.  Insira **EnumCodeFirst** como o nome do projeto e clique em **OK**

## <a name="define-a-new-model-using-code-first"></a>Definir um novo modelo usando Code First

Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio). O código a seguir define a classe Department.

O código também define a enumeração Departmentnames. Por padrão, a enumeração é do tipo **int** . A propriedade Name na classe Department é do tipo Departmentnames.

Abra o arquivo Program.cs e cole as definições de classe a seguir.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Definir o tipo derivado de DbContext

Além de definir entidades, você precisa definir uma classe derivada de DbContext e expõe DbSet&lt;TEntity&gt; Propriedades. As propriedades DbSet&lt;TEntity&gt; permitem que o contexto saiba quais tipos você deseja incluir no modelo.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, o que inclui o preenchimento de objetos com dados de um Database, controle de alterações e persistência de dados no banco de dado.

Os tipos DbContext e DbSet são definidos no assembly do EntityFramework. Adicionaremos uma referência a essa DLL usando o pacote NuGet do EntityFramework.

1.  Em Gerenciador de Soluções, clique com o botão direito do mouse no nome do projeto.
2.  Selecione **gerenciar pacotes NuGet...**
3.  Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework** .
4.  Clique em **Instalar**

Observe que, além do assembly do EntityFramework, as referências a assemblies System. ComponentModel. Annotations e System. Data. Entity também são adicionadas.

Na parte superior do arquivo Program.cs, adicione a seguinte instrução Using:

``` csharp
using System.Data.Entity;
```

No Program.cs, adicione a definição de contexto. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main está definido. Adicione o código a seguir à função main. O código adiciona um novo objeto Department ao contexto. Em seguida, ele salva os dados. O código também executa uma consulta LINQ que retorna um departamento onde o nome é Departmentnames. inglês.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compile e execute o aplicativo. O programa produz a seguinte saída:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Exibir o banco de dados gerado

Quando você executa o aplicativo pela primeira vez, o Entity Framework cria um banco de dados para você. Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância de LocalDB. Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (para este exemplo, **EnumCodeFirst. EnumTestContext**). As próximas vezes que o banco de dados existente será usado.  

Observe que, se você fizer alterações em seu modelo depois que o banco de dados tiver sido criado, deverá usar Migrações do Code First para atualizar o esquema de banco de dados. Consulte [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar migrações.

Para exibir o banco de dados e os dados, faça o seguinte:

1.  No menu principal do Visual Studio 2012, selecione **exibir** -&gt; **pesquisador de objetos do SQL Server**.
2.  Se o LocalDB não estiver na lista de servidores, clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar à instância do LocalDB
3.  Expandir o nó LocalDB
4.  Desdobrar a pasta **databases** para ver o novo banco de dados e navegar até a tabela **Department** Observe que Code First não cria uma tabela que é mapeada para o tipo de enumeração
5.  Para exibir dados, clique com o botão direito do mouse na tabela e selecione **exibir dados**

## <a name="summary"></a>Resumo

Neste tutorial, vimos como usar tipos enum com Entity Framework Code First. 
