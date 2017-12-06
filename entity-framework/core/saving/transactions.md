---
title: "Transações - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>Usando transações

Transações permitem que várias operações de banco de dados a serem processadas de forma atômica. Se a transação for confirmada, todas as operações são aplicadas com êxito no banco de dados. Se a transação for revertida, nenhuma das operações são aplicadas ao banco de dados.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) no GitHub.

## <a name="default-transaction-behavior"></a>Comportamento de transação padrão

Por padrão, se o provedor de banco de dados oferece suporte a transações, todas as alterações em uma única chamada para `SaveChanges()` são aplicados em uma transação. Se qualquer uma das alterações falhar, em seguida, a transação é revertida e nenhuma das alterações são aplicadas ao banco de dados. Isso significa que `SaveChanges()` é garantido completamente ser bem-sucedida, ou deixe o banco de dados não modificado se ocorrer um erro.

Para a maioria dos aplicativos, esse comportamento padrão é suficiente. Você deve controlar transações somente manualmente se seus requisitos de aplicativo julguem necessário.

## <a name="controlling-transactions"></a>Controle de transações

Você pode usar o `DbContext.Database` API para iniciar, confirmar e reverter transações. O exemplo a seguir mostra duas `SaveChanges()` operações e um LINQ de consulta que está sendo executada em uma única transação.

Nem todos os provedores de banco de dados oferecer suporte a transações. Alguns provedores podem gerar ou não-operacional quando transações são chamadas de APIs.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Transação de contexto entre (bancos de dados relacionais somente)

Você também pode compartilhar uma transação entre várias instâncias de contexto. Essa funcionalidade está disponível somente ao usar um provedor de banco de dados relacional, porque ele requer o uso de `DbTransaction` e `DbConnection`, que são específicos para bancos de dados relacionais.

Para compartilhar uma transação, os contextos devem compartilhar ambos um `DbConnection` e um `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Permitir a conexão a ser fornecido externamente

Compartilhando um `DbConnection` requer a capacidade de transmitir uma conexão em um contexto ao construir a ele.

A maneira mais fácil para permitir `DbConnection` a ser fornecido externamente é parar de usar o `DbContext.OnConfiguring` método para configurar o contexto e criar externamente `DbContextOptions` e passá-los para o construtor de contexto.

> [!TIP]  
> `DbContextOptionsBuilder`é a API usada na `DbContext.OnConfiguring` para configurar o contexto, você agora vai usá-lo externamente para criar `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

Uma alternativa é continuar usando `DbContext.OnConfiguring`, mas aceitam uma `DbConnection` que é salvo e, em seguida, usado em `DbContext.OnConfiguring`.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Compartilhamento de conexão e de transação

Agora você pode criar várias instâncias de contexto que compartilham a mesma conexão. Em seguida, use o `DbContext.Database.UseTransaction(DbTransaction)` API para inscrever-se os dois contextos na mesma transação.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a>Usando DbTransactions externo (bancos de dados relacionais somente)

Se você estiver usando várias tecnologias de acesso a dados para acessar um banco de dados relacional, pode ser que você deseja compartilhar uma transação entre operações executadas por essas tecnologias diferentes.

O exemplo a seguir mostra como executar uma operação do SqlClient do ADO.NET e uma operação de Entity Framework Core na mesma transação.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
