---
title: Suporte a enum - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: d4c5528c4dc13ab7189421feebf84c2cb2f4b2bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995631"
---
# <a name="enum-support---ef-designer"></a>Suporte a enum - EF Designer
> [!NOTE]
> **EF5 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este passo a passo e vídeo passo a passo mostra como usar tipos de enumeração com o Entity Framework Designer. Ele também demonstra como usar enums em uma consulta LINQ.

Este passo a passo usará Model First para criar um novo banco de dados, mas o EF Designer também pode ser usado com o [Database First](~/ef6/modeling/designer/workflows/database-first.md) fluxo de trabalho para mapear para um banco de dados existente.

Suporte a enum foi introduzido no Entity Framework 5. Para usar os novos recursos, como enums, tipos de dados espaciais e funções com valor de tabela, você deve ter como destino .NET Framework 4.5. Visual Studio 2012 tem como alvo o .NET 4.5 por padrão.

No Entity Framework, uma enumeração pode ter os seguintes tipos de base: **bytes**, **Int16**, **Int32**, **Int64** , ou **SByte**.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como usar tipos de enumeração com o Entity Framework Designer. Ele também demonstra como usar enums em uma consulta LINQ.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012 Ultimate, Premium, Professional ou Web Express edition instalado para concluir este passo a passo.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abra o Visual Studio 2012
2.  Sobre o **arquivo** , aponte para **New**e, em seguida, clique em **projeto**
3.  No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo
4.  Insira **EnumEFDesigner** como o nome do projeto e clique em **Okey**

## <a name="create-a-new-model-using-the-ef-designer"></a>Criar um novo modelo usando o Designer de EF

1.  Clique no nome do projeto no Gerenciador de soluções, aponte para **Add**e, em seguida, clique em **Novo Item**
2.  Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos
3.  Insira **EnumTestModel.edmx** para o nome de arquivo e clique **adicionar**
4.  Na página do Assistente de modelo de dados de entidade, selecione **modelo vazio** na caixa de diálogo Escolher conteúdo do modelo
5.  Clique em **concluir**

O Designer de entidade, que fornece uma superfície de design para editar seu modelo, é exibido.

O assistente executa as seguintes ações:

-   Gera o arquivo EnumTestModel.edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles. Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no Assembly de saída para que os arquivos de metadados gerados obterem inseridos no assembly.
-   Adiciona uma referência aos assemblies a seguir: EntityFramework, DataAnnotations e Entity.
-   Cria arquivos EnumTestModel.tt e EnumTestModel.Context.tt e adiciona-o sob o arquivo. edmx. Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e tipos POCO que mapeiam para as entidades no modelo. edmx.

## <a name="add-a-new-entity-type"></a>Adicionar um novo tipo de entidade

1.  Com uma área vazia da superfície de design, selecione o botão direito **Add -&gt; entidade**, será exibida a caixa de diálogo nova entidade
2.  Especificar **departamento** para o tipo de nome e especifique **DepartmentID** para o nome de propriedade de chave, deixe o tipo como **Int32**
3.  Clique em **OK**
4.  A entidade com o botão direito e selecione **adicionar novo -&gt; propriedade escalar**
5.  Renomeie a nova propriedade **nome**
6.  Alterar o tipo da nova propriedade para **Int32** (por padrão, a nova propriedade é do tipo cadeia de caracteres) para alterar o tipo, abra a janela Propriedades e altere a propriedade de tipo para **Int32**
7.  Adicionar outra propriedade escalar e renomeie-o para **orçamento**, altere o tipo para **Decimal**

## <a name="add-an-enum-type"></a>Adicionar um tipo de enumeração

1.  No Entity Framework Designer, clique com botão direito a propriedade de nome, selecione **converter para enum**

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  No **adicionar Enum** tipo de caixa de diálogo **DepartmentNames** para o nome do tipo Enum, altere o tipo subjacente para **Int32**, e, em seguida, adicione os seguintes membros para o tipo: inglês dos EUA, Matemática e economia

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  Pressione **Okey**
4.  Salvar o modelo e compilar o projeto
    > [!NOTE]
    > Quando você compila, avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros. Você pode ignorar esses avisos porque depois que podemos optar por gerar o banco de dados do modelo, os erros serão eliminados.

Se você observar a janela Propriedades, você observará que o tipo da propriedade nome foi alterado para **DepartmentNames** e o tipo de enumeração recém-adicionado foi adicionado à lista de tipos.

Se você alternar para a janela do navegador de modelo, você verá que o tipo também foi adicionado ao nó de tipos Enum.

![ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Você também pode adicionar novos tipos de enum dessa janela clicando o botão direito do mouse e selecionando **Adicionar tipo de enumeração**. Depois que o tipo é criado ele aparecerá na lista de tipos e você poderá associar uma propriedade

## <a name="generate-database-from-model"></a>Gerar o banco de dados de modelo

Agora podemos gerar um banco de dados com base no modelo.

1.  Clique com botão direito no Entity Designer na superfície e selecione uma área vazia **gerar banco de dados de modelo**
2.  Escolha sua caixa de diálogo de Conexão de dados do Assistente para gerar banco de dados é exibido clique a **nova Conexão** botão especificar **(localdb)\\mssqllocaldb** para o nome do servidor e o  **EnumTest** para o banco de dados e clique em **Okey**
3.  Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.
4.  Clique em **próxima** e o Assistente para criar banco de dados gera linguagem de definição de dados (DDL) para criar um banco de dados DDL gerado é exibido no resumo e as configurações de caixa de diálogo caixa de anotação, que o DDL não contém uma definição para um tabela que mapeia para o tipo de enumeração
5.  Clique em **concluir** clicar em Concluir não executa o script DDL.
6.  O Assistente para criar banco de dados faz o seguinte: abre a **EnumTest.edmx.sql** no Editor T-SQL gera as seções de esquema e mapeamento de armazenamento do EDMX arquivo adiciona informações de cadeia de caracteres de conexão no arquivo App. config
7.  Clique o botão direito do mouse no Editor T-SQL e selecione **Execute** conectar-se a caixa de diálogo do servidor for exibida, insira as informações de conexão da etapa 2 e clique em **Connect**
8.  Para exibir o esquema gerado, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **atualizar**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main é definido. Adicione o seguinte código na função principal. O código adiciona um novo objeto de departamento para o contexto. Em seguida, ele salva os dados. O código também executa uma consulta LINQ que retorna um departamento em que o nome é DepartmentNames.English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compile e execute o aplicativo. O programa produz a seguinte saída:

```
DepartmentID: 1 Name: English
```

Para exibir dados no banco de dados, clique com botão direito no nome do banco de dados no Pesquisador de objetos do SQL Server e selecione **Refresh**. Em seguida, clique o botão direito do mouse na tabela e selecione **exibir dados**.

## <a name="summary"></a>Resumo

Neste passo a passo, nós examinamos como mapear tipos de enumeração usando o Entity Framework Designer e como usar enums no código. 
