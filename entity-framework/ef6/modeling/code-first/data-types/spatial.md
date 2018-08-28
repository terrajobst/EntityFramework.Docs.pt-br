---
title: Espaciais - Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: d15b407203a7dd7ddf92d0759af00f3ad5e41f57
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995206"
---
# <a name="spatial---code-first"></a>Espaciais - Code First
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O passo a passo e vídeo passo a passo mostra como mapear tipos espaciais com o Entity Framework Code First. Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.

Este passo a passo usar Code First para criar um novo banco de dados, mas você também pode usar [Code First para um banco de dados](~/ef6/modeling/code-first/workflows/existing-database.md).

Suporte a tipo espacial foi introduzido no Entity Framework 5. Observe que, para usar os novos recursos, como o tipo espacial, enumerações e funções com valor de tabela, você deve direcionar o .NET Framework 4.5. Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.

Para usar tipos de dados espaciais, você também deve usar um provedor do Entity Framework que tenha suporte espacial. Ver [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.

Há dois tipos de dados espaciais principal: geography e geometry. O tipo de dados geography armazena dados elipsoidais (por exemplo, GPS latitude e longitude coordenadas). O tipo de dados geometry representa o sistema de coordenadas Euclidiano (plano).

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como mapear tipos espaciais com o Entity Framework Code First. Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abra o Visual Studio 2012
2.  Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**
3.  No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo
4.  Insira **SpatialCodeFirst** como o nome do projeto e clique em **Okey**

## <a name="define-a-new-model-using-code-first"></a>Definir um novo modelo usando o Code First

Ao usar o desenvolvimento Code First que geralmente começar pela criação de classes do .NET Framework que definem o modelo conceitual (domínio). O código a seguir define a classe de Universidade.

A Universidade tiver a propriedade Location do tipo DbGeography. Para usar o tipo de DbGeography, você deve adicionar uma referência ao assembly System e também adicionar o Spatial usando a instrução.

Abra o arquivo Program.cs e cole o seguinte usando instruções na parte superior do arquivo:

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

## <a name="define-the-dbcontext-derived-type"></a>Definir tipo derivado de DbContext

Além de definir entidades, você precisa definir uma classe que deriva de DbContext e expõe um DbSet&lt;TEntity&gt; propriedades. O DbSet&lt;TEntity&gt; propriedades permitem que o contexto de saber quais tipos que você deseja incluir no modelo.

Uma instância do tipo derivado de DbContext gerencia os objetos de entidade durante o tempo de execução, que inclui a população de objetos com os dados de um banco de dados, alterar os dados de rastreamento e persistência para o banco de dados.

Os tipos DbContext e DbSet são definidos no assembly EntityFramework. Adicionaremos uma referência a essa DLL usando o pacote EntityFramework NuGet.

1.  No Gerenciador de soluções, clique com botão direito no nome do projeto.
2.  Selecione **gerenciar pacotes NuGet...**
3.  Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote.
4.  Clique em **instalar**

Observe que o assembly EntityFramework, além de uma referência ao assembly DataAnnotations também é adicionada.

Na parte superior do arquivo Program.cs, adicione a seguinte instrução using:

``` csharp
using System.Data.Entity;
```

De Program.cs, adicione a definição de contexto. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main é definido. Adicione o seguinte código na função principal.

O código adiciona dois novos objetos University para o contexto. Propriedades espaciais são inicializadas por meio do método DbGeography.FromText. O ponto de Geografia representado como WellKnownText é passado para o método. O código, em seguida, salva os dados. Em seguida, a consulta LINQ que que retorna um objeto University onde é mais próxima do local especificado, a sua localização é construído e executado.

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

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Exibir o banco de dados gerado

Quando você executa o aplicativo na primeira vez, o Entity Framework cria um banco de dados para você. Como temos o Visual Studio 2012 instalado, o banco de dados será criado na instância do LocalDB. Por padrão, o Entity Framework nomeia o banco de dados após o nome totalmente qualificado do contexto derivado (neste exemplo que é **SpatialCodeFirst.UniversityContext**). As próximas vezes que o banco de dados existente será usado.  

Observe que, se você fizer alterações ao seu modelo depois que o banco de dados for criado, você deve usar migrações do Code First para atualizar o esquema de banco de dados. Ver [Code First para um novo banco de dados](~/ef6/modeling/code-first/workflows/new-database.md) para obter um exemplo de como usar as migrações.

Para exibir o banco de dados e os dados, faça o seguinte:

1.  No menu principal do Visual Studio 2012, selecione **modo de exibição**  - &gt; **Pesquisador de objetos do SQL Server**.
2.  Se LocalDB não estiver na lista de servidores, clique o botão direito do mouse em **do SQL Server** e selecione **adicionar SQL Server** Use o padrão **autenticação do Windows** para conectar-se para o a instância de LocalDB
3.  Expanda o nó do LocalDB
4.  Desdobrar o **bancos de dados** pasta para ver o novo banco de dados e navegue até a **universidades** tabela
5.  Para exibir os dados, clique com botão direito na tabela e selecione **exibir dados**

## <a name="summary"></a>Resumo

Neste passo a passo analisamos como usar tipos espaciais com o Entity Framework Code First. 
