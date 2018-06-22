---
title: Testando com SQLite - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052696"
---
# <a name="testing-with-sqlite"></a>Testando com SQLite

SQLite tem um modo de memória que permite que você use SQLite para escrever testes em relação a um banco de dados relacional, sem a sobrecarga de operações de banco de dados real.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) no GitHub

## <a name="example-testing-scenario"></a>Exemplo de cenário de teste

Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas à blogs. Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server. Seria útil trocar este contexto para se conectar a um banco de dados na memória SQLite para que podemos escrever testes eficientes para esse serviço sem a necessidade de modificar o código ou muito trabalho para criar um teste duplo do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Os testes que serão externamente, configurar o contexto para usar o provedor de InMemory. Se você estiver configurando um provedor de banco de dados, substituindo `OnConfiguring` em seu contexto, em seguida, você precisa adicionar código condicional para garantir que você configure apenas o provedor de banco de dados se um já não tiver sido configurado.

> [!TIP]  
> Se você estiver usando o ASP.NET Core, em seguida, não será preciso esse código como o provedor de banco de dados é configurado fora do contexto (em Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Adicione um construtor para teste

É a maneira mais simples para habilitar o teste em relação a um banco de dados diferente modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`informa o contexto de todas as suas configurações, como o banco de dados para se conectar ao. Este é o mesmo objeto que é criado ao executar o método OnConfiguring em seu contexto.

## <a name="writing-tests"></a>Testes de gravação

A chave de teste com esse provedor é a capacidade para informar o contexto para usar SQLite e controlar o escopo do banco de dados na memória. O escopo do banco de dados é controlado abrindo e fechando a conexão. O banco de dados é restrita a duração que a conexão está aberta. Normalmente você deseja limpar um banco de dados para cada método de teste.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
