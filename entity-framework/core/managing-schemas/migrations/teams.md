---
title: Migrações em ambientes de equipe-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655538"
---
# <a name="migrations-in-team-environments"></a>Migrações em ambientes de equipe

Ao trabalhar com migrações em ambientes de equipe, preste atenção extra ao arquivo de instantâneo do modelo. Esse arquivo pode informar se a migração de seu colega de equipe é mesclada corretamente com sua ou se você precisa resolver um conflito recriando a migração antes de compartilhá-la.

## <a name="merging"></a>Mesclando

Ao mesclar migrações de seus colegas de equipe, você poderá obter conflitos no arquivo de instantâneo do modelo. Se ambas as alterações não estiverem relacionadas, a mesclagem será trivial e as duas migrações poderão coexistir. Por exemplo, você pode obter um conflito de mesclagem na configuração do tipo de entidade Customer que tem esta aparência:

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

Como essas duas propriedades precisam existir no modelo final, conclua a mesclagem adicionando ambas as propriedades. Em muitos casos, seu sistema de controle de versão pode mesclar automaticamente essas alterações para você.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

Nesses casos, a migração e a migração de seu colega de equipe são independentes umas das outras. Como qualquer um deles pode ser aplicado primeiro, você não precisa fazer nenhuma alteração adicional na sua migração antes de compartilhá-la com sua equipe.

## <a name="resolving-conflicts"></a>Resolvendo conflitos

Às vezes, você encontra um verdadeiro conflito ao mesclar o modelo de instantâneo do modelo. Por exemplo, você e seu colega de equipe podem ter renomeado a mesma propriedade.

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

Se você encontrar esse tipo de conflito, resolva-o recriando a migração. Siga estas etapas:

1. Anular a mesclagem e reversão para seu diretório de trabalho antes da mesclagem
2. Remova sua migração (mas mantenha suas alterações no modelo)
3. Mescle as alterações de seu colega de equipe em seu diretório de trabalho
4. Adicione novamente sua migração

Depois de fazer isso, as duas migrações podem ser aplicadas na ordem correta. A migração é aplicada primeiro, renomeando a coluna para *alias*, depois que sua migração a renomeia para *username*.

Sua migração pode ser compartilhada com segurança com o restante da equipe.
