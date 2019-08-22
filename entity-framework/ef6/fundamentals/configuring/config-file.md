---
title: Configurações do arquivo de configuração-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886562"
---
# <a name="configuration-file-settings"></a>Configurações do arquivo de configuração
Entity Framework permite que várias configurações sejam especificadas no arquivo de configuração. Em geral, o EF segue um princípio de "Convenção sobre configuração": todas as configurações abordadas nesta postagem têm um comportamento padrão, você só precisa se preocupar em alterar a configuração quando o padrão não atender mais às suas necessidades.  

## <a name="a-code-based-alternative"></a>Uma alternativa baseada em código  

Todas essas configurações também podem ser aplicadas usando código. A partir do EF6, introduzimos a [Configuração baseada em código](code-based.md), que fornece uma maneira central de aplicar a configuração do código. Antes do EF6, a configuração ainda pode ser aplicada a partir do código, mas você precisa usar várias APIs para configurar áreas diferentes. A opção arquivo de configuração permite que essas configurações sejam alteradas facilmente durante a implantação sem Atualizar seu código.

## <a name="the-entity-framework-configuration-section"></a>A seção de configuração de Entity Framework  

A partir do EF 4.1, você pode definir o inicializador de banco de dados para um contexto usando a seção appSettings do arquivo de configuração. No EF 4,3, introduzimos a seção personalizada do **entityFramework** para lidar com as novas configurações. Entity Framework ainda reconhecerá inicializadores de banco de dados definidos usando o formato antigo, mas é recomendável migrar para o novo formato sempre que possível.

A seção **entityFramework** foi adicionada automaticamente ao arquivo de configuração do seu projeto quando você instalou o pacote NuGet do entityFramework.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Cadeias de caracteres de conexão  

[Esta página](~/ef6/fundamentals/configuring/connection-strings.md) fornece mais detalhes sobre como Entity Framework determina o banco de dados a ser usado, incluindo cadeias de conexão no arquivo de configuração.  

As cadeias de conexão entram no elemento connectionStrings padrão e não exigem a seção **entityFramework** .  

Os modelos baseados em Code First usam cadeias de conexão ADO.NET normais. Por exemplo:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Os modelos baseados no EF designer usam cadeias de conexão especiais do EF. Por exemplo:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Tipo de configuração baseada em código (EF6 em diante)  

A partir do EF6, você pode especificar o DbConfiguration para o EF a ser usado para a [Configuração baseada em código](code-based.md) em seu aplicativo. Na maioria dos casos, você não precisa especificar essa configuração, pois o EF descobrirá automaticamente seu DbConfiguration. Para obter detalhes de quando talvez seja necessário especificar DbConfiguration no arquivo de configuração, consulte a seção **movendo DbConfiguration** da [Configuração baseada em código](code-based.md).  

Para definir um tipo de DbConfiguration, especifique o nome do tipo qualificado do assembly no elemento **codeConfigurationType** .  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado do namespace, seguido por uma vírgula, em seguida, o assembly no qual reside o tipo. Opcionalmente, você também pode especificar a versão do assembly, a cultura e o token de chave pública.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Provedores de banco de dados EF (EF6 em diante)  

Antes do EF6, as partes específicas Entity Framework de um provedor de banco de dados tinham que ser incluídas como parte do provedor ADO.NET principal. A partir do EF6, as partes específicas do EF agora são gerenciadas e registradas separadamente.  

Normalmente, você não precisará registrar provedores por conta própria. Isso normalmente será feito pelo provedor quando você instalá-lo.  

Os provedores são registrados por meio da inclusão de um elemento de **provedor** na seção filho de **provedores** da seção **entityFramework** . Há dois atributos necessários para uma entrada de provedor:  

- o Invariable identifica o provedor ADO.net principal para o qual este provedor do EF se destina  
- **Type** é o nome do tipo qualificado do assembly da implementação do provedor do EF  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado do namespace, seguido por uma vírgula, em seguida, o assembly no qual reside o tipo. Opcionalmente, você também pode especificar a versão do assembly, a cultura e o token de chave pública.  

Por exemplo, aqui está a entrada criada para registrar o provedor de SQL Server padrão quando você instala o Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptadores (EF 6.1 em diante)  

A partir do EF 6.1, você pode registrar interceptadores no arquivo de configuração. Os interceptores permitem que você execute lógica adicional quando o EF executa determinadas operações, como executar consultas de banco de dados, abrir conexões etc.  

Os interceptores são registrados com a inclusão de um elemento Interceptor na seção filho do **Interceptor** da seção **entityFramework** . Por exemplo, a configuração a seguir registra o interceptador **DatabaseLogger** interno que registrará todas as operações de banco de dados no console.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Registrando em log operações de banco de dados em um arquivo (EF 6.1 em diante)  

O registro de interceptores por meio do arquivo de configuração é especialmente útil quando você deseja adicionar o registro em log a um aplicativo existente para ajudar a depurar um problema. O **DatabaseLogger** dá suporte ao registro em log em um arquivo fornecendo o nome do arquivo como um parâmetro de construtor.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Por padrão, isso fará com que o arquivo de log seja substituído por um novo arquivo toda vez que o aplicativo for iniciado. Em vez disso, anexe ao arquivo de log, caso ele já exista, use algo como:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Para obter informações adicionais sobre **DatabaseLogger** e registrar interceptores, consulte a postagem [no blog EF 6,1: Ativando o registro em log](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/)sem recompilação.  

## <a name="code-first-default-connection-factory"></a>Code First fábrica de conexões padrão  

A seção de configuração permite que você especifique um alocador de conexão padrão que Code First deve usar para localizar um banco de dados a ser usado para um contexto. O alocador de conexão padrão é usado somente quando nenhuma cadeia de conexão foi adicionada ao arquivo de configuração para um contexto.  

Quando você instalou o pacote NuGet do EF, uma fábrica de conexão padrão foi registrada que aponta para o SQL Express ou LocalDB, dependendo de qual deles você instalou.  

Para definir uma fábrica de conexões, especifique o nome do tipo qualificado do assembly no elemento **defaultConnectionFactory** .  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado do namespace, seguido por uma vírgula, em seguida, o assembly no qual reside o tipo. Opcionalmente, você também pode especificar a versão do assembly, a cultura e o token de chave pública.  

Veja um exemplo de como configurar sua própria fábrica de conexões padrão:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

O exemplo acima requer que a fábrica personalizada tenha um construtor sem parâmetros. Se necessário, você pode especificar parâmetros de construtor usando o elemento Parameters.  

Por exemplo, o SqlCeConnectionFactory, que está incluído no Entity Framework, exige que você forneça um nome invariável do provedor para o construtor. O nome invariável do provedor identifica a versão do SQL Compact que você deseja usar. A configuração a seguir fará com que contextos usem o SQL Compact versão 4,0 por padrão.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Se você não definir uma fábrica de conexões padrão, Code First usará o SqlConnectionFactory, `.\SQLEXPRESS`apontando para. SqlConnectionFactory também tem um construtor que permite que você substitua partes da cadeia de conexão. Se você quiser usar uma instância SQL Server diferente de `.\SQLEXPRESS` você pode usar esse construtor para definir o servidor.  

A configuração a seguir fará com que Code First use **MyDatabaseServer** para contextos que não tenham uma cadeia de conexão explícita definida.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Por padrão, presume-se que os argumentos do Construtor sejam do tipo cadeia de caracteres. Você pode usar o atributo Type para alterar isso.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicializadores de banco de dados  

Inicializadores de banco de dados são configurados por contexto. Eles podem ser definidos no arquivo de configuração usando o elemento **Context** . Esse elemento usa o nome qualificado do assembly para identificar o contexto que está sendo configurado.  

Por padrão, Code First contextos são configurados para usar o inicializador CreateDatabaseIfNotExists. Há um atributo **disableDatabaseInitialization** no elemento **Context** que pode ser usado para desabilitar a inicialização do banco de dados.  

Por exemplo, a configuração a seguir desabilita a inicialização do banco de dados para o contexto Blogs. BlogContext definido em MyAssembly. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Você pode usar o elemento **databaseInitializer** para definir um inicializador personalizado.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Os parâmetros do Construtor usam a mesma sintaxe que as fábricas de conexão padrão.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Você pode configurar um dos inicializadores de banco de dados genéricos que estão incluídos no Entity Framework. O atributo **Type** usa o formato .NET Framework para tipos genéricos.  

Por exemplo, se você estiver usando migrações do Code First, poderá configurar o banco de dados a ser migrado automaticamente usando `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` o inicializador.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
