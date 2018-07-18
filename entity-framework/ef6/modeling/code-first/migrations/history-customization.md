---
title: Personalizando a tabela de histórico de migrações - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
caps.latest.revision: 3
ms.openlocfilehash: 3805d99be6d37d037096f5c5fb69fd30197c1e91
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119890"
---
# <a name="customizing-the-migrations-history-table"></a>Personalizando a tabela de histórico de migrações
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

> [!NOTE]
> Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos. Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="what-is-migrations-history-table"></a>O que é a tabela de histórico de migrações?

Tabela de histórico de migrações é uma tabela usada pelo migrações do Code First para armazenar detalhes sobre a migração aplicada ao banco de dados. Por padrão é o nome da tabela no banco de dados \_ \_MigrationHistory e ele é criado ao aplicar o primeiro faça de migração do banco de dados. No Entity Framework 5 desta tabela era uma tabela do sistema se o aplicativo usado o banco de dados do Microsoft Sql Server. Isso mudou no Entity Framework 6 no entanto, e a tabela de histórico de migrações não está marcado como uma tabela do sistema.

## <a name="why-customize-migrations-history-table"></a>Por que personalizar a tabela de histórico de migrações?

Tabela de histórico de migrações deve ser usada exclusivamente por migrações do Code First e alterá-la manualmente pode interromper as migrações. No entanto, às vezes, a configuração padrão não é adequada e a tabela precisa ser personalizado, por exemplo:

-   Você precisará alterar os nomes e/ou facetas de colunas para habilitar um 3<sup>área de trabalho remota</sup> fornecedor migrações
-   Você deseja alterar o nome da tabela
-   Você precisa usar um esquema diferente do padrão para o \_ \_tabela MigrationHistory
-   Você precisa armazenar dados adicionais para uma determinada versão do contexto e, portanto, você precisará adicionar uma coluna adicional à tabela

## <a name="words-of-precaution"></a>Palavras de precaução

Alterar a tabela de histórico de migração é poderoso, mas você precisa ter cuidado para não exagerar. Atualmente, o tempo de execução do EF não verifica se a tabela de histórico de migrações personalizadas é compatível com o tempo de execução. Se não for seu aplicativo pode quebrar no tempo de execução ou se comportam de maneiras imprevisíveis. Isso é ainda mais importante, se você usar vários contextos por banco de dados, caso em que vários contextos podem usar a mesma tabela de histórico de migração para armazenar informações sobre as migrações.

## <a name="how-to-customize-migrations-history-table"></a>Como personalizar a tabela de histórico de migrações?

Antes de começar, você precisa saber o que você pode personalizar a tabela de histórico de migrações somente antes de aplicar a primeira migração. Agora, no código.

Primeiro, você precisará criar uma classe derivada da classe System.Data.Entity.Migrations.History.HistoryContext. A classe HistoryContext é derivada da classe DbContext para que configurar a tabela de histórico de migrações é muito semelhante à configuração de modelos do EF com a API fluente. Basta substituir o método OnModelCreating e usar a API fluente para configurar a tabela.

>[!NOTE]
> Normalmente, quando você configura os modelos do EF não precisa chamar base. OnModelCreating () do método OnModelCreating substituído, pois o DbContext.OnModelCreating() tem corpo vazio. Isso não é o caso ao configurar a tabela de histórico de migrações. Nesse caso, a primeira coisa a fazer em sua substituição OnModelCreating () é, na verdade, chamar base. OnModelCreating (). Isso irá configurar a tabela de histórico de migrações da maneira padrão que você ajustá-la no método de substituição.

Digamos que você deseja renomear a tabela de histórico de migrações e colocá-lo a um esquema personalizado chamado "admin". Além disso seu DBA que você renomeie a coluna MigrationId para migração\_ID.  Você pode fazer isso criando a seguinte classe derivada de HistoryContext:

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Depois que o HistoryContext personalizado está pronto você precisa fazer o EF ciente dele, registrando-a por meio [configuração baseada em código](http://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

Isso é basicamente isso. Agora você pode ir para o Console do Gerenciador de pacotes, Enable-Migrations, Add-Migration e, finalmente, a atualização de banco de dados. Isso deve resultar na adição de uma tabela de histórico de migrações configurada acordo com os detalhes que você especificou na sua classe derivada de HistoryContext no banco de dados.

![Banco de Dados](~/ef6/media/database.png)
