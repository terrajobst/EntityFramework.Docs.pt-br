---
title: Spatial-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419097"
---
# <a name="spatial---code-first"></a>Code First espacial
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O vídeo e instruções passo a passo mostram como mapear tipos espaciais com Entity Framework Code First. Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.

Este passo a passos usará Code First para criar um novo banco de dados, mas você também pode usar [Code First para um banco de dados existente](~/ef6/modeling/code-first/workflows/existing-database.md).

O suporte ao tipo espacial foi introduzido no Entity Framework 5. Observe que para usar os novos recursos, como tipo espacial, enums e funções com valor de tabela, você deve direcionar .NET Framework 4,5. O Visual Studio 2012 visa o .NET 4,5 por padrão.

Para usar os tipos de dados espaciais, você também deve usar um provedor de Entity Framework que tenha suporte espacial. Consulte [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.

Há dois tipos de dados espaciais principais: Geografia e Geometry. O tipo de dados geography armazena dados dados elipsoidais (por exemplo, coordenadas de latitude e longitude de GPS). O tipo de dados geometry representa o sistema de coordenadas euclidiana (simples).

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como mapear tipos espaciais com Entity Framework Code First. Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.

**Apresentado por**: Julia Kornich

**Vídeo**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio 2012
2.  No menu **arquivo** , aponte para **novo**e clique em **projeto**
3.  No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**
4.  Insira **SpatialCodeFirst** como o nome do projeto e clique em **OK**

## <a name="define-a-new-model-using-code-first"></a>Definir um novo modelo usando Code First

Ao usar o desenvolvimento de Code First, você geralmente começa escrevendo .NET Framework classes que definem seu modelo conceitual (de domínio). O código a seguir define a classe University.

A Universidade tem a propriedade Location do tipo DbGeography. Para usar o tipo DbGeography, você deve adicionar uma referência ao assembly System. Data. Entity e também adicionar a instrução System. Data. espacial using.

Abra o arquivo Program.cs e cole o seguinte usando as instruções na parte superior do arquivo:

``` csharp
using System.Data.Spatial;
```

Adicione a seguinte definição de classe de Universidade ao arquivo Program.cs.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
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

Observe que, além do assembly do EntityFramework, uma referência ao assembly System. ComponentModel. Annotations também é adicionada.

Na parte superior do arquivo Program.cs, adicione a seguinte instrução Using:

``` csharp
using System.Data.Entity;
```

No Program.cs, adicione a definição de contexto. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main está definido. Adicione o código a seguir à função main.

O código adiciona dois novos objetos University ao contexto. As propriedades espaciais são inicializadas usando o método DbGeography. FromText. O ponto de Geografia representado como WellKnownText é passado para o método. Em seguida, o código salva os dados. Em seguida, a consulta LINQ que retorna um objeto de universidade onde seu local está mais próximo do local especificado é construída e executada.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compile e execute o aplicativo. O programa produz a seguinte saída:

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Exibir o banco de dados gerado

Quando você executa o aplicativo pela primeira vez, o Entity Framework cria um banco de dados para você. Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância de LocalDB. Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (neste exemplo, é **SpatialCodeFirst. UniversityContext**). As próximas vezes que o banco de dados existente será usado.  

Observe que, se você fizer alterações em seu modelo depois que o banco de dados tiver sido criado, deverá usar Migrações do Code First para atualizar o esquema de banco de dados. Consulte [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar migrações.

Para exibir o banco de dados e os dados, faça o seguinte:

1.  No menu principal do Visual Studio 2012, selecione **exibir** -&gt; **pesquisador de objetos do SQL Server**.
2.  Se o LocalDB não estiver na lista de servidores, clique com o botão direito do mouse em **SQL Server** e selecione **Adicionar SQL Server** usar a autenticação padrão do **Windows** para se conectar à instância de LocalDB
3.  Expandir o nó LocalDB
4.  Desdobrar a pasta **databases** para ver o novo banco de dados e navegar até a tabela **universidades**
5.  Para exibir dados, clique com o botão direito do mouse na tabela e selecione **exibir dados**

## <a name="summary"></a>Resumo

Neste tutorial, vimos como usar tipos espaciais com Entity Framework Code First. 
