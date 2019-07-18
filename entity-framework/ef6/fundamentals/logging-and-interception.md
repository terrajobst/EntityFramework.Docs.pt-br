---
title: Log e interceptação de operações de banco de dados-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: be32ed114269543ac36b256a202e0494d466e4f7
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306539"
---
# <a name="logging-and-intercepting-database-operations"></a>Log e interceptação de operações de banco de dados
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

A partir do Entity Framework 6, a qualquer momento Entity Framework envia um comando para o banco de dados esse comando pode ser interceptado pelo código do aplicativo. Isso é mais comumente usado para registrar em log o SQL, mas também pode ser usado para modificar ou anular o comando.  

Especificamente, o EF inclui:  

- Uma propriedade de log para o contexto semelhante a DataContext. log em LINQ to SQL  
- Um mecanismo para personalizar o conteúdo e a formatação da saída enviada ao log  
- Blocos de construção de baixo nível para interceptação, proporcionando maior controle/flexibilidade  

## <a name="context-log-property"></a>Propriedade de log de contexto  

A propriedade DbContext. Database. log pode ser definida como um delegado para qualquer método que usa uma cadeia de caracteres. Normalmente, ele é usado com qualquer TextWriter definindo-o como o método de "gravação" desse TextWriter. Todo o SQL gerado pelo contexto atual será registrado nesse gravador. Por exemplo, o código a seguir irá registrar o SQL no console:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Observe que o contexto. Database. log está definido como console. Write. Isso é tudo o que é necessário para registrar o SQL no console.  

Vamos adicionar um código simples de consulta/inserção/atualização para que possamos ver alguma saída:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Isso irá gerar a seguinte saída:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Observe que essa é a saída supondo que qualquer inicialização de banco de dados já tenha ocorrido. Se a inicialização do banco de dados ainda não tiver acontecido, haveria muito mais saída mostrando que todas as migrações de trabalho faz isso nos bastidores para verificar ou criar um novo banco de dados.)  

## <a name="what-gets-logged"></a>O que é registrado em log?  

Quando a propriedade log for definida, todos os itens a seguir serão registrados em log:  

- SQL para todos os diferentes tipos de comandos. Por exemplo:  
    - Consultas, incluindo consultas comuns do LINQ, consultas do eSQL e consultas brutas de métodos como SQLQuery  
    - Inserções, atualizações e exclusões geradas como parte do SaveChanges  
    - Relações de carregamento de relacionamento como aquelas geradas pelo carregamento lento  
- Parâmetros  
- Se o comando está sendo executado de forma assíncrona  
- Um carimbo de data/hora que indica quando o comando iniciou a execução  
- Se o comando foi concluído com êxito, se houve falha ao lançar uma exceção ou, para Async, foi cancelado  
- Alguma indicação do valor de resultado  
- A quantidade aproximada de tempo necessário para executar o comando. Observe que essa é a hora de enviar o comando para obter o objeto de resultado de volta. Ele não inclui tempo para ler os resultados.  

Observando a saída de exemplo acima, cada um dos quatro comandos registrados é:  

- A consulta resultante da chamada ao contexto. Blogs. primeiro  
    - Observe que o método ToString de obter o SQL não teria trabalhado para essa consulta, pois "First" não fornece um IQueryable no qual ToString poderia ser chamado  
- A consulta resultante do carregamento lento do blog. Postagens  
    - Observe os detalhes do parâmetro para o valor de chave para o qual o carregamento lento está ocorrendo  
    - Somente as propriedades do parâmetro que são definidas como valores não padrão são registradas em log. Por exemplo, a propriedade Size só será mostrada se for diferente de zero.  
- Dois comandos resultantes de SaveChangesAsync; um para a atualização alterar um título de postagem, o outro para uma inserção para adicionar uma nova postagem  
    - Observe os detalhes do parâmetro para as propriedades FK e title  
    - Observe que esses comandos estão sendo executados de forma assíncrona  

## <a name="logging-to-different-places"></a>Registro em locais diferentes  

Como mostrado acima, o registro em log no console é muito fácil. Também é fácil fazer logon na memória, no arquivo, etc. usando diferentes tipos de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Se você estiver familiarizado com LINQ to SQL pode observar que, em LINQ to SQL a propriedade log é definida como o objeto TextWriter real (por exemplo, console. out) enquanto estiver no EF, a propriedade log é definida como um método que aceita uma cadeia de caracteres (por exemplo, , Console. Write ou console. out. Write). O motivo disso é desacoplar o EF do TextWriter aceitando qualquer delegado que possa atuar como um coletor de cadeias de caracteres. Por exemplo, imagine que você já tem alguma estrutura de registro em log e define um método de log como este:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Isso pode ser conectado à propriedade log do EF da seguinte maneira:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Log de resultados  

O agente de log padrão registra o texto do comando (SQL), os parâmetros e a linha "executando" com um carimbo de data/hora antes de o comando ser enviado ao banco de dados. Uma linha "concluída" contendo tempo decorrido é registrada após a execução do comando.  

Observe que, para comandos assíncronos, a linha "concluída" não é registrada até que a tarefa assíncrona seja realmente concluída, falhe ou seja cancelada.  

A linha "concluída" contém informações diferentes dependendo do tipo de comando e se a execução foi ou não bem-sucedida.  

### <a name="successful-execution"></a>Execução bem-sucedida  

Para comandos que são concluídos com êxito a saída é "concluído em x ms com resultado:" seguido por alguma indicação do que é o resultado. Para comandos que retornam um leitor de dados, a indicação de resultado é o tipo de [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) retornado. Para comandos que retornam um valor inteiro, como o comando Update mostrado acima, o resultado mostrado é o inteiro.  

### <a name="failed-execution"></a>Falha na execução  

Para comandos que falham lançando uma exceção, a saída contém a mensagem da exceção. Por exemplo, usar SQLQuery para consultar uma tabela que exista resultará na saída do log, algo assim:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Execução cancelada  

Para comandos assíncronos em que a tarefa é cancelada, o resultado pode ser uma falha com uma exceção, pois isso é o que o provedor ADO.NET subjacente geralmente faz quando é feita uma tentativa de cancelamento. Se isso não acontecer e a tarefa for cancelada corretamente, a saída terá uma aparência semelhante a esta:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Alterando o conteúdo e a formatação do log  

Nos bastidores, a Propriedade Database. log usa um objeto DatabaseLogFormatter. Esse objeto associa efetivamente uma implementação de IDbCommandInterceptor (veja abaixo) a um delegado que aceita cadeias de caracteres e um DbContext. Isso significa que os métodos em DatabaseLogFormatter são chamados antes e depois da execução de comandos pelo EF. Esses métodos DatabaseLogFormatter reúnem e formatam a saída de log e a enviam para o delegado.  

### <a name="customizing-databaselogformatter"></a>Personalizando o DatabaseLogFormatter  

Alterar o que é registrado em log e como ele é formatado pode ser obtido criando uma nova classe que deriva de DatabaseLogFormatter e substitui métodos conforme apropriado. Os métodos mais comuns a serem substituídos são:  

- LogCommand – substitua isso para alterar como os comandos são registrados antes de serem executados. Por padrão, o LogCommand chama LogParameter para cada parâmetro; Você pode optar por fazer o mesmo em sua substituição ou manipular parâmetros de maneira diferente em vez disso.  
- LogResult – substitua isso para alterar a forma como o resultado da execução de um comando é registrado.  
- LogParameter – substitua isso para alterar a formatação e o conteúdo do log de parâmetros.  

Por exemplo, suponha que queríamos registrar apenas uma única linha antes de cada comando ser enviado ao banco de dados. Isso pode ser feito com duas substituições:  

- Substituir LogCommand para formatar e gravar a única linha de SQL  
- Substitua LogResult para não fazer nada.  

O código seria semelhante a este:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Para a saída de log, basta chamar o método Write que enviará a saída para o delegado de gravação configurado.  

(Observe que esse código faz uma remoção simplista de quebras de linha, apenas como um exemplo. Provavelmente, não funcionará bem para exibir SQL complexo.)  

### <a name="setting-the-databaselogformatter"></a>Configurando o DatabaseLogFormatter  

Depois que uma nova classe DatabaseLogFormatter tiver sido criada, precisará ser registrada com o EF. Isso é feito usando a configuração baseada em código. Resumindo, isso significa criar uma nova classe que deriva de DbConfiguration no mesmo assembly que a classe DbContext e, em seguida, chamando SetDatabaseLogFormatter no construtor dessa nova classe. Por exemplo:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Usando o novo DatabaseLogFormatter  

Esse novo DatabaseLogFormatter agora será usado sempre que Database. log for definido. Assim, executar o código da parte 1 agora resultará na seguinte saída:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Blocos de construção de interceptação  

Até aqui, vimos como usar DbContext. Database. log para registrar em log o SQL gerado pelo EF. Mas esse código é, na verdade, uma fachada relativamente fina em alguns blocos de construção de baixo nível para uma interceptação mais geral.  

### <a name="interception-interfaces"></a>Interfaces de interceptação  

O código de interceptação é criado em relação ao conceito de interfaces de interceptação. Essas interfaces herdam de IDbInterceptor e definem métodos que são chamados quando o EF executa alguma ação. A intenção é ter uma interface por tipo de objeto que está sendo interceptado. Por exemplo, a interface IDbCommandInterceptor define métodos que são chamados antes do EF fazer uma chamada para ExecuteNonQuery, ExecuteScalar, ExecuteReader e métodos relacionados. Da mesma forma, a interface define os métodos que são chamados quando cada uma dessas operações é concluída. A classe DatabaseLogFormatter que vimos acima implementa essa interface para fazer log de comandos.  

### <a name="the-interception-context"></a>O contexto de interceptação  

Observando os métodos definidos em qualquer uma das interfaces do Interceptor, é aparente que cada chamada recebe um objeto do tipo DbInterceptionContext ou algum tipo derivado dele, como DbCommandInterceptionContext\<.\> Este objeto contém informações contextuais sobre a ação que o EF está assumindo. Por exemplo, se a ação estiver sendo executada em nome de um DbContext, o DbContext será incluído no DbInterceptionContext. Da mesma forma, para comandos que estão sendo executados de forma assíncrona, o sinalizador IsAsync é definido em DbCommandInterceptionContext.  

### <a name="result-handling"></a>Manipulação de resultados  

A classe\< DbCommandInterceptionContext\> contém uma propriedade chamada Result, OriginalResult, Exception e OriginalException. Essas propriedades são definidas como NULL/zero para chamadas para os métodos de interceptação que são chamados antes da execução da operação — ou seja, para o... Executando métodos. Se a operação for executada e tiver sucesso, Result e OriginalResult serão definidos como o resultado da operação. Esses valores podem ser observados nos métodos de interceptação que são chamados depois que a operação é executada — ou seja, no... Métodos executados. Da mesma forma, se a operação for lançada, a exceção e as propriedades OriginalException serão definidas.  

#### <a name="suppressing-execution"></a>Suprimindo a execução  

Se um interceptor definir a propriedade Result antes da execução do comando (em um dos... Executando métodos) em seguida, o EF não tentará executar o comando de fato, mas, em vez disso, usará apenas o conjunto de resultados. Em outras palavras, o interceptador pode suprimir a execução do comando, mas tem o EF continuar como se o comando tivesse sido executado.  

Um exemplo de como isso pode ser usado é o comando em lotes que tradicionalmente foi feito com um provedor de encapsulamento. O Interceptor armazenaria o comando para execução posterior como um lote, mas "fingir" para o EF que o comando executou normalmente. Observe que ele requer mais do que isso para implementar o envio em lote, mas este é um exemplo de como a alteração do resultado da interceptação pode ser usada.  

A execução também pode ser suprimida com a definição da propriedade Exception em um dos... Executando métodos. Isso faz com que o EF continue como se a execução da operação tivesse falhado lançando a exceção fornecida. Isso pode, naturalmente, fazer com que o aplicativo falhe, mas também pode ser uma exceção transitória ou alguma outra exceção que seja tratada pelo EF. Por exemplo, isso pode ser usado em ambientes de teste para testar o comportamento de um aplicativo quando a execução do comando falha.  

#### <a name="changing-the-result-after-execution"></a>Alterando o resultado após a execução  

Se um interceptor definir a propriedade Result depois que o comando for executado (em um dos... Métodos executados), o EF usará o resultado alterado em vez do resultado que foi realmente retornado da operação. Da mesma forma, se um interceptor definir a propriedade Exception depois que o comando for executado, o EF gerará a exceção Set como se a operação tivesse gerado a exceção.  

Um interceptor também pode definir a propriedade Exception como NULL para indicar que nenhuma exceção deve ser lançada. Isso pode ser útil se a execução da operação falhar, mas o Interceptor desejar que o EF continue como se a operação tivesse sido bem-sucedida. Isso geralmente também envolve a definição do resultado para que o EF tenha algum valor de resultado para trabalhar enquanto continua.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult e OriginalException  

Depois que o EF tiver executado uma operação, ele definirá as propriedades Result e OriginalResult se a execução não falhar ou a exceção e as propriedades OriginalException se a execução falhar com uma exceção.  

As propriedades OriginalResult e original são somente leitura e só são definidas pelo EF depois de realmente executar uma operação. Essas propriedades não podem ser definidas por interceptores. Isso significa que qualquer interceptador pode distinguir entre uma exceção ou resultado que foi definido por outro interceptador, em oposição à exceção real ou resultado que ocorreu quando a operação foi executada.  

### <a name="registering-interceptors"></a>Registrando interceptores  

Depois que uma classe que implementa uma ou mais das interfaces de interceptação tiver sido criada, ela poderá ser registrada com o EF usando a classe DbInterception. Por exemplo:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Os interceptores também podem ser registrados no nível de domínio do aplicativo usando o mecanismo de configuração com base em código DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Exemplo: Registrando em log no NLog  

Vamos reunir tudo isso em um exemplo que usa IDbCommandInterceptor e [NLog](http://nlog-project.org/) para:  

- Registrar um aviso para qualquer comando executado de forma não assíncrona  
- Registrar um erro para qualquer comando que é acionado quando executado  

Aqui está a classe que faz o registro em log, que deve ser registrado como mostrado acima:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Observe como esse código usa o contexto de interceptação para descobrir quando um comando está sendo executado de forma não assíncrona e descobrir quando ocorreu um erro ao executar um comando.  
