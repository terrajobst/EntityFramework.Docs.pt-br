---
title: Testando com o SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417298"
---
# <a name="testing-with-sqlite"></a>Testar com SQLite

O SQLite tem um modo na memória que permite usar o SQLite para gravar testes em relação a um banco de dados relacional, sem a sobrecarga das operações reais do banco de dados.

> [!TIP]  
> Você pode exibir o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no github

## <a name="example-testing-scenario"></a>Cenário de teste de exemplo

Considere o seguinte serviço que permite que o código do aplicativo execute algumas operações relacionadas a Blogs. Internamente, ele usa um `DbContext` que se conecta a um banco de dados SQL Server. Seria útil trocar esse contexto para se conectar a um banco de dados SQLite na memória para que possamos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um duplo de teste do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Em seus testes, você vai configurar externamente o contexto para usar o provedor de inmemory. Se você estiver configurando um provedor de banco de dados substituindo `OnConfiguring` em seu contexto, será necessário adicionar algum código condicional para garantir que você só configure o provedor de banco de dados se ainda não tiver sido configurado.

> [!TIP]  
> Se você estiver usando ASP.NET Core, não deverá precisar desse código, pois o provedor de banco de dados está configurado fora do contexto (em Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Adicionar um construtor para teste

A maneira mais simples de habilitar os testes em relação a um banco de dados diferente é modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informa ao contexto todas as suas configurações, como a qual banco de dados se conectar. Esse é o mesmo objeto criado com a execução do método onconfiguring em seu contexto.

## <a name="writing-tests"></a>Escrevendo testes

A chave para testar esse provedor é a capacidade de informar ao contexto para usar o SQLite e controlar o escopo do banco de dados na memória. O escopo do banco de dados é controlado abrindo e fechando a conexão. O banco de dados tem o escopo definido para a duração em que a conexão é aberta. Normalmente, você deseja um banco de dados limpo para cada método de teste.

>[!TIP]
> Para usar `SqliteConnection()` e o método de extensão de `.UseSqlite()`, referencie o pacote NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
