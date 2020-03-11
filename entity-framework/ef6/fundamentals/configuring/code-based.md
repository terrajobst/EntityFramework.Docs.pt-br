---
title: Configuração baseada em código-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417865"
---
# <a name="code-based-configuration"></a>Configuração baseada em código
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

A configuração de um aplicativo Entity Framework pode ser especificada em um arquivo de configuração (App. config/Web. config) ou por meio de código. O último é conhecido como configuração baseada em código.  

A configuração em um arquivo de configuração é descrita em um [artigo separado](config-file.md). O arquivo de configuração tem precedência sobre a configuração baseada em código. Em outras palavras, se uma opção de configuração for definida no código e no arquivo de configuração, a configuração no arquivo de configuração será usada.  

## <a name="using-dbconfiguration"></a>Usando DbConfiguration  

A configuração baseada em código no EF6 e acima é obtida com a criação de uma subclasse de System. Data. Entity. config. DbConfiguration. As diretrizes a seguir devem ser seguidas durante a subclasse DbConfiguration:  

- Crie apenas uma classe DbConfiguration para seu aplicativo. Essa classe especifica as configurações de todo o domínio do aplicativo.  
- Coloque sua classe DbConfiguration no mesmo assembly que sua classe DbContext. (Consulte a seção *movendo DbConfiguration* se desejar alterar isso.)  
- Dê à sua classe DbConfiguration um construtor público sem parâmetros.  
- Defina as opções de configuração chamando métodos DbConfiguration protegidos de dentro deste construtor.  

Seguir essas diretrizes permite que o EF descubra e use sua configuração automaticamente pelas duas ferramentas que precisam acessar seu modelo e quando seu aplicativo é executado.  

## <a name="example"></a>Exemplo  

Uma classe derivada de DbConfiguration pode ser parecida com esta:  

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

Essa classe configura o EF para usar a estratégia de execução de SQL Azure – para repetir automaticamente as operações de banco de dados com falha e para usar o BD local para bancos de dados criados por convenção de Code First.  

## <a name="moving-dbconfiguration"></a>Movendo DbConfiguration  

Há casos em que não é possível posicionar sua classe DbConfiguration no mesmo assembly que a classe DbContext. Por exemplo, você pode ter duas classes DbContext cada em assemblies diferentes. Há duas opções para lidar com isso.  

A primeira opção é usar o arquivo de configuração para especificar a instância de DbConfiguration a ser usada. Para fazer isso, defina o atributo codeConfigurationType da seção entityFramework. Por exemplo:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

O valor de codeConfigurationType deve ser o assembly e o nome qualificado do namespace da sua classe DbConfiguration.  

A segunda opção é posicionar DbConfigurationTypeAttribute em sua classe de contexto. Por exemplo:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

O valor passado para o atributo pode ser seu tipo DbConfiguration, conforme mostrado acima, ou a cadeia de caracteres de nome de tipo qualificado de namespace e assembly. Por exemplo:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>Configurando DbConfiguration explicitamente  

Há algumas situações em que a configuração pode ser necessária antes que qualquer tipo de DbContext tenha sido usado. Exemplos disso incluem:  

- Usando DbModelBuilder para criar um modelo sem um contexto  
- Usando algum outro código de estrutura/utilitário que utiliza um DbContext onde esse contexto é usado antes do contexto do aplicativo ser usado  

Em tais situações, o EF não pode descobrir a configuração automaticamente e, em vez disso, você deve executar um dos seguintes procedimentos:  

- Defina o tipo DbConfiguration no arquivo de configuração, conforme descrito na seção *movendo DbConfiguration* acima
- Chamar o método DbConfiguration. setconfiguração estático durante a inicialização do aplicativo  

## <a name="overriding-dbconfiguration"></a>Substituindo DbConfiguration  

Há algumas situações em que você precisa substituir o conjunto de configurações no DbConfiguration. Isso normalmente não é feito por desenvolvedores de aplicativos, mas sim por provedores de terceiros e plug-ins que não podem usar uma classe DbConfiguration derivada.  

Para isso, o EntityFramework permite que um manipulador de eventos seja registrado que possa modificar a configuração existente antes de ser bloqueada.  Ele também fornece um método de simplificação especificamente para substituir qualquer serviço retornado pelo localizador de serviço do EF. É assim que ele deve ser usado:  

- Na inicialização do aplicativo (antes do EF ser usado), o plug-in ou o provedor deve registrar o método do manipulador de eventos para esse evento. (Observe que isso deve ocorrer antes que o aplicativo use o EF.)  
- O manipulador de eventos faz uma chamada para ReplaceService para cada serviço que precisa ser substituído.  

Por exemplo, para substituir IDbConnectionFactory e DbProviderService, você registraria um manipulador como este:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

No código acima, myproviderservices e MyConnectionFactory representam suas implementações do serviço.  

Você também pode adicionar manipuladores de dependência adicionais para obter o mesmo efeito.  

Observe que você também pode encapsular o DbProviderFactory dessa forma, mas isso afetará apenas o EF e não os usos do DbProviderFactory fora do EF. Por esse motivo, você provavelmente desejará continuar a encapsular o DbProviderFactory como antes.  

Você também deve ter em mente os serviços que executa externamente para seu aplicativo, por exemplo, ao executar migrações do console do Gerenciador de pacotes. Quando você executa a migração do console, ele tentará localizar seu DbConfiguration. No entanto, se ele receberá ou não o serviço encapsulado depende de onde o manipulador de eventos ele registrou. Se ele estiver registrado como parte da construção de seu DbConfiguration, o código deverá ser executado e o serviço deverá ser encapsulado. Normalmente, esse não será o caso e isso significa que as ferramentas não terão o serviço encapsulado.  
