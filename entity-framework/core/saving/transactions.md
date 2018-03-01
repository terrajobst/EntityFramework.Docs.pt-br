---
title: "Transações - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a>Usando transações

Transações permitem que várias operações de banco de dados a serem processadas de forma atômica. Se a transação for confirmada, todas as operações são aplicadas com êxito no banco de dados. Se a transação for revertida, nenhuma das operações são aplicadas ao banco de dados.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) deste artigo no GitHub.

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
> `DbContextOptionsBuilder` é a API usada na `DbContext.OnConfiguring` para configurar o contexto, você agora vai usá-lo externamente para criar `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

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

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Usando DbTransactions externo (bancos de dados relacionais somente)

Se você estiver usando várias tecnologias de acesso a dados para acessar um banco de dados relacional, pode ser que você deseja compartilhar uma transação entre operações executadas por essas tecnologias diferentes.

O exemplo a seguir mostra como executar uma operação do SqlClient do ADO.NET e uma operação de Entity Framework Core na mesma transação.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Usando System. Transactions

> [!NOTE]  
> Esse recurso é novo no EF Core 2.1.

É possível usar as transações ambiente se você precisar coordenar em um escopo mais amplo.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Também é possível se inscrever em uma transação explícita.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>Limitações de System. Transactions  

1. Núcleo EF depende de provedores de banco de dados para implementar o suporte para System. Transactions. Embora o suporte é muito comum entre os provedores ADO.NET para .NET Framework, a API só foi adicionado recentemente ao .NET Core e suporte, portanto, não estará tão difundido. Se um provedor não implementa o suporte para System. Transactions, é possível que chamadas para essas APIs serão ignoradas completamente. SqlClient para .NET Core dá suporte a ele em 2.1 em diante. SqlClient para .NET Core 2.0 lançará uma exceção de você tentar usar o recurso. 

   > [!IMPORTANT]  
   > É recomendável que você teste que a API funcione corretamente com seu provedor antes de você confiar para o gerenciamento de transações. Você é incentivado a entrar em contato com o mantenedor do provedor de banco de dados se não existir. 

2. A partir da versão 2.1, a implementação de System. Transactions no núcleo do .NET não dá suporte para transações distribuídas, portanto você não pode usar `TransactionScope` ou `CommitableTransaction`para coordenar transações em vários gerenciadores de recursos. 
