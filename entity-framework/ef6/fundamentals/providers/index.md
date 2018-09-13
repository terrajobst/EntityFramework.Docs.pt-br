---
title: Provedores do Entity Framework – EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: c9afb32caeeef5111b32251c62019460b62f48b3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489434"
---
# <a name="entity-framework-6-providers"></a>Provedores do Entity Framework 6
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O Entity Framework agora está sendo desenvolvido sob uma licença de software livre e o EF6 e acima não serão enviados como parte do .NET Framework. Isso tem muitas vantagens, mas também requer que os provedores do EF sejam recompilados nos assemblies do EF6. Isso significa que os provedores do EF para EF5 e abaixo não funcionarão com o EF6 até que eles sejam recompilados.

## <a name="which-providers-are-available-for-ef6"></a>Quais provedores estão disponíveis para o EF6?

Os provedores que sabemos que foram recompilados para o EF6 incluem:

*   **Provedor do Microsoft SQL Server**
    *   Compilado da [base de código de software livre do Entity Framework](http://github.com/aspnet/EntityFramework6)
    *   Enviado como parte do [pacote NuGet EntityFramework](http://nuget.org/packages/EntityFramework)
*   **Provedor do Microsoft SQL Server Compact Edition**
    *   Compilado da [base de código de software livre do Entity Framework](http://github.com/aspnet/EntityFramework6)
    *   Enviado no [pacote NuGet EntityFramework.SqlServerCompact](http://nuget.org/packages/EntityFramework.SqlServerCompact)
*   [**Provedores de dados do Devart dotConnect**](http://www.devart.com/dotconnect/)
    *   Há provedores de terceiros da [Devart](http://www.devart.com/) para uma variedade de bancos de dados, incluindo Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 e SQL Server
*   [**Provedores do CDATA Software**](http://www.cdata.com/ado/)
    *   Há provedores de terceiros da [CData Software](http://www.cdata.com/ado/) para uma variedade de armazenamentos de dados, incluindo Salesforce, Armazenamento de Tabelas do Azure, MySql e muito mais
*   **Provedor do Firebird**
    *   Disponível como um [Pacote NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)
*   **Provedor do Visual Fox Pro**
    *   Disponível como um [Pacote NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)
*   **MySQL**
    *   [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/)
*   **PostgreSQL**
    *   O Npgsql está disponível como um [pacote NuGet](http://www.nuget.org/packages/Npgsql.EF6/)
*   **Oracle**
    *   O ODP.NET está disponível como um [pacote NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)

Observe que a inclusão nesta lista não indica o nível de funcionalidade ou suporte para um determinado provedor, apenas que um build para o EF6 foi disponibilizado.

## <a name="registering-ef-providers"></a>Registrando provedores do EF

Começando com o Entity Framework 6, os provedores do EF podem ser registrados usando a configuração baseada em código ou no arquivo de configuração do aplicativo.

### <a name="config-file-registration"></a>Registro do arquivo de configuração

O registro do provedor do EF em app.config ou web.config tem o seguinte formato:


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

Observe que muitas vezes se o provedor do EF for instalado do NuGet, o pacote NuGet automaticamente adicionará esse registro ao arquivo de configuração. Se você instalar o pacote NuGet em um projeto que não é o projeto de inicialização para seu aplicativo, poderá ser necessário copiar o registro para o arquivo de configuração para seu projeto de inicialização.

O "invariantName" nesse registro é o mesmo nome invariável usado para identificar um provedor ADO.NET. Isso pode ser encontrado como o atributo "invariável" em um registro de DbProviderFactories e como o atributo "providerName" em um registro de cadeia de conexão. O nome invariável a ser usado também deve ser incluído na documentação do provedor. Alguns exemplos de nomes invariáveis são "System.Data.SqlClient" para o SQL Server e "System.Data.SqlServerCe.4.0" para o SQL Server Compact.

O "tipo" desse registro é o nome qualificado pelo assembly do tipo de provedor derivado de "System.Data.Entity.Core.Common.DbProviderServices". Por exemplo, a cadeia de caracteres a ser usada para o SQL Compact é “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”. O tipo a ser usado aqui deve ser incluído na documentação do provedor.

### <a name="code-based-registration"></a>Registro baseado em código

Começando com o Entity Framework 6, a configuração de todo o aplicativo para o EF pode ser especificada no código. Para obter os detalhes completos, consulte _[Configuração baseada em código do Entity Framework](https://msdn.microsoft.com/en-us/data/jj680699)_. A maneira normal de registrar um provedor do EF usando a configuração baseada em código é criar uma nova classe derivada de System.Data.Entity.DbConfiguration e colocá-la no mesmo assembly que a classe DbContext. A classe DbConfiguration deve, então, registrar o provedor em seu construtor. Por exemplo, para registrar o provedor do SQL Compact, a classe DbConfiguration esta aparência:

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

Nesse código "SqlCeProviderServices.ProviderInvariantName" é uma conveniência para a cadeia de caracteres do nome invariável do SQL Server Compact (“System.Data.SqlServerCe.4.0”) e SqlCeProviderServices.Instance retorna a instância singleton do provedor do EF do SQL Compact.

## <a name="what-if-the-provider-i-need-isnt-available"></a>E se o provedor necessário não estiver disponível?

Se o provedor estiver disponível para versões anteriores do EF, incentivamos você a entrar em contato com o proprietário do provedor e solicitar que ele crie uma versão do EF6. Você deve incluir uma referência para a [documentação para o modelo de provedor do EF6](~/ef6/fundamentals/providers/provider-model.md).

## <a name="can-i-write-a-provider-myself"></a>Posso eu mesmo escrever um provedor?

Certamente é possível criar um provedor do EF por conta própria, embora essa não deva ser considerada uma tarefa trivial. O link acima sobre o modelo de provedor do EF6 é um bom lugar para começar. Você também pode considerar útil usar o código para o provedor do SQL Server e do SQL CE incluído na [base de código de software livre do EF](https://github.com/aspnet/EntityFramework6) como um ponto de partida ou para referência.

Observe que a partir do EF6 o provedor do EF está menos rigidamente acoplado ao provedor ADO.NET subjacente. Isso torna mais fácil escrever um provedor do EF sem a necessidade de escrever ou encapsular as classes do ADO.NET.
