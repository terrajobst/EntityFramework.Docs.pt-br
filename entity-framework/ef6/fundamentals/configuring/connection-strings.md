---
title: Cadeias de conexão e modelos-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419293"
---
# <a name="connection-strings-and-models"></a>Cadeias de conexão e modelos
Este tópico aborda como Entity Framework descobre qual conexão de banco de dados usar e como você pode alterá-la. Os modelos criados com Code First e o designer do EF são abordados neste tópico.  

Normalmente, um aplicativo Entity Framework usa uma classe derivada de DbContext. Essa classe derivada chamará um dos construtores na classe base DbContext a ser controlada:  

- Como o contexto se conectará a um banco de dados — ou seja, como uma cadeia de conexão é encontrada/usada  
- Se o contexto usará calcular um modelo usando Code First ou carregar um modelo criado com o designer do EF  
- Opções avançadas adicionais  

Os fragmentos a seguir mostram algumas das maneiras como os construtores DbContext podem ser usados.  

## <a name="use-code-first-with-connection-by-convention"></a>Usar Code First com conexão por convenção  

Se você não tiver feito nenhuma outra configuração em seu aplicativo, chamar o construtor sem parâmetros no DbContext fará com que o DbContext seja executado no modo de Code First com uma conexão de banco de dados criada por convenção. Por exemplo:  

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

Neste exemplo, DbContext usa o nome qualificado do namespace da classe de contexto derivada — demo. EF. BloggingContext — como o nome do banco de dados e cria uma cadeia de conexão para esse banco de dados usando o SQL Express ou o LocalDB. Se ambos estiverem instalados, o SQL Express será usado.  

O Visual Studio 2010 inclui o SQL Express por padrão e o Visual Studio 2012 e posterior inclui o LocalDB. Durante a instalação, o pacote NuGet do EntityFramework verifica qual servidor de banco de dados está disponível. O pacote NuGet atualizará o arquivo de configuração definindo o servidor de banco de dados padrão que o Code First usa ao criar uma conexão por convenção. Se o SQL Express estiver em execução, ele será usado. Se o SQL Express não estiver disponível, o LocalDB será registrado como o padrão. Nenhuma alteração será feita no arquivo de configuração se ele já contiver uma configuração para a fábrica de conexões padrão.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Usar Code First com conexão por convenção e nome de banco de dados especificado  

Se você não tiver feito nenhuma outra configuração em seu aplicativo, chamar o construtor de cadeia de caracteres em DbContext com o nome do banco de dados que você deseja usar fará com que o DbContext seja executado no modo de Code First com uma conexão de banco de dados criada por convenção ao banco de dados de Esse nome. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

Neste exemplo, DbContext usa "BloggingDatabase" como o nome do banco de dados e cria uma cadeia de conexão para esse banco de dados usando o SQL Express (instalado com o Visual Studio 2010) ou o LocalDB (instalado com o Visual Studio 2012). Se ambos estiverem instalados, o SQL Express será usado.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Usar Code First com cadeia de conexão no arquivo app. config/Web. config  

Você pode optar por colocar uma cadeia de conexão em seu arquivo app. config ou Web. config. Por exemplo:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Essa é uma maneira fácil de dizer ao DbContext para usar um servidor de banco de dados diferente do SQL Express ou LocalDB — o exemplo acima Especifica um banco de dados do SQL Server Compact Edition.  

Se o nome da cadeia de conexão corresponder ao nome do seu contexto (com ou sem qualificação de namespace), ele será encontrado por DbContext quando o construtor sem parâmetros for usado. Se o nome da cadeia de conexão for diferente do nome do seu contexto, você poderá dizer ao DbContext para usar essa conexão no modo de Code First passando o nome da cadeia de conexão para o Construtor DbContext. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Como alternativa, você pode usar a forma "Name =\<nome da cadeia de conexão\>" para a cadeia de caracteres passada para o Construtor DbContext. Por exemplo:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Esse formulário torna explícito que você espera que a cadeia de conexão seja encontrada no arquivo de configuração. Uma exceção será gerada se uma cadeia de conexão com o nome fornecido não for encontrada.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Banco de dados/Model First com cadeia de conexão no arquivo app. config/Web. config  

Os modelos criados com o designer do EF são diferentes dos Code First em que o modelo já existe e não é gerado a partir do código quando o aplicativo é executado. O modelo normalmente existe como um arquivo EDMX em seu projeto.  

O designer adicionará uma cadeia de conexão do EF ao arquivo app. config ou Web. config. Essa cadeia de conexão é especial, pois contém informações sobre como localizar as informações em seu arquivo EDMX. Por exemplo:  

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

O designer do EF também gerará código que informa ao DbContext para usar essa conexão, passando o nome da cadeia de conexão para o Construtor DbContext. Por exemplo:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

O DbContext sabe carregar o modelo existente (em vez de usar Code First para calculá-lo do código), pois a cadeia de conexão é uma cadeia de conexão do EF que contém detalhes do modelo a ser usado.  

## <a name="other-dbcontext-constructor-options"></a>Outras opções do Construtor DbContext  

A classe DbContext contém outros construtores e padrões de uso que habilitam Alguns cenários mais avançados. Alguns deles são:  

- Você pode usar a classe DbModelBuilder para criar um modelo de Code First sem instanciar uma instância DbContext. O resultado disso é um objeto DbModel. Em seguida, você pode passar esse objeto DbModel para um dos construtores DbContext quando estiver pronto para criar sua instância DbContext.  
- Você pode passar uma cadeia de conexão completa para DbContext em vez de apenas o banco de dados ou o nome da cadeia de conexão. Por padrão, essa cadeia de conexão é usada com o provedor System. Data. SqlClient; Isso pode ser alterado definindo uma implementação diferente de IConnectionFactory no contexto. Database. DefaultConnectionFactory.  
- Você pode usar um objeto DbConnection existente passando-o para um Construtor DbContext. Se o objeto de conexão for uma instância de EntityConnection, o modelo especificado na conexão será usado em vez de calcular um modelo usando Code First. Se o objeto for uma instância de algum outro tipo — por exemplo, SqlConnection —, o contexto o usará para o modo de Code First.  
- Você pode passar um ObjectContext existente para um Construtor DbContext a fim de criar um DbContext encapsulando o contexto existente. Isso pode ser usado para aplicativos existentes que usam ObjectContext, mas que desejam aproveitar o DbContext em algumas partes do aplicativo.  
