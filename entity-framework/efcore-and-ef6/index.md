---
title: Comparar o EF6 e o EF Core
description: Orientação sobre como escolher entre o EF6 e o EF Core.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888142"
---
# <a name="compare-ef-core--ef6"></a>Comparar EF Core e EF6

## <a name="ef-core"></a>EF Core

O Entity Framework Core ([EF Core](../core/index.md)) é um mapeador de banco de dados de objetos moderno para .NET. Ele dá suporte a consultas LINQ, controle de alterações, atualizações e migrações de esquema.

O EF Core funciona com SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL e muitos outros bancos de dados por meio de um [modelo de plug-in do provedor de banco de dados](../core/providers/index.md).

## <a name="ef6"></a>EF6

O Entity Framework 6 ([EF6](../ef6/index.md)) é um mapeador relacional de objeto projetado para .NET Framework, mas com suporte para .NET Core. O EF6 é um produto estável e com suporte, mas não está mais sendo desenvolvido ativamente.

## <a name="feature-comparison"></a>Comparação entre recursos

O EF Core oferece novos recursos que não serão implementados no EF6. No entanto, nem todos os recursos do EF6 estão atualmente implementados no EF Core.

As tabelas a seguir comparam os recursos disponíveis no EF Core e no EF6. Trata-se de uma comparação de alto nível e não lista todos os recursos nem explica as diferenças entre o mesmo recurso em diferentes versões do EF.

A coluna EF Core indica a versão do produto na qual o recurso apareceu pela primeira vez.

### <a name="creating-a-model"></a>Criar um modelo

| **Recurso**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapeamento de classe básico                                   | Sim      | 1.0                                   |
| Construtores com parâmetros                          |          | 2.1                                   |
| Conversões de valor da propriedade                            |          | 2.1                                   |
| Tipos mapeados sem chaves                             |          | 2.1                                   |
| Convenções                                           | Sim      | 1.0                                   |
| Convenções personalizadas                                    | Sim      | 1.0 (parcial; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Anotações de dados                                      | Sim      | 1.0                                   |
| API fluente                                            | Sim      | 1.0                                   |
| Herança: Tabela por hierarquia (TPH)                | Sim      | 1.0                                   |
| Herança: Tabela por tipo (TPT)                     | Sim      | Planejado para 5.0 ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Herança: Tabela por classe concreta (TPC)           | Sim      | Ampliar para 5.0 ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Propriedades de estado sombra                               |          | 1.0                                   |
| Chaves alternativas                                        |          | 1.0                                   |
| Navegações muitos para muitos                              | Sim      | Planejado para 5.0 ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Muitos para muitos sem entidade de ingresso                      | Sim      | Na lista de pendências ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Geração de chave: Banco de Dados                              | Sim      | 1.0                                   |
| Geração de chave: Cliente                                |          | 1.0                                   |
| Tipos complexos/de propriedade                                   | Sim      | 2.0                                   |
| Dados espaciais                                          | Sim      | 2.2                                   |
| Formato de modelo: Código                                    | Sim      | 1.0                                   |
| Criar modelo do banco de dados: Linha de comando              | Sim      | 1.0                                   |
| Atualizar modelo do banco de dados                            | Parcial  | Na lista de pendências ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Filtros de consulta global                                  |          | 2.0                                   |
| Divisão de tabela                                       | Sim      | 2.0                                   |
| Divisão de entidade                                      | Sim      | Ampliar para 5.0 ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Mapeamento de função escalar do banco de dados                      | Ruim     | 2.0                                   |
| Mapeamento de campo                                         |          | 1.1                                   |
| Tipos de referência anuláveis (C# 8.0)                     |          | 3.0                                   |
| Visualização gráfica do modelo                      | Sim      | Não há suporte planejado <sup>(2)</sup>     |
| Editor de gráfico de modelo                                | Sim      | Não há suporte planejado <sup>(2)</sup>     |
| Formato de modelo: EDMX (XML)                              | Sim      | Não há suporte planejado <sup>(2)</sup>     |
| Criar modelo do banco de dados: Assistente do VS                 | Sim      | Não há suporte planejado <sup>(2)</sup>     |

### <a name="querying-data"></a>Consultar dados

| **Recurso**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Consultas LINQ                                          | Sim      | 1.0                                   |
| SQL gerado legível                                | Ruim     | 1.0                                   |
| Conversão de GroupBy                                   | Sim      | 2.1                                   |
| Carregando dados relacionados: Adiantado                           | Sim      | 1.0                                   |
| Carregando dados relacionados: Carregamento adiantado para tipos derivados |          | 2.1                                   |
| Carregando dados relacionados: Ocioso                            | Sim      | 2.1                                   |
| Carregando dados relacionados: Explícito                        | Sim      | 1.1                                   |
| Consultas SQL brutas: Tipos de entidade                         | Sim      | 1.0                                   |
| Consultas SQL brutas: Tipos de entidade sem chave                 | Sim      | 2.1                                   |
| Consultas SQL brutas: Como compor com o LINQ                  |          | 1.0                                   |
| Consultas explicitamente compiladas                           | Ruim     | 2.0                                   |
| await foreach (C# 8.0)                                |          | 3.0                                   |
| Linguagem de consulta baseada em texto (Entity SQL)                | Sim      | Não há suporte planejado <sup>(2)</sup>     |

### <a name="saving-data"></a>Salvar dados

| **Recurso**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Controle de alterações: Instantâneo                             | Sim      | 1.0                                   |
| Controle de alterações: Notificação                         | Sim      | 1.0                                   |
| Controle de alterações: Proxies                              | Sim      | Mesclado para 5.0 ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| Acesso ao estado controlado                               | Sim      | 1.0                                   |
| Simultaneidade otimista                                | Sim      | 1.0                                   |
| Transações                                          | Sim      | 1.0                                   |
| Envio em lote de instruções                                |          | 1.0                                   |
| Mapeamento de procedimento armazenado                              | Sim      | Na lista de pendências ([#245](https://github.com/dotnet/efcore/issues/245)) |
| APIs de baixo nível de grafo desconectado                     | Ruim     | 1.0                                   |
| Gráfico desconectado ponta a ponta                         |          | 1.0 (parcial; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Outros recursos

| **Recurso**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrações                                            | Sim      | 1.0                                   |
| APIs de criação/exclusão de banco de dados                       | Sim      | 1.0                                   |
| Dados de propagação                                             | Sim      | 2.1                                   |
| Resiliência da conexão                                 | Sim      | 1.1                                   |
| Interceptores                                          | Sim      | 3.0                                   |
| Eventos                                                | Sim      | 3.0 (parcial; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Registro em log simples (Database.Log)                         | Sim      | Mesclado para 5.0 ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| Pool de DbContext                                     |          | 2.0                                   |

### <a name="database-providers-sup3sup"></a>Provedores de banco de dados <sup>(3)</sup>

| **Recurso**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Sim      | 1.0                                   |
| MySQL                                                 | Sim      | 1.0                                   |
| PostgreSQL                                            | Sim      | 1.0                                   |
| Oracle                                                | Sim      | 1.0                                   |
| SQLite                                                | Sim      | 1.0                                   |
| SQL Server Compact                                    | Sim      | 1.0 <sup>(4)</sup>                    |
| DB2                                                   | Sim      | 1.0                                   |
| Firebird                                              | Sim      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| Na memória (para teste)                               |          | 1.0                                   |

<sup>1</sup> Não é provável que as metas de ampliação sejam obtidas para uma determinada versão. No entanto, se tudo estiver bem, tentaremos efetuar pull.

<sup>2</sup> Alguns recursos do EF6 não serão implementados no EF Core. Esses recursos dependem do EDM (Modelo de Dados de Entidade) subjacente do EF6 e/ou são recursos complexos com retorno sobre o investimento relativamente baixo. Sempre agradecemos os comentários, mas embora o EF Core permita muitas tarefas que não são possíveis no EF6, não é possível para o EF Core dar suporte a todos os recursos do EF6.

<sup>3</sup> Os provedores de banco de dados do EF Core implementados por terceiros podem ter atraso na atualização para novas versões principais do EF Core. Confira [Provedores de banco de dados](../core/providers/index.md) para obter mais informações.

<sup>4</sup> Os provedores do SQL Server Compact e do Jet só funcionam no .NET Framework (não no .NET Core).

### <a name="supported-platforms"></a>Plataformas com suporte

O EF Core 3.1 é executado no .NET Core e .NET Framework, por meio do uso do .NET Standard 2.0. No entanto, EF Core 5.0 não será executado no .NET Framework. Consulte [Plataformas](../core/platforms/index.md) para obter mais detalhes.

O EF 6.4 é executado no .NET Core e .NET Framework, por meio de vários destinos.

## <a name="guidance-for-new-applications"></a>Orientação para novos aplicativos

Use o EF Core no .NET Core para todos os novos aplicativos, a menos que o aplicativo precise de algo que [seja compatível apenas no .NET Framework](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

## <a name="guidance-for-existing-ef6-applications"></a>Orientação para aplicativos EF6 existentes

O EF Core não é uma substituição solta para o EF6. A mudança de EF6 para EF Core provavelmente exigirá alterações no seu aplicativo.

Ao mover um aplicativo EF6 para o .NET Core:
* Continue usando o EF6 se o código de acesso a dados estiver estável e provavelmente não evoluir nem precisar de novos recursos.
* Passe para o EF Core se o código de acesso a dados estiver em evolução ou se o aplicativo precisar de novos recursos disponíveis somente no EF Core.
* A portabilidade para o EF Core também é realizada com frequência para ter desempenho. No entanto, nem todos os cenários são mais rápidos, portanto, faça alguma criação de perfil primeiro.

Para saber mais, confira [Portabilidade do EF6 para o EF Core](porting/index.md).
