---
title: Relações - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490624"
---
# <a name="relationships---ef-designer"></a>Relações - EF Designer
> [!NOTE]
> Esta página fornece informações sobre como configurar as relações no seu modelo usando o EF Designer. Para obter informações gerais sobre as relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).

Associações de definem relações entre os tipos de entidade em um modelo. Este tópico mostra como mapear as associações com o Entity Framework Designer (Designer de EF). A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando você compila o modelo conceitual, os avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros. Você pode ignorar esses avisos porque depois que você optar por gerar o banco de dados do modelo, os erros serão eliminados.

## <a name="associations-overview"></a>Visão geral de associações

Ao projetar seu modelo usando o EF Designer, um arquivo. edmx representa seu modelo. No arquivo. edmx, uma **associação** elemento define uma relação entre dois tipos de entidade. Uma associação deve especificar os tipos de entidade envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como a multiplicidade. A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (entre 0 e 1) ou muitas (\*). Essas informações são especificadas em dois filhos **final** elementos.

Em tempo de execução, instâncias do tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras (se você optar por expor as chaves estrangeiras nas suas entidades). Com chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um **ReferentialConstraint** elemento (um elemento filho do **associação** elemento). É recomendável que você exponha sempre chaves estrangeiras para relações nas suas entidades.

> [!NOTE]
> Em muitos-para-muitos (\*:\*) você não pode adicionar chaves estrangeiras para as entidades. Em um \*:\* relação, as informações de associação é gerenciada com um objeto independente.

Para obter informações sobre os elementos CSDL (**ReferentialConstraint**, **associação**, etc.) consulte o [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Criar e excluir associações

Criando uma associação com as atualizações do Designer de EF com o conteúdo do modelo do arquivo. edmx. Depois de criar uma associação, você deve criar os mapeamentos para a associação (discutido posteriormente neste tópico).

> [!NOTE]
> Esta seção pressupõe que você já adicionou as entidades que você deseja criar uma associação entre a seu modelo.

### <a name="to-create-an-association"></a>Para criar uma associação

1.  Clique em uma área vazia da superfície de design, aponte para **adicionar novo**e selecione **associação...** .
2.  Preencha as configurações para a associação na **Adicionar associação** caixa de diálogo.

    ![Adicionar associação](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Você pode optar por não adicionar propriedades de navegação ou propriedades de chave estrangeira para as entidades nas extremidades da associação, limpando o * * propriedade de navegação * * e * * adicionar propriedades de chave estrangeira para a &lt;nome do tipo de entidade&gt; entidade * * caixas de seleção. Se você adicionar apenas uma propriedade de navegação, a associação será navegável em uma única direção. Se você não adicionar nenhuma propriedade de navegação, você deve optar por adicionar propriedades de chave estrangeira para acessar as entidades nas extremidades da associação.
    
3.  Clique em **OK**.

### <a name="to-delete-an-association"></a>Para excluir uma associação

Para excluir uma associação faça o seguinte:

-   Clique com botão direito a associação no EF Designer na superfície e selecione **excluir**.

- OU-

-   Selecione uma ou mais associações e pressione a tecla DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Incluir propriedades de chave estrangeira nas suas entidades (restrições referenciais)

É recomendável que você exponha sempre chaves estrangeiras para relações nas suas entidades. Entity Framework usa uma restrição referencial para identificar que uma propriedade atua como a chave estrangeira para uma relação.

Se você tiver marcado a ***adicionar propriedades de chave estrangeira para a &lt;nome do tipo de entidade&gt; entidade*** caixa de seleção ao criar uma relação, essa restrição referencial foi adicionada para você.

Quando você usa o EF Designer para adicionar ou editar uma restrição referencial, o EF Designer adiciona ou modifica um **ReferentialConstraint** elemento no conteúdo do arquivo. edmx CSDL.

-   Clique duas vezes a associação que você deseja editar.
    O **restrição referencial** caixa de diálogo é exibida.
-   Dos **Principal** lista suspensa, selecione a entidade principal na restrição referencial.
    Propriedades de chave da entidade são adicionadas para o **chave da entidade** lista na caixa de diálogo.
-   Dos **dependentes** lista suspensa, selecione a entidade dependente na restrição referencial.
-   Para cada chave de entidade que tem uma chave dependente, selecione uma chave de dependente correspondente nas listas suspensos a **chave dependente** coluna.

    ![Restrição ref](~/ef6/media/refconstraint.png)

-   Clique em **OK**.

## <a name="create-and-edit-association-mappings"></a>Criar e editar os mapeamentos de associação

Você pode especificar como uma associação é mapeada para o banco de dados do **Mapping Details** janela do Designer de EF.

> [!NOTE]
> Você só pode mapear os detalhes para as associações que não têm uma restrição referencial especificada. Se uma restrição referencial for especificada, em seguida, uma propriedade de chave estrangeira está incluída na entidade e você pode usar os detalhes de mapeamento para a entidade para controlar qual coluna de chave estrangeira é mapeado para.

### <a name="create-an-association-mapping"></a>Criar um mapeamento de associação

-   Com uma associação na superfície do design e selecione o botão direito **mapeamento de tabela**.
    Isso exibe o mapeamento de associação na **detalhes de mapeamento** janela.
-   Clique em **adicionar uma tabela ou exibição**.
    É exibida uma lista suspensa que inclui todas as tabelas no modelo de armazenamento.
-   Selecione a tabela à qual a associação será mapeado.
    O **Mapping Details** janela exibe ambas as extremidades de associação e as propriedades de chave para o tipo de entidade em cada **final**.
-   Para cada propriedade de chave, clique o **coluna** campo e, em seguida, selecione a coluna à qual a propriedade será mapeado.

    ![Detalhes do mapeamento de 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Editar um mapeamento de associação

-   Com uma associação na superfície do design e selecione o botão direito **mapeamento de tabela**.
    Isso exibe o mapeamento de associação na **detalhes de mapeamento** janela.
-   Clique em **mapeia para &lt;nome da tabela&gt;**.
    É exibida uma lista suspensa que inclui todas as tabelas no modelo de armazenamento.
-   Selecione a tabela à qual a associação será mapeado.
    O **Mapping Details** janela exibe ambas as extremidades de associação e as propriedades de chave para o tipo de entidade em cada extremidade.
-   Para cada propriedade de chave, clique o **coluna** campo e, em seguida, selecione a coluna à qual a propriedade será mapeado.

## <a name="edit-and-delete-navigation-properties"></a>Editar e excluir propriedades de navegação

Propriedades de navegação são propriedades de atalho que são usadas para localizar as entidades nas extremidades de uma associação em um modelo. Propriedades de navegação podem ser criadas quando você cria uma associação entre dois tipos de entidade.

#### <a name="to-edit-navigation-properties"></a>Para editar as propriedades de navegação

-   Selecione uma propriedade de navegação na superfície de Designer de EF.
    Informações sobre a propriedade de navegação são exibidas no Visual Studio **propriedades** janela.
-   Alterar as configurações de propriedade na **propriedades** janela.

#### <a name="to-delete-navigation-properties"></a>Para excluir as propriedades de navegação

-   Se as chaves estrangeiras não são expostas nos tipos de entidade no modelo conceitual, a exclusão de uma propriedade de navegação pode tornar a associação correspondente navegável apenas em uma direção ou não navegável em todos os.
-   Clique com botão direito no Designer de EF superfície e selecione uma propriedade de navegação **excluir**.
