---
title: Cadeias de caracteres de Conexão e modelos - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490734"
---
# <a name="connection-strings-and-models"></a>Modelos e cadeias de caracteres de Conexão
Este tópico aborda como o Entity Framework descobre qual conexão de banco de dados para usar e como você pode alterá-la. Modelos criados com o Code First e o EF Designer são abordados neste tópico.  

Normalmente, um aplicativo do Entity Framework usa uma classe derivada de DbContext. Essa classe derivada chamará um dos construtores na classe base DbContext ao controle:  

- Como o contexto se conectará ao banco de dados — ou seja, como uma cadeia de caracteres de conexão é usado/encontrado  
- Se usará o contexto de calcular um modelo usando o Code First ou carregar um modelo criado com o EF Designer  
- Opções avançadas  

Os fragmentos a seguir mostram que algumas maneiras dos construtores DbContext podem ser usadas.  

## <a name="use-code-first-with-connection-by-convention"></a>Usar o Code First com conexão por convenção  

Se você não tiver feito qualquer outra configuração em seu aplicativo, em seguida, chamar o construtor sem parâmetros em DbContext fará com que DbContext para executar no modo de Code First com uma conexão de banco de dados criado por convenção. Por exemplo:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

Neste exemplo DbContext usa o nome qualificado de namespace de seu contexto derivado de class—Demo.EF.BloggingContext—as o nome do banco de dados e cria uma cadeia de conexão para este banco de dados usando o SQL Express ou LocalDB. Se ambos estiverem instaladas, o SQL Express será usado.  

Visual Studio 2010 inclui o SQL Express por padrão e o Visual Studio 2012 e posterior inclui LocalDB. Durante a instalação, o pacote EntityFramework NuGet verifica qual servidor de banco de dados está disponível. O pacote do NuGet, em seguida, atualizará o arquivo de configuração, definindo o servidor de banco de dados padrão que usa o Code First durante a criação de uma conexão por convenção. Se estiver executando o SQL Express, ele será usado. Se o SQL Express não está disponível, em seguida, LocalDB será registrado como o padrão em vez disso. Nenhuma alteração é feita ao arquivo de configuração, se ela já contém uma configuração para a fábrica de conexão padrão.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Usar o Code First com conexão por convenção e o nome do banco de dados especificado  

Se você não tiver feito qualquer outra configuração em seu aplicativo, em seguida, chamar o construtor de cadeia de caracteres no DbContext com o nome do banco de dados que você deseja usar fará com que DbContext para executar no modo de Code First com uma conexão de banco de dados criado por convenção, o banco de dados de Esse nome. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

Neste exemplo DbContext usa "BloggingDatabase" como o nome do banco de dados e cria uma cadeia de conexão para este banco de dados usando o SQL Express (instalado com o Visual Studio 2010) ou LocalDB (instalado com o Visual Studio 2012). Se ambos estiverem instaladas, o SQL Express será usado.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Usar o Code First com a cadeia de caracteres de conexão no arquivo app.config/web.config  

Você pode optar por colocar uma cadeia de caracteres de conexão em seu arquivo App. config ou Web. config. Por exemplo:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Isso é uma maneira fácil de informar ao DbContext para usar um servidor de banco de dados diferente do SQL Express ou LocalDB — o exemplo anterior Especifica um banco de dados do SQL Server Compact Edition.  

Se o nome da cadeia de caracteres de conexão corresponder ao nome de seu contexto (com ou sem qualificação de namespace), em seguida, ele será encontrado pelo DbContext quando o construtor sem parâmetros é usado. Se o nome de cadeia de caracteres de conexão é diferente do nome de seu contexto, em seguida, você pode dizer DbContext para usar essa conexão em modo de Code First, passando o nome de cadeia de caracteres de conexão para o construtor DbContext. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Como alternativa, você pode usar o formulário "nome =\<nome da cadeia de conexão\>" para a cadeia de caracteres passada para o construtor DbContext. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Essa forma faz explícita que você espera que a cadeia de caracteres de conexão a ser encontrado no arquivo de configuração. Uma exceção será lançada se uma cadeia de caracteres de conexão com o nome fornecido não for encontrada.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Banco de dados/Model First com a cadeia de caracteres de conexão no arquivo app.config/web.config  

Modelos criados com o EF Designer são diferentes de Code First em que seu modelo já existe e não é gerado a partir do código quando o aplicativo é executado. O modelo normalmente existe como um arquivo EDMX em seu projeto.  

O designer adicionará uma cadeia de caracteres de conexão de EF ao seu arquivo App. config ou Web. config. Essa cadeia de caracteres de conexão é especial que contém informações sobre como localizar as informações em seu arquivo EDMX. Por exemplo:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

O EF Designer também irá gerar o código que informa ao DbContext para usar essa conexão, passando o nome de cadeia de caracteres de conexão para o construtor DbContext. Por exemplo:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

O DbContext sabe para carregar o modelo existente (em vez de usar o Code First para calculá-lo a partir do código) porque a cadeia de caracteres de conexão é uma cadeia de caracteres de conexão de EF que contém detalhes do modelo para usar.  

## <a name="other-dbcontext-constructor-options"></a>Outras opções do construtor DbContext  

A classe DbContext contém outros construtores e os padrões de uso que permitem alguns cenários mais avançados. Alguns deles são:  

- Você pode usar a classe DbModelBuilder para criar um modelo Code First sem instanciar uma instância de DbContext. O resultado disso é um objeto de DbModel. Você pode passar esse objeto DbModel para um dos construtores DbContext quando estiver pronto para criar a instância de DbContext.  
- Você pode passar uma cadeia de caracteres de conexão completa para o DbContext em vez de apenas o nome de cadeia de caracteres de conexão ou de banco de dados. Por padrão, essa cadeia de caracteres de conexão é usada com o provedor de.NET; Isso pode ser alterado definindo uma implementação diferente de IConnectionFactory no contexto. Database.DefaultConnectionFactory.  
- Você pode usar um objeto DbConnection existente, passando-o para um construtor DbContext. Se o objeto de conexão é uma instância de EntityConnection, em seguida, o modelo especificado na conexão será usado em vez de calcular um modelo usando Code First. Se o objeto é uma instância de algum outro tipo — por exemplo, SqlConnection — e em seguida, o contexto de usá-lo para o modo de Code First.  
- Você pode passar um ObjectContext existente para um construtor DbContext para criar um DbContext quebra automática de contexto existente. Isso pode ser usado para aplicativos existentes que usam o ObjectContext, mas que desejam tirar proveito do DbContext em algumas partes do aplicativo.  
