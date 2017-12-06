---
title: "Migrações em ambientes de equipe - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migrações em ambientes de equipe
===============================
Ao trabalhar com migrações em ambientes de equipe, preste atenção extra para o arquivo de instantâneo do modelo. Esse arquivo pode informar se migração do companheiro mescla corretamente com o seu de se você precisa resolver um conflito, recriando a migração antes de compartilhá-lo.

<a name="merging"></a>Mesclando
-------
Quando você mescla as migrações de sua equipe, você pode receber conflitos em seu arquivo de instantâneo do modelo. Se ambas as alterações estão relacionadas, a mesclagem é tão simples e duas migrações podem coexistir. Por exemplo, você pode receber um conflito de mesclagem em que a configuração do tipo de entidade customer tem esta aparência:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Já que ambas as propriedades precisam existir no modelo final, conclua a mesclagem com a adição de ambas as propriedades. Em muitos casos, o sistema de controle de versão pode mesclar automaticamente essas alterações para você.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Nesses casos, a migração e migração do companheiro são independentes umas das outras. Desde que um deles pode ser aplicado primeiro, você não precisa fazer qualquer alteração adicional para a migração antes de compartilhá-lo com sua equipe.

<a name="resolving-conflicts"></a>A resolução de conflitos
-------------------
Às vezes você encontrar um conflito de true ao mesclar o modelo de instantâneo do modelo. Por exemplo, você e seu companheiro podem cada renomeou a mesma propriedade.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Se você encontrar esse tipo de conflito, resolvê-lo, recriando a migração. Siga estas etapas:

1. Anular a mesclagem e a reversão para o diretório de trabalho antes de mesclar
2. Remover a migração (mas manter as alterações de modelo)
3. Mesclar alterações do companheiro em seu diretório de trabalho
4. Adicionar novamente a migração

Depois de fazer isso, as dois migrações podem ser aplicadas na ordem correta. A migração é aplicada primeiro, renomear a coluna *Alias*, depois disso, a migração renomeia para *nome de usuário*.

A migração pode ser compartilhada com segurança com o restante da equipe.
