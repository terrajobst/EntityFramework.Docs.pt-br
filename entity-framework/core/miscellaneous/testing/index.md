---
title: Testar componentes usando o EF Core – EF Core
description: Abordagens diferentes para testar aplicativos que usam o EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634246"
---
# <a name="testing-code-that-uses-ef-core"></a>Testar código que usa o EF Core

Para testar um código que acessa um banco de dados, é necessário:
* Executar consultas e atualizações no mesmo sistema de banco de dados usado na produção.
* Executar consultas e atualizações em outro sistema de banco de dados mais fácil de gerenciar.
* Usar duplicatas de teste ou algum outro mecanismo para evitar por completo o uso de um banco de dados.

Este documento descreve as compensações envolvidas em cada uma dessas opções e mostra como o EF Core pode ser usado com cada abordagem.  

## <a name="all-database-providers-are-not-equal"></a>Os provedores de banco de dados não são todos iguais

É muito importante entender que o EF Core não foi projetado para abstrair todos os aspectos do sistema de banco de dados subjacente.
Em vez disso, o EF Core é um conjunto comum de padrões e conceitos que podem ser usados com qualquer sistema de banco de dados.
Os provedores de banco de dados do EF Core usam então uma funcionalidade e um comportamento específicos a um banco de dados como camadas sobre essa estrutura comum.
Isso permite que cada sistema de banco de dados seja usado para aquilo que ele faz de melhor, mantendo ainda a convergência, quando apropriado, com outros sistemas de banco de dados. 

Fundamentalmente, isso significa que a troca do provedor de banco de dados alterará o comportamento do EF Core. Além disso, não podemos esperar que o aplicativo funcione corretamente, a menos que ele leve explicitamente em conta todas as diferenças no comportamento.
Dito isso, em muitos casos, isso funcionará devido à existência de um alto grau de convergência entre bancos de dados relacionais.
Isso tem um lado bom e outro ruim.
Bom porque a movimentação entre bancos de dados pode ser relativamente fácil.
Ruim porque isso pode dar uma falsa sensação de segurança caso o aplicativo não seja totalmente testado no novo sistema de banco de dados.  

## <a name="approach-1-production-database-system"></a>Abordagem 1: Sistema de banco de dados de produção

Conforme descrito na seção anterior, a única maneira de ter certeza de que você está testando o que é executado em produção é usar o mesmo sistema de banco de dados.
Por exemplo, se o aplicativo implantado usar o SQL Azure, o teste também deverá ser feito no SQL Azure.

No entanto, fazer com que cada desenvolvedor execute testes no SQL Azure enquanto trabalha ativamente no código seria uma alternativa lenta e cara.
Isso ilustra a principal compensação envolvida em todas essas abordagens: quando é apropriado deixar de usar o sistema de banco de dados de produção a fim de melhorar a eficiência do teste?

Nesse caso, a resposta é bem fácil: use um SQL Server local para testes por desenvolvedores.
O SQL Azure e o SQL Server são extremamente semelhantes, portanto, o teste em relação ao SQL Server geralmente é uma compensação razoável.
Dito isso, ainda é inteligente executar testes no SQL Azure antes de entrar em um cenário de produção.
 
### <a name="localdb"></a>LocalDb 

Todos os principais sistemas de banco de dados têm alguma forma de "Developer Edition" para testes locais.
O SQL Server também tem um recurso chamado [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).
A principal vantagem do LocalDb é que ele ativa a instância do banco de dados sob demanda.
Isso evita a execução de um serviço de banco de dados em seu computador mesmo quando você não está executando testes.

O LocalDb também tem suas desvantagens:
* O escopo de suporte dele é menor do que o do [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).
* Ele não está disponível no Linux.
* Isso pode causar atraso na primeira execução de teste conforme o serviço é ativado.

Pessoalmente, nunca considerei um problema ter um serviço de banco de dados em execução no meu computador de desenvolvimento e geralmente recomendo, em vez disso, o uso da Developer Edition.
No entanto, isso pode ser apropriado para algumas pessoas, especialmente em computadores de desenvolvimento menos poderosos.  

## <a name="approach-2-sqlite"></a>Abordagem 2: SQLite

O EF Core testa o provedor do SQL Server principalmente executando-o em uma instância do SQL Server local.
Esses testes executam dezenas de milhares de consultas em alguns minutos em um computador rápido.
Isso ilustra que o uso do sistema de banco de dados real pode ser uma solução de alto desempenho.
A noção de que usar um banco de dados menos poderoso é a única maneira de executar testes rapidamente é um mito.

Dito isso, e se por qualquer motivo você não puder executar testes em algo próximo ao seu sistema de banco de dados de produção?
Nesse caso, a próxima melhor opção é usar algo com funcionalidade semelhante.
Isso geralmente significa outro banco de dados relacional, para o qual o [SQLite](https://sqlite.org/index.html) é a escolha óbvia.

O SQLite é uma boa escolha porque:
* Ele é executado em processo com seu aplicativo e, portanto, tem baixa sobrecarga.
* Ele usa arquivos simples, criados automaticamente para bancos de dados e, portanto, não requer gerenciamento de banco de dados.
* Ele tem um modo na memória que evita até mesmo a criação do arquivo.

No entanto, lembre-se de que:
* O escopo de suporte do SQLite é inevitavelmente menor que o do seu sistema banco de dados de produção.
* O SQLite se comportará de maneira diferente do sistema de banco de dados de produção para algumas consultas.

Portanto, se você usar o SQLite para alguns testes, certifique-se também de testar seu sistema de banco de dados real.

Confira [Testes com o SQLite](xref:core/miscellaneous/testing/sqlite) para obter diretrizes específicas do EF Core. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Abordagem 3: Banco de dados em memória do EF Core

O EF Core vem com um banco de dados em memória que usamos para testes internos do EF Core propriamente dito.
Esse banco de dados **não é adequado, em termos gerais, como substituto para aplicativos de teste que usam o EF Core**. Especificamente:
* Ele não é um banco de dados relacional
* Ele não dá suporte a transações
* Ele não é otimizado para desempenho

Nada disso é muito importante ao testar elementos internos do EF Core, pois nós o usamos especificamente em situações nas quais o banco de dados é irrelevante para o teste.
Por outro lado, essas coisas tendem a ser muito importantes ao testar um aplicativo que usa o EF Core.

## <a name="unit-testing"></a>Teste de unidade

Considere testar uma parte da lógica de negócios que talvez precise usar alguns dados de um banco de dados, mas não esteja testando de maneira inerente as interações de banco de dados.
Uma opção é usar uma [duplicata de teste](https://en.wikipedia.org/wiki/Test_double), como uma imitação ou cópia.

Usamos duplicatas de teste para testes internos do EF Core.
No entanto, nunca tentamos fazer duplicatas de DbContext ou IQueryable.
Fazer isso é difícil, complicado e perigoso.
**Não tente fazer isso.**

Em vez disso, usamos o banco de dados em memória quando o teste de unidade é algo que usa DbContext.
Nesse caso, o uso do banco de dados em memória é apropriado porque o teste não depende do comportamento do banco de dados.
Apenas não faça isso para testar as consultas ou atualizações reais do banco de dados.   

Confira [Testes com o provedor em memória](xref:core/miscellaneous/testing/in-memory) para obter diretrizes específicas do EF Core sobre como usar o banco de dados em memória para testes de unidade.
