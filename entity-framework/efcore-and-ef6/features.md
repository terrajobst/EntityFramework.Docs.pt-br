---
title: "Comparação detalhada de recursos do EF Core e EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 3f05fbe53439826a4e1e1b188a7c03951dc109ec
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Comparação detalhada de recursos do EF Core e EF6

A tabela a seguir compara os recursos disponíveis no EF Core e no EF6. A intenção é fornecer uma comparação de alto nível, não listar todos os recursos, ou tentar fornecer detalhes sobre as possíveis diferenças de funcionamento do mesmo recurso.

A coluna EF Core contém o número da versão do produto no qual o recurso apareceu pela primeira vez.

| **Criação de um modelo**                                  | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapeamento de classe básico                                   | Sim      | 1.0                                   |
| Construtores com parâmetros                          |          | 2.1                                   |
| Conversões de valor da propriedade                            |          | 2.1                                   |
| Tipos mapeados sem chaves (tipos de consulta)               |          | 2.1                                   |
| Convenções                                           | Sim      | 1.0                                   |
| Convenções personalizadas                                    | Sim      | 1.0 (parcial)                         |
| Anotações de dados                                      | Sim      | 1.0                                   |
| API fluente                                            | Sim      | 1.0                                   |
| Herança: Tabela por hierarquia (TPH)                | Sim      | 1.0                                   |
| Herança: Tabela por tipo (TPT)                     | Sim      |                                       |
| Herança: Tabela por classe concreta (TPC)           | Sim      |                                       |
| Propriedades de estado sombra                               |          | 1.0                                   |
| Chaves alternativas                                        |          | 1.0                                   |
| Muitos para muitos sem entidade de ingresso                      | Sim      |                                       |
| Geração de chaves: Banco de dados                              | Sim      | 1.0                                   |
| Geração de chaves: Cliente                                |          | 1.0                                   |
| Tipos complexos/de propriedade                                   | Sim      | 2.0                                   |
| Dados espaciais                                          | Sim      |                                       |
| Visualização gráfica do modelo                      | Sim      |                                       |
| Editor de gráfico de modelo                                | Sim      |                                       |
| Formato de modelo: Código                                    | Sim      | 1.0                                   |
| Formato de modelo: EDMX (XML)                              | Sim      |                                       |
| Criar um modelo do banco de dados: Linha de comando              | Sim      | 1.0                                   |
| Criar um modelo do banco de dados: Assistente de VS                 | Sim      |                                       |
| Atualizar modelo do banco de dados                            | Parcial  |                                       |
| Filtros de consulta global                                  |          | 2.0                                   |
| Divisão de tabela                                       | Sim      | 2.0                                   |
| Divisão de entidade                                      | Sim      |                                       |
| Mapeamento de função escalar do banco de dados                      | Ruim     | 2.0                                   |
| Mapeamento de campo                                         |          | 1.1                                   |
|                                                       |          |                                       |
| **Consultar Dados**                                     | **EF6**  | **EF Core**                           |
| Consultas LINQ                                          | Sim      | 1.0 (em andamento para consultas complexas) |
| SQL gerado legível                                | Ruim     | 1.0                                   |
| Avaliação mista de cliente/servidor                        |          | 1.0                                   |
| Conversão de GroupBy                                   | Sim      | 2.1                                   |
| Carregamento de dados relacionados: Eager                           | Sim      | 1.0                                   |
| Carregamento de dados relacionados: carregamento adiantado de tipos derivados |          | 2.1                                   |
| Carregamento de dados relacionados: Lazy                            | Sim      | 2.1                                   |
| Carregamento de dados relacionados: Explicit                        | Sim      | 1.1                                   |
| Consultas SQL brutas: tipos de entidade                         | Sim      | 1.0                                   |
| Consultas SQL brutas: tipos que não são de entidade (por exemplo, tipos de consulta)  | Sim      | 2.1                                   |
| Consultas SQL brutas: Composição com LINQ                  |          | 1.0                                   |
| Consultas explicitamente compiladas                           | Ruim     | 2.0                                   |
| Linguagem de consulta baseada em texto (por exemplo, Entity SQL)           | 1.0      |                                       |
|                                                       |          |                                       |
| **Salvando dados**                                       | **EF6**  | **EF Core**                           |
| Controle de alterações: Instantâneo                             | Sim      | 1.0                                   |
| Controle de alterações: Notificação                         | Sim      | 1.0                                   |
| Controle de alterações: Proxies                              | Sim      |                                       |
| Acesso ao estado controlado                               | Sim      | 1.0                                   |
| Simultaneidade otimista                                | Sim      | 1.0                                   |
| Transações                                          | Sim      | 1.0                                   |
| Envio em lote de instruções                                |          | 1.0                                   |
| Mapeamento de procedimento armazenado                              | Sim      |                                       |
| APIs de baixo nível de grafo desconectado                     | Ruim     | 1.0                                   |
| Gráfico desconectado ponta a ponta                         |          | 1.0 (parcial)                         |
|                                                       |          |                                       |
| **Outros recursos**                                    | **EF6**  | **EF Core**                           |
| Migrações                                            | Sim      | 1.0                                   |
| APIs de criação/exclusão de banco de dados                       | Sim      | 1.0                                   |
| Dados de propagação                                             | Sim      | 2.1                                   |
| Resiliência da conexão                                 | Sim      | 1.1                                   |
| Ganchos de ciclo de vida (eventos, interceptação)                | Sim      |                                       |
| Registro em log simples (por exemplo, Database.Log)                    | Sim      |                                       |
| Pool de DbContext                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Provedores de Banco de Dados**                                | **EF6**  | **EF Core**                           |
| SQL Server                                            | Sim      | 1.0                                   |
| MySQL                                                 | Sim      | 1.0                                   |
| PostgreSQL                                            | Sim      | 1.0                                   |
| Oracle                                                | Sim      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Sim      | 1.0                                   |
| SQL Server Compact                                    | Sim      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Sim      | 1.0                                   |
| Firebird                                              | Sim      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| Na memória (para teste)                               |          | 1.0                                   |
|                                                       |          |                                       |
| **Plataformas**                                         | **EF6**  | **EF Core**                           |
| .NET Framework (Console, WinForms, WPF, ASP.NET)      | Sim      | 1.0                                   |
| .NET Core (Console, ASP.NET Core)                     |          | 1.0                                   |
| Mono e Xamarin                                        |          | 1.0 (em andamento)                     |
| UWP                                                   |          | 1.0 (em andamento)                     |

<sup>1</sup> Atualmente há um provedor pago disponível. Estamos trabalhando em um provedor oficial gratuito para Oracle.
<sup>2</sup> Esse provedor só funciona no .NET Framework (não no .NET Core).
