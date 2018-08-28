---
title: Portando de EF6 para EF Core – portando um modelo baseado em EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997405"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portando um modelo baseado em EDMX do EF6 para EF Core

O EF Core não suporta o formato de arquivo EDMX para modelos. A melhor opção para esses modelos, a porta é gerar um novo modelo baseado em código do banco de dados para seu aplicativo.

## <a name="install-ef-core-nuget-packages"></a>Instalar os pacotes NuGet do EF Core

Instalar o `Microsoft.EntityFrameworkCore.Tools` pacote do NuGet.

## <a name="regenerate-the-model"></a>Gerar novamente o modelo

Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.

Execute o seguinte comando no Console do Gerenciador de pacotes (ferramentas do Gerenciador de pacotes NuGet de –> –> Package Manager Console). Ver [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opções de comando para criar o scaffolding de um subconjunto de tabelas etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por exemplo, aqui está o comando para criar o scaffolding de um modelo do banco de dados de blogs em sua instância de LocalDB do SQL Server.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Remover o modelo do EF6

Agora, você deve remover o modelo do EF6 de seu aplicativo.

É suficiente deixar o pacote NuGet do EF6 (EntityFramework) instalado, como o EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo. No entanto, se você não estiver pretendendo usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.

## <a name="update-your-code"></a>Atualizar seu código

Neste ponto, é uma questão de endereçamento de erros de compilação e revisão de código para ver se as alterações de comportamento entre o EF6 e EF Core afetarão você.

## <a name="test-the-port"></a>A porta de teste

Só porque seu aplicativo for compilado, não significa que ele é movido com êxito para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tiver afetado negativamente seu aplicativo.

> [!TIP]
> Ver [Introdução ao EF Core no ASP.NET Core com o banco de dados existente](xref:core/get-started/aspnetcore/existing-db) para uma referência adicional sobre como trabalhar com um banco de dados existente 
