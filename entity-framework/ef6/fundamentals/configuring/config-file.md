---
title: Arquivo de configuração - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 88c2439f3a89c9fb9ee22f828789df4decf39cc5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996497"
---
# <a name="configuration-file-settings"></a>Arquivo de configuração
Entity Framework permite uma série de configurações seja especificada do arquivo de configuração. Em geral o EF segue um princípio de 'convenção em detrimento da configuração': todas as configurações discutidas neste post têm um comportamento padrão, você só precisa se preocupar sobre como alterar a configuração, quando o padrão não atendem às suas necessidades.  

## <a name="a-code-based-alternative"></a>Uma alternativa baseada em código  

Todas essas configurações também podem ser aplicadas usando código. A partir do EF6, apresentamos [configuração baseada em código](code-based.md), que fornece uma maneira centralizada da aplicação de configuração do código. Antes do EF6, ainda pode ser aplicada a configuração do código, mas você precisa usar várias APIs para configurar diferentes áreas. A opção de arquivo de configuração permite que essas configurações sejam alteradas com facilidade durante a implantação sem atualizar seu código.

## <a name="the-entity-framework-configuration-section"></a>A seção de configuração do Entity Framework  

Começando com o ef4.1 em diante, você pode definir o inicializador de banco de dados para um contexto usando o **appSettings** seção do arquivo de configuração. No EF 4.3, apresentamos personalizado **entityFramework** seção para lidar com as novas configurações. Entity Framework ainda reconhecerá os inicializadores de banco de dados definidos usando o formato antigo, mas recomendamos a migração para o novo formato sempre que possível.

O **entityFramework** seção foi adicionada automaticamente ao arquivo de configuração do seu projeto quando você instalou o pacote EntityFramework NuGet.  

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

[Esta página](~/ef6/fundamentals/configuring/connection-strings.md) fornece mais detalhes sobre como o Entity Framework determina o banco de dados a ser usado, incluindo cadeias de caracteres de conexão no arquivo de configuração.  

Vá de cadeias de caracteres de Conexão no padrão **connectionStrings** elemento e não exigem o **entityFramework** seção.  

Modelos de código pela primeira vez com base em usam cadeias de caracteres de conexão ADO.NET normais. Por exemplo:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Designer EF com base em modelos use EF conexão cadeias de caracteres especiais. Por exemplo:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Tipo de configuração baseada em código (EF6 em diante)  

Começando com o EF6, você pode especificar o DbConfiguration para ser usado para o EF [configuração baseada em código](code-based.md) em seu aplicativo. Na maioria dos casos, você não precisa especificar essa configuração como EF descobrirá automaticamente seu DbConfiguration. Para obter detalhes de quando talvez você precise especificar DbConfiguration no arquivo de configuração, consulte o **movendo DbConfiguration** seção [configuração baseada em código](code-based.md).  

Para definir um tipo de DbConfiguration, que você especificar o nome de tipo qualificado do assembly para o **codeConfigurationType** elemento.  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo. Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Provedores de banco de dados do EF (EF6 em diante)  

Antes do EF6, partes de específicos do Entity Framework de um provedor de banco de dados tinham que ser incluído como parte do provedor ADO.NET core. Começando com o EF6, as partes específicas do EF são agora gerenciadas e registradas separadamente.  

Normalmente, não será necessário registrar provedores por conta própria. Isso será feito normalmente pelo provedor quando você o instala.  

Provedores são registrados, incluindo uma **provedor** sob o elemento a **provedores** seção filho a **entityFramework** seção. Há dois atributos obrigatórios para uma entrada do provedor:  

- **invariantName** identifica o provedor ADO.NET de núcleo que este destinos de provedor do EF  
- **tipo** é o nome de tipo qualificado do assembly da implementação do provedor do EF  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo. Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.  

Por exemplo, aqui está a entrada criada para registrar o provedor do SQL Server padrão quando você instala o Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptores (EF6.1 em diante)  

Começando com o EF6.1, você pode registrar interceptores no arquivo de configuração. Os interceptores permitem que você execute lógica adicional quando o EF executa determinadas operações, como a execução de consultas de banco de dados, abrir conexões, etc.  

Os interceptores são registrados, incluindo uma **interceptador** sob o elemento a **interceptores** seção filho do **entityFramework** seção. Por exemplo, a configuração a seguir registra o interno **DatabaseLogger** interceptador que registrará em log todas as operações de banco de dados para o Console.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Operações de banco de dados de registro em log para um arquivo (EF6.1 em diante)  

Registrar interceptores por meio do arquivo de configuração é especialmente útil quando você deseja adicionar o registro em log a um aplicativo existente para ajudar a depurar um problema. **DatabaseLogger** dá suporte ao registro em log para um arquivo, fornecendo o nome do arquivo como um parâmetro de construtor.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Por padrão, isso fará com que o arquivo de log seja substituído por um novo arquivo cada vez que o aplicativo é iniciado. Para acrescentar em vez disso, o log de arquivo se ele já existir usar algo como:  

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

Para saber mais sobre **DatabaseLogger** e registrando interceptores, consulte o postagem de blog [EF 6.1: ativar o registro em log sem recompilar](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Fábrica de Conexão padrão primeiro código  

A seção de configuração permite que você especifique uma fábrica de conexão padrão que o Code First deve usar para localizar um banco de dados a ser usado para um contexto. O alocador de conexão padrão é usado apenas quando nenhuma cadeia de conexão tiver sido adicionada ao arquivo de configuração para um contexto.  

Quando você instalou o pacote NuGet do EF uma fábrica de conexão padrão foi registrada que aponta para o SQL Express ou LocalDB, dependendo de qual delas você instalou.  

Para definir uma fábrica de conexão, você especifica o nome de tipo qualificado do assembly para o **deafultConnectionFactory** elemento.  

> [!NOTE]
> Um nome qualificado do assembly é o nome qualificado de namespace, seguido por uma vírgula, em seguida, o assembly que reside o tipo. Você pode, opcionalmente, especifique também a versão do assembly, cultura e token de chave pública.  

Aqui está um exemplo de como definir sua própria fábrica de conexão padrão:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

O exemplo acima exige a fábrica personalizada para ter um construtor sem parâmetros. Se necessário, você pode especificar parâmetros do construtor usando o **parâmetros** elemento.  

Por exemplo, SqlCeConnectionFactory, que está incluído no Entity Framework, requer que você forneça um nome invariável do provedor para o construtor. O nome invariável do provedor identifica a versão do SQL Compact você deseja usar. A configuração a seguir fará com que os contextos usar a versão do SQL Compact 4.0 por padrão.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Se você não definir uma fábrica de conexão padrão, o Code First usa SqlConnectionFactory, apontando para `.\SQLEXPRESS`. SqlConnectionFactory também tem um construtor que permite substituir partes da cadeia de conexão. Se você quiser usar uma instância do SQL Server diferente de `.\SQLEXPRESS` você pode usar esse construtor para definir o servidor.  

A configuração a seguir fará com que Code First usar **MyDatabaseServer** para contextos que não têm uma cadeia de caracteres de conexão explícita definida.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Por padrão, supõe-se que os argumentos de construtor são do tipo cadeia de caracteres. Você pode usar o atributo de tipo para alterar isso.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inicializadores de banco de dados  

Inicializadores de banco de dados são configurados em uma base por contexto. Elas podem ser definidas no arquivo de configuração usando o **contexto** elemento. Esse elemento usa o nome qualificado do assembly para identificar o contexto que está sendo configurado.  

Por padrão, o Code First contextos são configurados para usar o inicializador de CreateDatabaseIfNotExists. Há um **disableDatabaseInitialization** atributo as **contexto** elemento que pode ser usado para desabilitar a inicialização do banco de dados.  

Por exemplo, a configuração a seguir desabilita a inicialização do banco de dados para o contexto de Blogging.BlogContext definido em myAssembly. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Você pode usar o **databaseInitializer** elemento para definir um inicializador personalizado.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Parâmetros do construtor usam a mesma sintaxe como fábricas de conexão padrão.  

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

Você pode configurar um dos inicializadores de banco de dados genérico que estão incluídos no Entity Framework. O **tipo** atributo usa o formato do .NET Framework para tipos genéricos.  

Por exemplo, se você estiver usando as migrações Code First, você pode configurar o banco de dados a serem migrados automaticamente usando o `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inicializador.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
