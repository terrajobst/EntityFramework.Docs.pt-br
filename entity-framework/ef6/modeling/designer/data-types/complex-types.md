---
title: Tipos complexos-designer de EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418647"
---
# <a name="complex-types---ef-designer"></a>Tipos complexos – designer de EF
Este tópico mostra como mapear tipos complexos com o Entity Framework Designer (EF designer) e como consultar entidades que contêm propriedades de tipo complexo.

A imagem a seguir mostra as janelas principais que são usadas ao trabalhar com o designer do EF.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando você cria o modelo conceitual, os avisos sobre entidades e associações não mapeadas podem aparecer na Lista de Erros. Você pode ignorar esses avisos porque depois de optar por gerar o banco de dados do modelo, os erros serão desaparecedos.

## <a name="what-is-a-complex-type"></a>O que é um tipo complexo

Tipos complexos são propriedades não escalares de tipos de entidade que permitem que propriedades escalares sejam organizadas dentro das entidades. Como as entidades, os tipos complexos consistem em Propriedades escalares ou outras propriedades de tipo complexo.

Quando você trabalha com objetos que representam tipos complexos, esteja atento ao seguinte:

-   Tipos complexos não têm chaves e, portanto, não podem existir de forma independente. Tipos complexos só podem existir como propriedades de tipos de entidade ou outros tipos complexos.
-   Tipos complexos não podem participar de associações e não podem conter Propriedades de navegação.
-   As propriedades de tipo complexo não podem ser **nulas**. Um **InvalidOperationException **ocorre quando **DbContext. SaveChanges** é chamado e um objeto complexo nulo é encontrado. As propriedades escalares de objetos complexos podem ser **nulas**.
-   Tipos complexos não podem herdar de outros tipos complexos.
-   Você deve definir o tipo complexo como uma **classe**. 
-   O EF detecta alterações nos membros de um objeto de tipo complexo quando **DbContext. DetectChanges** é chamado. Entity Framework chama **DetectChanges** automaticamente quando os seguintes membros são chamados: **DbSet. Find**, **DbSet. local**, **DbSet. Remove**, **DbSet. Adicionar**, **DbSet. Attach**, **DbContext. SaveChanges**, **DbContext. getvalidationerrors**, **DbContext. entry**, **DbChangeTracker. Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Refatorar as propriedades de uma entidade em um novo tipo complexo

Se você já tiver uma entidade em seu modelo conceitual, talvez queira refatorar algumas das propriedades em uma propriedade de tipo complexo.

Na superfície do designer, selecione uma ou mais Propriedades (excluindo as propriedades de navegação) de uma entidade, clique com o botão direito do mouse e selecione **refatorar-&gt; mover para o novo tipo complexo**.

![Refatoração](~/ef6/media/refactor.png)

Um novo tipo complexo com as propriedades selecionadas é adicionado ao **navegador de modelo**. O tipo complexo recebe um nome padrão.

Uma propriedade complexa do tipo recém-criado substitui as propriedades selecionadas. Todos os mapeamentos de propriedade são preservados.

![Refatorar 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Criar um novo tipo complexo

Você também pode criar um novo tipo complexo que não contenha propriedades de uma entidade existente.

Clique com o botão direito do mouse na pasta **tipos complexos** no navegador de modelos, aponte para **tipo complexo AddNew...** . Como alternativa, você pode selecionar a pasta **tipos complexos** e pressionar a tecla **Insert** no teclado.

![Adicionar novo tipo complexo](~/ef6/media/addnewcomplextype.png)

Um novo tipo complexo é adicionado à pasta com um nome padrão. Agora você pode adicionar propriedades ao tipo.

## <a name="add-properties-to-a-complex-type"></a>Adicionar propriedades a um tipo complexo

As propriedades de um tipo complexo podem ser tipos escalares ou tipos complexos existentes. No entanto, as propriedades de tipo complexo não podem ter referências circulares. Por exemplo, um tipo complexo **OnsiteCourseDetails** não pode ter uma propriedade de tipo complexo **OnsiteCourseDetails**.

Você pode adicionar uma propriedade a um tipo complexo em qualquer uma das maneiras listadas abaixo.

-   Clique com o botão direito do mouse em um tipo complexo no navegador de modelos, aponte para **Adicionar**, aponte para **Propriedade escalar** ou **Propriedade complexa**e selecione o tipo de propriedade desejado. Como alternativa, você pode selecionar um tipo complexo e pressionar a tecla **Insert** no teclado.  

    ![Adicionar propriedades ao tipo complexo](~/ef6/media/addpropertiestocomplextype.png)

    Uma nova propriedade é adicionada ao tipo complexo com um nome padrão.

- OU –

-   Clique com o botão direito do mouse em uma propriedade de entidade na superfície do **designer do EF** e selecione **copiar**, clique com o botão direito do mouse no tipo complexo no **navegador modelo** e selecione **colar**.

## <a name="rename-a-complex-type"></a>Renomear um tipo complexo

Quando você renomeia um tipo complexo, todas as referências ao tipo são atualizadas em todo o projeto.

-   Clique duas vezes em um tipo complexo no **navegador de modelos**.
    O nome será selecionado e no modo de edição.

- OU –

-   Clique com o botão direito do mouse em um tipo complexo no **navegador de modelos** e selecione **renomear**.

- OU –

-   Selecione um tipo complexo no navegador de modelos e pressione a tecla F2.

- OU –

-   Clique com o botão direito do mouse em um tipo complexo no **navegador de modelos** e selecione **Propriedades**. Edite o nome na janela **propriedades** .

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Adicionar um tipo complexo existente a uma entidade e mapear suas propriedades para colunas de tabela

1.  Clique com o botão direito do mouse em uma entidade, aponte para **Adicionar nova**e selecione **Propriedade complexa**.
    Uma propriedade de tipo complexo com um nome padrão é adicionada à entidade. Um tipo padrão (escolhido dos tipos complexos existentes) é atribuído à propriedade.
2.  Atribua o tipo desejado à propriedade na janela de **Propriedades** .
    Depois de adicionar uma propriedade de tipo complexo a uma entidade, você deve mapear suas propriedades para colunas de tabela.
3.  Clique com o botão direito do mouse em um tipo de entidade na superfície de design ou no **navegador de modelos** e selecione **mapeamentos de tabela**.
    Os mapeamentos de tabela são exibidos na janela **detalhes do mapeamento** .
4.  Expanda os **mapas para &lt;nome da tabela&gt;** nó .
    Um nó **mapeamentos de coluna** é exibido.
5.  Expanda os **mapeamentos de coluna** nó.
    É exibida uma lista de todas as colunas na tabela. As propriedades padrão (se houver) às quais o mapa de colunas são listadas no título **valor/propriedade** .
6.  Selecione a coluna que você deseja mapear e clique com o botão direito do mouse no campo **valor/propriedade** correspondente.
    Uma lista suspensa de todas as propriedades escalares é exibida.
7.  Selecione a propriedade apropriada.

    ![Mapear tipo complexo](~/ef6/media/mapcomplextype.png)

8.  Repita as etapas 6 e 7 para cada coluna de tabela.

>[!NOTE]
> Para excluir um mapeamento de coluna, selecione a coluna que você deseja mapear e, em seguida, clique no campo **valor/propriedade** . Em seguida, selecione **excluir** na lista suspensa.

## <a name="map-a-function-import-to-a-complex-type"></a>Mapear uma importação de função para um tipo complexo

As importações de função são baseadas em procedimentos armazenados. Para mapear uma importação de função para um tipo complexo, as colunas retornadas pelo procedimento armazenado correspondente devem corresponder às propriedades do tipo complexo em número e devem ter tipos de armazenamento compatíveis com os tipos de propriedade.

-   Clique duas vezes em uma função importada que você deseja mapear para um tipo complexo.

    ![Importações de função](~/ef6/media/functionimports.png)

-   Preencha as configurações para a nova importação de função, da seguinte maneira:
    -   Especifique o procedimento armazenado para o qual você está criando uma importação de função no campo **nome do procedimento armazenado** . Esse campo é uma lista suspensa que exibe todos os procedimentos armazenados no modelo de armazenamento.
    -   Especifique o nome da função de importação no campo **nome da importação de função** .
    -   Selecione  **complexas** como o tipo de retorno e, em seguida, especifique o tipo de retorno complexo específico escolhendo o tipo apropriado na lista suspensa.

        ![Editar importação de função](~/ef6/media/editfunctionimport.png)

-   Clique em **OK**.
    A entrada de importação de função é criada no modelo conceitual.

### <a name="customize-column-mapping-for-function-import"></a>Personalizar o mapeamento de coluna para importação de função

-   Clique com o botão direito do mouse na função importar no navegador de modelos e selecione **função importar mapeamento**.
    A janela **detalhes do mapeamento** é exibida e mostra o mapeamento padrão para a importação de função. As setas indicam os mapeamentos entre valores de coluna e valores de propriedade. Por padrão, supõe-se que os nomes de coluna sejam iguais aos nomes de Propriedade do tipo complexo. Os nomes de coluna padrão aparecem em texto cinza.
-   Se necessário, altere os nomes de coluna para coincidir com os nomes de coluna retornados pelo procedimento armazenado que corresponde à importação de função.

## <a name="delete-a-complex-type"></a>Excluir um tipo complexo

Quando você exclui um tipo complexo, o tipo é excluído do modelo conceitual e os mapeamentos para todas as instâncias do tipo são excluídos. No entanto, as referências ao tipo não são atualizadas. Por exemplo, se uma entidade tiver uma propriedade de tipo complexo do tipo ComplexType1 e ComplexType1 for excluído no **navegador de modelo**, a propriedade de entidade correspondente não será atualizada. O modelo não será validado porque contém uma entidade que faz referência a um tipo complexo excluído. Você pode atualizar ou excluir referências para tipos complexos excluídos usando o Entity Designer.

-   Clique com o botão direito do mouse em um tipo complexo no navegador de modelos e selecione **excluir**.

- OU –

-   Selecione um tipo complexo no navegador de modelos e pressione a tecla Delete no teclado.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Consulta de entidades que contêm propriedades de tipo complexo

O código a seguir mostra como executar uma consulta que retorna uma coleção de objetos de tipo entidade que contêm uma propriedade de tipo complexo.

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
