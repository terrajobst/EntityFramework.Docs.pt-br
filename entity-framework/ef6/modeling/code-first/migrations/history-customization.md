---
title: Personalizando a tabela de histórico de migrações - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: e3faefc4b812ec4bc440ed2bb48747053d8cb1b3
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283687"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="bea63-102">Personalizando a tabela de histórico de migrações</span><span class="sxs-lookup"><span data-stu-id="bea63-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="bea63-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bea63-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="bea63-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="bea63-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="bea63-105">Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos.</span><span class="sxs-lookup"><span data-stu-id="bea63-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="bea63-106">Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="bea63-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="bea63-107">O que é a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="bea63-107">What is Migrations History Table?</span></span>

<span data-ttu-id="bea63-108">Tabela de histórico de migrações é uma tabela usada pelo migrações do Code First para armazenar detalhes sobre a migração aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bea63-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="bea63-109">Por padrão é o nome da tabela no banco de dados \_ \_MigrationHistory e ele é criado ao aplicar o primeiro faça de migração do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bea63-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="bea63-110">No Entity Framework 5 desta tabela era uma tabela do sistema se o aplicativo usado o banco de dados do Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="bea63-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="bea63-111">Isso mudou no Entity Framework 6 no entanto, e a tabela de histórico de migrações não está marcado como uma tabela do sistema.</span><span class="sxs-lookup"><span data-stu-id="bea63-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="bea63-112">Por que personalizar a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="bea63-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="bea63-113">Tabela de histórico de migrações deve ser usada exclusivamente por migrações do Code First e alterá-la manualmente pode interromper as migrações.</span><span class="sxs-lookup"><span data-stu-id="bea63-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="bea63-114">No entanto, às vezes, a configuração padrão não é adequada e a tabela precisa ser personalizado, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="bea63-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="bea63-115">Você precisará alterar os nomes e/ou facetas de colunas para habilitar um 3<sup>área de trabalho remota</sup> fornecedor migrações</span><span class="sxs-lookup"><span data-stu-id="bea63-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="bea63-116">Você deseja alterar o nome da tabela</span><span class="sxs-lookup"><span data-stu-id="bea63-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="bea63-117">Você precisa usar um esquema diferente do padrão para o \_ \_tabela MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="bea63-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="bea63-118">Você precisa armazenar dados adicionais para uma determinada versão do contexto e, portanto, você precisará adicionar uma coluna adicional à tabela</span><span class="sxs-lookup"><span data-stu-id="bea63-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="bea63-119">Palavras de precaução</span><span class="sxs-lookup"><span data-stu-id="bea63-119">Words of precaution</span></span>

<span data-ttu-id="bea63-120">Alterar a tabela de histórico de migração é poderoso, mas você precisa ter cuidado para não exagerar.</span><span class="sxs-lookup"><span data-stu-id="bea63-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="bea63-121">Atualmente, o tempo de execução do EF não verifica se a tabela de histórico de migrações personalizadas é compatível com o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="bea63-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="bea63-122">Se não for seu aplicativo pode quebrar no tempo de execução ou se comportam de maneiras imprevisíveis.</span><span class="sxs-lookup"><span data-stu-id="bea63-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="bea63-123">Isso é ainda mais importante, se você usar vários contextos por banco de dados, caso em que vários contextos podem usar a mesma tabela de histórico de migração para armazenar informações sobre as migrações.</span><span class="sxs-lookup"><span data-stu-id="bea63-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="bea63-124">Como personalizar a tabela de histórico de migrações?</span><span class="sxs-lookup"><span data-stu-id="bea63-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="bea63-125">Antes de começar, você precisa saber o que você pode personalizar a tabela de histórico de migrações somente antes de aplicar a primeira migração.</span><span class="sxs-lookup"><span data-stu-id="bea63-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="bea63-126">Agora, no código.</span><span class="sxs-lookup"><span data-stu-id="bea63-126">Now, to the code.</span></span>

<span data-ttu-id="bea63-127">Primeiro, você precisará criar uma classe derivada da classe System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="bea63-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="bea63-128">A classe HistoryContext é derivada da classe DbContext para que configurar a tabela de histórico de migrações é muito semelhante à configuração de modelos do EF com a API fluente.</span><span class="sxs-lookup"><span data-stu-id="bea63-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="bea63-129">Basta substituir o método OnModelCreating e usar a API fluente para configurar a tabela.</span><span class="sxs-lookup"><span data-stu-id="bea63-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="bea63-130">Normalmente, quando você configura os modelos do EF não precisa chamar base. OnModelCreating () do método OnModelCreating substituído, pois o DbContext.OnModelCreating() tem corpo vazio.</span><span class="sxs-lookup"><span data-stu-id="bea63-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="bea63-131">Isso não é o caso ao configurar a tabela de histórico de migrações.</span><span class="sxs-lookup"><span data-stu-id="bea63-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="bea63-132">Nesse caso, a primeira coisa a fazer em sua substituição OnModelCreating () é, na verdade, chamar base. OnModelCreating ().</span><span class="sxs-lookup"><span data-stu-id="bea63-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="bea63-133">Isso irá configurar a tabela de histórico de migrações da maneira padrão que você ajustá-la no método de substituição.</span><span class="sxs-lookup"><span data-stu-id="bea63-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="bea63-134">Digamos que você deseja renomear a tabela de histórico de migrações e colocá-lo a um esquema personalizado chamado "admin".</span><span class="sxs-lookup"><span data-stu-id="bea63-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="bea63-135">Além disso seu DBA que você renomeie a coluna MigrationId para migração\_ID.</span><span class="sxs-lookup"><span data-stu-id="bea63-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="bea63-136">Você pode fazer isso criando a seguinte classe derivada de HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="bea63-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="bea63-137">Depois que o HistoryContext personalizado está pronto você precisa fazer o EF ciente dele, registrando-a por meio [configuração baseada em código](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="bea63-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="bea63-138">Isso é basicamente isso.</span><span class="sxs-lookup"><span data-stu-id="bea63-138">That’s pretty much it.</span></span> <span data-ttu-id="bea63-139">Agora você pode ir para o Console do Gerenciador de pacotes, Enable-Migrations, Add-Migration e, finalmente, a atualização de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bea63-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="bea63-140">Isso deve resultar na adição de uma tabela de histórico de migrações configurada acordo com os detalhes que você especificou na sua classe derivada de HistoryContext no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bea63-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Banco de Dados](~/ef6/media/database.png)
