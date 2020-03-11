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
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="7aa90-102">Personalizando a tabela de histórico de migrações</span><span class="sxs-lookup"><span data-stu-id="7aa90-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="7aa90-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7aa90-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="7aa90-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="7aa90-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="7aa90-105">Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos.</span><span class="sxs-lookup"><span data-stu-id="7aa90-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="7aa90-106">Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7aa90-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="7aa90-107">O que é a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="7aa90-107">What is Migrations History Table?</span></span>

<span data-ttu-id="7aa90-108">A tabela de histórico de migrações é uma tabela usada pelo Migrações do Code First para armazenar detalhes sobre as migrações aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7aa90-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="7aa90-109">Por padrão, o nome da tabela no banco de dados é \_\_MigrationHistory e é criado ao aplicar a primeira migração para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7aa90-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="7aa90-110">No Entity Framework 5, essa tabela era uma tabela do sistema se o aplicativo usava o banco de dados do Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7aa90-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="7aa90-111">Isso mudou no Entity Framework 6, no entanto, e a tabela de histórico de migrações não está mais marcada como uma tabela do sistema.</span><span class="sxs-lookup"><span data-stu-id="7aa90-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="7aa90-112">Por que personalizar a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="7aa90-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="7aa90-113">A tabela de histórico de migrações deve ser usada exclusivamente por Migrações do Code First e alterá-la manualmente pode interromper as migrações.</span><span class="sxs-lookup"><span data-stu-id="7aa90-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="7aa90-114">No entanto, às vezes a configuração padrão não é adequada e a tabela precisa ser personalizada, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7aa90-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="7aa90-115">Você precisa alterar nomes e/ou facetas das colunas para habilitar um provedor de migrações<sup>de terceiros</sup></span><span class="sxs-lookup"><span data-stu-id="7aa90-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="7aa90-116">Você deseja alterar o nome da tabela</span><span class="sxs-lookup"><span data-stu-id="7aa90-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="7aa90-117">Você precisa usar um esquema não padrão para a tabela \_\_MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="7aa90-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="7aa90-118">Você precisa armazenar dados adicionais para uma determinada versão do contexto e, portanto, precisa adicionar uma coluna adicional à tabela</span><span class="sxs-lookup"><span data-stu-id="7aa90-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="7aa90-119">Palavras de precaução</span><span class="sxs-lookup"><span data-stu-id="7aa90-119">Words of precaution</span></span>

<span data-ttu-id="7aa90-120">A alteração da tabela de histórico de migração é eficiente, mas você precisa ter cuidado para não exagerar-la.</span><span class="sxs-lookup"><span data-stu-id="7aa90-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="7aa90-121">Atualmente, o tempo de execução do EF não verifica se a tabela de histórico de migrações personalizadas é compatível com o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="7aa90-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="7aa90-122">Se não for, seu aplicativo poderá falhar em tempo de execução ou se comportar de maneiras imprevisíveis.</span><span class="sxs-lookup"><span data-stu-id="7aa90-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="7aa90-123">Isso é ainda mais importante se você usar vários contextos por banco de dados, caso em que vários contextos podem usar a mesma tabela de histórico de migração para armazenar informações sobre migrações.</span><span class="sxs-lookup"><span data-stu-id="7aa90-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="7aa90-124">Como personalizar a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="7aa90-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="7aa90-125">Antes de começar, você precisa saber que é possível personalizar a tabela de histórico de migrações somente antes de aplicar a primeira migração.</span><span class="sxs-lookup"><span data-stu-id="7aa90-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="7aa90-126">Agora, para o código.</span><span class="sxs-lookup"><span data-stu-id="7aa90-126">Now, to the code.</span></span>

<span data-ttu-id="7aa90-127">Primeiro, será necessário criar uma classe derivada da classe System. Data. Entity. Migrations. History. HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="7aa90-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="7aa90-128">A classe HistoryContext é derivada da classe DbContext para que a configuração da tabela de histórico de migrações seja muito semelhante à configuração de modelos do EF com a API fluente.</span><span class="sxs-lookup"><span data-stu-id="7aa90-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="7aa90-129">Você só precisa substituir o método OnModelCreating e usar a API fluente para configurar a tabela.</span><span class="sxs-lookup"><span data-stu-id="7aa90-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="7aa90-130">Normalmente, quando você configura modelos do EF, não precisa chamar base. OnModelCreating () do método OnModelCreating substituído, pois DbContext. OnModelCreating () tem corpo vazio.</span><span class="sxs-lookup"><span data-stu-id="7aa90-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="7aa90-131">Esse não é o caso ao configurar a tabela de histórico de migrações.</span><span class="sxs-lookup"><span data-stu-id="7aa90-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="7aa90-132">Nesse caso, a primeira coisa a ser feita na substituição de OnModelCreating () é realmente chamar base. OnModelCreating ().</span><span class="sxs-lookup"><span data-stu-id="7aa90-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="7aa90-133">Isso configurará a tabela de histórico de migrações da maneira padrão que você ajusta no método de substituição.</span><span class="sxs-lookup"><span data-stu-id="7aa90-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="7aa90-134">Digamos que você deseja renomear a tabela de histórico de migrações e colocá-la em um esquema personalizado chamado "admin".</span><span class="sxs-lookup"><span data-stu-id="7aa90-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="7aa90-135">Além disso, o DBA gostaria que você renomear a coluna Migraid para a ID de\_de migração.</span><span class="sxs-lookup"><span data-stu-id="7aa90-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="7aa90-136"> Você poderia conseguir isso criando a seguinte classe derivada de HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="7aa90-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="7aa90-137">Depois que o HistoryContext personalizado estiver pronto, você precisará fazer com que o EF se reconheça ao registrá-lo por meio da [Configuração baseada em código](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="7aa90-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="7aa90-138">É bem assim.</span><span class="sxs-lookup"><span data-stu-id="7aa90-138">That’s pretty much it.</span></span> <span data-ttu-id="7aa90-139">Agora você pode acessar o console do Gerenciador de pacotes, Enable-Migrations, Add-Migration e, por fim, Update-Database.</span><span class="sxs-lookup"><span data-stu-id="7aa90-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="7aa90-140">Isso deve resultar na adição ao banco de dados uma tabela de histórico de migrações configurada de acordo com os detalhes especificados em sua classe derivada HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="7aa90-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Banco de dados](~/ef6/media/database.png)
