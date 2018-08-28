---
title: Testando com InMemory – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997886"
---
# <a name="testing-with-inmemory"></a>Testando com InMemory

O provedor InMemory é útil quando você deseja testar componentes usando algo que se aproxime da conexão ao banco de dados real, sem a sobrecarga de operações de banco de dados real.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory não é um banco de dados relacional

Provedores de banco de dados do EF Core não precisa ser bancos de dados relacionais. InMemory é projetado para ser um banco de dados de uso geral para teste e não foi projetado para simular um banco de dados relacional.

Alguns exemplos incluem:

* InMemory permitirá que você salve os dados que possam violar as restrições de integridade referencial em um banco de dados relacional.
* Se você usar DefaultValueSql(string) para uma propriedade em seu modelo, isso é uma API de banco de dados relacional e não terá efeito ao executar em InMemory.
* [Simultaneidade por meio da versão de linha/carimbo de hora](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` ou `IsRowVersion`) não tem suporte. Não [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) será lançada se uma atualização é feita usando um token de simultaneidade antigo.

> [!TIP]  
> Para muitos fins de teste, essas diferenças não serão relevante. No entanto, se você quiser testar em relação a algo que se comporta como um verdadeiro banco de dados relacional, considere usar [modo na memória do SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Cenário de teste de exemplo

Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas a blogs. Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server. Seria útil trocar neste contexto, como se conectar a um banco de dados InMemory para que podemos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um teste duplo do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Em seus testes você pretende configurar o contexto para usar o provedor InMemory externamente. Se você estiver configurando um provedor de banco de dados por meio da substituição `OnConfiguring` em seu contexto, em seguida, você precisará adicionar algum código condicional para garantir que apenas configurar o provedor de banco de dados se um já não tiver sido configurado.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se você estiver usando o ASP.NET Core, em seguida, você não deve precisar esse código, pois o provedor de banco de dados já estiver configurado fora do contexto (em Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Adicione um construtor para teste

A maneira mais simples de habilitar o teste em relação a outro banco de dados é modificar o contexto para expor um construtor que aceita um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informa o contexto de todas as suas configurações, como qual banco de dados para se conectar ao. Isso é o mesmo objeto que é criado, executando o método OnConfiguring em seu contexto.

## <a name="writing-tests"></a>Escrever testes

A chave para teste com esse provedor é a capacidade de informar o contexto para usar o provedor InMemory e controlar o escopo do banco de dados na memória. Normalmente você deseja limpar um banco de dados para cada método de teste.

Aqui está um exemplo de uma classe de teste que usa o banco de dados InMemory. Cada método de teste especifica um nome de banco de dados exclusivo, que significa que cada método possui seu próprio banco de dados InMemory.

>[!TIP]
> Para usar o `.UseInMemoryDatabase()` método de extensão, o pacote NuGet de referência `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
