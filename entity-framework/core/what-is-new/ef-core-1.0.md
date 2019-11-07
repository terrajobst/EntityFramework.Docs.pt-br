---
title: Novidades no EF Core 1.0 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655851"
---
# <a name="features-included-in-ef-core-10"></a>Recursos incluídos no EF Core 1.0

## <a name="platforms"></a>Plataformas

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Inclui Console, WPF, WinForms, ASP.NET 4 etc.

### <a name="net-standard-13"></a>.NET Standard 1.3

Incluindo o ASP.NET Core direcionado para o .NET Framework e .NET Core no Windows, OSX e Linux.

## <a name="modelling"></a>Modelagem

### <a name="basic-modelling"></a>Modelagem básica

Com base em entidades POCO com propriedades get/set de tipos escalares comuns (`int`, `string` etc.).

### <a name="relationships-and-navigation-properties"></a>Relações e propriedades de navegação

As relações de um para muitos e um para zero ou um podem ser especificadas no modelo com base em uma chave estrangeira. É possível associar propriedades de navegação de coleção simples ou tipos de referência a essas relações.

### <a name="built-in-conventions"></a>Convenções internas

Elas compilam um modelo inicial com base na forma das classes de entidade.

### <a name="fluent-api"></a>API fluente

Permite que você substitua o método `OnModelCreating` em seu contexto para configurar ainda mais o modelo que foi descoberto por convenção.

### <a name="data-annotations"></a>Anotações de dados

São atributos que podem ser adicionados às suas propriedades/classes de entidade e influenciarão o modelo do EF. Por exemplo, adicionar `[Required]` permitirá que o EF saiba que uma propriedade é necessária.

### <a name="relational-table-mapping"></a>Mapeamento de tabela relacional

Permite o mapeamento de entidades para tabelas/colunas.

### <a name="key-value-generation"></a>Geração de valor de chave

Incluindo geração do lado do cliente e a geração de banco de dados.

### <a name="database-generated-values"></a>Valores gerados no banco de dados

Permite a geração de valores pelo banco de dados na inserção (valores padrão) ou atualização (colunas computadas).

### <a name="sequences-in-sql-server"></a>Sequências no SQL Server

Permite a definição de objetos de sequência no modelo.

### <a name="unique-constraints"></a>Restrições exclusivas

Permite a definição de chaves alternativas e a capacidade de definir relações direcionadas para essa chave.

### <a name="indexes"></a>Índices

A definição de índices no modelo introduz automaticamente índices no banco de dados. Também há suporte para índices exclusivos.

### <a name="shadow-state-properties"></a>Propriedades de estado sombra

Permite a definição de propriedades no modelo que não são declaradas e não são armazenadas na classe do .NET, mas podem ser controladas e atualizadas pelo EF Core. Normalmente usado para propriedades de chave estrangeira, quando a exposição delas no objeto não for desejada.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Padrão de herança de tabela por hierarquia

Permite a gravação de entidades em uma hierarquia de herança em uma única tabela usando uma coluna discriminadora para identificar o tipo de entidade para um determinado registro no banco de dados.

### <a name="model-validation"></a>Validação de modelo

Detecta padrões inválidos no modelo e fornece mensagens de erro úteis.

## <a name="change-tracking"></a>Change tracking

### <a name="snapshot-change-tracking"></a>Controle de alterações de instantâneo

Permite a detecção automática de alterações em entidades por meio da comparação do estado atual com uma cópia (instantâneo) do estado original.

### <a name="notification-change-tracking"></a>Controle de alterações de notificação

Permite que as entidades notifiquem o controlador de alteração quando houver alteração nos valores de propriedade.

### <a name="accessing-tracked-state"></a>Acesso ao estado controlado

Via `DbContext.Entry` e `DbContext.ChangeTracker`.

### <a name="attaching-detached-entitiesgraphs"></a>Anexar entidades/gráficos desanexados

A nova API do `DbContext.AttachGraph` ajuda a reanexar entidades a um contexto para salvar as entidades novas/modificadas.

## <a name="saving-data"></a>Salvar dados

### <a name="basic-save-functionality"></a>Funcionalidade de gravação básica

Permite que as alterações para instâncias de entidade persistam para o banco de dados.

### <a name="optimistic-concurrency"></a>Simultaneidade otimista

Protege contra a substituição das alterações feitas por outro usuário, já que os dados foram obtidos do banco de dados.

### <a name="async-savechanges"></a>SaveChanges assíncrono

Pode liberar o thread atual para processar outras solicitações enquanto o banco de dados processa os comandos emitidos do `SaveChanges`.

### <a name="database-transactions"></a>Transações de banco de dados

Significa que `SaveChanges` sempre é atômica (isso significa que, ou a alteração é concluída com sucesso, ou nenhuma alteração é feita no banco de dados). Também há APIs relacionadas à transação para permitir o compartilhamento de transações entre instâncias de contexto etc.

### <a name="relational-batching-of-statements"></a>Relacional: envio em lote de instruções

Fornece um desempenho melhor colocando em um único lote vários comandos INSERT/UPDATE/DELETE em uma única ida e volta ao banco de dados.

## <a name="query"></a>Consulta

### <a name="basic-linq-support"></a>Suporte básico a LINQ

Fornece a capacidade de usar o LINQ para recuperar dados do banco de dados.

### <a name="mixed-clientserver-evaluation"></a>Avaliação mista de cliente/servidor

Permite que as consultas contenham lógica que não pode ser avaliada no banco de dados e, portanto, devem ser avaliadas após a recuperação dos dados na memória.

### <a name="notracking"></a>NoTracking

As consultas permitem uma execução mais rápida de consultas quando o contexto não precisar monitorar a existência de alterações nas instâncias de entidade (isso será útil se os resultados forem somente leitura).

### <a name="eager-loading"></a>Carregamento adiantado

Fornece os métodos `Include` e `ThenInclude` para identificar dados relacionados que também devem ser obtidos ao consultar.

### <a name="async-query"></a>Consulta assíncrona

Pode liberar o thread atual (e seus recursos associados) para processar outras solicitações enquanto o banco de dados processa a consulta.

### <a name="raw-sql-queries"></a>Consultas SQL brutas

Fornece o método `DbSet.FromSql` para usar consultas SQL brutas a fim de buscar dados. Essas consultas também podem ser compostas usando LINQ.

## <a name="database-schema-management"></a>Gerenciamento de esquemas de banco de dados

### <a name="database-creationdeletion-apis"></a>APIs de criação/exclusão de banco de dados

São projetadas principalmente para teste, em que você deseja criar/excluir rapidamente o banco de dados sem o uso de migrações.

### <a name="relational-database-migrations"></a>Migrações de banco de dados relacional

Permitir a evolução de um esquema de banco de dados relacional após o expediente, à medida que o modelo sofre alterações.

### <a name="reverse-engineer-from-database"></a>Engenharia reversa do banco de dados

Realiza o scaffolding de um modelo EF com base em um esquema de banco de dados relacional existente.

## <a name="database-providers"></a>Provedores de banco de dados

### <a name="sql-server"></a>SQL Server

Conecta-se ao Microsoft SQL Server 2008 e posteriores.

### <a name="sqlite"></a>SQLite

Conecta-se a um banco de dados SQLite 3.

### <a name="in-memory"></a>Na memória

Foi projetado para habilitar facilmente o teste sem se conectar a um banco de dados real.

### <a name="3rd-party-providers"></a>Provedores de terceiros

Há vários provedores disponíveis para outros mecanismos de banco de dados. Veja [Provedores de banco de dados](../providers/index.md) para obter uma lista completa.
