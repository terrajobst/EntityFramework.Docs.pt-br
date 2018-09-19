---
title: Suporte a enum - Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283714"
---
# <a name="enum-support---code-first"></a>Suporte a enum - Code First
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este passo a passo e vídeo passo a passo mostra como usar tipos de enumeração com o Entity Framework Code First. Ele também demonstra como usar enums em uma consulta LINQ.

Este passo a passo usar Code First para criar um novo banco de dados, mas você também pode usar [Code First para mapear para um banco de dados](~/ef6/modeling/code-first/workflows/existing-database.md).

Suporte a enum foi introduzido no Entity Framework 5. Para usar os novos recursos, como enums, tipos de dados espaciais e funções com valor de tabela, você deve ter como destino .NET Framework 4.5. Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.

No Entity Framework, uma enumeração pode ter os seguintes tipos de base: **bytes**, **Int16**, **Int32**, **Int64** , ou **SByte**.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como usar tipos de enumeração com o Entity Framework Code First. Ele também demonstra como usar enums em uma consulta LINQ.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.

 

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abra o Visual Studio 2012
2.  Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**
3.  No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo
4.  Insira **EnumCodeFirst** como o nome do projeto e clique em **Okey**

## <a name="define-a-new-model-using-code-first"></a>Definir um novo modelo usando o Code First

Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio). O código a seguir define a classe de departamento.

O código também define a enumeração DepartmentNames. Por padrão, a enumeração é de **int** tipo. A propriedade de nome na classe departamento é do tipo DepartmentNames.

Abra o arquivo Program.cs e cole as seguintes definições de classe.

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
 

## <a name="define-the-dbcontext-derived-type"></a>Definir tipo derivado de DbContext

Além de definir entidades, você precisa definir uma classe que deriva de DbContext e expõe um DbSet&lt;TEntity&gt; propriedades. O DbSet&lt;TEntity&gt; propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.

Os tipos DbContext e DbSet são definidos no assembly EntityFramework. Adicionaremos uma referência a essa DLL usando o pacote EntityFramework NuGet.

1.  No Gerenciador de soluções, clique com botão direito no nome do projeto.
2.  Selecione **gerenciar pacotes NuGet...**
3.  Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote.
4.  Clique em **instalar**

Observe que, além do assembly EntityFramework, as referências aos assemblies System e de DataAnnotations são adicionadas também.

Na parte superior do arquivo Program.cs, adicione a seguinte instrução using:

``` csharp
using System.Data.Entity;
```

De Program.cs, adicione a definição de contexto. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main é definido. Adicione o seguinte código na função principal. O código adiciona um novo objeto de departamento para o contexto. Em seguida, ele salva os dados. O código também executa uma consulta LINQ que retorna um departamento em que o nome é DepartmentNames.English.

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

Quando você executa o aplicativo na primeira vez, o Entity Framework cria um banco de dados para você. Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância do LocalDB. Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (neste exemplo que é **EnumCodeFirst.EnumTestContext**). As próximas vezes que o banco de dados existente será usado.  

Observe que, se você fizer alterações ao seu modelo depois que o banco de dados for criado, você deve usar migrações do Code First para atualizar o esquema de banco de dados. Ver [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar as migrações.

Para exibir o banco de dados e os dados, faça o seguinte:

1.  No menu principal do Visual Studio 2012, selecione **modo de exibição**  - &gt; **Pesquisador de objetos do SQL Server**.
2.  Se LocalDB não estiver na lista de servidores, clique o botão direito do mouse em **do SQL Server** e selecione **adicionar SQL Server** Use o padrão **autenticação do Windows** para conectar-se para o Instância de LocalDB
3.  Expanda o nó do LocalDB
4.  Desdobrar o **bancos de dados** pasta para ver o novo banco de dados e navegue até a **departamento** Observação, o que o Code First não cria uma tabela que mapeia para o tipo de enumeração de tabela
5.  Para exibir os dados, clique com botão direito na tabela e selecione **exibir dados**

## <a name="summary"></a>Resumo

Neste passo a passo analisamos como usar tipos de enumeração com o Entity Framework Code First. 
