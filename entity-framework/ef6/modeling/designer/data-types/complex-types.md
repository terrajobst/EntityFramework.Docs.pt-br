---
title: Tipos complexos - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489902"
---
# <a name="complex-types---ef-designer"></a>Tipos complexos - EF Designer
Este tópico mostra como mapear tipos complexos com o Entity Framework Designer (Designer de EF) e como consultar entidades que contêm as propriedades de tipo complexo.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando você compila o modelo conceitual, os avisos sobre não mapeadas entidades e associações podem aparecer na lista de erros. Você pode ignorar esses avisos porque depois que você optar por gerar o banco de dados do modelo, os erros serão eliminados.

## <a name="what-is-a-complex-type"></a>O que é um tipo complexo

Tipos complexos são propriedades não escalares de tipos de entidade que permitem que propriedades escalares sejam organizadas dentro das entidades. Como as entidades, tipos complexos consistem em propriedades escalares ou outras propriedades de tipo complexo.

Quando você trabalha com objetos que representam tipos complexos, esteja ciente das seguintes opções:

-   Tipos complexos não têm chaves e, portanto, não podem existir independente. Tipos complexos podem existir somente como propriedades de tipos de entidade ou outros tipos complexos.
-   Tipos complexos não podem participar de associações e não podem conter propriedades de navegação.
-   Propriedades de tipo complexo não podem ser **nulo**. Um * * InvalidOperationException * * ocorre quando **SaveChanges** é chamado e um objeto complexo nulo é encontrado. As propriedades escalares de objetos complexos podem ser **nulo**.
-   Tipos complexos não podem herdar de outros tipos complexos.
-   Você deve definir o tipo complexo como uma **classe**. 
-   O EF detecta alterações nos membros em um objeto de tipo complexo quando **DbContext.DetectChanges** é chamado. Entity Framework chama **DetectChanges** automaticamente quando os seguintes membros são chamados: **DbSet**, **DbSet**, **DbSet.Remove**, **DbSet**, **entenderão**, **SaveChanges**, **DbContext.GetValidationErrors**, **Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refatorar as propriedades de uma entidade no novo tipo complexo

Se você já tiver uma entidade no modelo conceitual que talvez você queira refatorar algumas das propriedades em uma propriedade de tipo complexo.

Na superfície do designer, selecione um ou mais propriedades (exceto as propriedades de navegação) de uma entidade, em seguida, clique com botão direito e selecione **refatorar -&gt; mover para o novo tipo complexo**.

![Refatoração](~/ef6/media/refactor.png)

Um novo tipo complexo com as propriedades selecionadas é adicionado para o **Model Browser**. O tipo complexo recebe um nome padrão.

Uma propriedade complexa do tipo recém-criado substitui as propriedades selecionadas. Todos os mapeamentos de propriedade são preservados.

![Refatorar 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Criar um novo tipo complexo

Você também pode criar um novo tipo complexo que contém as propriedades de uma entidade existente.

Clique com botão direito do **tipos complexos** pasta no navegador de modelo, aponte para **tipo complexo de AddNew...** . Como alternativa, você pode selecionar o **tipos complexos** pasta e pressione a **inserir** em seu teclado.

![Adicionar um tipo complexo de novo](~/ef6/media/addnewcomplextype.png)

Um novo tipo complexo é adicionado à pasta com um nome padrão. Agora você pode adicionar propriedades ao tipo.

## <a name="add-properties-to-a-complex-type"></a>Adicionar propriedades a um tipo complexo

Propriedades de um tipo complexo podem ser tipos escalares ou tipos complexos existentes. No entanto, as propriedades de tipo complexo não podem ter referências circulares. Por exemplo, um tipo complexo **OnsiteCourseDetails** não pode ter uma propriedade de tipo complexo **OnsiteCourseDetails**.

Você pode adicionar uma propriedade para um tipo complexo em qualquer um dos modos listados abaixo.

-   Um tipo complexo no modelo de navegador com o botão direito, aponte para **Add**, aponte para **propriedade escalar** ou **propriedade complexa**, em seguida, selecione o tipo de propriedade desejada. Como alternativa, você pode selecionar um tipo complexo e, em seguida, pressione a **inserir** em seu teclado.  

    ![Adicionar propriedades ao tipo complexo](~/ef6/media/addpropertiestocomplextype.png)

    Uma nova propriedade é adicionada para o tipo complexo com um nome padrão.

- OU-

-   Com o botão direito em uma propriedade de entidade na **EF Designer** superfície e selecione **cópia**, em seguida, clique com botão direito no tipo complexo a **Model Browser** e selecione  **Colar**.

## <a name="rename-a-complex-type"></a>Renomear um tipo complexo

Quando você renomear um tipo complexo, todas as referências para o tipo são atualizadas em todo o projeto.

-   Lentamente clique duas vezes em um tipo complexo na **Model Browser**.
    O nome será selecionado e em modo de edição.

- OU-

-   Um tipo complexo em com o botão direito do **Model Browser** e selecione **Renomear**.

- OU-

-   Selecione um tipo complexo no modelo de navegador e pressione a tecla F2.

- OU-

-   Um tipo complexo em com o botão direito do **Model Browser** e selecione **propriedades**. Editar o nome na **propriedades** janela.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Adicionar um tipo complexo existente a uma entidade e mapeie suas propriedades para colunas de tabela

1.  Uma entidade com o botão direito, aponte para **adicionar novo**e selecione **propriedade complexa**.
    Uma propriedade de tipo complexo com um nome padrão é adicionada à entidade. Um tipo de padrão (escolhido dentre os tipos complexos existentes) é atribuído à propriedade.
2.  Atribuir o tipo desejado para a propriedade na **propriedades** janela.
    Depois de adicionar uma propriedade de tipo complexo para uma entidade, você deve mapear suas propriedades para colunas de tabela.
3.  Clique com botão direito na superfície de design ou em um tipo de entidade de **Model Browser** e selecione **mapeamentos de tabela**.
    Os mapeamentos de tabela são exibidos na **Mapping Details** janela.
4.  Expanda o **mapeia para &lt;nome da tabela&gt;**  nó.
    Um **mapeamentos de coluna** nó é exibido.
5.  Expanda o **mapeamentos de coluna** nó.
    É exibida uma lista de todas as colunas na tabela. As propriedades padrão (se houver) que mapeiam as colunas são listados sob o **propriedade/valor** título.
6.  Selecione a coluna que você deseja mapear e, em seguida, clique com botão direito correspondente **propriedade/valor** campo.
    Uma lista suspensa de todas as propriedades escalares é exibida.
7.  Selecione a propriedade apropriada.

    ![Tipo complexo de mapa](~/ef6/media/mapcomplextype.png)

8.  Repita as etapas 6 e 7 para cada coluna de tabela.

>[!NOTE]
> Para excluir um mapeamento de coluna, selecione a coluna que você deseja mapear e, em seguida, clique no **propriedade/valor** campo. Em seguida, selecione **excluir** na lista suspensa.

## <a name="map-a-function-import-to-a-complex-type"></a>Mapear uma importação de função para um tipo complexo

Importações de função são baseadas em procedimentos armazenados. Para mapear uma importação de função para um tipo complexo, as colunas retornadas pelo procedimento armazenado correspondente devem coincidir com as propriedades do tipo complexo em número e devem ter tipos de armazenamento que são compatíveis com os tipos de propriedade.

-   Clique duas vezes em uma função importada que você deseja mapear para um tipo complexo.

    ![Importações de função](~/ef6/media/functionimports.png)

-   Preencha as configurações para a nova importação de função, da seguinte maneira:
    -   Especifique o procedimento armazenado para o qual você está criando uma importação de função do **nome do procedimento armazenado** campo. Este campo é uma lista suspensa que exibe todos os procedimentos armazenados no modelo de armazenamento.
    -   Especifique o nome da função de importação na **nome da função importar** campo.
    -   Selecione **complexos** como o retorno de tipo e, em seguida, especifique o tipo de retorno complexo específico, escolhendo o tipo apropriado na lista suspensa.

        ![Editar função de importação](~/ef6/media/editfunctionimport.png)

-   Clique em **OK**.
    A entrada de importação de função é criada no modelo conceitual.

### <a name="customize-column-mapping-for-function-import"></a>Personalizar o mapeamento para a importação de função de coluna

-   A importação de função no modelo de navegador com o botão direito e selecione **Importar mapeamento de função**.
    O **detalhes de mapeamento** aparece e mostra o mapeamento padrão para a importação de função. As setas indicam os mapeamentos entre valores de coluna e valores de propriedade. Por padrão, os nomes de coluna devem para ser o mesmo que os nomes de propriedade do tipo complexo. Os nomes de coluna padrão são exibidos em texto cinza.
-   Se necessário, altere os nomes de coluna para coincidir com os nomes de coluna são retornados pelo procedimento armazenado que corresponde à importação de função.

## <a name="delete-a-complex-type"></a>Excluir um tipo complexo

Quando você exclui um tipo complexo, o tipo é excluído do modelo conceitual e mapeamentos de todas as instâncias do tipo são excluídos. No entanto, as referências para o tipo não são atualizadas. Por exemplo, se uma entidade tem uma propriedade de tipo complexo do tipo ComplexType1 e ComplexType1 for excluído na **Model Browser**, a propriedade de entidade correspondente não é atualizada. O modelo não validará porque ela contém uma entidade que faz referência a um tipo complexo excluído. Você pode atualizar ou excluir as referências a tipos complexos excluídos usando o Designer de entidade.

-   Um tipo complexo no modelo de navegador com o botão direito e selecione **excluir**.

- OU-

-   Selecione um tipo complexo no modelo de navegador e pressione a tecla Delete no teclado.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Consultar entidades que contém as propriedades de tipo complexo

O código a seguir mostra como executar uma consulta que retorna uma coleção de entidade em objetos do tipo que contêm uma propriedade de tipo complexo.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
