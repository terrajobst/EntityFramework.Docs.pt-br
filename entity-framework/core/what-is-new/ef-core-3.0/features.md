---
title: Novos recursos no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921682"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Novos recursos incluídos no EF Core 3.0 (atualmente em versão prévia)

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

A lista a seguir inclui os principais novos recursos planejados para o EF Core 3.0.
A maioria desses recursos não está incluída na versão prévia atual, mas ficará disponível à medida que avançarmos em direção a RTM.

A razão é que, no início do lançamento, estamos nos concentrando na implementação de [alterações da falha](xref:core/what-is-new/ef-core-3.0/breaking-changes) planejadas.
Muitas dessas alterações de falha são melhorias para o EF Core por conta própria.
Muitas outras são necessárias para possibilitar mais melhorias. 

Para obter uma lista completa de correções de bug e aprimoramentos em andamento, você pode ver [esta consulta em nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Melhorias do LINQ 

[Acompanhamento de problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.

O LINQ permite que você escreva consultas de banco de dados sem sair da linguagem de sua escolha, aproveitando as informações de tipo avançado para obter IntelliSense e a verificação do tipo em tempo de compilação.
Mas o LINQ também permite que você escreva um número ilimitado de consultas complicadas, o que sempre foi um enorme desafio para provedores do LINQ.
Nas primeiras versões do EF Core, solucionamos parte disso ao descobrir quais partes de uma consulta podem ser convertidas para o SQL e depois permitindo que o restante da consulta seja executada na memória no cliente.
Essa execução do lado do cliente pode ser desejável em algumas situações, mas, em muitos outros casos, pode resultar em consultas ineficientes que podem não ser identificadas até que um aplicativo seja implantado na produção.
No EF Core 3.0, planejamos fazer alterações profundas no funcionamento da implementação do LINQ e em como faremos os testes.
Os objetivos são torná-lo mais robusto (por exemplo, evitar a interrupção de consultas em lançamentos de patch), habilitar a conversão correta de mais expressões em SQL, gerar consultas eficientes em mais casos e impedir que as consultas ineficientes não sejam detectadas.

## <a name="cosmos-db-support"></a>Suporte do Cosmos DB 

[Acompanhamento de problema nº 8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Esse recurso está incluído na versão prévia atual, mas ainda não está completo. 

Estamos trabalhando em um provedor do Cosmos DB para o EF Core para permitir que desenvolvedores familiarizados com o modelo de programação EF conduzam facilmente o Azure Cosmos DB como um banco de dados de aplicativo.
A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência.
O provedor ativará a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.
Iniciamos este trabalho antes do EF Core 2.2 e [fizemos algumas versões prévias do provedor disponível](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
O novo plano é continuar desenvolvendo o provedor junto com o EF Core 3.0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais

[Acompanhamento de questões nº 9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Esse recurso será introduzido no EF Core 3.0 – versão prévia 4.

Considere o modelo a seguir:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

No EF Core 3.0 em diante, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, será possível adicionar um `Order` sem um `OrderDetails` e todas as propriedades `OrderDetails`, com exceção da chave primária, serão mapeadas para colunas que permitem valor nulo.

Ao realizar consultas, o EF Core definirá `OrderDetails` como `null` se uma de suas propriedades necessárias não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.

## <a name="c-80-support"></a>Suporte do C# 8.0

[Acompanhamento do problema nº 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Acompanhamento de problema nº 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.

Queremos que nossos clientes aproveitem alguns dos [novos recursos que virão no C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como os fluxos assíncronos (incluindo `await foreach`) e os tipos de referência nula durante o uso do EF Core.

## <a name="reverse-engineering-of-database-views"></a>Engenharia reversa de exibições de banco de dados

[Acompanhamento de problema nº 1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Esse recurso não está incluído na versão prévia atual.

[Tipos de consulta](xref:core/modeling/query-types), introduzidos no EF Core 2.1 e considerados tipos de entidade sem chaves no EF Core 3.0, representam dados que podem ser lidos do banco de dados, mas não podem ser atualizados.
Essa característica as torna uma excelente escolha para exibições de banco de dados na maioria dos cenários, portanto, planejamos automatizar a criação de tipos de entidade sem chaves ao realizar engenharia reversa de exibições de banco de dados.

## <a name="ef-63-on-net-core"></a>EF 6.3 no .NET Core

[Acompanhamento de problema EF6#271](https://github.com/aspnet/EntityFramework6/issues/271)

O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual. 

Entendemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.
Por esse motivo, adaptaremos a próxima versão do EF 6 para execução no .NET Core 3.0.
Faremos isso para facilitar a portabilidade de aplicativos existentes com alterações mínimas.
Haverá algumas limitações. Por exemplo:
- Exigirá que novos provedores trabalham com outros bancos de dados, além de suporte do SQL Server incluído no .NET Core
- Suporte espacial com o SQL Server não será habilitado

Observe também que não há novos recursos planejados para o EF 6 neste momento.
