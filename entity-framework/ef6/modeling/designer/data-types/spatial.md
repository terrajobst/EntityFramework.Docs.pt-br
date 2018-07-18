---
title: Espaciais - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119891"
---
# <a name="spatial---ef-designer"></a>Espaciais - EF Designer
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O passo a passo e vídeo passo a passo mostra como mapear tipos espaciais com o Entity Framework Designer. Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.

Este passo a passo usará Model First para criar um novo banco de dados, mas o EF Designer também pode ser usado com o [Database First](~/ef6/modeling/designer/workflows/database-first.md) fluxo de trabalho para mapear para um banco de dados existente.

Suporte a tipo espacial foi introduzido no Entity Framework 5. Observe que, para usar os novos recursos, como o tipo espacial, enumerações e funções com valor de tabela, você deve direcionar o .NET Framework 4.5. Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.

Para usar tipos de dados espaciais, você também deve usar um provedor do Entity Framework que tenha suporte espacial. Ver [suporte do provedor para tipos espaciais](~/ef6/fundamentals/providers/spatial-support.md) para obter mais informações.

Há dois tipos de dados espaciais principal: geography e geometry. O tipo de dados geography armazena dados elipsoidais (por exemplo, GPS latitude e longitude coordenadas). O tipo de dados geometry representa o sistema de coordenadas Euclidiano (plano).

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como mapear tipos espaciais com o Entity Framework Designer. Ele também demonstra como usar uma consulta LINQ para localizar uma distância entre dois locais.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abra o Visual Studio 2012
2.  Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**
3.  No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo
4.  Insira **SpatialEFDesigner** como o nome do projeto e clique em **Okey**

## <a name="create-a-new-model-using-the-ef-designer"></a>Criar um novo modelo usando o Designer de EF

1.  Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**
2.  Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos
3.  Insira **UniversityModel.edmx** para o nome de arquivo e clique **adicionar**
4.  Na página do Assistente de modelo de dados de entidade, selecione **modelo vazio** na caixa de diálogo Escolher conteúdo do modelo
5.  Clique em **concluir**

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.

O assistente executa as seguintes ações:

-   Gera o arquivo EnumTestModel.edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles. Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no Assembly de saída para que os arquivos de metadados gerados obterem inseridos no assembly.
-   Adiciona uma referência aos assemblies a seguir: EntityFramework, DataAnnotations e Entity.
-   Cria arquivos UniversityModel.tt e UniversityModel.Context.tt e adiciona-o sob o arquivo. edmx. Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e tipos POCO que mapeiam para as entidades no modelo. edmx

## <a name="add-a-new-entity-type"></a>Adicionar um novo tipo de entidade

1.  Com uma área vazia da superfície de design, selecione o botão direito **Add -&gt; entidade**, será exibida a caixa de diálogo nova entidade
2.  Especificar **University** para o tipo de nome e especifique **UniversityID** para o nome de propriedade de chave, deixe o tipo como **Int32**
3.  Clique em **OK**
4.  A entidade com o botão direito e selecione **adicionar novo -&gt; propriedade escalar**
5.  Renomeie a nova propriedade **nome**
6.  Adicionar outra propriedade escalar e renomeie-o para **local** abra a janela Propriedades e altere o tipo da nova propriedade para **geografia**
7.  Salvar o modelo e compilar o projeto
    > [!NOTE]
    > Quando você compila, avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros. Você pode ignorar esses avisos porque depois que podemos optar por gerar o banco de dados do modelo, os erros serão eliminados.

## <a name="generate-database-from-model"></a>Gerar o banco de dados de modelo

Agora podemos gerar um banco de dados com base no modelo.

1.  Clique com botão direito no Entity Designer na superfície e selecione uma área vazia **gerar banco de dados de modelo**
2.  Escolha sua caixa de diálogo de Conexão de dados do Assistente para gerar banco de dados é exibido clique a **nova Conexão** botão especificar **(localdb)\\mssqllocaldb** para o nome do servidor e o  **Universidade** para o banco de dados e clique em **Okey**
3.  Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.
4.  Clique em **próxima** e o Assistente para criar banco de dados gera linguagem de definição de dados (DDL) para criar um banco de dados DDL gerado é exibido no resumo e as configurações de caixa de diálogo caixa de anotação, que o DDL não contém uma definição para um tabela que mapeia para o tipo de enumeração
5.  Clique em **concluir** clicar em Concluir não executa o script DDL.
6.  O Assistente para criar banco de dados faz o seguinte: abre a **UniversityModel.edmx.sql** no Editor T-SQL gera as seções de esquema e mapeamento de armazenamento do EDMX arquivo adiciona informações de cadeia de caracteres de conexão no arquivo App. config
7.  Clique o botão direito do mouse no Editor T-SQL e selecione **Execute** conectar-se a caixa de diálogo do servidor for exibida, insira as informações de conexão da etapa 2 e clique em **Connect**
8.  Para exibir o esquema gerado, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **atualizar**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main é definido. Adicione o seguinte código na função principal.

O código adiciona dois novos objetos University para o contexto. Propriedades espaciais são inicializadas por meio do método DbGeography.FromText. O ponto de Geografia representado como WellKnownText é passado para o método. O código, em seguida, salva os dados. Em seguida, a consulta LINQ que que retorna um objeto University onde é mais próxima do local especificado, a sua localização é construído e executado.

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

```
The closest University to you is: School of Fine Art.
```

Para exibir dados no banco de dados, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **Refresh**. Em seguida, clique o botão direito do mouse na tabela e selecione **exibir dados**.

## <a name="summary"></a>Resumo

Neste passo a passo, nós examinamos como mapear tipos espaciais usando o Entity Framework Designer e como usar tipos espaciais no código. 
