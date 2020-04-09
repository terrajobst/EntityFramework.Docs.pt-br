---
title: Portando de EF6 para EF Core - Portando um modelo baseado em EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416919"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portando um modelo baseado em EDMX EF6 para o EF Core

O EF Core não suporta o formato de arquivo EDMX para modelos. A melhor opção para portar esses modelos, é gerar um novo modelo baseado em código a partir do banco de dados para sua aplicação.

## <a name="install-ef-core-nuget-packages"></a>Instale pacotes EF Core NuGet

Instale o pacote do NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Regenerar o modelo

Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo baseado no seu banco de dados existente.

Execute o seguinte comando no Console do Gerenciador de Pacotes (Ferramentas –> NuGet Package Manager –> Console de Gerenciador de Pacotes). Consulte [O Console do Gerenciador de Pacotes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para obter opções de comando para andaime de um subconjunto de tabelas etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por exemplo, aqui está o comando para andaime um modelo do banco de dados Blogging na instância SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Remover modelo EF6

Agora você removeria o modelo EF6 da sua aplicação.

É bom deixar o pacote EF6 NuGet (EntityFramework) instalado, pois eF Core e EF6 podem ser usados lado a lado na mesma aplicação. No entanto, se você não pretende usar o EF6 em qualquer área do seu aplicativo, então a desinstalação do pacote ajudará a dar erros de compilação em pedaços de código que precisam de atenção.

## <a name="update-your-code"></a>Atualize seu código

Neste ponto, é uma questão de abordar erros de compilação e revisar código para ver se as mudanças de comportamento entre eF6 e EF Core irão impactá-lo.

## <a name="test-the-port"></a>Teste a porta

Só porque seu aplicativo compila, não significa que ele seja portado com sucesso para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tenha afetado negativamente sua aplicação.
