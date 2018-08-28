---
title: Atalhos de teclado do Designer do Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: 70c9705956b58f4d00908dd9cca6ad0e0a078fc6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997757"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Atalhos de teclado do Designer do Entity Framework
Esta página fornece uma lista de shorcuts de teclado que estão disponíveis em várias telas do Entity Framework Tools para Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Assistente de modelo de dados de entidade ADO.NET

### <a name="step-one-choose-model-contents"></a>Etapa 1: Escolher conteúdo do modelo

![WizardOne](~/ef6/media/wizardone.png)

| Atalho  | Ação                                                     | Observações                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Mover para a próxima tela                                        | Não disponível para todas as seleções de conteúdo do modelo. |
| **ALT + f** | Concluir o Assistente                                              | Não disponível para todas as seleções de conteúdo do modelo. |
| **ALT + w** | Alternar o foco para o "o que deve o modelo contém?" painel. |                                                     |

### <a name="step-two-choose-your-connection"></a>Etapa 2: Escolher sua Conexão

![WizardTwo](~/ef6/media/wizardtwo.png)

| Atalho  | Ação                                                     | Observações                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Mover para a próxima tela                                        |                                                         |
| **ALT + p** | Mover para a tela anterior                                    |                                                         |
| **ALT + w** | Alternar o foco para o "o que deve o modelo contém?" painel. |                                                         |
| **ALT + c** | Abra a janela "Propriedades de Conexão"                    | Permite a definição de uma nova conexão de banco de dados. |
| **ALT + e** | Exclua os dados confidenciais da cadeia de conexão          |                                                         |
| **Alt + i** | Incluir dados confidenciais na cadeia de conexão            |                                                         |
| **ALT + s** | Ativar/desativar a opção "Salvar configurações de conexão no App. config" |                                                         |

### <a name="step-three-choose-your-version"></a>Etapa três: Escolha sua versão

![WizardThree](~/ef6/media/wizardthree.png)

| Atalho  | Ação                                             | Observações                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Mover para a próxima tela                                |                                                                                       |
| **ALT + p** | Mover para a tela anterior                            |                                                                                       |
| **ALT + w** | Alternar o foco para a seleção de versão do Entity Framework | Permite especificar uma versão diferente do Entity Framework para uso no projeto. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Etapa quatro: Escolha seus objetos de banco de dados e configurações

![WizardFour](~/ef6/media/wizardfour.png)

| Atalho  | Ação                                                                                    | Observações                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Concluir o Assistente                                                                             |                                                                     |
| **ALT + p** | Mover para a tela anterior                                                                   |                                                                     |
| **ALT + w** | Alternar o foco para o painel de seleção de objeto de banco de dados                                            | Possibilita especificar objetos de banco de dados a serem reverso projetado.    |
| **ALT + s** | Ativar/desativar o "Pluralizar ou singularizar nomes de objetos gerados" opção                       |                                                                     |
| **ALT + k** | Ativar/desativar a opção "Incluir colunas de chave estrangeira no modelo de"                              | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **Alt + i** | Ativar/desativar a opção "Import selected procedimentos armazenados e funções no modelo de entidade" | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **ALT + m** | Alterna o foco para o campo de texto "Namespace de modelo"                                        | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **Espaço** | Alternar seleção no elemento                                                               | Se o elemento tiver filhos, todos os elementos filho serão alternados também |
| **Esquerda**  | Recolher árvore de filho                                                                       |                                                                     |
| **Direita** | Expanda a árvore filho                                                                         |                                                                     |
| **Para cima**    | Navegue até o elemento anterior na árvore                                                      |                                                                     |
| **Para baixo**  | Navegue até o próximo elemento na árvore                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Superfície do Designer de EF

![DesignerSurface](~/ef6/media/designersurface.png)

| Atalho                                                                                | Ação                      | Observações                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espaço/Enter**                                                                         | Alternar seleção            | Alterna a seleção no objeto com o foco.                                                                                                                                                                                         |
| **Esc**                                                                                 | Cancelar seleção            | Cancela a seleção atual.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Selecionar Tudo                  | Seleciona todas as formas na superfície de design.                                                                                                                                                                                       |
| **Seta para cima**                                                                            | Mover para cima                     | Move selecionadas a entidade de incremento de uma grade. <br/> Em caso de uma lista, move o subcampo irmão anterior.                                                                                                                            |
| **Seta para baixo**                                                                          | Mover para baixo                   | Move selecionadas entidade para baixo de incremento de uma grade. <br/> Em caso de uma lista, move para o próximo subcampo de irmão.                                                                                                                              |
| **Seta para a esquerda**                                                                          | Mover para a esquerda                   | Move selecionadas incremento de uma grade à esquerda da entidade. <br/> Em caso de uma lista, move o subcampo irmão anterior.                                                                                                                          |
| **Seta para a direita**                                                                         | Mover para a direita                  | Move selecionadas incremento de grade à direita de uma entidade. <br/> Em caso de uma lista, move para o próximo subcampo de irmão.                                                                                                                             |
| **SHIFT + seta para a esquerda**                                                                  | Forma de tamanho para a esquerda             | Reduz a largura da entidade selecionada, o incremento de uma grade.                                                                                                                                                                     |
| **SHIFT + seta para a direita**                                                                 | Forma de tamanho certa            | Aumenta a largura da entidade selecionada, o incremento de uma grade.                                                                                                                                                                   |
| **Início**                                                                                | Primeiro ponto a ponto                  | Move o foco e a seleção para o primeiro objeto na superfície de design no mesmo nível de ponto a ponto.                                                                                                                                         |
| **End**                                                                                 | Último ponto a ponto                   | Move o foco e a seleção até o último objeto na superfície de design no mesmo nível de ponto a ponto.                                                                                                                                          |
| **CTRL + Home**                                                                         | Primeiro ponto a ponto (foco)          | Mesmo que o primeiro ponto a ponto, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                          |
| **CTRL + End**                                                                          | Último ponto a ponto (foco)           | Mesmo que o último par, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                           |
| **Tab**                                                                                 | Próximo ponto a ponto                   | Move o foco e a seleção para o próximo objeto na superfície de design no mesmo nível de ponto a ponto.                                                                                                                                          |
| **Shift+Tab**                                                                           | Ponto a ponto anterior               | Move o foco e a seleção para o objeto anterior na superfície de design no mesmo nível de ponto a ponto.                                                                                                                                      |
| **Ctrl + Alt + Tab**                                                                        | Próximo ponto a ponto (foco)           | Mesmo que o próximo par, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | Ponto a ponto anterior (foco)       | Mesmo que o ponto anterior, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | Move para o próximo objeto sobre o design surface um nível superior na hierarquia. Se não houver nenhuma forma acima desta forma na hierarquia (ou seja, o objeto é colocado diretamente na superfície de design), o diagrama é selecionado. |
| **&gt;**                                                                                | Descendem                     | Move para o próximo objeto contido no nível de uma superfície de design abaixo na hierarquia. Se não houver nenhum objeto contido, isso é não operacional.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (foco)              | Mesmo que ascend comando, mas, move o foco sem seleção.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Descendem (foco)             | Mesmo que descendem comando, mas, move o foco sem seleção.                                                                                                                                                                         |
| **SHIFT + End**                                                                         | Siga para conectado         | De uma entidade, move para uma entidade à qual esta entidade está conectada ao.                                                                                                                                                               |
| **Del**                                                                                 | Excluir                      | Exclua um objeto ou o conector do diagrama.                                                                                                                                                                                     |
| **Ins**                                                                                 | Inserir                      | Adiciona uma nova propriedade a uma entidade quando o cabeçalho do compartimento de "Propriedades escalares" ou uma propriedade em si é selecionada.                                                                                                           |
| **PG para cima**                                                                               | Diagrama de rolagem para cima           | Rola para a superfície de design, em incrementos iguais a 75% da altura da superfície de design visível no momento.                                                                                                                    |
| **PG para baixo**                                                                             | Diagrama de rolagem para baixo         | Rola a superfície de design para baixo.                                                                                                                                                                                                    |
| **SHIFT + Pg para baixo**                                                                     | Diagrama de rolagem à direita        | Rola para a superfície de design para a direita.                                                                                                                                                                                            |
| **SHIFT + Pg para cima**                                                                       | Diagrama de rolagem à esquerda         | Rola para a superfície de design para a esquerda.                                                                                                                                                                                             |
| **F2**                                                                                  | Entrar no modo de edição             | Atalho de teclado padrão para modo de edição para um controle de texto.                                                                                                                                                               |
| **SHIFT + F10**                                                                         | Exibir menu de atalho       | Atalho de teclado padrão para exibir o menu de atalho de um item selecionado.                                                                                                                                                          |
| **Ctrl + Shift + Mouse com o botão esquerdo**  <br/> **Control + Shift + MouseWheel para frente**  | Zoom semântico em            | Amplia a área de exibição de diagrama abaixo do ponteiro do mouse.                                                                                                                                                                 |
| **Ctrl + Shift + Mouse clique com botão direito** <br/> **Control + Shift + MouseWheel com versões anteriores** | Zoom semântico Out           | Zoom da área de exibição de diagrama abaixo do ponteiro do mouse. Novamente, ele centraliza o diagrama ao zoom longe demais para o centro do diagrama atual.                                                                          |
| **Ctrl + Shift + '+'** <br/> **Control + MouseWheel para frente**                        | Ampliar                     | Amplia o centro da exibição de diagrama.                                                                                                                                                                                         |
| **Ctrl + Shift + '-'** <br/> **Control + MouseWheel com versões anteriores**                       | Reduza a imagem                    | Zoom da área de exibição de diagrama clicada. Novamente, ele centraliza o diagrama ao zoom longe demais para o centro do diagrama atual.                                                                                            |
| **Ctrl + Shift + desenhar um retângulo com o botão esquerdo do mouse para baixo**                  | Área de zoom                   | Amplia centralizada na área que você selecionou. Quando você pressiona CTRL + teclas Shift, você verá que o cursor muda para uma lupa, que permite que você definir a área para ampliar.                        |
| **Tecla de Menu de contexto + estou '**                                                              | Abra a janela de detalhes de mapeamento | Abre a janela de detalhes de mapeamento para editar mapeamentos para a entidade selecionada                                                                                                                                                               |

## <a name="mapping-details-window"></a>Janela de detalhes de mapeamento

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| Atalho                  | Ação         | Observações                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tab**                   | Altere o contexto | Alterna entre a área da janela principal e a barra de ferramentas à esquerda                                                                     |
| **Teclas de direção**            | Navegação     | Mover para cima e para linhas, ou à direita e esquerda entre colunas na área de janela principal. Mover entre os botões na barra de ferramentas à esquerda. |
| **Enter** <br/> **Espaço** | Selecionar         | Seleciona um botão na barra de ferramentas à esquerda.                                                                                          |
| **ALT + seta para baixo**      | Abrir a lista      | Exibir uma lista suspensa, se uma célula é selecionada que tem uma lista suspensa.                                                                     |
| **Enter**                 | Lista, selecione    | Seleciona um elemento em uma lista suspensa.                                                                                               |
| **Esc**                   | Listar fechar     | Fecha uma lista suspensa.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navegação do Visual Studio

Entity Framework também fornece uma série de ações que podem ter mapeados de atalhos de teclado personalizados (não há atalhos são mapeados por padrão). Para criar esses atalhos personalizados, clique no menu Ferramentas e opções.  Em ambiente, escolha o teclado.  Role para baixo a lista no meio até que você pode selecionar o comando desejado, digite o atalho na caixa de texto "Pressione as teclas de atalho" e clique em atribuir. Os atalhos de possíveis são os seguintes:

| Atalho                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
