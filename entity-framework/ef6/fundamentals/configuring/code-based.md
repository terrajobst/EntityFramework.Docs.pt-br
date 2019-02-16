---
title: Configuração baseada em código – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325347"
---
# <a name="code-based-configuration"></a>Configuração baseada em código
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Configuração de um aplicativo do Entity Framework pode ser especificada em um arquivo de configuração (app.config/web.config) ou por meio de código. A última opção é conhecida como configuração baseada em código.  

Configuração em um arquivo de configuração é descrita em uma [artigo separado](config-file.md). O arquivo de configuração tem precedência sobre a configuração baseada em código. Em outras palavras, se uma opção de configuração está definida no código e no arquivo de configuração, a configuração no arquivo de configuração é usada.  

## <a name="using-dbconfiguration"></a>Usando DbConfiguration  

Configuração baseada em código no EF6 e superior é obtida com a criação de uma subclasse de System.Data.Entity.Config.DbConfiguration. As diretrizes a seguir devem ser seguidas ao subclasses DbConfiguration:  

- Crie apenas uma classe DbConfiguration para seu aplicativo. Essa classe especifica configurações de domínio de aplicativo.  
- Coloque sua classe DbConfiguration no mesmo assembly que sua classe DbContext. (Consulte a *DbConfiguration movendo* seção se você quiser alterar isso.)  
- Fornecer um construtor público sem parâmetros de sua classe DbConfiguration.  
- Definir opções de configuração chamando os métodos protegidos DbConfiguration, de dentro desse construtor.  

As diretrizes a seguir permite que o EF descubram e usem sua configuração automaticamente por ambas as ferramentas que precisa acessar o seu modelo e quando seu aplicativo é executado.  

## <a name="example"></a>Exemplo  

Uma classe derivada de DbConfiguration pode ter esta aparência:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

Essa classe define EF para usar a estratégia de execução do SQL Azure - para repetir automaticamente as operações de banco de dados com falha - e usar o banco de dados Local para bancos de dados que são criados por convenção do Code First.  

## <a name="moving-dbconfiguration"></a>Movendo DbConfiguration  

Há casos em que não é possível colocar sua classe DbConfiguration no mesmo assembly como sua classe DbContext. Por exemplo, você pode ter duas classes DbContext cada em diferentes assemblies. Há duas opções para lidar com isso.  

A primeira opção é usar o arquivo de configuração para especificar a instância DbConfiguration usar. Para fazer isso, defina o atributo codeConfigurationType da seção entityFramework. Por exemplo:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

O valor de codeConfigurationType deve ser o assembly e o nome qualificado do namespace da sua classe DbConfiguration.  

A segunda opção é colocar DbConfigurationTypeAttribute em sua classe de contexto. Por exemplo:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

O valor passado para o atributo pode ser seu tipo DbConfiguration - conforme mostrado acima - ou o assembly e namespace qualificado a cadeia de caracteres de nome de tipo. Por exemplo:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Configurar DbConfiguration explicitamente  

Há algumas situações em que configurações podem ser necessárias antes de qualquer tipo DbContext tem sido usado. Exemplos incluem:  

- Usando DbModelBuilder para criar um modelo sem contexto  
- Usando algum outro código de framework/utilitário que utiliza um DbContext em que esse contexto é usado antes de seu contexto de aplicativo é usado  

Em tais situações EF não conseguir descobrir a configuração automaticamente e, em vez disso, você deve fazer o seguinte:  

- Defina o tipo de DbConfiguration no arquivo de configuração, conforme descrito na *DbConfiguration movendo* seção acima
- Chame o método DbConfiguration.SetConfiguration estático durante a inicialização do aplicativo  

## <a name="overriding-dbconfiguration"></a>Substituindo DbConfiguration  

Há algumas situações em que você precisa substituir a configuração definida no DbConfiguration. Isso não é normalmente feito por desenvolvedores de aplicativos, mas em vez disso, por provedores de terceiros e plug-ins que não é possível usar uma classe derivada de DbConfiguration.  

Para isso, o EntityFramework permite que um manipulador de eventos a ser registrado que pode modificar a configuração existente antes que ele é bloqueado.  Ele também fornece um método de açúcar especificamente para a substituição de qualquer serviço retornado pelo localizador de serviço do EF. Isso é como ele se destina a ser usado:  

- Na inicialização do aplicativo (antes do EF é usado) o plug-in ou o provedor deve se registrar para o método de manipulador de eventos para este evento. (Observe que isso deve ocorrer antes que o aplicativo usa o EF).  
- O manipulador de eventos faz uma chamada para replaceservice tenha para cada serviço que precisa ser substituído.  

Por exemplo, repalce IDbConnectionFactory e DbProviderService, você deve registrar um manipulador algo assim:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

No código acima MyProviderServices e MyConnectionFactory representam suas implementações do serviço.  

Você também pode adicionar manipuladores de dependência adicional para obter o mesmo efeito.  

Observe que você pode também encapsular DbProviderFactory dessa maneira, mas fazer então afetará apenas EF e não os usos do DbProviderFactory fora do EF. Por esse motivo provavelmente você desejará continuar a encapsular o DbProviderFactory quando você tem antes.  

Você também deve ter em mente os serviços que são executados externamente para seu aplicativo - por exemplo, ao executar migrações no Console do Gerenciador de pacote. Quando você executa migre do console, que ele tentará localizar seu DbConfiguration. No entanto, se deseja ou não obterá o serviço encapsulado depende de onde o manipulador de eventos registrado por ele. Se ele estiver registrado como parte da construção do seu DbConfiguration, em seguida, o código deve ser executado e o serviço deve obter encapsulado. Normalmente, isso não será o caso, e isso significa que as ferramentas não obterá o serviço encapsulado.  
