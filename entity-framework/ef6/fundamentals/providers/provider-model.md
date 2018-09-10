---
title: O modelo de provedor do Entity Framework 6 - EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 7d9e2f49b9ef59fb63b024646911ec0d8dfcfc60
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251096"
---
# <a name="the-entity-framework-6-provider-model"></a>O modelo de provedor do Entity Framework 6

O modelo de provedor do Entity Framework permite que o Entity Framework a ser usado com diferentes tipos de servidor de banco de dados. Por exemplo, um provedor pode estar conectado para permitir que o EF seja usado contra o Microsoft SQL Server, enquanto outro provedor pode ser conectado para permitir que o EF seja usado contra o Microsoft SQL Server Compact Edition. Os provedores para o EF6 que estamos cientes de que podem ser encontrados na [provedores de Entity Framework](~/ef6/fundamentals/providers/index.md) página.

Determinadas alterações eram necessárias para a maneira como o EF interage com provedores para permitir que o EF ser liberadas sob uma licença de software livre. Essas alterações requerem a recriação dos provedores do EF nos assemblies do EF6, junto com novos mecanismos para o registro do provedor.

## <a name="rebuilding"></a>Recompilar

Com o EF6 do código-fonte que antes fazia parte do .NET Framework agora é fornecido como assemblies do fora de banda (OOB). Detalhes sobre como criar aplicativos em relação ao EF6 podem ser encontrados na [atualização de aplicativos para o EF6](~/ef6/what-is-new/upgrading-to-ef6.md) página. Provedores também precisará ser recriado usando estas instruções.

## <a name="provider-types-overview"></a>Visão geral dos tipos de provedor

Um provedor do EF realmente é uma coleção de serviços específicos do provedor definidas por tipos CLR que esses serviços estendidos de (para uma classe base) ou implementam (para uma interface). Dois desses serviços são fundamentais e necessárias para o EF funcionar em todos os. Outros são opcionais e só precisam ser implementados se uma funcionalidade específica é necessária e/ou as implementações padrão desses serviços não funciona para o servidor de banco de dados específico que está sendo direcionado.

## <a name="fundamental-provider-types"></a>Tipos fundamentais do provedor

### <a name="dbproviderfactory"></a>DbProviderFactory

O EF depende da presença de um tipo derivado [DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) para executar todo o acesso de baixo nível do banco de dados. DbProviderFactory não é parte do EF, mas em vez disso, é uma classe no .NET Framework que serve um ponto de entrada para os provedores ADO.NET que pode ser usada pelo EF, outro O/RMs, ou diretamente por um aplicativo para obter as instâncias de conexões, comandos, parâmetros e outras abstrações ADO.NET em um provedor de maneira independente. Para obter mais informações sobre o DbProviderFactory um ser encontrado na [documentação do MSDN para o ADO.NET](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

O EF depende da presença de um tipo derivado de DbProviderServices para fornecer funcionalidade adicional necessária pelo EF sobre a funcionalidade já fornecida pelo provedor ADO.NET. Em versões anteriores do EF, a classe de DbProviderServices fazia parte do .NET Framework e foi encontrada no namespace Common. Começando com o EF6 esta classe agora é parte do EntityFramework e está no namespace System.Data.Entity.Core.Common.

Mais detalhes sobre a funcionalidade fundamental de uma implementação de DbProviderServices podem ser encontrados em [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx). No entanto, observe que a partir da hora de escrever essa informação não é atualizado para o EF6 Embora a maioria dos conceitos ainda é válidas. As implementações do SQL Server e SQL Server Compact de DbProviderServices também são verificadas para o [base de código do código-fonte aberto](https://github.com/aspnet/EntityFramework6/) e pode servir como referências úteis para outras implementações.

Em versões anteriores do EF para usar a implementação de DbProviderServices foi obtida diretamente de um provedor ADO.NET. Isso foi feito convertendo DbProviderFactory para IServiceProvider e chamando o método GetService. Esse acoplamento rígido o provedor do EF com o DbProviderFactory. Esse acoplamento bloqueado EF que estão sendo movidos para fora do .NET Framework e, portanto, para o EF6 esse acoplamento rígido foi removido e uma implementação de DbProviderServices agora é registrada diretamente no arquivo de configuração do aplicativo ou em baseado em código configuração, conforme descrito mais detalhadamente os _DbProviderServices registrando_ seção abaixo.

## <a name="additional-services"></a>Serviços adicionais

Além dos serviços de fundamentais descritos acima também há muitos outros serviços usados pelo EF que são sempre ou, às vezes, específico do provedor. Implementações específicas do provedor de padrão desses serviços podem ser fornecidas por uma implementação de DbProviderServices. Aplicativos também podem substituir as implementações de um desses serviços ou fornecem implementações quando um tipo de DbProviderServices não fornece um padrão. Isso é descrito mais detalhadamente a _Resolvendo serviços adicionais_ seção abaixo.

Os tipos de serviço adicional que um provedor pode ser interessante para um provedor estão listados abaixo. Mais detalhes sobre cada um desses tipos de serviço podem ser encontrados na documentação da API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Isso é um serviço opcional que permite que um provedor implementar novas tentativas ou outro comportamento quando consultas e comandos são executados no banco de dados. Se nenhuma implementação for fornecida, em seguida, EF será simplesmente execute os comandos e propagar todas as exceções geradas. Para o SQL Server, esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseado em nuvem, como SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Isso é um serviço opcional que permite que um provedor criar objetos de DbConnection por convenção, quando é fornecido apenas um nome de banco de dados. Observe que, embora este serviço pode ser resolvido com uma implementação de DbProviderServices ele presente desde o EF 4.1 e também pode ser definido explicitamente no arquivo de configuração ou no código. O provedor só terá uma chance para resolver esse serviço se ele registrado como o provedor padrão (consulte _o provedor padrão_ abaixo) e, se uma fábrica de conexão padrão não foi definida em outro lugar.

### <a name="dbspatialservices"></a>DbSpatialServices

Esse é um serviços opcional que permite que um provedor adicionar suporte para tipos espaciais, geography e geometry. Uma implementação desse serviço deve ser fornecida para que um aplicativo para usar o EF com tipos espaciais. DbSptialServices é solicitado de duas maneiras. Primeiro, específico do provedor de serviços espaciais são solicitados usando um objeto DbProviderInfo (que contém o invariável de token de manifesto e o nome) como chave. Em segundo lugar, DbSpatialServices podem ser solicitados sem chave. Isso é usado para resolver o "global espacial provedor" que é usado durante a criação de tipos de DbGeography ou DbGeometry autônomos.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Isso é um serviço opcional que permite migrações do EF ser usado para a geração de SQL usado na criação e modificação de esquemas de banco de dados Code First. Uma implementação é necessário para dar suporte a migrações. Se uma implementação for fornecida, em seguida, também poderá ser usado quando os bancos de dados são criados usando os inicializadores de banco de dados ou o método Database.Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < cadeia de caracteres, HistoryContextFactory DbConnection >

Isso é um serviço opcional que permite que um provedor configurar o mapeamento do HistoryContext ao `__MigrationHistory` tabela usada pelo migrações do EF. O HistoryContext é um código primeiro DbContext e pode ser configurado usando a API fluente normal para alterar coisas como o nome da tabela e as especificações de mapeamento de coluna. A implementação padrão desse serviço retornado pelo EF para todos os provedores pode funcionar para um servidor de banco de dados fornecido, se todos os mapeamentos padrão de tabela e coluna têm suporte pelo provedor. Nesse caso, o provedor não precisa fornecer uma implementação desse serviço.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Isso é um serviço opcional para obter o DbProviderFactory correto de um determinado objeto DbConnection. A implementação padrão desse retornado pelo EF para todos os provedores de serviço destina-se a funcionar para todos os provedores. No entanto, quando em execução no .NET 4, o DbProviderFactory não é publicamente acessível a partir de um, se seu DbConnections. Portanto, o EF usa algumas heurísticas para pesquisar os provedores registrados para localizar uma correspondência. É possível que para alguns provedores esses métodos heurísticos falhará e, em tais situações, o provedor deve fornecer uma nova implementação.

## <a name="registering-dbproviderservices"></a>Registrando DbProviderServices

A implementação de DbProviderServices para usar pode ser registrada no arquivo de configuração (App. config ou Web. config) ou usando a configuração baseada em código do aplicativo. Em ambos os casos o registro usa o nome do provedor"invariável" como uma chave. Isso permite que vários provedores ser registrado e usado em um único aplicativo. O nome invariável usado para registros do EF é o mesmo que o nome invariável usado para cadeias de conexão e o registro do provedor ADO.NET. Por exemplo, para o SQL Server o nome invariável "System.Data.SqlClient" é usado.

### <a name="config-file-registration"></a>Registro do arquivo de configuração

O tipo de DbProviderServices para usar é registrado como um elemento de provedor na lista de provedores da seção entityFramework do arquivo de configuração do aplicativo. Por exemplo:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

O _tipo_ cadeia de caracteres deve ser o nome de tipo qualificado pelo assembly de usar a implementação de DbProviderServices.

### <a name="code-based-registration"></a>Registro baseado em código

Começando com o EF6 provedores também pode ser registrado usando o código. Isso permite que um provedor do EF ser usado sem qualquer alteração ao arquivo de configuração do aplicativo. Para usar a configuração com base no código de um aplicativo deve criar uma classe DbConfiguration conforme descrito na [documentação de configuração com base em código](http://msdn.com/data/jj680699). O construtor da classe DbConfiguration, em seguida, deve chamar SetProviderServices para registrar o provedor do EF. Por exemplo:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Determinar os serviços adicionais

Conforme mencionado acima na _visão geral de tipos de provedor_ seção um DbProviderServices classe também pode ser usada para resolver os serviços adicionais. Isso é possível porque DbProviderServices implementa IDbDependencyResolver e cada tipo de DbProviderServices registrado é adicionado como um resolvedor"padrão". O mecanismo IDbDpendencyResolver é descrito mais detalhadamente [resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md). No entanto, não é necessário compreender todos os conceitos nesta especificação para resolver os serviços adicionais em um provedor.

A maneira mais comum para um provedor resolver os serviços adicionais é chamar DbProviderServices.AddDependencyResolver para cada serviço no construtor da classe DbProviderServices. Por exemplo, SqlProviderServices (o provedor do EF para o SQL Server) possui código semelhante a este para inicialização:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Este construtor usa as classes de auxiliar a seguir:

*   SingletonDependencyResolver: fornece uma maneira simples de resolver serviços Singleton — ou seja, os serviços para o qual a mesma instância é retornada sempre que GetService é chamada. Serviços temporários geralmente são registrados como um alocador singleton que será usado para criar instâncias transitórias sob demanda.
*   ExecutionStrategyResolver: um resolvedor específico retornando IExecutionStrategy implementações.

Em vez de usar DbProviderServices.AddDependencyResolver também é possível substituir DbProviderServices.GetService e resolver os serviços adicionais diretamente. Esse método será chamado quando o EF precisa de um serviço definido por um determinado tipo e, em alguns casos, para uma determinada chave. O método deve retornar o serviço, se possível, ou retornar um valor nulo a recusa de retornar o serviço e em vez disso, permitir que outra classe para resolvê-lo. Por exemplo, para resolver o alocador de conexão padrão o código em GetService poderia ser algo assim:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Ordem de registro

Quando várias implementações de DbProviderServices são registradas no arquivo de configuração do aplicativo, eles serão adicionados como secundários resolvedores na ordem em que estão listados. Uma vez que os resolvedores sempre sejam adicionados à parte superior da cadeia de resolvedor secundário que isso significa que o provedor no final da lista terá uma chance para resolver as dependências antes dos outros. (Isso pode parecer um pouco contraintuitivo a princípio, mas faz sentido se você imaginar considerando cada provedor de lista e o empilhamento sobre os provedores existentes.)

Normalmente, essa ordem não importa porque a maioria dos serviços de provedor são específicas do provedor e com a chave, nome invariável do provedor. No entanto, para os serviços que não são criptografadas pelo nome invariável do provedor ou alguma outro específico do provedor chave que o serviço será resolvido com base nesta ordem. Por exemplo, se ele não for explicitamente definido diferentemente em algum lugar caso contrário, o alocador de conexão padrão virá do provedor de nível mais alto na cadeia.

## <a name="additional-config-file-registrations"></a>Registros de arquivo de configuração adicionais

É possível registrar explicitamente alguns dos serviços adicionais do provedor descritos acima, diretamente no arquivo de configuração do aplicativo. Quando isso for feito o registro no arquivo de configuração será usado em vez de qualquer coisa retornada pelo método GetService da implementação de DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Registrar o alocador de conexão padrão

Começando com o EF5 pacote EntityFramework NuGet automaticamente registrado no arquivo de configuração a fábrica de conexão do SQL Express ou a fábrica de conexão LocalDb.

Por exemplo:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

O _tipo_ é o nome de tipo qualificado pelo assembly para a fábrica de conexão padrão, que deve implementar IDbConnectionFactory.

É recomendável que um pacote do NuGet de provedor definido o alocador de conexão padrão dessa forma, quando instalado. Ver _pacotes do NuGet para provedores de_ abaixo.

## <a name="additional-ef6-provider-changes"></a>Alterações adicionais do provedor de EF6

### <a name="spatial-provider-changes"></a>Alterações de provedor espacial

Provedores que dão suporte a tipos espaciais agora devem implementar alguns métodos adicionais em classes derivadas de DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Também há novas versões assíncronas dos métodos existentes que são recomendados para ser substituída, pois as implementações padrão de delegar para os métodos síncronos e, portanto, não executar de forma assíncrona:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Suporte nativo para Enumerable

EF6 apresenta um novo tipo de expressão, DbInExpression, que foi adicionado para solucionar problemas de desempenho em torno do uso de Enumerable em consultas LINQ. A classe de DbProviderManifest tem um novo método virtual SupportsInExpression, que é chamado pelo EF para determinar se um provedor manipula o novo tipo de expressão. Para compatibilidade com implementações de provedor existente, o método retornará false. Para aproveitar essa melhoria, um provedor de EF6 pode adicionar código para lidar com DbInExpression e substituir SupportsInExpression retornar true. Uma instância de DbInExpression pode ser criada chamando o método DbExpressionBuilder.In. Uma instância de DbInExpression é composta de uma DbExpression, normalmente, que representa uma coluna de tabela e uma lista de DbConstantExpression para testar uma correspondência.

## <a name="nuget-packages-for-providers"></a>Pacotes do NuGet para provedores

É uma maneira de disponibilizar um provedor de EF6 para liberá-lo como um pacote do NuGet. Usar um pacote do NuGet tem as seguintes vantagens:

*   É fácil de usar NuGet para adicionar o registro do provedor ao arquivo de configuração do aplicativo
*   Alterações adicionais podem ser feitas ao arquivo de configuração para definir a fábrica de conexão padrão para que as conexões feitas por convenção, use o provedor registrado
*   O NuGet trata a adicionar redirecionamentos de associação para que o provedor do EF6 deve continuar a funcionar mesmo após o lançamento de um novo pacote do EF

Um exemplo disso é o que está incluído no pacote de EntityFramework.SqlServerCompact a [livre codebase](http://github.com/aspnet/entityframework6). Esse pacote fornece um bom modelo para a criação de pacotes do NuGet de provedor do EF.

### <a name="powershell-commands"></a>Comandos do PowerShell

Quando o pacote EntityFramework NuGet está instalado, ele registra um módulo do PowerShell que contém os dois comandos que são muito úteis para pacotes de provedor:

*   EFProvider adicionar adiciona uma nova entidade para o provedor no arquivo de configuração do projeto de destino e torna-se de que ele está no final da lista de provedores registrados.
*   Adicionar-EFDefaultConnectionFactory adiciona ou atualiza o registro de defaultConnectionFactory no arquivo de configuração do projeto de destino.

Os dois desses comandos cuidam de adicionar uma seção entityFramework ao arquivo de configuração e adicionar uma coleção de provedores, se necessário.

Ele destina-se que esses comandos ser chamado do script de NuGet install.ps1. Por exemplo, install.ps1 para o provedor SQL Compact é semelhante a este:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Mais informações sobre esses comandos podem ser obtidas usando get-help na janela do Console do Gerenciador de pacotes.

## <a name="wrapping-providers"></a>Provedores de encapsulamento

Um provedor de encapsulamento é um provedor do EF e/ou ADO.NET que encapsula um provedor existente para estendê-lo com outras funcionalidades como criação de perfil ou capacidades de rastreamento. Encapsulamento provedores pode ser registrado da maneira normal, mas geralmente é mais conveniente para configurar o provedor de encapsulamento em tempo de execução interceptando a resolução de serviços relacionados ao provedor. O evento estático OnLockingConfiguration na classe DbConfiguration pode ser usado para fazer isso.

OnLockingConfiguration é chamado depois que o EF determinou onde todas as configurações do EF para o domínio de aplicativo serão obtidas a partir, mas antes que seja bloqueado para uso. Na inicialização do aplicativo (antes do EF é usado), o aplicativo deve registrar um manipulador de eventos para este evento. (Estamos considerando a adição de suporte para registrar esse manipulador no arquivo de configuração, mas isso ainda não é suportado.) O manipulador de eventos, em seguida, deve fazer uma chamada para replaceservice tenha para cada serviço que precisa ser encapsulado.  

Por exemplo, para encapsular IDbConnectionFactory e DbProviderService, deve ser registrado um manipulador de algo assim:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

O serviço que foi resolvido e agora deve ser encapsulado juntamente com a chave que foi usada para resolver o serviço são passados para o manipulador. O manipulador pode encapsular esse serviço e substitua o serviço retornado com a versão encapsulada.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Resolvendo um DbProviderFactory com o EF

DbProviderFactory é um dos tipos fundamentais do provedor necessários pelo EF, conforme descrito na _visão geral de tipos de provedor_ seção acima. Como já mencionado, não é um tipo EF e registro geralmente não é parte da configuração do EF, mas em vez disso, é o registro do provedor ADO.NET normal no arquivo Machine. config e/ou o arquivo de configuração do aplicativo.

Apesar deste EF ainda usa seu mecanismo de resolução de dependência normal durante a procura de um DbProviderFactory para usar. O resolvedor padrão usa o registro do ADO.NET normal nos arquivos de configuração de modo que é normalmente transparente. Devido à resolução de dependência normal mecanismo é usado, mas isso significa que um IDbDependencyResolver pode ser usado para resolver um DbProviderFactory mesmo quando o registro do ADO.NET normal não foi feito.

Resolver DbProviderFactory dessa maneira tem várias implicações:

*   Um aplicativo usando a configuração baseada em código pode adicionar chamadas em sua classe DbConfiguration para registrar o DbProviderFactory apropriado. Isso é especialmente útil para aplicativos que não desejam (ou não podem) fazer uso de qualquer configuração baseada em arquivo em todos os.
*   O serviço pode ser encapsulado ou substituído usando replaceservice tenha conforme descrito na _provedores de encapsulamento_ seção acima
*   Teoricamente, uma implementação de DbProviderServices foi possível resolver um DbProviderFactory.

O ponto importante a observar sobre como fazer qualquer uma dessas coisas é que eles só afetará a pesquisa de DbProviderFactory pelo EF. Outro código não são do EF ainda pode esperar que o provedor ADO.NET a ser registrado da maneira normal e poderá falhar se o registro não foi encontrado. Por esse motivo, é normalmente melhor para um DbProviderFactory ser registrado da maneira normal do ADO.NET.

### <a name="related-services"></a>Serviços relacionados

Se o EF é usado para resolver um DbProviderFactory, em seguida, ele também deve resolver os serviços IProviderInvariantName e IDbProviderFactoryResolver.

IProviderInvariantName é um serviço que é usado para determinar um nome invariável do provedor para um determinado tipo de DbProviderFactory. A implementação padrão desse serviço usa o registro do provedor ADO.NET. Isso significa que se o provedor ADO.NET não estiver registrado da maneira normal, porque o DbProviderFactory está sendo resolvido pelo EF, em seguida, também será necessário resolver esse serviço. Observe que um resolvedor para esse serviço é adicionado automaticamente ao usar o método DbConfiguration.SetProviderFactory.

Conforme descrito na _visão geral de tipos de provedor_ seção acima, o IDbProviderFactoryResolver é usado para obter o DbProviderFactory correto de um determinado objeto DbConnection. A implementação padrão desse serviço quando em execução no .NET 4 usa o registro do provedor ADO.NET. Isso significa que se o provedor ADO.NET não estiver registrado da maneira normal, porque o DbProviderFactory está sendo resolvido pelo EF, em seguida, também será necessário resolver esse serviço.
