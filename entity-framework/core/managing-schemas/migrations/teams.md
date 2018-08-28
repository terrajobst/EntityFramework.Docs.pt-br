---
title: Migrações em ambientes de equipe – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997689"
---
<a name="migrations-in-team-environments"></a>Migrações em ambientes de equipe
===============================
Ao trabalhar com migrações em ambientes de equipe, preste atenção extra ao arquivo de instantâneo do modelo. Esse arquivo pode informar se a migração do seu colega de equipe mescla corretamente com o seu ou se você precisar resolver um conflito, recriando a migração antes de compartilhá-lo.

<a name="merging"></a>Mesclando
-------
Quando você mescla as migrações de seus colegas de equipe, você pode obter conflitos em seu arquivo de instantâneo do modelo. Se ambas as alterações não estão relacionadas, a mesclagem é trivial e duas migrações podem coexistir. Por exemplo, você pode obter um conflito de mesclagem em que a configuração do tipo de entidade customer tem esta aparência:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Uma vez que ambas as propriedades precisam existir no modelo final, conclua a mesclagem, adicionando ambas as propriedades. Em muitos casos, o sistema de controle de versão pode mesclar essas alterações automaticamente para você.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Nesses casos, sua migração e a migração do seu colega de equipe são independentes uns dos outros. Uma vez que qualquer um deles pode ser aplicado primeiro, você não precisa fazer nenhuma alteração adicional para a migração antes de compartilhá-lo com sua equipe.

<a name="resolving-conflicts"></a>A resolução de conflitos
-------------------
Às vezes, você encontrar um conflito de true ao mesclar o modelo de instantâneo do modelo. Por exemplo, você e seu colega de equipe talvez cada renomeou a mesma propriedade.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Se você encontrar esse tipo de conflito, resolvê-lo ao criar novamente sua migração. Siga estas etapas:

1. Anular a mesclagem e a reversão para seu diretório de trabalho antes da mesclagem
2. Remover sua migração (mas manter as alterações de modelo)
3. Mesclar alterações do seu colega de equipe em seu diretório de trabalho
4. Adicione novamente a migração

Depois de fazer isso, duas migrações podem ser aplicadas na ordem correta. Sua migração é aplicada primeiro, a coluna para a renomeação *Alias*, depois disso, sua migração renomeia-o para *nome de usuário*.

A migração pode ser compartilhada com segurança com o restante da equipe.
