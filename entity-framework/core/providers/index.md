---
title: Provedores de Banco de Dados – EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 7acc00c5e78ac5bc82f6d55d8b74682ff5f5a799
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74823979"
---
# <a name="database-providers"></a>Provedores de Banco de Dados

O Entity Framework Core pode acessar muitos bancos de dados diferentes por meio de bibliotecas de plug-in chamadas de provedores de banco de dados.

## <a name="current-providers"></a>Provedores atuais

> [!IMPORTANT]  
> Os provedores do EF Core são criados por uma variedade de origens. Nem todos os provedores são mantidos como parte do [Projeto do Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Ao considerar um provedor, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os seus requisitos. Examine também a documentação de cada provedor para obter informações detalhadas de compatibilidade de versão.

| Pacote NuGet                                                                                                        | Mecanismos de banco de dados com suporte | Mantenedor / fornecedor                                                           | Notas / requisitos | Links úteis                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 em diante    | [Projeto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 em diante         | [Projeto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Banco de dados em memória do EF Core | [Projeto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Somente para teste     | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | API do SQL do Azure Cosmos DB    | [Projeto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [docs](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Equipe de desenvolvimento do Npgsql](https://github.com/npgsql)                          |                      | [docs](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation Project](https://github.com/PomeloFoundation)              |                      | [Leiame](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Servidor MyCAT               | [Pomelo Foundation Project](https://github.com/PomeloFoundation)              | Somente pré-lançamento      | [Leiame](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3,5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 e 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 e 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Projeto MySQL](https://dev.mysql.com) (Oracle)                               |                      | [docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle Database 11.2 em diante     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Pré-lançamento           | [site](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versão do Windows      | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versão do Linux        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versão do macOS        | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Arquivos do Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Leiame](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Leiame](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle Database 9.2.0.4 em diante  | [DevArt](https://www.devart.com/)                                             | Pago                 | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 em diante     | [DevArt](https://www.devart.com/)                                             | Pago                 | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 em diante           | [DevArt](https://www.devart.com/)                                             | Pago                 | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 em diante            | [DevArt](https://www.devart.com/)                                             | Pago                 | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                 | Armazena dados em arquivos            | [Morris Janatzek](https://github.com/morrisjdev)                                             | Para fins de desenvolvimento                 | [Leiame](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |

## <a name="adding-a-database-provider-to-your-application"></a>Adicionando um provedor de banco de dados ao seu aplicativo

A maioria dos provedores de banco de dados para o EF Core é distribuída como pacotes NuGet e pode ser instalada da seguinte maneira:

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Depois de instalado, você configurará o provedor no `DbContext`, no método `OnConfiguring` ou no método `AddDbContext` se você estiver usando um contêiner de injeção de dependência.
Por exemplo, a seguinte linha configura o provedor do SQL Server com a cadeia de conexão passada:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Os provedores de banco de dados podem estender o EF Core para habilitar a funcionalidade exclusiva para bancos de dados específicos.
Alguns conceitos são comuns à maioria dos bancos de dados e estão incluídos nos componentes principais do EF Core.
Esses conceitos incluem expressar consultas no LINQ, em transações, e rastrear alterações aos objetos depois que eles forem carregados do banco de dados.
Alguns conceitos são específicos para um determinado provedor.
Por exemplo, o provedor SQL Server permite [configurar as tabelas com otimização de memória](xref:core/providers/sql-server/memory-optimized-tables) (um recurso específico do SQL Server).
Outros conceitos são específicos de uma classe de provedores.
Por exemplo, provedores do EF Core para bancos de dados relacionais são desenvolvidos com base na biblioteca `Microsoft.EntityFrameworkCore.Relational` comum, que fornece APIs para configurar mapeamentos de tabela e coluna, restrições de chave estrangeira etc. Os provedores geralmente são distribuídos como pacotes NuGet.

> [!IMPORTANT]  
> Quando uma nova versão de patch do EF Core é lançada, ela geralmente inclui atualizações para o pacote `Microsoft.EntityFrameworkCore.Relational`.
> Quando você adiciona um provedor de banco de dados relacional, esse pacote se torna uma dependência transitiva do seu aplicativo.
> Mas muitos provedores são lançados de forma independente do EF Core e podem não ser atualizados para dependerem da versão de patch mais recente do pacote.
> Para obter todas as correções de bug, é recomendável que você adicione a versão de patch do `Microsoft.EntityFrameworkCore.Relational` como uma dependência direta do seu aplicativo.
