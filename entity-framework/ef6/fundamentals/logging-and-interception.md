---
title: Registro em log e interceptar operações de banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 9a8be81af45d9f27caa8c26f66d219dc568b6604
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251265"
---
# <a name="logging-and-intercepting-database-operations"></a>Registro em log e interceptar operações de banco de dados
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Começando com o Entity Framework 6, sempre que o Entity Framework envia um comando para o banco de dados desse comando pode ser interceptada pelo código do aplicativo. Isso é mais comumente usado para registrar em log SQL, mas também pode ser usado para modificar ou anule o comando.  

Especificamente, o EF inclui:  

- Uma propriedade de Log para o contexto semelhante ao DataContext.Log em LINQ to SQL  
- Um mecanismo para personalizar o conteúdo e a formatação da saída enviada para o log  
- Blocos de construção de baixo nível para interceptação, oferecendo maior controle/flexibilidade  

## <a name="context-log-property"></a>Propriedade de registro de contexto  

A propriedade DbContext.Database.Log pode ser definida para um delegado para qualquer método que usa uma cadeia de caracteres. Mais comumente, ele é usado com qualquer TextWriter definindo-a para o método de "Gravação" do que TextWriter. Todos os SQL gerado pelo contexto atual será registrado para o gravador. Por exemplo, o código a seguir registrará em log SQL no console:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Observe que o contexto. Database. log é definido como console. Write. Isso é tudo o que é necessário para fazer logon SQL no console.  

Vamos adicionar alguns códigos simples de consulta/insert/update para que podemos ver algumas saídas:  

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

Isso irá gerar a saída a seguir:  

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

(Observe que isso é a saída, supondo que a inicialização de qualquer banco de dados já aconteceu. Se a inicialização do banco de dados já não ocorreu, em seguida, haverá muito mais saída, mostrando todo o trabalho migrações faz nos bastidores para verificar ou criar um novo banco de dados.)  

## <a name="what-gets-logged"></a>O que é registrado em log?  

Quando a propriedade de registro é definida todos os itens a seguir será registrada:  

- SQL para todos os diferentes tipos de comandos. Por exemplo:  
    - Consultas, incluindo consultas do LINQ normais, consultas eSQL e consultas brutas de métodos como SqlQuery  
    - Inserções, atualizações e exclusões geradas como parte de SaveChanges  
    - Carregar consultas, como aqueles gerados pelo carregamento lento de relação  
- Parâmetros  
- Se o comando está sendo executado assincronamente  
- Um carimbo de hora que indica que o comando iniciado executando  
- Se o comando foi concluído com êxito, falha, lançando uma exceção ou, para async, foi cancelada  
- Alguma indicação sobre o valor do resultado  
- O período de tempo aproximado necessário para executar o comando. Observe que esta é a vez de enviar o comando para obter o objeto de resultado de volta. Ele não inclui o tempo para ler os resultados.  

Examinar a saída de exemplo acima, cada um dos quatro comandos registrados são:  

- A consulta resultante da chamada para o contexto. Blogs.First  
    - Observe que o método ToString de obter o SQL não funcionaria para esta consulta desde "First" não fornece um IQueryable na qual ToString poderia ser chamado.  
- A consulta resultante do carregamento lento de blog. Postagens  
    - Observe que os detalhes do parâmetro para o valor da chave para a qual o carregamento lento está acontecendo  
    - Somente as propriedades do parâmetro que são definidas como valores não padrão são registradas. Por exemplo, a propriedade de tamanho é mostrada somente se ele for diferente de zero.  
- Dois comandos resultante de SaveChangesAsync; uma para a atualização alterar um título de postagem, o outro para uma instrução insert adicionar uma nova postagem  
    - Observe os detalhes do parâmetro para as propriedades Title e de FK  
    - Observe que esses comandos estão sendo executados de forma assíncrona  

## <a name="logging-to-different-places"></a>Registro em log para locais diferentes  

Conforme mostrado acima do registro em log para o console é muito fácil. Também é fácil fazer a memória, arquivo, etc. usando tipos diferentes de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Se você estiver familiarizado com o LINQ to SQL, você pode observar que em LINQ to SQL, a propriedade de Log é definida como TextWriter objeto real (por exemplo, console. out) ao mesmo tempo no EF, a propriedade de Log é definida como um método que aceita uma cadeia de caracteres (por exemplo Console. Write ou Console.Out.Write). A razão para isso é desacoplar o EF TextWriter, aceitando qualquer delegado que pode agir como um coletor para cadeias de caracteres. Por exemplo, imagine que você já tiver alguma estrutura de registro em log e define um método de registro em log da seguinte forma:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Isso pode ser vinculado a propriedade Log EF como este:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Registro em log o resultado  

O agente de log padrão registra o texto do comando (SQL), parâmetros e a linha "Executando" com um carimbo de hora antes do comando é enviado ao banco de dados. Uma linha "concluída", que contém o tempo decorrido é registrada seguindo a execução do comando.  

Observe que para comandos assíncronos a linha "concluída" não está registrada até que a tarefa assíncrona, na verdade, concluído, falha ou é cancelada.  

A linha "concluída" contém informações diferentes dependendo do tipo de comando e ou não a execução foi bem-sucedida.  

### <a name="successful-execution"></a>Execução bem-sucedida  

Para comandos que concluída com êxito a saída é "concluído em x ms com resultado:" seguido por alguma indicação de qual foi o resultado. Para comandos que retornam o resultado de um leitor de dados indicação é o tipo de [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) retornado. Para comandos que retornam um valor inteiro como a atualização de comando mostrado acima o resultado mostrado é esse inteiro.  

### <a name="failed-execution"></a>Execução com falha  

Para comandos que falham ao lançar uma exceção, a saída contém a mensagem da exceção. Por exemplo, usando SqlQuery para consulta em uma tabela que existe o resultado no log produzirá algo parecido com isto:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Execução cancelada  

Para comandos assíncronos em que a tarefa é cancelada o resultado poderia ser falha com uma exceção, já que este é o que o provedor ADO.NET subjacente geralmente faz quando é feita uma tentativa de cancelar. Se isso não acontecer e a tarefa é cancelada corretamente, em seguida, a saída será algo parecido com isso:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Alterar o conteúdo do log e formatação  

Nos bastidores a Database. log propriedade torna o uso de um objeto DatabaseLogFormatter. Esse objeto efetivamente associa uma implementação de IDbCommandInterceptor (veja abaixo) para um delegado que aceita cadeias de caracteres e um DbContext. Isso significa que métodos em DatabaseLogFormatter são chamados antes e após a execução de comandos pelo EF. Esses métodos DatabaseLogFormatter reunir e formatar a saída de log e enviá-lo ao delegado.  

### <a name="customizing-databaselogformatter"></a>Personalizando DatabaseLogFormatter  

Alterando o que é registrado e como ele é formatado pode ser obtido com a criação de uma nova classe que deriva de DatabaseLogFormatter e substitui os métodos conforme apropriado. Para substituir métodos mais comuns são:  

- LogCommand – substituí-lo para alterar como os comandos são registrados antes que eles são executados. Por padrão LogCommand chama LogParameter para cada parâmetro; Você pode optar por fazer o mesmo em sua substituição ou manipular parâmetros de forma diferente.  
- LogResult – substituí-lo para alterar como o resultado da execução de um comando é registrado.  
- LogParameter – substitui-lo para alterar a formatação e o conteúdo do registro em log do parâmetro.  

Por exemplo, vamos supor que queremos fazer apenas uma única linha antes de cada comando é enviado para o banco de dados. Isso pode ser feito com duas substituições:  

- Substituir LogCommand para formatar e gravar a única linha de SQL  
- Substitua LogResult para não fazer nada.  

O código seria algo parecido com isto:

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

Para registrar em log de saída chamada simplesmente o método de gravação para o qual envia a saída para o delegado de gravação configurado.  

(Observe que esse código faz remoção simplista de quebras de linha apenas como exemplo. Ele provavelmente não funcionará bem para a exibição SQL complexa.)  

### <a name="setting-the-databaselogformatter"></a>Definindo o DatabaseLogFormatter  

Depois que uma nova classe DatabaseLogFormatter tiver sido criada, ele precisa ser registrado com o EF. Isso é feito usando a configuração baseada em código. Em poucas palavras, isso significa criar uma nova classe que deriva de DbConfiguration no mesmo assembly como sua classe DbContext e, em seguida, chamando SetDatabaseLogFormatter no construtor dessa classe nova. Por exemplo:  

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

Esse novo DatabaseLogFormatter agora será usado sempre que o Database. log é definido. Portanto, executar o código da parte 1 agora resultará na seguinte saída:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Blocos de construção de interceptação  

Até agora, examinamos como usar DbContext.Database.Log para registrar o SQL gerado pelo EF. Mas esse código é realmente uma fachada relativamente fina sobre alguns blocos de construção de baixo nível para interceptação mais geral.  

### <a name="interception-interfaces"></a>Interfaces de interceptação  

O código de intercepção é criado em torno do conceito de interfaces de interceptação. Essas interfaces herdam de IDbInterceptor e definem métodos que são chamados quando o EF executa alguma ação. A intenção é ter uma interface por tipo de objeto a serem interceptado. Por exemplo, a interface de IDbCommandInterceptor define métodos que são chamados antes que o EF faz uma chamada para ExecuteNonQuery, ExecuteScalar, ExecuteReader e métodos relacionados. Da mesma forma, a interface define métodos que são chamados quando cada uma dessas operações é concluída. A classe DatabaseLogFormatter que analisamos acima implementa essa interface para fazer logon de comandos.  

### <a name="the-interception-context"></a>O contexto de interceptação  

Examinando os métodos definidos em qualquer uma das interfaces interceptador, ele fica aparente que cada chamada é fornecido um objeto do tipo DbInterceptionContext ou algum tipo derivada de isso, como DbCommandInterceptionContext\<\>. Este objeto contém informações contextuais sobre a ação que o EF está demorando. Por exemplo, se a ação está sendo executada em nome de um DbContext, o DbContext está incluído no DbInterceptionContext. Da mesma forma, para comandos que estão sendo executados de forma assíncrona, o sinalizador de IsAsync é definido em DbCommandInterceptionContext.  

### <a name="result-handling"></a>Tratamento de resultado  

O DbCommandInterceptionContext\< \> classe contém um propriedades chamadas OriginalException, OriginalResult, exceção e resultado. Essas propriedades são definidas como nulo/zero para chamadas para os métodos de interceptação que são chamados antes da execução da operação — ou seja, para o... Executar métodos. Se a operação é executada e for bem-sucedida, em seguida, resultado e OriginalResult são definidos para o resultado da operação. Esses valores, em seguida, podem ser observados nos métodos de interceptação que são chamados após a operação executada — isto é, na... Métodos executados. Da mesma forma, se a operação gera, em seguida, as propriedades de exceção e OriginalException serão definidas.  

#### <a name="suppressing-execution"></a>Suprimir a execução  

Se um interceptor define a propriedade de resultado antes do comando foi executado (em uma da... Executar métodos), em seguida, o EF não tentar executar o comando, mas em vez disso, basta usar o conjunto de resultados. Em outras palavras, o interceptador pode suprimir a execução do comando mas EF continuará como se o comando foi executado.  

Um exemplo de como isso pode ser usado é o comando de envio em lote que tradicionalmente foi feito com um provedor de encapsulamento. O interceptador seria armazenar o comando para execução posterior como um lote, mas seria "fingem" EF que executou o comando como de costume. Observe que ela requer mais do que isso para implementar o envio em lote, mas este é um exemplo de como alterar o resultado de interceptação pode ser usado.  

Execução também pode ser suprimida, definindo a propriedade de exceção em uma da... Executar métodos. Isso faz com que o EF continuar como se a execução da operação falhou, lançando a exceção apresentada. Isso pode, obviamente, fazem com que o aplicativo falhe, mas também pode ser uma exceção transitória ou outra exceção que é tratada pelo EF. Por exemplo, isso pode ser usado em ambientes de teste para testar o comportamento de um aplicativo quando ocorre falha na execução do comando.  

#### <a name="changing-the-result-after-execution"></a>Alterar o resultado após a execução  

Se um interceptor define a propriedade Result depois que o comando for executado (em uma da... Executado métodos), o EF usará o resultado alterado em vez do resultado que, na verdade, foi retornado da operação. Da mesma forma, se um interceptor define a propriedade de exceção depois que o comando for executado, em seguida, EF gerará a exceção de conjunto como se a operação teve lançou a exceção.  

Um interceptor também pode definir a propriedade de exceção como nulo para indicar que nenhuma exceção deverá ser gerada. Isso pode ser útil se a falha na execução da operação, mas deseja que o interceptador de EF para continuar como se a operação foi executado com êxito. Isso geralmente também envolve a configuração o resultado de forma que o EF tem algum valor de resultado para trabalhar com como ele continua.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult e OriginalException  

Depois que o EF executou uma operação definirá as propriedades de resultado e OriginalResult se a execução não falhou ou as propriedades de exceção e OriginalException se a falha na execução com uma exceção.  

As propriedades OriginalResult e OriginalException são somente leitura e somente são definidas pelo EF após a execução, na verdade, uma operação. Essas propriedades não podem ser definidas por interceptores. Isso significa que qualquer interceptador pode distinguir entre uma exceção ou o resultado foi definido por algum outro interceptador em vez da exceção real ou que ocorreram quando a operação foi executada.  

### <a name="registering-interceptors"></a>Registrando interceptores  

Após a criação de uma classe que implementa uma ou mais das interfaces de interceptação pode ser registrado com o EF usando a classe DbInterception. Por exemplo:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Interceptadores também podem ser registrados no nível do domínio de aplicativo usando o mecanismo de configuração com base em código DbConfiguration.  

### <a name="example-logging-to-nlog"></a>Exemplo: O log NLog  

Vamos reunir tudo isso em um exemplo que o uso de IDbCommandInterceptor e [NLog](http://nlog-project.org/) para:  

- Registrar um aviso para qualquer comando que é executado não de forma assíncrona  
- Registrar um erro para qualquer comando que gera quando executado  

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

Observe como esse código usa o contexto de interceptação para descobrir quando um comando está sendo executado não de forma assíncrona e para descobrir quando houve um erro ao executar um comando.  
