---
title: Entity Framework Designer atalhos de teclado – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418514"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework Designer atalhos de teclado
Esta página fornece uma lista de shorcuts de teclado que estão disponíveis nas várias telas do Entity Framework Tools para o Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Assistente de Modelo de Dados de Entidade de ADO.NET

### <a name="step-one-choose-model-contents"></a>Etapa 1: escolher conteúdo do modelo

![Assistente um](~/ef6/media/wizardone.png)

| Atalho  | Ação                                                     | Observações                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | Mover para a próxima tela                                        | Não disponível para todas as seleções de conteúdo do modelo. |
| **ALT + f** | Conclua o assistente                                              | Não disponível para todas as seleções de conteúdo do modelo. |
| **Alt + w** | Alternar o foco para o "o que o modelo deve conter?" painel. |                                                     |

### <a name="step-two-choose-your-connection"></a>Etapa dois: escolha sua conexão

![Assistente dois](~/ef6/media/wizardtwo.png)

| Atalho  | Ação                                                     | Observações                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | Mover para a próxima tela                                        |                                                         |
| **Alt + p** | Mover para a tela anterior                                    |                                                         |
| **Alt + w** | Alternar o foco para o "o que o modelo deve conter?" painel. |                                                         |
| **Alt + c** | Abrir a janela "Propriedades da conexão"                    | Permite a definição de uma nova conexão de banco de dados. |
| **ALT + e** | Excluir dados confidenciais da cadeia de conexão          |                                                         |
| **Alt + i** | Incluir dados confidenciais na cadeia de conexão            |                                                         |
| **Alt + s** | Alternar a opção "salvar configurações de conexão na App. config" |                                                         |

### <a name="step-three-choose-your-version"></a>Etapa três: escolha sua versão

![Assistente três](~/ef6/media/wizardthree.png)

| Atalho  | Ação                                             | Observações                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | Mover para a próxima tela                                |                                                                                       |
| **Alt + p** | Mover para a tela anterior                            |                                                                                       |
| **Alt + w** | Alternar o foco para Entity Framework seleção de versão | Permite especificar uma versão diferente do Entity Framework para uso no projeto. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Etapa quatro: escolher os objetos e as configurações do banco de dados

![Assistente quatro](~/ef6/media/wizardfour.png)

| Atalho  | Ação                                                                                    | Observações                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Conclua o assistente                                                                             |                                                                     |
| **Alt + p** | Mover para a tela anterior                                                                   |                                                                     |
| **Alt + w** | Alternar o foco para o painel de seleção de objeto de banco de dados                                            | Permite a especificação de objetos de banco de dados para a engenharia reversa.    |
| **Alt + s** | Alternar a opção "Pluralize ou singularize nomes de objeto gerados"                       |                                                                     |
| **Alt + k** | Alternar a opção "incluir colunas de chave estrangeira no modelo"                              | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **Alt + i** | Alternar a opção "importar procedimentos armazenados selecionados e funções para o modelo de entidade" | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **Alt + m** | Alterna o foco para o campo de texto "namespace do modelo"                                        | Não disponível para todas as seleções de conteúdo do modelo.                 |
| **Espaço** | Alternar seleção no elemento                                                               | Se o elemento tiver filhos, todos os elementos filho também serão alternados |
| **Left**  | Recolher árvore filho                                                                       |                                                                     |
| **Right** | Expandir árvore filho                                                                         |                                                                     |
| **Operante**    | Navegar até o elemento anterior na árvore                                                      |                                                                     |
| **Para baixo**  | Navegar para o próximo elemento na árvore                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Superfície do designer do EF

![Superfície do Designer](~/ef6/media/designersurface.png)

| Atalho                                                                                | Ação                      | Observações                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espaço/Enter**                                                                         | Alternar seleção            | Alterna a seleção no objeto com foco.                                                                                                                                                                                         |
| **Esc**                                                                                 | Cancelar seleção            | Cancela a seleção atual.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Selecionar Tudo                  | Seleciona todas as formas na superfície de design.                                                                                                                                                                                       |
| **Seta para cima**                                                                            | Mover para cima                     | Move a entidade selecionada para cima um incremento de grade. <br/> Se estiver em uma lista, será movido para o subcampo irmão anterior.                                                                                                                            |
| **Seta para baixo**                                                                          | Mover para baixo                   | Move a entidade selecionada para baixo em um incremento de grade. <br/> Se estiver em uma lista, será movido para o próximo subcampo irmão.                                                                                                                              |
| **Seta para a esquerda**                                                                          | Mover para a esquerda                   | Move a entidade selecionada para a esquerda um incremento de grade. <br/> Se estiver em uma lista, será movido para o subcampo irmão anterior.                                                                                                                          |
| **Seta para a direita**                                                                         | Mover para a direita                  | Move a entidade selecionada um incremento à direita da grade. <br/> Se estiver em uma lista, será movido para o próximo subcampo irmão.                                                                                                                             |
| **Shift + seta para a esquerda**                                                                  | Dimensionar forma à esquerda             | Reduz a largura da entidade selecionada por um incremento de grade.                                                                                                                                                                     |
| **Shift + seta para a direita**                                                                 | Dimensionar forma à direita            | Aumenta a largura da entidade selecionada por um incremento de grade.                                                                                                                                                                   |
| **Página Inicial**                                                                                | Primeiro par                  | Move o foco e a seleção para o primeiro objeto na superfície de design no mesmo nível de par.                                                                                                                                         |
| **Término**                                                                                 | Último par                   | Move o foco e a seleção para o último objeto na superfície de design no mesmo nível de par.                                                                                                                                          |
| **CTRL + página inicial**                                                                         | Primeiro par (foco)          | O mesmo que o primeiro par, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                          |
| **CTRL + fim**                                                                          | Último par (foco)           | O mesmo que o último par, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                           |
| **Tab**                                                                                 | Próximo par                   | Move o foco e a seleção para o próximo objeto na superfície de design no mesmo nível de par.                                                                                                                                          |
| **Shift+Tab**                                                                           | Par anterior               | Move o foco e a seleção para o objeto anterior na superfície de design no mesmo nível de par.                                                                                                                                      |
| **Alt + Ctrl + Tab**                                                                        | Próximo par (foco)           | Igual ao próximo par, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Tab**                                                                  | Par anterior (foco)       | Mesmo que o par anterior, mas move o foco em vez de mover o foco e a seleção.                                                                                                                                                       |
| **&lt;**                                                                                | Código                      | Move para o próximo objeto na superfície de design um nível acima na hierarquia. Se não houver formas acima dessa forma na hierarquia (ou seja, o objeto for colocado diretamente na superfície de Design), o diagrama será selecionado. |
| **&gt;**                                                                                | Ascende                     | Move para o próximo objeto contido na superfície de design um nível abaixo dele na hierarquia. Se não houver nenhum objeto contido, isso será não operacional.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (foco)              | O mesmo que o comando Ascend, mas move o foco sem seleção.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Descender (foco)             | O mesmo que o comando descendente, mas move o foco sem seleção.                                                                                                                                                                         |
| **Shift + fim**                                                                         | Seguir para conectado         | De uma entidade, move para uma entidade à qual essa entidade está conectada.                                                                                                                                                               |
| **Del**                                                                                 | Excluir                      | Exclua um objeto ou conector do diagrama.                                                                                                                                                                                     |
| **Los**                                                                                 | Insert                      | Adiciona uma nova propriedade a uma entidade quando o cabeçalho de compartimento "Propriedades escalares" ou uma propriedade em si é selecionada.                                                                                                           |
| **PG para cima**                                                                               | Rolar diagrama para cima           | Rola a superfície de design para cima, em incrementos igual a 75% da altura da superfície de design visível no momento.                                                                                                                    |
| **PG para baixo**                                                                             | Rolar diagrama para baixo         | Rola a superfície de design para baixo.                                                                                                                                                                                                    |
| **Shift + PG para baixo**                                                                     | Rolar diagrama para a direita        | Rola a superfície de design para a direita.                                                                                                                                                                                            |
| **Shift + PG para cima**                                                                       | Diagrama de rolagem à esquerda         | Rola a superfície de design para a esquerda.                                                                                                                                                                                             |
| **F2**                                                                                  | Entrar no modo de edição             | Atalho de teclado padrão para entrar no modo de edição para um controle de texto.                                                                                                                                                               |
| **Shift + F10**                                                                         | Exibir menu de atalho       | Atalho de teclado padrão para exibir o menu de atalho de um item selecionado.                                                                                                                                                          |
| **Control + Shift + clique com o botão esquerdo do mouse**  <br/> **Control + Shift + MouseWheel avançar**  | Zoom semântico            | Amplia a área da exibição de diagrama abaixo do ponteiro do mouse.                                                                                                                                                                 |
| **Control + Shift + clique com o botão direito do mouse** <br/> **Control + Shift + MouseWheel para trás** | Reduzir semântica           | Reduz a área da exibição de diagrama abaixo do ponteiro do mouse. Ele recentraliza o diagrama quando você reduz muito longe para o centro de diagramas atual.                                                                          |
| **Control + Shift + ' + '** <br/> **Controle + MouseWheel Forward**                        | Ampliar                     | Aumenta o zoom no centro do modo de exibição de diagrama.                                                                                                                                                                                         |
| **Control + Shift + '-'** <br/> **Controle + MouseWheel retroativamente**                       | Reduzir                    | Reduz a área clicada da exibição de diagrama. Ele recentraliza o diagrama quando você reduz muito longe para o centro de diagramas atual.                                                                                            |
| **Control + Shift + desenhar um retângulo com o botão esquerdo do mouse abaixo**                  | Área de zoom                   | Amplia o centrado na área que você selecionou. Quando você mantiver as teclas Control + Shift, verá que o cursor muda para uma lupa, que permite que você defina a área para ampliar.                        |
| **Tecla do menu de contexto + M'**                                                              | Abrir janela de detalhes de mapeamento | Abre a janela de detalhes de mapeamento para editar mapeamentos para a entidade selecionada                                                                                                                                                               |

## <a name="mapping-details-window"></a>Janela de detalhes do mapeamento

![Atalhos de detalhes de mapeamento](~/ef6/media/mappingdetailsshortcuts.png)

| Atalho                  | Ação         | Observações                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tab**                   | Alternar contexto | Alterna entre a área da janela principal e a barra de ferramentas à esquerda                                                                     |
| **Teclas de direção**            | Navegação     | Mover linhas para cima e para baixo, ou direita e esquerda entre colunas na área da janela principal. Mova entre os botões na barra de ferramentas à esquerda. |
| **Enter** <br/> **Espaço** | Selecionar         | Seleciona um botão na barra de ferramentas à esquerda.                                                                                          |
| **Alt + seta para baixo**      | Abrir lista      | Selecione uma lista suspensa se for selecionada uma célula que tenha uma lista suspensa.                                                                     |
| **Enter**                 | Listar seleção    | Seleciona um elemento em uma lista suspensa.                                                                                               |
| **Esc**                   | Fechar lista     | Fecha uma lista suspensa.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navegação do Visual Studio

O Entity Framework também fornece várias ações que podem ter atalhos de teclado personalizados mapeados (nenhum atalho é mapeado por padrão). Para criar esses atalhos personalizados, clique no menu ferramentas e em opções.  Em ambiente, escolha teclado.  Role a lista no meio até que você possa selecionar o comando desejado, insira o atalho na caixa de texto "Pressione as teclas de atalho" e clique em atribuir. Os atalhos possíveis são os seguintes:

| Atalho                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Add. ComplexProperty. ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. addenumtype**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Association**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexProperty**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. complexType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. FunctionImport**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Inheritance**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. NavigationProperty**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Close**                                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. CollapseAll**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. ExpandAll**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. ExportasImage**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. LayoutDiagram**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Edit**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. EntityKey**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Expand**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. addgrid**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. Down**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. Down5**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. toBottom**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. ToTop**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. up**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Moveproperties. Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Open**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Refactor. MovetoNewComplexType**           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Refactor. Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Rename**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat. DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. BaseType**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Property**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. subtipo**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Validate**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 10**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 100**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 125**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 150**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 200**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 25**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 300**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 33**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 400**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 50**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 66**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 75**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. Custom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. zoom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomOut**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomtoFit**                          |
| **Exibir. EntityDataModelBrowser**                                                                |
| **Exibir. EntityDataModelMappingDetails**                                                         |
