---
title: O que há de novo no EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052028"
---
# <a name="whats-new-in-ef-core-50"></a>O que há de novo no EF Core 5,0

O EF Core 5,0 está em desenvolvimento no momento.
Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.
A primeira visualização do EF Core 5,0 é provisoriamente esperada no primeiro trimestre de 2020.

Esta página não duplica o [plano para EF Core 5,0](plan.md).
O plano descreve os temas gerais para EF Core 5,0, incluindo tudo o que estamos planejando incluir antes de enviar a versão final.

Vamos adicionar links daqui à documentação oficial conforme ele é publicado.

## <a name="preview-1-not-yet-shipped"></a>Versão prévia 1 (ainda não enviada)

### <a name="simple-logging"></a>Log simples

Esse recurso adiciona uma funcionalidade semelhante à `Database.Log` em EF6.
Ou seja, ele fornece uma maneira simples de obter logs de EF Core sem a necessidade de configurar qualquer tipo de estrutura de log externa.

A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é acompanhada pelo problema [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Maneira simples de obter o SQL gerado

EF Core 5,0 apresenta o método de extensão `ToQueryString`, que retornará o SQL que EF Core gerará ao executar uma consulta LINQ.

A documentação preliminar está incluída no [status semanal do EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

A documentação adicional é acompanhada pelo problema [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).

### <a name="enhanced-debug-views"></a>Exibições de depuração aprimoradas

As exibições de depuração são uma maneira fácil de examinar os elementos internos de EF Core ao depurar problemas.
Uma exibição de depuração para o modelo foi implementada há algum tempo.
Para EF Core 5,0, tornamos a exibição de modelo mais fácil de ler e adicionamos uma nova exibição de depuração para entidades rastreadas no Gerenciador de estado.

A documentação preliminar está incluída no [status semanal do EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

A documentação adicional é acompanhada pelo problema [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>A conexão ou a cadeia de conexão pode ser alterada no DbContext inicializado

Agora é mais fácil criar uma instância DbContext sem nenhuma conexão ou cadeia de conexão.
Além disso, a conexão ou a cadeia de conexão agora pode ser modificada na instância de contexto.
Isso permite que a mesma instância de contexto se conecte dinamicamente a bancos de dados diferentes.

A documentação é controlada pelo problema [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxies de controle de alterações

EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Em seguida, eles relatam alterações de valor nas propriedades de entidade diretamente em EF Core, evitando a necessidade de verificar se há alterações.
No entanto, os proxies vêm com seu próprio conjunto de limitações, para que eles não sejam para todos.

A documentação é controlada pelo problema [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).

### <a name="improved-handling-of-database-null-semantics"></a>Tratamento aprimorado da semântica nula do banco de dados

Os bancos de dados relacionais normalmente tratam nulo como um valor desconhecido e, portanto, não são iguais a nenhum outro nulo.
C#, por outro lado, trata NULL como um valor definido que compara igual a qualquer outro nulo.
EF Core, por padrão, traduz as consultas para que elas C# usem a semântica nula.
EF Core 5,0 melhora muito a eficiência dessas traduções.

A documentação é controlada pelo problema [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Propriedades do indexador

EF Core 5,0 dá suporte ao C# mapeamento de propriedades do indexador.
Isso permite que as entidades atuem como pacotes de propriedades, em que as colunas são mapeadas para as propriedades nomeadas na bolsa.

A documentação é controlada pelo problema [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Geração de restrições de verificação para mapeamentos de enumeração

EF Core migrações 5,0 agora podem gerar restrições de verificação para mapeamentos de propriedade de enumeração.
Por exemplo:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

A documentação é controlada pelo problema [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).

### <a name="query-translations-for-more-datetime-constructs"></a>Conversões de consulta para mais construções DateTime

As consultas que contêm a nova construção DataTime agora estão traduzidas.
Além disso, a função SQL Server DateDiffWeek agora está mapeada.

A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Conversões de consulta para mais constructos de matriz de bytes

As consultas que usam Contains, Length, SequenceEqual, etc. nas propriedades byte [] agora são convertidas em SQL.

A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é acompanhada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Conversão de consulta para reversa

As consultas que usam `Reverse` agora são traduzidas.
Por exemplo:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Conversão de consulta para operadores bit a bit

Agora, as consultas que usam operadores de bit que são são traduzidas em mais casos, por exemplo:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Conversão de consulta para cadeias de caracteres em Cosmos

As consultas que usam os métodos de cadeia de caracteres contém, StartsWith e EndsWith agora são traduzidas ao usar o provedor de Azure Cosmos DB.

A documentação é controlada pelo problema [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).
