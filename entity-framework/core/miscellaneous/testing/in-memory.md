---
title: Testando com inmemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416505"
---
# <a name="testing-with-inmemory"></a>Testar com InMemory

O provedor de inmemory é útil quando você deseja testar os componentes usando algo que aproxima a conexão com o banco de dados real, sem a sobrecarga das operações reais do banco de dados.

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) deste artigo no GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>Inmemory não é um banco de dados relacional

Os provedores de banco de dados EF Core não precisam ser bancos de dados relacionais. A inmemory foi projetada para ser um banco de dados de uso geral para teste e não é projetada para imitar um banco de dados relacional.

Alguns exemplos disso incluem:

* A inmemory permitirá que você salve os dados que violariam as restrições de integridade referencial em um banco de dados relacional.
* Se você usar DefaultValueSql (cadeia de caracteres) para uma propriedade em seu modelo, essa será uma API de banco de dados relacional e não terá nenhum efeito durante a execução na inmemory.
* Não há suporte para a [simultaneidade via versão de carimbo de data/hora/linha](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` ou `IsRowVersion`). Nenhum [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) será gerado se uma atualização for feita usando um token de simultaneidade antigo.

> [!TIP]  
> Para muitos fins de teste, essas diferenças não importam. No entanto, se você quiser testar algo que se comporta mais como um banco de dados relacional verdadeiro, considere o uso [do SQLite no modo de memória](sqlite.md).

## <a name="example-testing-scenario"></a>Cenário de teste de exemplo

Considere o seguinte serviço que permite que o código do aplicativo execute algumas operações relacionadas a Blogs. Internamente, ele usa um `DbContext` que se conecta a um banco de dados SQL Server. Seria útil trocar esse contexto para se conectar a um banco de dados de inmemory, de forma que possamos escrever testes eficientes para esse serviço sem precisar modificar o código ou fazer muito trabalho para criar um duplo de teste do contexto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Prepare seu contexto

### <a name="avoid-configuring-two-database-providers"></a>Evite configurar dois provedores de banco de dados

Em seus testes, você vai configurar externamente o contexto para usar o provedor de inmemory. Se você estiver configurando um provedor de banco de dados substituindo `OnConfiguring` em seu contexto, será necessário adicionar algum código condicional para garantir que você só configure o provedor de banco de dados se ainda não tiver sido configurado.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se você estiver usando ASP.NET Core, não deverá precisar desse código, pois o provedor de banco de dados já está configurado fora do contexto (em Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Adicionar um construtor para teste

A maneira mais simples de habilitar os testes em relação a um banco de dados diferente é modificar o contexto para expor um construtor que aceite um `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` informa ao contexto todas as suas configurações, como a qual banco de dados se conectar. Esse é o mesmo objeto criado com a execução do método onconfiguring em seu contexto.

## <a name="writing-tests"></a>Escrevendo testes

A chave para testar esse provedor é a capacidade de informar ao contexto para usar o provedor de inmemory e controlar o escopo do banco de dados na memória. Normalmente, você deseja um banco de dados limpo para cada método de teste.

Aqui está um exemplo de uma classe de teste que usa o banco de dados de inmemory. Cada método de teste especifica um nome de banco de dados exclusivo, o que significa que cada método tem seu próprio banco de dados de inmemory.

>[!TIP]
> Para usar o método de extensão `.UseInMemoryDatabase()`, faça referência ao pacote NuGet [Microsoft. EntityFrameworkCore. inmemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
