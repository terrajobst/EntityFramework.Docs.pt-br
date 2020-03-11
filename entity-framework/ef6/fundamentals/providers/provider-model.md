---
title: O modelo de provedor Entity Framework 6-EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: 8bda3f51e8934f2add862c30e60f1185f068c515
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419356"
---
# <a name="the-entity-framework-6-provider-model"></a>O modelo de provedor Entity Framework 6

O modelo de provedor de Entity Framework permite que Entity Framework seja usado com diferentes tipos de servidor de banco de dados. Por exemplo, um provedor pode ser conectado para permitir que o EF seja usado em relação ao Microsoft SQL Server, enquanto outro provedor pode ser conectado para permitir que o EF seja usado na edição Microsoft SQL Server Compact. Os provedores de EF6 que estamos cientes podem ser encontrados na página provedores de [Entity Framework](~/ef6/fundamentals/providers/index.md) .

Algumas alterações foram exigidas para a maneira como o EF interage com provedores para permitir que o EF seja liberado em uma licença de software livre. Essas alterações exigem a recriação de provedores do EF em relação aos assemblies do EF6 junto com os novos mecanismos para o registro do provedor.

## <a name="rebuilding"></a>Recompilação

Com o EF6, o código principal que anteriormente era parte do .NET Framework agora está sendo enviado como assemblies OOB (fora de banda). Detalhes sobre como criar aplicativos com o EF6 podem ser encontrados na página [Atualizando aplicativos para o EF6](~/ef6/what-is-new/upgrading-to-ef6.md) . Os provedores também precisarão ser recriados usando estas instruções.

## <a name="provider-types-overview"></a>Visão geral de tipos de provedor

Um provedor do EF é, na verdade, uma coleção de serviços específicos do provedor definidos por tipos CLR dos quais esses serviços se estendem (para uma classe base) ou implementam (para uma interface). Dois desses serviços são fundamentais e necessários para que o EF funcione. Outras são opcionais e só precisam ser implementadas se uma funcionalidade específica for necessária e/ou se as implementações padrão desses serviços não funcionarem para o servidor de banco de dados específico que está sendo direcionado.

## <a name="fundamental-provider-types"></a>Tipos de provedor fundamentais

### <a name="dbproviderfactory"></a>DbProviderFactory

O EF depende de ter um tipo derivado de [System. Data. Common. DbProviderFactory](https://msdn.microsoft.com/library/system.data.common.dbproviderfactory.aspx) para executar todo o acesso de banco de dados de nível baixo. DbProviderFactory, na verdade, não faz parte do EF, mas é uma classe no .NET Framework que atende a um ponto de entrada para provedores ADO.NET que podem ser usados pelo EF, outro O/RMs ou diretamente por um aplicativo para obter instâncias de conexões, comandos, parâmetros e outras abstrações ADO.NET de forma independente de provedor. Mais informações sobre o DbProviderFactory podem ser encontradas na [documentação do MSDN para ADO.net](https://msdn.microsoft.com/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

O EF depende de ter um tipo derivado de DbProviderServices para fornecer funcionalidade adicional necessária pelo EF sobre a funcionalidade já fornecida pelo provedor ADO.NET. Em versões mais antigas do EF, a classe DbProviderServices era parte da .NET Framework e foi encontrada no namespace System. Data. Common. A partir do EF6, essa classe agora faz parte do EntityFramework. dll e está no namespace System. Data. Entity. Core. Common.

Mais detalhes sobre a funcionalidade fundamental de uma implementação de DbProviderServices podem ser encontrados no [msdn](https://msdn.microsoft.com/library/ee789835.aspx). No entanto, observe que, a partir do momento da gravação, essas informações não são atualizadas para EF6, embora a maioria dos conceitos ainda seja válida. As implementações SQL Server e SQL Server Compact do DbProviderServices também são verificadas na [base de código de código-fonte aberto](https://github.com/aspnet/EntityFramework6/) e podem servir como referências úteis para outras implementações.

Em versões mais antigas do EF, a implementação de DbProviderServices a ser usada foi obtida diretamente de um provedor de ADO.NET. Isso foi feito com a conversão de DbProviderFactory para IServiceProvider e a chamada do método GetService. Isso acoplamento firmemente o provedor do EF para o DbProviderFactory. Esse acoplamento bloqueou o EF de ser movido para fora do .NET Framework e, portanto, para EF6, esse acoplamento rígido foi removido e uma implementação de DbProviderServices agora está registrada diretamente no arquivo de configuração do aplicativo ou na configuração baseada em código, conforme descrito em mais detalhes a seção _registrando DbProviderServices_ abaixo.

## <a name="additional-services"></a>Serviços adicionais

Além dos serviços fundamentais descritos acima, também há muitos outros serviços usados pelo EF, que são sempre ou, às vezes, específicos do provedor. Implementações padrão específicas do provedor desses serviços podem ser fornecidas por uma implementação de DbProviderServices. Os aplicativos também podem substituir as implementações desses serviços ou fornecer implementações quando um tipo DbProviderServices não fornecer um padrão. Isso é descrito mais detalhadamente na seção _Resolvendo serviços adicionais_ abaixo.

Os tipos de serviço adicionais que um provedor pode ser de interesse de um provedor estão listados abaixo. Mais detalhes sobre cada um desses tipos de serviço podem ser encontrados na documentação da API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Esse é um serviço opcional que permite que um provedor implemente repetições ou outro comportamento quando consultas e comandos são executados no banco de dados. Se nenhuma implementação for fornecida, o EF simplesmente executará os comandos e propagará todas as exceções geradas. Por SQL Server esse serviço é usado para fornecer uma política de repetição que é especialmente útil ao executar em servidores de banco de dados baseados em nuvem, como SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Esse é um serviço opcional que permite que um provedor Crie objetos DbConnection por convenção, quando dado apenas um nome de banco de dados. Observe que embora esse serviço possa ser resolvido por uma implementação DbProviderServices, ele vem presente desde o EF 4,1 e também pode ser definido explicitamente no arquivo de configuração ou no código. O provedor só terá a chance de resolver esse serviço se ele estiver registrado como o provedor padrão (consulte _o provedor padrão_ abaixo) e se uma fábrica de conexão padrão não tiver sido definida em outro lugar.

### <a name="dbspatialservices"></a>DbSpatialServices

Estes são os serviços opcionais que permitem que um provedor Adicione suporte para tipos espaciais geográficos e geométricos. Uma implementação desse serviço deve ser fornecida para que um aplicativo use o EF com tipos espaciais. DbSptialServices é solicitado de duas maneiras. Primeiro, os serviços espaciais específicos do provedor são solicitados usando um objeto DbProviderInfo (que contém o nome invariável e o token do manifesto) como chave. Em segundo lugar, o DbSpatialServices pode ser solicitado sem nenhuma chave. Isso é usado para resolver o "provedor espacial global" que é usado ao criar tipos DbGeography ou DbGeometry autônomos.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Esse é um serviço opcional que permite que as migrações do EF sejam usadas para a geração de SQL usada na criação e modificação de esquemas de banco de dados por Code First. Uma implementação é necessária para dar suporte a migrações. Se uma implementação for fornecida, ela também será usada quando os bancos de dados forem criados usando inicializadores de banco de dados ou o método Database. Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, String, HistoryContextFactory >

Esse é um serviço opcional que permite que um provedor configure o mapeamento do HistoryContext para a tabela de `__MigrationHistory` usada por migrações do EF. O HistoryContext é um Code First DbContext e pode ser configurado usando a API Fluent normal para alterar itens como o nome da tabela e as especificações de mapeamento de coluna. A implementação padrão desse serviço retornado pelo EF para todos os provedores pode funcionar para um determinado servidor de banco de dados se todos os mapeamentos de tabela e coluna padrão forem suportados por esse provedor. Nesse caso, o provedor não precisa fornecer uma implementação desse serviço.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Esse é um serviço opcional para obter o DbProviderFactory correto de um determinado objeto DbConnection. A implementação padrão desse serviço retornado pelo EF para todos os provedores destina-se a funcionar para todos os provedores. No entanto, quando executado no .NET 4, o DbProviderFactory não é acessível publicamente de uma se suas DbConnections. Portanto, o EF usa alguns heurísticos para pesquisar os provedores registrados para encontrar uma correspondência. É possível que, para alguns provedores, essas heurísticas falhem e, nessas situações, o provedor deve fornecer uma nova implementação.

## <a name="registering-dbproviderservices"></a>Registrando DbProviderServices

A implementação de DbProviderServices a ser usada pode ser registrada no arquivo de configuração do aplicativo (App. config ou Web. config) ou usando a configuração baseada em código. Em ambos os casos, o registro usa o "nome invariável" do provedor como uma chave. Isso permite que vários provedores sejam registrados e usados em um único aplicativo. O nome invariável usado para registros do EF é o mesmo que o nome invariável usado para o registro do provedor ADO.NET e cadeias de conexão. Por exemplo, para SQL Server o nome invariável "System. Data. SqlClient" é usado.

### <a name="config-file-registration"></a>Registro do arquivo de configuração

O tipo de DbProviderServices a ser usado é registrado como um elemento de provedor na lista de provedores da seção entityFramework do arquivo de configuração do aplicativo. Por exemplo:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

A cadeia de caracteres de _tipo_ deve ser o nome do tipo qualificado por assembly da implementação de DbProviderServices a ser usada.

### <a name="code-based-registration"></a>Registro baseado em código

Começar com provedores EF6 também pode ser registrado usando código. Isso permite que um provedor do EF seja usado sem qualquer alteração no arquivo de configuração do aplicativo. Para usar a configuração baseada em código, um aplicativo deve criar uma classe DbConfiguration, conforme descrito na [documentação de configuração baseada em código](https://msdn.com/data/jj680699). O construtor da classe DbConfiguration deve então chamar setproviderservices para registrar o provedor do EF. Por exemplo:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Resolvendo serviços adicionais

Conforme mencionado acima na seção _visão geral de tipos de provedor_ , uma classe DbProviderServices também pode ser usada para resolver serviços adicionais. Isso é possível porque DbProviderServices implementa IDbDependencyResolver e cada tipo de DbProviderServices registrado é adicionado como um "resolvedor padrão". O mecanismo IDbDpendencyResolver é descrito mais detalhadamente na [resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md). No entanto, não é necessário entender todos os conceitos nessa especificação para resolver serviços adicionais em um provedor.

A maneira mais comum de um provedor resolver serviços adicionais é chamar DbProviderServices. AddDependencyResolver para cada serviço no construtor da classe DbProviderServices. Por exemplo, SqlProviderServices (o provedor do EF para SQL Server) tem um código semelhante a este para inicialização:

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

Esse construtor usa as seguintes classes auxiliares:

*   SingletonDependencyResolver: fornece uma maneira simples de resolver serviços singleton, ou seja, serviços para os quais a mesma instância é retornada cada vez que GetService é chamado. Os serviços transitórios geralmente são registrados como um alocador singleton que será usado para criar instâncias transitórias sob demanda.
*   ExecutionStrategyResolver: um resolvedor específico para retornar implementações de IExecutionStrategy.

Em vez de usar DbProviderServices. AddDependencyResolver, também é possível substituir DbProviderServices. GetService e resolver serviços adicionais diretamente. Esse método será chamado quando o EF precisar de um serviço definido por um determinado tipo e, em alguns casos, para uma determinada chave. O método deve retornar o serviço se puder, ou retornar NULL para recusar o retorno do serviço e, em vez disso, permitir que outra classe o resolva. Por exemplo, para resolver a fábrica de conexões padrão, o código em GetService pode ser semelhante a este:

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

Quando várias implementações de DbProviderServices são registradas no arquivo de configuração de um aplicativo, elas serão adicionadas como resolvedores secundários na ordem em que estão listadas. Como os resolvedores são sempre adicionados à parte superior da cadeia de resolvedor secundário, isso significa que o provedor no final da lista terá a chance de resolver dependências antes das outras. (Isso pode parecer um pouco intuitivo, mas faz sentido se você imaginar que você está tirando cada provedor da lista e empilhando-o sobre os provedores existentes.)

Normalmente, essa ordenação não importa porque a maioria dos serviços de provedor são específicas do provedor e são codificados pelo nome invariável do provedor. No entanto, para serviços que não são codificados pelo nome invariável do provedor ou alguma outra chave específica do provedor, o serviço será resolvido com base nessa ordenação. Por exemplo, se não for definido explicitamente de modo diferente em outro lugar, a fábrica de conexão padrão será proveniente do provedor de nível superior na cadeia.

## <a name="additional-config-file-registrations"></a>Registros de arquivo de configuração adicionais

É possível registrar explicitamente alguns dos serviços de provedor adicionais descritos acima diretamente no arquivo de configuração de um aplicativo. Quando isso for feito, o registro no arquivo de configuração será usado em vez de qualquer item retornado pelo método GetService da implementação DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Registrando a fábrica de conexões padrão

Começando com o EF5, o pacote NuGet do EntityFramework registrou automaticamente a fábrica de conexão do SQL Express ou a fábrica de conexões do LocalDb no arquivo de configuração.

Por exemplo:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

O _tipo_ é o nome do tipo qualificado por assembly para a fábrica de conexão padrão, que deve implementar IDbConnectionFactory.

É recomendável que um pacote NuGet do provedor defina o alocador de conexão padrão dessa maneira quando instalado. Consulte _pacotes NuGet para provedores_ abaixo.

## <a name="additional-ef6-provider-changes"></a>Alterações de provedor EF6 adicionais

### <a name="spatial-provider-changes"></a>Alterações de provedor espacial

Provedores que dão suporte a tipos espaciais agora devem implementar alguns métodos adicionais em classes derivadas de DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Também há novas versões assíncronas de métodos existentes que são recomendáveis para serem substituídas, pois as implementações padrão delegam para os métodos síncronos e, portanto, não são executadas de forma assíncrona:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Suporte nativo para Enumerable. Contains

EF6 introduz um novo tipo de expressão, DbInExpression, que foi adicionado para resolver problemas de desempenho em relação ao uso de Enumerable. Contains em consultas LINQ. A classe DbProviderManifest tem um novo método virtual, SupportsInExpression, que é chamado pelo EF para determinar se um provedor manipula o novo tipo de expressão. Para compatibilidade com as implementações de provedor existentes, o método retorna false. Para se beneficiar dessa melhoria, um provedor EF6 pode adicionar código para manipular o DbInExpression e substituir SupportsInExpression para retornar true. Uma instância de DbInExpression pode ser criada chamando o método DbExpressionBuilder.In. Uma instância de DbInExpression é composta de um DbExpression, geralmente representando uma coluna de tabela e uma lista de DbConstantExpression para testar uma correspondência.

## <a name="nuget-packages-for-providers"></a>Pacotes NuGet para provedores

Uma maneira de disponibilizar um provedor EF6 é liberá-lo como um pacote NuGet. O uso de um pacote NuGet tem as seguintes vantagens:

*   É fácil usar o NuGet para adicionar o registro do provedor ao arquivo de configuração do aplicativo
*   Alterações adicionais podem ser feitas no arquivo de configuração para definir a fábrica de conexões padrão para que as conexões feitas por convenção usem o provedor registrado
*   O NuGet lida com a adição de redirecionamentos de associação para que o provedor EF6 deva continuar funcionando mesmo após o lançamento de um novo pacote do EF

Um exemplo disso é o pacote do EntityFramework. SqlServerCompact que está incluído na base de código de [código aberto](https://github.com/aspnet/entityframework6). Este pacote fornece um bom modelo para criar pacotes NuGet do provedor do EF.

### <a name="powershell-commands"></a>Comandos do PowerShell

Quando o pacote NuGet do EntityFramework é instalado, ele registra um módulo do PowerShell que contém dois comandos que são muito úteis para pacotes de provedor:

*   Add-EFProvider adiciona uma nova entidade para o provedor no arquivo de configuração do projeto de destino e verifica se ele está no final da lista de provedores registrados.
*   Add-EFDefaultConnectionFactory adiciona ou atualiza o registro defaultConnectionFactory no arquivo de configuração do projeto de destino.

Esses dois comandos cuidam da adição de uma seção do entityFramework ao arquivo de configuração e da adição de uma coleção de provedores, se necessário.

O objetivo é que esses comandos sejam chamados do script de NuGet install. ps1. Por exemplo, install. ps1 para o provedor do SQL Compact é semelhante a este:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Mais informações sobre esses comandos podem ser obtidas usando Get-Help na janela do console do Gerenciador de pacotes.

## <a name="wrapping-providers"></a>Provedores de encapsulamento

Um provedor de encapsulamento é um provedor de EF e/ou ADO.NET que encapsula um provedor existente para estendê-lo com outras funcionalidades, como recursos de criação de perfil ou rastreamento. Os provedores de encapsulamento podem ser registrados de forma normal, mas geralmente é mais conveniente configurar o provedor de encapsulamento em tempo de execução interceptando a resolução de serviços relacionados ao provedor. O evento estático OnLockingConfiguration na classe DbConfiguration pode ser usado para fazer isso.

OnLockingConfiguration é chamado após o EF determinar onde toda a configuração do EF para o domínio do aplicativo será obtida, mas antes de ser bloqueada para uso. Na inicialização do aplicativo (antes de o EF ser usado), o aplicativo deve registrar um manipulador de eventos para esse evento. (Estamos considerando adicionar suporte para registrar esse manipulador no arquivo de configuração, mas ainda não há suporte para isso.) O manipulador de eventos deve fazer uma chamada para ReplaceService para cada serviço que precisa ser encapsulado.  

Por exemplo, para encapsular IDbConnectionFactory e DbProviderService, um manipulador como este deve ser registrado:

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

O serviço que foi resolvido e agora deve ser encapsulado junto com a chave que foi usada para resolver o serviço é passado para o manipulador. O manipulador pode encapsular esse serviço e substituir o serviço retornado pela versão encapsulada.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Resolvendo um DbProviderFactory com o EF

DbProviderFactory é um dos tipos de provedor fundamentais necessários para o EF, conforme descrito na seção _visão geral de tipos de provedor_ acima. Como já mencionado, não é um tipo de EF e o registro geralmente não faz parte da configuração do EF, mas é o registro normal do provedor de ADO.NET no arquivo Machine. config e/ou no arquivo de configuração do aplicativo.

Apesar disso, o EF ainda usa seu mecanismo normal de resolução de dependência ao procurar um DbProviderFactory a ser usado. O resolvedor padrão usa o registro normal de ADO.NET nos arquivos de configuração e, portanto, isso geralmente é transparente. Mas, devido ao mecanismo de resolução de dependência normal ser usado, isso significa que um IDbDependencyResolver pode ser usado para resolver um DbProviderFactory mesmo quando o registro normal de ADO.NET não tiver sido feito.

A resolução de DbProviderFactory dessa maneira tem várias implicações:

*   Um aplicativo que usa a configuração baseada em código pode adicionar chamadas em sua classe DbConfiguration para registrar o DbProviderFactory apropriado. Isso é especialmente útil para aplicativos que não querem (ou não podem) usar qualquer configuração baseada em arquivo.
*   O serviço pode ser encapsulado ou substituído usando ReplaceService conforme descrito na seção _provedores de encapsulamento_ acima
*   Teoricamente, uma implementação DbProviderServices poderia resolver um DbProviderFactory.

O ponto importante a ser observado sobre a realização de qualquer uma dessas coisas é que elas só afetarão a pesquisa de DbProviderFactory pelo EF. Outro código que não seja o EF ainda pode esperar que o provedor de ADO.NET seja registrado da maneira normal e possa falhar se o registro não for encontrado. Por esse motivo, normalmente é melhor que um DbProviderFactory seja registrado na maneira normal do ADO.NET.

### <a name="related-services"></a>Serviços relacionados

Se o EF for usado para resolver um DbProviderFactory, ele também deverá resolver os serviços IProviderInvariantName e IDbProviderFactoryResolver.

IProviderInvariantName é um serviço que é usado para determinar um nome invariável de provedor para um determinado tipo de DbProviderFactory. A implementação padrão desse serviço usa o registro do provedor ADO.NET. Isso significa que, se o provedor ADO.NET não estiver registrado de forma normal porque o DbProviderFactory está sendo resolvido pelo EF, também será necessário resolver esse serviço. Observe que um resolvedor para esse serviço é adicionado automaticamente ao usar o método DbConfiguration. SetProviderFactory.

Conforme descrito na seção de _visão geral de tipos de provedor_ acima, o IDbProviderFactoryResolver é usado para obter o DbProviderFactory correto de um determinado objeto DbConnection. A implementação padrão desse serviço quando executado no .NET 4 usa o registro do provedor ADO.NET. Isso significa que, se o provedor ADO.NET não estiver registrado de forma normal porque o DbProviderFactory está sendo resolvido pelo EF, também será necessário resolver esse serviço.
