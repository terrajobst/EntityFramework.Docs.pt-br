---
title: Log de alterações que afetam o provedor – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998360"
---
# <a name="provider-impacting-changes"></a>Alterações que afetam o provedor

Esta página contém links para efetuar pull de solicitações feitas no repositório do EF Core que pode exigir que os autores de outros provedores de banco de dados para reagir. A intenção é fornecer um ponto de partida para autores de provedores de banco de dados de terceiros existente ao atualizar o seu provedor para uma nova versão.

Esse log estamos começando com alterações da 2.1 para 2.2. Antes de 2.1 usamos a [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) rótulos em nossos problemas e solicitações de pull.

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>Alterações de teste

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Permitir delimitadores personalizáveis do SQL nos testes
  * Testar as alterações que permitem que comparações de ponto flutuante não restrito em BuiltInDataTypesTestBase
  * Alterações de teste que permitem testes de consulta ser reutilizado com diferentes dos delimitadores SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Adicionar testes de DbFunction para os testes de especificação relacional
  * De modo que esses testes podem ser executados em relação a todos os provedores de banco de dados
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Limpeza de teste Async
  * Remover `Wait` chamadas, desnecessários async e renomeado alguns métodos de teste
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Unificar a infraestrutura de teste do log
  * Adicionado `CreateListLoggerFactory` e removidos alguns infraestrutura de log anterior, o que exigirá provedores que usam esses testes para reagir
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Executar mais testes de consulta de forma síncrona e assíncrona
  * Nomes de teste e a fatoração foi alterado, que exigirá provedores que usam esses testes para reagir
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Renomeação navegações no modelo ComplexNavigations
  * Talvez seja necessário reagir provedores que usam esses testes
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Retornar o contexto para o pool em vez de descartar em testes funcionais
  * Essa alteração inclui certa refatoração do teste que podem exigir provedores reagir


#### <a name="test-and-product-code-changes"></a>Alterações de código do produto e de teste

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidar RelationalTypeMapping.Clone métodos
  * As alterações no 2.1 para a RelationalTypeMapping permitidas para uma simplificação em classes derivadas. Não acreditamos que isso estava interrompendo aos provedores, mas provedores podem tirar proveito dessa alteração no seu tipo derivado classes de mapeamento.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Consultas nomeadas ou marcadas
  * Adiciona a infraestrutura para marcação de consultas LINQ e ter essas marcas que aparecem como comentários no SQL. Isso pode exigir provedores reagir na geração de SQL.
