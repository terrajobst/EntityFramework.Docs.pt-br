---
title: Portando de EF6 EF núcleo - portando um modelo baseado em EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portabilidade de um modelo de baseado em EDMX EF6 EF Core

EF Core não suporta o formato do arquivo EDMX para modelos. É a melhor opção para a porta esses modelos gerar um novo modelo com base em código do banco de dados para seu aplicativo.

## <a name="install-ef-core-nuget-packages"></a>Instalar os pacotes NuGet do núcleo de EF

Instalar o `Microsoft.EntityFrameworkCore.Tools` pacote NuGet.

## <a name="regenerate-the-model"></a>Gerar novamente o modelo

Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.

Execute o seguinte comando no Console do Gerenciador de pacotes (Ferramentas –> NuGet Package Manager –> Package Manager Console). Consulte [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opções de comando Scaffold um subconjunto de tabelas etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por exemplo, aqui está o comando Scaffold um modelo do banco de dados de blogs em sua instância de LocalDB do SQL Server.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Remover modelo EF6

Agora, você deverá remover o modelo de EF6 do seu aplicativo.

Há problema em manter o pacote do NuGet EF6 (EntityFramework) instalado, como EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo. No entanto, se você não pretender usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.

## <a name="update-your-code"></a>Atualize seu código

Neste ponto, é uma questão de endereçamento erros de compilação e revisão de código para ver se as alterações de comportamento entre EF6 e Core EF afetará.

## <a name="test-the-port"></a>A porta de teste

Só porque seu aplicativo é compilado, não significa-lo com êxito é movido para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento ter afetado negativamente seu aplicativo.

> [!TIP]
> Consulte [guia de Introdução ao EF Core no núcleo do ASP.NET com um banco de dados](xref:core/get-started/aspnetcore/existing-db) de uma referência adicional sobre como trabalhar com um banco de dados existente 
