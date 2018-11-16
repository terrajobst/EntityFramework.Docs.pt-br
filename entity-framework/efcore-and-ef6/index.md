---
title: Comparar o Entity Framework 6 e o Entity Framework Core
description: Fornece orientação sobre como escolher entre o Entity Framework 6 e o Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: d5fe9b388707f653fdeb2d6a5daa7215ced71c1d
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688713"
---
# <a name="compare-ef-core--ef6"></a>Comparar EF Core e EF6

O Entity Framework é um O/RM (mapeador de objeto relacional) para o .NET. Este artigo compara as duas versões: o Entity Framework 6 e o Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

O EF6 (Entity Framework 6) é uma tecnologia de acesso a dados testada. Ele foi lançado pela primeira vez em 2008 como parte do .NET Framework 3.5 SP1 e do Visual Studio 2008 SP1. Começando com a versão 4.1, é fornecido como o pacote NuGet do [EntityFramework](https://www.nuget.org/packages/EntityFramework/). O EF6 é executado no .NET Framework 4.x, o que significa que ele é executado apenas no Windows. 

O EF6 continua sendo um produto compatível e continuaremos a observar correções de bug e pequenas melhorias.

## <a name="entity-framework-core"></a>Entity Framework Core

O EF Core (Entity Framework Core) é uma reformulação completa do EF6 lançada pela primeira vez em 2016. É fornecido nos pacotes Nuget, com o principal sendo [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). O EF Core é um produto multiplataforma que pode ser executado no .NET Core ou no .NET Framework.

O EF Core foi criado para fornecer uma experiência de desenvolvedor semelhante ao EF6. A maioria das APIs de nível superior continua igual, portanto o EF Core soará familiar para os desenvolvedores que usaram o EF6.

## <a name="feature-comparison"></a>Comparação entre recursos

O EF Core oferece novos recursos que não serão implementados no EF6 (como [chaves alternativas](xref:core/modeling/alternate-keys), [atualizações em lote](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements) e [avaliação mista cliente/banco de dados em consultas LINQ](xref:core/querying/client-eval). Mas, como ele é uma nova base de código, ele também não tem alguns recursos que o EF6.

As tabelas a seguir comparam os recursos disponíveis no EF Core e no EF6. É uma comparação de alto nível e não lista todos os recursos nem explicam as diferenças entre o mesmo recurso em diferentes versões do EF.

A coluna EF Core indica a versão do produto na qual o recurso apareceu pela primeira vez.

### <a name="creating-a-model"></a>Criar um modelo

| **Recurso**                                           | **EF 6** | **EF Core**                           |
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
| Dados espaciais                                          | Sim      | 2.2                                   |
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

### <a name="querying-data"></a>Consultar dados

| **Recurso**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Consultas LINQ                                          | Sim      | 1.0 (em andamento para consultas complexas) |
| SQL gerado legível                                | Ruim     | 1.0                                   |
| Avaliação mista de cliente/servidor                        |          | 1.0                                   |
| Conversão de GroupBy                                   | Sim      | 2.1                                   |
| Carregamento de dados relacionados: Eager                           | Sim      | 1.0                                   |
| Carregamento de dados relacionados: carregamento adiantado de tipos derivados |          | 2.1                                   |
| Carregamento de dados relacionados: Lazy                            | Sim      | 2.1                                   |
| Carregamento de dados relacionados: Explicit                        | Sim      | 1.1                                   |
| Consultas SQL brutas: tipos de entidade                         | Sim      | 1.0                                   |
| Consultas SQL brutas: tipos que não são de entidade (tipos de consulta)       | Sim      | 2.1                                   |
| Consultas SQL brutas: Composição com LINQ                  |          | 1.0                                   |
| Consultas explicitamente compiladas                           | Ruim     | 2.0                                   |
| Linguagem de consulta baseada em texto (Entity SQL)                | Sim      |                                       |

### <a name="saving-data"></a>Salvar dados

| **Recurso**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
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

### <a name="other-features"></a>Outros recursos

| **Recurso**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrações                                            | Sim      | 1.0                                   |
| APIs de criação/exclusão de banco de dados                       | Sim      | 1.0                                   |
| Dados de propagação                                             | Sim      | 2.1                                   |
| Resiliência da conexão                                 | Sim      | 1.1                                   |
| Ganchos de ciclo de vida (eventos, interceptação)                | Sim      |                                       |
| Registro em log simples (Database.Log)                         | Sim      |                                       |
| Pool de DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Provedores de banco de dados

| **Recurso**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
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

<sup>1</sup> No momento, há um provedor pago disponível para Oracle. Estamos trabalhando em um provedor oficial gratuito para Oracle.

<sup>2</sup> O provedor do SQL Server Compact e do Jet só funciona no .NET Framework (não no .NET Core).

### <a name="net-implementations"></a>Implementações do .NET

| **Recurso**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| .NET Framework (Console, WinForms, WPF, ASP.NET)      | Sim      | 1.0                                   |
| .NET Core (Console, ASP.NET Core)                     |          | 1.0                                   |
| Mono e Xamarin                                        |          | 1.0 (em andamento)                     |
| UWP                                                   |          | 1.0 (em andamento)                     |

## <a name="guidance-for-new-applications"></a>Orientação para novos aplicativos

Considere usar o EF Core para um novo aplicativo se as duas condições a seguir forem verdadeiras:
* O aplicativo precisa das funcionalidades do .NET Core. Para obter mais informações, confira [Escolhendo entre o .NET Core e o .NET Framework para aplicativos de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* O EF Core dá suporte a todos os recursos que o aplicativo requer. Se um recurso desejado estiver ausente, verifique o [Roteiro do EF Core](xref:core/what-is-new/roadmap) para descobrir se há planos para lhe dar suporte no futuro. 

Considere usar o EF6 se as duas condições a seguir forem verdadeiras:
* O aplicativo será executado no Windows e no .NET Framework 4.0 ou posterior.
* O EF6 é compatível com todos os recursos exigidos pelo aplicativo.

## <a name="guidance-for-existing-ef6-applications"></a>Orientação para aplicativos EF6 existentes

Devido às mudanças fundamentais no EF Core, não recomendamos migrar um aplicativo EF6 para o EF Core, a menos que haja um motivo convincente para fazê-lo. Se desejar migrar para o EF Core para usar novos recursos, esteja ciente de suas limitações. Para saber mais, confira [Portando de EF6 para EF Core](porting/index.md). **A migração do EF6 para o EF Core é mais uma portabilidade do que uma atualização.** 

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, confira a documentação:
* <xref:core/index>
* <xref:ef6/index>
