---
title: Testar com SQLite – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562527"
---
# <a name="testing-with-sqlite"></a>Testar com SQLite

SQLite tem um modo na memória que permite que você use o SQLite para escrever testes em relação a um banco de dados relacional, sem a sobrecarga de operações de banco de dados real.

> [!TIP]  
> Você pode exibir este artigo [amostra](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) no GitHub

## <a name="example-testing-scenario"></a>Cenário de teste de exemplo

Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas a blogs. Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server. Seria útil trocar neste contexto, como se conectar ao banco de dados SQLite na memória para que podemos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um teste duplo do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Em seus testes você pretende configurar o contexto para usar o provedor InMemory externamente. Se você estiver configurando um provedor de banco de dados por meio da substituição `OnConfiguring` em seu contexto, em seguida, você precisará adicionar algum código condicional para garantir que apenas configurar o provedor de banco de dados se um já não tiver sido configurado.

> [!TIP]  
> Se você estiver usando o ASP.NET Core, em seguida, você não deve precisar esse código, pois o provedor de banco de dados estiver configurado fora do contexto (em Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Adicione um construtor para teste

A maneira mais simples de habilitar o teste em relação a outro banco de dados é modificar o contexto para expor um construtor que aceita um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informa o contexto de todas as suas configurações, como qual banco de dados para se conectar ao. Isso é o mesmo objeto que é criado, executando o método OnConfiguring em seu contexto.

## <a name="writing-tests"></a>Escrever testes

A chave para teste com esse provedor é a capacidade de informar o contexto para usar o SQLite e controlar o escopo do banco de dados na memória. O escopo do banco de dados é controlado pelo abrindo e fechando a conexão. O banco de dados está no escopo para a duração em que a conexão está aberta. Normalmente você deseja limpar um banco de dados para cada método de teste.

>[!TIP]
> Para usar `SqliteConnection()` e o `.UseSqlite()` método de extensão, o pacote NuGet de referência [entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
