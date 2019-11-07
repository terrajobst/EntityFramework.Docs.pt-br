---
title: Transações – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 952cb891d145a47666f1d506ec00f066be9f245d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654746"
---
# <a name="using-transactions"></a>Como usar transações

As transações permitem que várias operações de banco de dados sejam processadas de forma atômica. Se a transação for confirmada, todas as operações serão aplicadas com êxito ao banco de dados. Se a transação for revertida, nenhuma operação será aplicadas ao banco de dados.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) deste artigo no GitHub.

## <a name="default-transaction-behavior"></a>Comportamento de transação padrão

Por padrão, se o provedor de banco de dados oferecer suporte a transações, todas as alterações em uma única chamada para `SaveChanges()` serão aplicadas em uma transação. Se qualquer uma das alterações falhar, a transação é revertida e nenhuma das alterações será aplicada ao banco de dados. Isso significa que é garantido que o `SaveChanges()` terá êxito ou sairá do banco de dados sem modificação caso ocorra algum erro.

Para a maioria dos aplicativos, esse comportamento padrão é suficiente. Você só deve controlar as transações manualmente se os requisitos do aplicativo considerarem necessário.

## <a name="controlling-transactions"></a>Como controlar transações

Você pode usar a API do `DbContext.Database` para iniciar, confirmar e reverter transações. O exemplo a seguir mostra duas operações do `SaveChanges()` e uma consulta LINQ sendo executada em uma única transação.

Nem todos os provedores de banco de dados oferecem suporte a transações. Alguns provedores podem gerar ou ficar não operacional quando as APIs de transação forem chamadas.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transação entre diferentes contextos (somente bancos de dados relacionais)

Você também pode compartilhar uma transação em várias instâncias de contexto. Esta funcionalidade só está disponível ao usar um provedor de banco de dados relacional, porque ele requer o uso do `DbTransaction` e `DbConnection`, que são específicos para bancos de dados relacionais.

Para compartilhar uma transação, os contextos devem compartilhar um `DbConnection` e um `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Permitir que a conexão seja fornecido externamente

Compartilhar um `DbConnection` requer a capacidade de aprovar uma conexão em um contexto ao construí-la.

A maneira mais fácil de permitir que o `DbConnection` seja fornecido externamente é parar de usar o método `DbContext.OnConfiguring` para configurar o contexto e criar externamente `DbContextOptions` e aprová-los para o construtor de contexto.

> [!TIP]  
> `DbContextOptionsBuilder` é a API usada no `DbContext.OnConfiguring` para configurar o contexto e agora você irá usá-la externamente para criar `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Uma alternativa é continuar usando o `DbContext.OnConfiguring`, mas aceitar um `DbConnection` que seja salvo e, depois, usado no `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Compartilhar uma conexão e transação

Agora você pode criar várias instâncias de contexto que compartilham a mesma conexão. Em seguida, use a API do `DbContext.Database.UseTransaction(DbTransaction)` para inscrever os dois contextos na mesma transação.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Usando DbTransactions externos (somente bancos de dados relacionais)

Se você estiver usando várias tecnologias de acesso a dados para acessar um banco de dados relacional, é possível compartilhar uma transação entre operações executadas por essas diferentes tecnologias.

O exemplo a seguir mostra como executar uma operação do SqlClient do ADO.NET e uma operação do Entity Framework Core na mesma transação.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Como usar System.Transactions

> [!NOTE]  
> Este recurso é novo no EF Core 2.1.

É possível usar transações ambientes se você precisar coordenar um escopo mais amplo.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Também é possível se inscrever em uma transação explícita.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Limitações do System.Transactions  

1. O EF Core depende dos provedores de banco de dados para implementar o suporte para System.Transactions. Embora o suporte seja muito comum entre os provedores de ADO.NET para .NET Framework, a API só foi adicionada recentemente ao .NET Core e, portanto, o suporte ainda não está tão difundido. Se um provedor não implementar o suporte para System.Transactions, é possível que as chamadas para essas APIs sejam ignoradas completamente. O SqlClient para .NET Core não oferecerá suporte do 2.1 em diante. O SqlClient para .NET Core 2.0 gerará uma exceção se você tentar usar o recurso.

   > [!IMPORTANT]  
   > Recomendamos testar se a API funciona corretamente com seu provedor antes de depender dela para o gerenciamento de transações. Caso ela não funcione, fale com o mantenedor do provedor do banco de dados.

2. A partir da versão 2.1, a implementação do System.Transactions no .NET Core não incluirá o suporte a transações distribuídas, portanto, você não pode usar `TransactionScope` ou `CommittableTransaction` para coordenar transações em vários gerenciadores de recursos.
