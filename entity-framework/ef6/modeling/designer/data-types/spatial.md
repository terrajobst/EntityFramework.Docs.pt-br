---
title: Spatial-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182510"
---
# <a name="spatial---ef-designer"></a>Espacial-designer de EF
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O vídeo e instruções passo a passo mostram como mapear tipos espaciais com o Entity Framework Designer. Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.

Este passo a passos usará Model First para criar um novo banco de dados, mas o designer do EF também pode ser usado com o fluxo de trabalho de [Database First](~/ef6/modeling/designer/workflows/database-first.md) para mapear para um banco de dados existente.

O suporte ao tipo espacial foi introduzido no Entity Framework 5. Observe que para usar os novos recursos, como tipo espacial, enums e funções com valor de tabela, você deve direcionar .NET Framework 4,5. O Visual Studio 2012 visa o .NET 4,5 por padrão.

Para usar os tipos de dados espaciais, você também deve usar um provedor de Entity Framework que tenha suporte espacial. Consulte [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.

Há dois tipos de dados espaciais principais: Geografia e Geometry. O tipo de dados geography armazena dados dados elipsoidais (por exemplo, coordenadas de latitude e longitude de GPS). O tipo de dados geometry representa o sistema de coordenadas euclidiana (simples).

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como mapear tipos espaciais com o Entity Framework Designer. Ele também demonstra como usar uma consulta LINQ para encontrar uma distância entre dois locais.

**Apresentado por**: Julia Kornich

**Vídeo**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (zip)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio 2012
2.  No menu **arquivo** , aponte para **novo**e clique em **projeto**
3.  No painel esquerdo, clique em **Visual C\#** e, em seguida, selecione o modelo de **console**
4.  Insira **SpatialEFDesigner** como o nome do projeto e clique em **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Criar um novo modelo usando o designer do EF

1.  Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**
2.  Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos
3.  Digite **UniversityModel. edmx** para o nome do arquivo e clique em **Adicionar**
4.  Na página Assistente de Modelo de Dados de Entidade, selecione **modelo vazio** na caixa de diálogo escolher conteúdo do modelo
5.  Clique em **concluir**

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.

O assistente executa as seguintes ações:

-   Gera o arquivo EnumTestModel. edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles. Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no assembly de saída para que os arquivos de metadados gerados sejam inseridos no assembly.
-   Adiciona uma referência aos seguintes assemblies: EntityFramework, System. ComponentModel. Annotations e System. Data. Entity.
-   Cria arquivos UniversityModel.tt e UniversityModel.Context.tt e os adiciona no arquivo. edmx. Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e os tipos POCO que são mapeados para as entidades no modelo. edmx

## <a name="add-a-new-entity-type"></a>Adicionar um novo tipo de entidade

1.  Clique com o botão direito do mouse em uma área vazia da superfície de design, selecione **entidade Add-&gt;** , a caixa de diálogo nova entidade será exibida
2.  Especifique **University** para o nome do tipo e especifique **universityid** para o nome da propriedade de chave, deixe o tipo como **Int32**
3.  Clique em **OK**
4.  Clique com o botão direito do mouse na entidade e selecione **Adicionar novo-&gt; Propriedade escalar**
5.  Renomear a nova propriedade como **nome**
6.  Adicione outra propriedade escalar e renomeie-a para o **local** abra o janela Propriedades e altere o tipo da nova propriedade para **geografia**
7.  Salvar o modelo e compilar o projeto
    > [!NOTE]
    > Quando você cria, avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros. Você pode ignorar esses avisos porque, depois de escolhermos para gerar o banco de dados do modelo, os erros serão descontinuados.

## <a name="generate-database-from-model"></a>Gerar banco de dados do modelo

Agora, podemos gerar um banco de dados baseado no modelo.

1.  Clique com o botão direito do mouse em um espaço vazio na superfície de Entity Designer e selecione **gerar banco de dados do modelo**
2.  A caixa de diálogo escolher sua conexão de dados do assistente para gerar banco de dados é exibida clique no botão **nova conexão** especificar **(LocalDB)\\mssqllocaldb** para o nome do servidor e a **Universidade** para o banco de dados e clique em **OK**
3.  Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.
4.  Clique em **Avançar** e o assistente para criar banco de dados gera DDL (linguagem de definição de dado) para criar um banco de dados o DDL gerado é exibido na caixa de diálogo Resumo e configurações Observe que o DDL não contém uma definição para uma tabela que é mapeada para o tipo de enumeração
5.  Clique em **concluir** clicando em concluir não executa o script DDL.
6.  O assistente para criar banco de dados faz o seguinte: abre o **UniversityModel. edmx. SQL** no Editor T-SQL gera as seções esquema de armazenamento e mapeamento do arquivo EDMX adiciona informações de cadeia de conexão ao arquivo app. config
7.  Clique com o botão direito do mouse no Editor T-SQL e selecione **executar** a caixa de diálogo conectar ao servidor aparece, insira as informações de conexão da etapa 2 e clique em **conectar**
8.  Para exibir o esquema gerado, clique com o botão direito do mouse no nome do banco de dados em Pesquisador de Objetos do SQL Server e selecione **Atualizar**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main está definido. Adicione o código a seguir à função main.

O código adiciona dois novos objetos University ao contexto. As propriedades espaciais são inicializadas usando o método DbGeography. FromText. O ponto de Geografia representado como WellKnownText é passado para o método. Em seguida, o código salva os dados. Em seguida, a consulta LINQ que retorna um objeto de universidade onde seu local está mais próximo do local especificado é construída e executada.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

Para exibir dados no banco de dados, clique com o botão direito do mouse no nome do banco de dado em Pesquisador de Objetos do SQL Server e selecione **Atualizar**. Em seguida, clique com o botão direito do mouse na tabela e selecione **exibir dados**.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como mapear tipos espaciais usando o Entity Framework Designer e como usar tipos espaciais no código. 
