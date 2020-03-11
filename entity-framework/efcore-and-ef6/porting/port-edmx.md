---
title: Portando de EF6 para EF Core portando um modelo baseado em EDMX – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416919"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portando um modelo baseado em EF6 EDMX para EF Core

EF Core não oferece suporte ao formato de arquivo EDMX para modelos. A melhor opção para portar esses modelos é gerar um novo modelo baseado em código a partir do banco de dados para seu aplicativo.

## <a name="install-ef-core-nuget-packages"></a>Instalar EF Core pacotes NuGet

Instale o pacote do NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Regenerar o modelo

Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.

Execute o seguinte comando no console do Gerenciador de pacotes (Ferramentas – > Gerenciador de pacotes NuGet – console do Gerenciador de pacotes do >). Consulte [console do Gerenciador de pacotes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para obter opções de comando para Scaffold um subconjunto de tabelas, etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por exemplo, aqui está o comando para Scaffold um modelo do banco de dados de Blogs em sua instância do SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Remover modelo EF6

Agora, você removeria o modelo EF6 do seu aplicativo.

É bom deixar o EntityFramework (EF6 NuGet Package) instalado, pois EF Core e EF6 podem ser usados lado a lado no mesmo aplicativo. No entanto, se você não pretende usar o EF6 em nenhuma área do seu aplicativo, a desinstalação do pacote ajudará a fornecer erros de compilação em partes de código que precisam de atenção.

## <a name="update-your-code"></a>Atualizar seu código

Neste ponto, é uma questão de resolver erros de compilação e revisar o código para ver se o comportamento é alterado entre EF6 e EF Core afetará você.

## <a name="test-the-port"></a>Testar a porta

Só porque seu aplicativo é compilado, não significa que ele foi portado com êxito para EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento afetou negativamente seu aplicativo.
