---
title: Suporte a enum-EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182514"
---
# <a name="enum-support---ef-designer"></a>Suporte a enum-designer de EF
> [!NOTE]
> **Somente EF5 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 5. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este vídeo e instruções passo a passo mostram como usar tipos de enumeração com o Entity Framework Designer. Ele também demonstra como usar enums em uma consulta LINQ.

Este passo a passos usará Model First para criar um novo banco de dados, mas o designer do EF também pode ser usado com o fluxo de trabalho de [Database First](~/ef6/modeling/designer/workflows/database-first.md) para mapear para um banco de dados existente.

O suporte a enum foi introduzido no Entity Framework 5. Para usar os novos recursos como enums, tipos de dados espaciais e funções com valor de tabela, você deve direcionar .NET Framework 4,5. O Visual Studio 2012 visa o .NET 4,5 por padrão.

No Entity Framework, uma enumeração pode ter os seguintes tipos subjacentes: **Byte**, **Int16**, **Int32**, **Int64** ou **SByte**.

## <a name="watch-the-video"></a>Assista ao vídeo
Este vídeo mostra como usar tipos de enumeração com o Entity Framework Designer. Ele também demonstra como usar enums em uma consulta LINQ.

**Apresentado por**: Julia Kornich

**Vídeo**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Pré-requisitos

Você precisará ter o Visual Studio 2012, Ultimate, Premium, Professional ou Web Express Edition instalado para concluir este guia.

## <a name="set-up-the-project"></a>Configurar o projeto

1.  Abrir o Visual Studio 2012
2.  No menu **arquivo** , aponte para **novo**e clique em **projeto**
3.  No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console**
4.  Insira **EnumEFDesigner** como o nome do projeto e clique em **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Criar um novo modelo usando o designer do EF

1.  Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções, aponte para **Adicionar**e clique em **novo item**
2.  Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos
3.  Digite **EnumTestModel. edmx** para o nome do arquivo e clique em **Adicionar**
4.  Na página Assistente de Modelo de Dados de Entidade, selecione **modelo vazio** na caixa de diálogo escolher conteúdo do modelo
5.  Clique em **concluir**

O Entity Designer, que fornece uma superfície de design para editar seu modelo, é exibido.

O assistente executa as seguintes ações:

-   Gera o arquivo EnumTestModel. edmx que define o modelo conceitual, o modelo de armazenamento e o mapeamento entre eles. Define a propriedade de processamento de artefato de metadados do arquivo. edmx para inserir no assembly de saída para que os arquivos de metadados gerados sejam inseridos no assembly.
-   Adiciona uma referência aos seguintes assemblies: EntityFramework, System. ComponentModel. Annotations e System. Data. Entity.
-   Cria arquivos EnumTestModel.tt e EnumTestModel.Context.tt e os adiciona no arquivo. edmx. Esses arquivos de modelo T4 geram o código que define o tipo derivado de DbContext e os tipos POCO que são mapeados para as entidades no modelo. edmx.

## <a name="add-a-new-entity-type"></a>Adicionar um novo tipo de entidade

1.  Clique com o botão direito do mouse em uma área vazia da superfície de design, selecione **entidade Add-&gt;** , a caixa de diálogo nova entidade será exibida
2.  Especifique **Department** para o nome do tipo e especifique **DepartmentID** para o nome da propriedade de chave, deixe o tipo como **Int32**
3.  Clique em **OK**
4.  Clique com o botão direito do mouse na entidade e selecione **Adicionar New-&gt; Propriedade escalar**
5.  Renomear a nova propriedade como **nome**
6.  Altere o tipo da nova propriedade para **Int32** (por padrão, a nova propriedade é do tipo cadeia de caracteres) para alterar o tipo, abra o janela Propriedades e altere a propriedade Type para **Int32**
7.  Adicione outra propriedade escalar e renomeie-a para **orçamento**, altere o tipo para **decimal**

## <a name="add-an-enum-type"></a>Adicionar um tipo de enumeração

1.  Na Entity Framework Designer, clique com o botão direito do mouse na propriedade Name, selecione **converter em enum**

    ![Converter em enumeração](~/ef6/media/converttoenum.png)

2.  Na caixa de diálogo **Adicionar enum** , digite **departmentnames** para o nome do tipo de enumeração, altere o tipo subjacente para **Int32**e, em seguida, adicione os seguintes membros ao tipo: Inglês, matemática e economia

    ![Adicionar tipo de enumeração](~/ef6/media/addenumtype.png)

3.  Pressione **OK**
4.  Salvar o modelo e compilar o projeto
    > [!NOTE]
    > Quando você cria, avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros. Você pode ignorar esses avisos porque, depois de escolhermos para gerar o banco de dados do modelo, os erros serão descontinuados.

Se você observar a janela Propriedades, observará que o tipo da propriedade Name foi alterado para **departmentnames** e o tipo de enumeração recém-adicionado foi adicionado à lista de tipos.

Se você alternar para a janela do navegador de modelos, verá que o tipo também foi adicionado ao nó tipos de enumeração.

![Navegador de modelos](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Você também pode adicionar novos tipos de enumeração dessa janela clicando com o botão direito do mouse e selecionando **Adicionar tipo de enumeração**. Depois que o tipo for criado, ele aparecerá na lista de tipos e você poderá associá-lo a uma propriedade

## <a name="generate-database-from-model"></a>Gerar banco de dados do modelo

Agora, podemos gerar um banco de dados baseado no modelo.

1.  Clique com o botão direito do mouse em um espaço vazio na superfície de Entity Designer e selecione **gerar banco de dados do modelo**
2.  A caixa de diálogo escolher sua conexão de dados do assistente para gerar banco de dados é exibida clique no botão **nova conexão** especificar **(LocalDB) \\mssqllocaldb** para o nome do servidor e **EnumTest** para o banco de dados e clique em **OK**
3.  Uma caixa de diálogo perguntando se você deseja criar um novo banco de dados será exibida, clique em **Sim**.
4.  Clique em **Avançar** e o assistente para criar banco de dados gera DDL (linguagem de definição de dado) para criar um banco de dados o DDL gerado é exibido na caixa de diálogo Resumo e configurações Observe que o DDL não contém uma definição para uma tabela que mapeia para o tipo de enumeração
5.  Clique em **concluir** clicando em concluir não executa o script DDL.
6.  O assistente para criar banco de dados faz o seguinte: Abre o **EnumTest. edmx. SQL** no Editor T-SQL gera as seções esquema de armazenamento e mapeamento do arquivo EDMX adiciona informações de cadeia de conexão ao arquivo app. config
7.  Clique com o botão direito do mouse no Editor T-SQL e selecione **executar** a caixa de diálogo conectar ao servidor aparece, insira as informações de conexão da etapa 2 e clique em **conectar**
8.  Para exibir o esquema gerado, clique com o botão direito do mouse no nome do banco de dados em Pesquisador de Objetos do SQL Server e selecione **Atualizar**

## <a name="persist-and-retrieve-data"></a>Persistir e recuperar dados

Abra o arquivo Program.cs em que o método Main está definido. Adicione o código a seguir à função main. O código adiciona um novo objeto Department ao contexto. Em seguida, ele salva os dados. O código também executa uma consulta LINQ que retorna um departamento onde o nome é Departmentnames. inglês.

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

```console
DepartmentID: 1 Name: English
```

Para exibir dados no banco de dados, clique com o botão direito do mouse no nome do banco de dado em Pesquisador de Objetos do SQL Server e selecione **Atualizar**. Em seguida, clique com o botão direito do mouse na tabela e selecione **exibir dados**.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como mapear tipos de enumeração usando o Entity Framework Designer e como usar enums no código. 
