---
title: Personalizando a tabela de histórico de migrações-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418975"
---
# <a name="customizing-the-migrations-history-table"></a>Personalizando a tabela de histórico de migrações
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

> [!NOTE]
> Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos. Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="what-is-migrations-history-table"></a>O que é a tabela de histórico de migrações?

A tabela de histórico de migrações é uma tabela usada pelo Migrações do Code First para armazenar detalhes sobre as migrações aplicadas ao banco de dados. Por padrão, o nome da tabela no banco de dados é \_\_MigrationHistory e é criado ao aplicar a primeira migração para o banco de dados. No Entity Framework 5, essa tabela era uma tabela do sistema se o aplicativo usava o banco de dados do Microsoft SQL Server. Isso mudou no Entity Framework 6, no entanto, e a tabela de histórico de migrações não está mais marcada como uma tabela do sistema.

## <a name="why-customize-migrations-history-table"></a>Por que personalizar a tabela de histórico de migrações?

A tabela de histórico de migrações deve ser usada exclusivamente por Migrações do Code First e alterá-la manualmente pode interromper as migrações. No entanto, às vezes a configuração padrão não é adequada e a tabela precisa ser personalizada, por exemplo:

-   Você precisa alterar nomes e/ou facetas das colunas para habilitar um provedor de migrações<sup>de terceiros</sup>
-   Você deseja alterar o nome da tabela
-   Você precisa usar um esquema não padrão para a tabela \_\_MigrationHistory
-   Você precisa armazenar dados adicionais para uma determinada versão do contexto e, portanto, precisa adicionar uma coluna adicional à tabela

## <a name="words-of-precaution"></a>Palavras de precaução

A alteração da tabela de histórico de migração é eficiente, mas você precisa ter cuidado para não exagerar-la. Atualmente, o tempo de execução do EF não verifica se a tabela de histórico de migrações personalizadas é compatível com o tempo de execução. Se não for, seu aplicativo poderá falhar em tempo de execução ou se comportar de maneiras imprevisíveis. Isso é ainda mais importante se você usar vários contextos por banco de dados, caso em que vários contextos podem usar a mesma tabela de histórico de migração para armazenar informações sobre migrações.

## <a name="how-to-customize-migrations-history-table"></a>Como personalizar a tabela de histórico de migrações?

Antes de começar, você precisa saber que é possível personalizar a tabela de histórico de migrações somente antes de aplicar a primeira migração. Agora, para o código.

Primeiro, será necessário criar uma classe derivada da classe System. Data. Entity. Migrations. History. HistoryContext. A classe HistoryContext é derivada da classe DbContext para que a configuração da tabela de histórico de migrações seja muito semelhante à configuração de modelos do EF com a API fluente. Você só precisa substituir o método OnModelCreating e usar a API fluente para configurar a tabela.

>[!NOTE]
> Normalmente, quando você configura modelos do EF, não precisa chamar base. OnModelCreating () do método OnModelCreating substituído, pois DbContext. OnModelCreating () tem corpo vazio. Esse não é o caso ao configurar a tabela de histórico de migrações. Nesse caso, a primeira coisa a ser feita na substituição de OnModelCreating () é realmente chamar base. OnModelCreating (). Isso configurará a tabela de histórico de migrações da maneira padrão que você ajusta no método de substituição.

Digamos que você deseja renomear a tabela de histórico de migrações e colocá-la em um esquema personalizado chamado "admin". Além disso, o DBA gostaria que você renomear a coluna Migraid para a ID de\_de migração.  Você poderia conseguir isso criando a seguinte classe derivada de HistoryContext:

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

Depois que o HistoryContext personalizado estiver pronto, você precisará fazer com que o EF se reconheça ao registrá-lo por meio da [Configuração baseada em código](https://msdn.com/data/jj680699):

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

É bem assim. Agora você pode acessar o console do Gerenciador de pacotes, Enable-Migrations, Add-Migration e, por fim, Update-Database. Isso deve resultar na adição ao banco de dados uma tabela de histórico de migrações configurada de acordo com os detalhes especificados em sua classe derivada HistoryContext.

![Banco de dados](~/ef6/media/database.png)
