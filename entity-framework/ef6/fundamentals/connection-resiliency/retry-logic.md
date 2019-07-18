---
title: Resiliência de conexão e lógica de repetição-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: a01216c3399ca4a04943563435eacd0047337a5f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306570"
---
# <a name="connection-resiliency-and-retry-logic"></a>Resiliência de conexão e lógica de repetição
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Os aplicativos que se conectam a um servidor de banco de dados sempre foram vulneráveis a quebras de conexão devido a falhas de back-end e instabilidade de rede. No entanto, em um ambiente baseado em LAN que trabalha com servidores de banco de dados dedicados, esses erros são raros o suficiente para que a lógica extra para lidar com essas falhas não seja geralmente necessária. Com o aumento dos servidores de banco de dados baseados em nuvem, como o banco de dados SQL do Windows Azure e as conexões em redes menos confiáveis, agora é mais comum que as interrupções de conexão ocorram. Isso pode ser devido a técnicas defensivas que os bancos de dados de nuvem usam para garantir a imparcialidade do serviço, como a limitação de conexão ou a instabilidade na rede, causando tempos limite intermitentes e outros erros transitórios.  

A resiliência da conexão refere-se à capacidade do EF de repetir automaticamente os comandos que falharem devido a essas quebras de conexão.  

## <a name="execution-strategies"></a>Estratégias de execução  

A repetição de conexão é encarregada por uma implementação da interface IDbExecutionStrategy. As implementações do IDbExecutionStrategy serão responsáveis por aceitar uma operação e, se ocorrer uma exceção, determinando se uma repetição é apropriada e tentando novamente se for. Há quatro estratégias de execução fornecidas com o EF:  

1. **DefaultExecutionStrategy**: essa estratégia de execução não repete nenhuma operação, é o padrão para bancos de dados diferentes do SQL Server.  
2. **DefaultSqlExecutionStrategy**: essa é uma estratégia de execução interna que é usada por padrão. No entanto, essa estratégia não é repetida. no entanto, ela encapsulará as exceções que poderiam ser transitórias para informar os usuários de que eles podem querer habilitar a resiliência da conexão.  
3. **DbExecutionStrategy**: essa classe é adequada como uma classe base para outras estratégias de execução, incluindo suas próprias personalizadas. Ele implementa uma política de repetição exponencial, em que a nova tentativa inicial ocorre com um atraso zero e o atraso aumenta exponencialmente até que a contagem máxima de repetições seja atingida. Essa classe tem um método abstrato ShouldRetryOn que pode ser implementado em estratégias de execução derivadas para controlar quais exceções devem ser repetidas.  
4. **SqlAzureExecutionStrategy**: essa estratégia de execução herda de DbExecutionStrategy e tentará novamente as exceções conhecidas como possivelmente transitórias ao trabalhar com o banco de dados SQL do Azure.

> [!NOTE]
> As estratégias de execução 2 e 4 estão incluídas no provedor do SQL Server que acompanha o EF, que está no assembly do EntityFramework. SqlServer e foi projetado para trabalhar com SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Habilitando uma estratégia de execução  

A maneira mais fácil de dizer ao EF para usar uma estratégia de execução é com o método SetExecutionStrategy da classe [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Esse código informa ao EF para usar o SqlAzureExecutionStrategy ao se conectar a SQL Server.  

## <a name="configuring-the-execution-strategy"></a>Configurando a estratégia de execução  

O construtor de SqlAzureExecutionStrategy pode aceitar dois parâmetros, MaxRetryCount e MaxDelay. Contagem de MaxRetry é o número máximo de vezes que a estratégia tentará novamente. O MaxDelay é um TimeSpan que representa o atraso máximo entre as tentativas que serão usadas pela estratégia de execução.  

Para definir o número máximo de repetições como 1 e o atraso máximo para 30 segundos, você deve executar o seguinte:  

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

O SqlAzureExecutionStrategy tentará instantaneamente na primeira vez que ocorrer uma falha transitória, mas atrasará mais tempo entre cada repetição até que o limite máximo de tentativas seja excedido ou o tempo total atinja o atraso máximo.  

As estratégias de execução tentarão apenas um número limitado de exceções que normalmente são transitórias, você ainda precisará lidar com outros erros, bem como capturar a exceção RetryLimitExceeded para o caso em que um erro não é transitório ou leva muito tempo para ser resolvido próprio.  

Há algumas limitações conhecidas ao usar uma estratégia de execução de repetição:  

## <a name="streaming-queries-are-not-supported"></a>Não há suporte para consultas de streaming  

Por padrão, o EF6 e a versão posterior armazenarão em buffer os resultados da consulta em vez de transmiti-los. Se desejar que os resultados sejam transmitidos, você poderá usar o método de transmissão para alterar uma consulta de LINQ to Entities para streaming.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Não há suporte para streaming quando uma estratégia de execução de repetição é registrada. Essa limitação existe porque a conexão pode encaixar a parte por meio dos resultados retornados. Quando isso ocorre, o EF precisa executar novamente a consulta inteira, mas não tem uma maneira confiável de saber quais resultados já foram retornados (os dados podem ter sido alterados desde que a consulta inicial foi enviada, os resultados podem voltar em uma ordem diferente, os resultados podem não ter um identificador exclusivo , etc.).  

## <a name="user-initiated-transactions-are-not-supported"></a>Não há suporte para transações iniciadas pelo usuário  

Quando você configurou uma estratégia de execução que resulta em novas tentativas, há algumas limitações em relação ao uso de transações.  

Por padrão, o EF executará qualquer atualização de banco de dados em uma transação. Você não precisa fazer nada para habilitar isso, o EF sempre faz isso automaticamente.  

Por exemplo, no código a seguir, SaveChanges é executado automaticamente em uma transação. Se o SaveChanges falhar após a inserção de um dos novos sites, a transação seria revertida e nenhuma alteração foi aplicada ao banco de dados. O contexto também é deixado em um estado que permite que SaveChanges seja chamado novamente para tentar aplicar as alterações novamente.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

Quando não estiver usando uma estratégia de execução de repetição, você poderá encapsular várias operações em uma única transação. Por exemplo, o código a seguir encapsula duas chamadas SaveChanges em uma única transação. Se qualquer parte de uma operação falhar, nenhuma das alterações será aplicada.  

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

Isso não é suportado quando se usa uma estratégia de execução de repetição porque o EF não reconhece nenhuma operação anterior e como repeti-las. Por exemplo, se o segundo SaveChanges falhar, o EF não terá mais as informações necessárias para tentar novamente a primeira chamada SaveChanges.  

### <a name="workaround-suspend-execution-strategy"></a>Solução alternativa: Suspender estratégia de execução  

Uma solução possível é suspender a estratégia de execução de repetição para o trecho de código que precisa usar uma transação iniciada pelo usuário. A maneira mais fácil de fazer isso é adicionar um sinalizador SuspendExecutionStrategy à sua classe de configuração baseada em código e alterar o lambda da estratégia de execução para retornar a estratégia de execução padrão (sem revinculação) quando o sinalizador for definido.  

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

Observe que estamos usando CallContext para armazenar o valor do sinalizador. Isso fornece uma funcionalidade semelhante ao armazenamento local de thread, mas é seguro usá-lo com código assíncrono, incluindo a consulta assíncrona e salvar com Entity Framework.  

Agora podemos suspender a estratégia de execução para a seção de código que usa uma transação iniciada pelo usuário.  

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

Outra opção é usar manualmente a estratégia de execução e dar a ela o conjunto inteiro de lógica a ser executado, para que possa tentar novamente tudo se uma das operações falhar. Ainda precisamos suspender a estratégia de execução-usando a técnica mostrada acima, de modo que os contextos usados dentro do bloco de código com nova tentativa não tentem novamente.  

Observe que os contextos devem ser construídos dentro do bloco de código a ser repetido. Isso garante que estamos começando com um estado limpo para cada repetição.  

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
