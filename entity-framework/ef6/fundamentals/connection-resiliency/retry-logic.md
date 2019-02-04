---
title: Conexão resiliência e lógica de repetição - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 7d6aa870cc32a2b344457fbb04525a7c2c8d1c61
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668759"
---
# <a name="connection-resiliency-and-retry-logic"></a>Conexão resiliência e lógica de repetição
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Aplicativos que se conectam a um servidor de banco de dados sempre foram vulneráveis a interrupções de conexão devido a falhas de back-end e instabilidade de rede. No entanto, em um ambiente de LAN com base em trabalhar com servidores de banco de dados dedicado, esses erros são bastante raros que lógica extra para lidar com essas falhas geralmente não é necessária. Com o aumento da nuvem com base conexões e servidores de banco de dados como banco de dados SQL do Windows Azure em redes menos confiáveis agora é mais comum para quebras de conexão ocorra. Isso pode ser devido a técnicas de defesa que bancos de dados de uso para garantir a integridade do serviço, como as limitações de conexão ou à instabilidade na rede, causando tempos limite de intermitente e outros erros transitórios de nuvem.  

Resiliência de Conexão refere-se à capacidade para o EF repetir automaticamente a todos os comandos que falharem devido a essas quebras de conexão.  

## <a name="execution-strategies"></a>Estratégias de execução  

Repetição de Conexão é tratada por uma implementação da interface IDbExecutionStrategy. Implementações do IDbExecutionStrategy será responsáveis por aceitar uma operação e, se ocorrer uma exceção, determinando se uma repetição é apropriada e tentar novamente se ele for. Há quatro estratégias de execução que são fornecidos com o EF:  

1. **DefaultExecutionStrategy**: essa estratégia de execução não tenta novamente todas as operações, é o padrão para bancos de dados diferente do sql server.  
2. **DefaultSqlExecutionStrategy**: isso é uma estratégia de execução interna que é usada por padrão. Essa estratégia não é repetida, no entanto, ele vai encapsular quaisquer exceções que poderiam ser transitórias para informar os usuários que desejam habilitar a resiliência de conexão.  
3. **DbExecutionStrategy**: esta classe é adequada como uma classe base para outras estratégias de execução, incluindo seus próprios conectores personalizados. Ele implementa uma política de repetição exponencial, onde a repetição inicial acontece com zero atraso e o atraso aumenta exponencialmente até que a contagem máxima de novas tentativas for atingida. Essa classe tem um método abstrato ShouldRetryOn que pode ser implementado em estratégias de execução derivada para controlar quais exceções devem ser recuperadas.  
4. **SqlAzureExecutionStrategy**: essa estratégia de execução herda DbExecutionStrategy e tentará novamente no exceções que são conhecidas como transitório, possivelmente, ao trabalhar com o banco de dados SQL.

> [!NOTE]
> Estratégias de execução 2 e 4 são incluídas no provedor do Sql Server que é fornecido com o EF, que está no assembly EntityFramework. SQLServer e são projetadas para trabalhar com o SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Habilitando uma estratégia de execução  

A maneira mais fácil de informar ao EF para usar uma estratégia de execução é com o método SetExecutionStrategy a [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Esse código diz ao EF para usar o SqlAzureExecutionStrategy ao se conectar ao SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Configurando a estratégia de execução  

O construtor do SqlAzureExecutionStrategy pode aceitar dois parâmetros, MaxRetryCount e MaxDelay. Contagem de MaxRetry é o número máximo de vezes que a estratégia será tentada. O MaxDelay é um TimeSpan que representa o atraso máximo entre novas tentativas que usará a estratégia de execução.  

Para definir o número máximo de tentativas para 1 e o atraso máximo para 30 segundos, você faria execue o seguinte:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

A SqlAzureExecutionStrategy tentará instantaneamente na primeira vez que uma falha temporária ocorre, mas atrasará mais entre cada repetição até que o limite de repetição de ambos o máximo for excedida, ou o tempo total atinge o atraso máximo.  

As estratégias de execução tentará apenas um número limitado de exceções que são geralmente tansient, você ainda precisará lidar com outros erros, bem como capturar a exceção RetryLimitExceeded para o caso em que um erro não é transitório ou levar muito tempo resolver em si.  

Há alguns conhecidos de limitações ao usar uma estratégia de execução tentando novamente:  

## <a name="streaming-queries-are-not-supported"></a>Não há suporte para consultas de streaming  

Por padrão, EF6 e versões posteriores armazenará em buffer os resultados da consulta em vez de streaming-los. Se você quiser ter resultados transmitidos você pode usar o método AsStreaming para alterar um LINQ para consulta de entidades para streaming.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Não há suporte para o fluxo quando uma estratégia de execução tentando novamente é registrada. Essa limitação existe porque a conexão foi possível descartar metade do caminho pelos resultados que estão sendo retornados. Quando isso ocorrer, o EF precisa executar novamente a consulta inteira, mas não tem nenhuma maneira confiável de saber quais resultados já foram retornados (dados podem ter sido alterado desde a consulta inicial foi enviada, os resultados podem vir de volta em uma ordem diferente, os resultados não podem ter um identificador exclusivo etc.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Não há suporte para transações iniciadas pelo usuário  

Quando você tiver configurado uma estratégia de execução que resulta em tentativas, há algumas limitações em torno do uso de transações.  

Por padrão, o EF executará todas as atualizações de banco de dados dentro de uma transação. Você não precisa fazer nada para habilitar isso, o EF sempre faz isso automaticamente.  

Por exemplo, no código a seguir SaveChanges é executada automaticamente em uma transação. Se SaveChanges falhar depois de inserir uma do novo Site e em seguida, a transação deve ser revertida e nenhuma alteração aplicada ao banco de dados. O contexto também for deixado em um estado que permite que o SaveChanges ser chamado novamente para tentar novamente a aplicação das alterações.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Quando não estiver usando uma estratégia de execução tentando novamente, você pode encapsular várias operações em uma única transação. Por exemplo, o seguinte código conclui duas chamadas SaveChanges em uma única transação. Se qualquer parte de uma ou outra operação falhar, em seguida, nenhuma alteração será aplicada.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Isso não é suportado ao usar uma estratégia de execução tentando novamente porque o EF não está ciente de todas as operações anteriores e como repeti-los. Por exemplo, se o SaveChanges segunda falha, em seguida, EF não tem as informações necessárias para repetir a primeira chamada SaveChanges.  

### <a name="workaround-suspend-execution-strategy"></a>Solução alternativa: Suspender estratégia de execução  

Uma solução alternativa é possível suspender a estratégia de execução tentando novamente para o trecho de código que precisa usar um usuário iniciou a transação. A maneira mais fácil de fazer isso é adicionar um sinalizador SuspendExecutionStrategy ao seu código com base em classe de configuração e altere o lambda de estratégia de execução para retornar a estratégia de execução padrão (não retying) quando o sinalizador é definido.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy") ?? false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Observe que estamos usando CallContext para armazenar o valor do sinalizador. Isso fornece uma funcionalidade semelhante para o armazenamento local de thread, mas é seguro usar com o código assíncrono - incluindo consulta assíncrona e salvar com o Entity Framework.  

Agora podemos pode suspender a estratégia de execução para a seção de código que usa uma transação iniciada pelo usuário.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a>Solução alternativa: Chamar manualmente a estratégia de execução  

Outra opção é usar a estratégia de execução manualmente e dê a ele o conjunto inteiro de lógica a ser executado, para que ele pode repetir tudo o que se uma das operações falhar. Ainda precisamos suspender a estratégia de execução - usando a técnica mostrada acima - para que nenhum contexto usado dentro do bloco de código com nova tentativa não tentará repetir a ação.  

Observe que nenhum contexto deve ser construído dentro do bloco de código para ser repetida. Isso garante que estamos começando com um estado limpo para cada repetição.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
