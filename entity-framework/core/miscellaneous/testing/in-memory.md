---
title: Testando com InMemory - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995581"
---
# <a name="testing-with-inmemory"></a>Testando com InMemory

O provedor InMemory é útil quando você deseja testar componentes usando algo que aproxima a conexão com o banco de dados real, sem a sobrecarga de operações de banco de dados real.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory não é um banco de dados relacional

Provedores de banco de dados principal EF não precisa ser bancos de dados relacionais. InMemory é projetado para ser um banco de dados de uso geral para teste e não foi projetado para simular um banco de dados relacional.

Alguns exemplos incluem:
* InMemory permitirá salvar os dados que possam violar as restrições de integridade referencial em um banco de dados relacional.

* Se você usar DefaultValueSql(string) para uma propriedade em seu modelo, isso é uma API de banco de dados relacional e não terá efeito ao executar em InMemory.

> [!TIP]  
> Para muitos fins de teste essas diferenças não serão importante. No entanto, se você quiser testar em relação a algo que se comporta como um banco de dados relacional true, considere usar [modo na memória de SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Exemplo de cenário de teste

Considere o seguinte serviço que permite que o código do aplicativo executar algumas operações relacionadas à blogs. Ele usa internamente um `DbContext` que se conecta a um banco de dados do SQL Server. Seria útil trocar este contexto para se conectar a um banco de dados InMemory para que podemos escrever testes eficientes para esse serviço sem a necessidade de modificar o código ou muito trabalho para criar um teste duplo do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Os testes que serão externamente, configurar o contexto para usar o provedor de InMemory. Se você estiver configurando um provedor de banco de dados, substituindo `OnConfiguring` em seu contexto, em seguida, você precisa adicionar código condicional para garantir que você configure apenas o provedor de banco de dados se um já não tiver sido configurado.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se você estiver usando o ASP.NET Core, em seguida, não será preciso esse código como o provedor de banco de dados já está configurado fora do contexto (em Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Adicione um construtor para teste

É a maneira mais simples para habilitar o teste em relação a um banco de dados diferente modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`informa o contexto de todas as suas configurações, como o banco de dados para se conectar ao. Este é o mesmo objeto que é criado ao executar o método OnConfiguring em seu contexto.

## <a name="writing-tests"></a>Testes de gravação

A chave de teste com esse provedor é a capacidade para informar o contexto para usar o provedor InMemory e controlar o escopo do banco de dados na memória. Normalmente você deseja limpar um banco de dados para cada método de teste.

Aqui está um exemplo de uma classe de teste que usa o banco de dados de InMemory. Cada método de teste especifica um nome de banco de dados exclusivo, que significa que cada método tem seu próprio banco de dados de InMemory.

>[!TIP]
> Para usar o `.UseInMemoryDatabase()` método de extensão, o pacote NuGet de referência `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
