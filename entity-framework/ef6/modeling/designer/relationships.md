---
title: Relações-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418241"
---
# <a name="relationships---ef-designer"></a>Relações-designer do EF
> [!NOTE]
> Esta página fornece informações sobre como configurar relações em seu modelo usando o designer do EF. Para obter informações gerais sobre relações no EF e como acessar e manipular dados usando relações, consulte [relações & Propriedades de navegação](~/ef6/fundamentals/relationships.md).

As associações definem as relações entre os tipos de entidade em um modelo. Este tópico mostra como mapear associações com o Entity Framework Designer (EF designer). A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando você cria o modelo conceitual, os avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros. Você pode ignorar esses avisos porque depois de optar por gerar o banco de dados do modelo, os erros serão desaparecedos.

## <a name="associations-overview"></a>Visão geral de associações

Quando você cria seu modelo usando o designer do EF, um arquivo. edmx representa seu modelo. No arquivo. edmx, um elemento **Association** define uma relação entre dois tipos de entidade. Uma associação deve especificar os tipos de entidade que estão envolvidos na relação e o número possível de tipos de entidade em cada extremidade da relação, que é conhecida como multiplicidade. A multiplicidade de uma extremidade de associação pode ter um valor de um (1), zero ou um (0.. 1) ou muitos (\*). Essas informações são especificadas em dois elementos **finais** filho.

Em tempo de execução, as instâncias de tipo de entidade em uma extremidade de uma associação podem ser acessadas por meio de propriedades de navegação ou chaves estrangeiras (se você optar por expor chaves estrangeiras em suas entidades). Com as chaves estrangeiras expostas, a relação entre as entidades é gerenciada com um elemento **ReferentialConstraint** (um elemento filho do elemento **Association** ). É recomendável que você sempre exponha chaves estrangeiras para relações em suas entidades.

> [!NOTE]
> Em muitos para muitos (\*:\*), não é possível adicionar chaves estrangeiras às entidades. Em uma relação \*:\*, as informações de associação são gerenciadas com um objeto independente.

Para obter informações sobre elementos CSDL (**ReferentialConstraint**, **Association**, etc.), consulte a [especificação CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Criar e excluir associações

A criação de uma associação com o designer do EF atualiza o conteúdo do modelo do arquivo. edmx. Depois de criar uma associação, você deve criar os mapeamentos para a associação (discutido posteriormente neste tópico).

> [!NOTE]
> Esta seção pressupõe que você já adicionou as entidades para as quais deseja criar uma associação entre o modelo.

### <a name="to-create-an-association"></a>Para criar uma associação

1.  Clique com o botão direito do mouse em uma área vazia da superfície de design, aponte para **Adicionar nova**e selecione **associação...** .
2.  Preencha as configurações da associação na caixa de diálogo **Adicionar Associação** .

    ![Adicionar Associação](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Você pode optar por não adicionar propriedades de navegação ou propriedades de chave estrangeira às entidades nas extremidades da associação, desmarcando a **propriedade de navegação **e **Adicionar propriedades de chave estrangeira ao &lt;nome do tipo de entidade&gt; **caixas de seleção de entidade. Se você adicionar apenas uma propriedade de navegação, a associação será percorrida em apenas uma direção. Se você não adicionar nenhuma propriedade de navegação, deverá optar por adicionar propriedades de chave estrangeira para acessar entidades nas extremidades da associação.
    
3.  Clique em **OK**.

### <a name="to-delete-an-association"></a>Para excluir uma associação

Para excluir uma associação, siga um destes procedimentos:

-   Clique com o botão direito do mouse na associação na superfície do designer do EF e selecione **excluir**.

- OU –

-   Selecione uma ou mais associações e pressione a tecla DELETE.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Incluir propriedades de chave estrangeira em suas entidades (restrições referenciais)

É recomendável que você sempre exponha chaves estrangeiras para relações em suas entidades. Entity Framework usa uma restrição referencial para identificar que uma propriedade age como a chave estrangeira para uma relação.

Se você marcou a caixa de seleção ***Adicionar propriedades de chave estrangeira ao nome do tipo de entidade &lt;&gt; entidade*** ao criar uma relação, essa restrição referencial foi adicionada para você.

Quando você usa o designer do EF para adicionar ou editar uma restrição referencial, o designer do EF adiciona ou modifica um elemento **ReferentialConstraint** no conteúdo CSDL do arquivo. edmx.

-   Clique duas vezes na associação que você deseja editar.
    A caixa de diálogo de **restrição referencial** é exibida.
-   Na lista suspensa  **principal** , selecione a entidade principal na restrição referencial.
    As propriedades de chave da entidade são adicionadas à lista  **chave principal** na caixa de diálogo.
-   Na lista suspensa  **dependente** , selecione a entidade dependente na restrição referencial.
-   Para cada chave principal que tenha uma chave dependente, selecione uma chave dependente correspondente nas listas suspensas na coluna **chave dependente** .

    ![Restrição REF](~/ef6/media/refconstraint.png)

-   Clique em **OK**.

## <a name="create-and-edit-association-mappings"></a>Criar e editar mapeamentos de associação

Você pode especificar como uma associação é mapeada para o banco de dados na janela **detalhes do mapeamento** do designer do EF.

> [!NOTE]
> Você só pode mapear detalhes para as associações que não têm uma restrição referencial especificada. Se uma restrição referencial for especificada, uma propriedade de chave estrangeira será incluída na entidade e você poderá usar os detalhes de mapeamento para a entidade para controlar a qual coluna a chave estrangeira é mapeada.

### <a name="create-an-association-mapping"></a>Criar um mapeamento de associação

-   Clique com o botão direito do mouse em uma associação na superfície de design e selecione **mapeamento de tabela**.
    Isso exibe o mapeamento de associação na janela **detalhes do mapeamento** .
-   Clique em **Adicionar uma tabela ou exibição**.
    Uma lista suspensa é exibida, incluindo todas as tabelas no modelo de armazenamento.
-   Selecione a tabela na qual a associação será mapeada.
    A janela **detalhes do mapeamento** exibe as extremidades da associação e as propriedades de chave para o tipo de entidade em cada **extremidade**.
-   Para cada propriedade de chave, clique na **coluna** campo e selecione a coluna na qual a propriedade será mapeada.

    ![Detalhes do mapeamento 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Editar um mapeamento de associação

-   Clique com o botão direito do mouse em uma associação na superfície de design e selecione **mapeamento de tabela**.
    Isso exibe o mapeamento de associação na janela **detalhes do mapeamento** .
-   Clique em **mapas para &lt;nome da tabela&gt;** .
    Uma lista suspensa é exibida, incluindo todas as tabelas no modelo de armazenamento.
-   Selecione a tabela na qual a associação será mapeada.
    A janela **detalhes do mapeamento** exibe as extremidades da associação e as propriedades de chave para o tipo de entidade em cada extremidade.
-   Para cada propriedade de chave, clique na **coluna** campo e selecione a coluna na qual a propriedade será mapeada.

## <a name="edit-and-delete-navigation-properties"></a>Editar e excluir propriedades de navegação

As propriedades de navegação são propriedades de atalho que são usadas para localizar as entidades nas extremidades de uma associação em um modelo. As propriedades de navegação podem ser criadas quando você cria uma associação entre dois tipos de entidade.

#### <a name="to-edit-navigation-properties"></a>Para editar propriedades de navegação

-   Selecione uma propriedade de navegação na superfície do designer do EF.
    As informações sobre a propriedade de navegação são exibidas na janela de de **Propriedades** do Visual Studio.
-   Altere as configurações de propriedade na janela **propriedades** .

#### <a name="to-delete-navigation-properties"></a>Para excluir propriedades de navegação

-   Se as chaves estrangeiras não forem expostas em tipos de entidade no modelo conceitual, a exclusão de uma propriedade de navegação poderá tornar a passagem de associação correspondente em apenas uma direção ou não pode ser atravessada.
-   Clique com o botão direito do mouse em uma propriedade de navegação na superfície do designer do EF e selecione **excluir**.
