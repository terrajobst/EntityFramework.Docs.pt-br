---
title: Usando Migrate. exe-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 34866ddbbf81f090a064af148a612dd354ae9401
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824817"
---
# <a name="using-migrateexe"></a>Usando o Migrate. exe
Migrações do Code First pode ser usado para atualizar um banco de dados de dentro do Visual Studio, mas também pode ser executado por meio da ferramenta de linha de comando Migrate. exe. Esta página fornecerá uma visão geral rápida sobre como usar o Migrate. exe para executar migrações em um banco de dados.

> [!NOTE]
> Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos. Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="copy-migrateexe"></a>Copiar Migrate. exe

Quando você instala Entity Framework usando o NuGet, o. exe estará dentro da pasta Ferramentas do pacote baixado. Na pasta &lt;projeto&gt;\\pacotes\\EntityFramework.&lt;versão&gt;ferramentas de \\

Depois de migrar. exe, você precisará copiá-lo para o local do assembly que contém as migrações.

Se o seu aplicativo tiver como alvo o .NET 4, e não 4,5, você precisará copiar o **redirect. config** no local também e renomeá-lo como **migrar. exe. config**. Isso é para que o Migrate. exe obtenha os redirecionamentos de ligação corretos para poder localizar o assembly Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Arquivos do .NET 4,5](~/ef6/media/net45files.png) | ![Arquivos do .NET 4,0](~/ef6/media/net40files.png) |

> [!NOTE]
> o Migrate. exe não dá suporte a assemblies x64.

Depois de mover o Migrate. exe para a pasta correta, você poderá usá-lo para executar migrações no banco de dados. Tudo o que o utilitário foi projetado para fazer é executar migrações. Ele não pode gerar migrações ou criar um script SQL.

## <a name="see-options"></a>Consulte Opções

``` console
Migrate.exe /?
```

O acima exibirá a página de ajuda associada a esse utilitário, observe que será necessário ter o EntityFramework. dll no mesmo local em que você está executando o Migrate. exe para que isso funcione.

## <a name="migrate-to-the-latest-migration"></a>Migrar para a migração mais recente

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile="..\\web.config"
```

Ao executar migre. exe, o único parâmetro obrigatório é o assembly, que é o assembly que contém as migrações que você está tentando executar, mas ele usará todas as configurações baseadas em convenção se você não especificar o arquivo de configuração.

## <a name="migrate-to-a-specific-migration"></a>Migrar para uma migração específica

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /targetMigration="AddTitle"
```

Se você quiser executar migrações para uma migração específica, poderá especificar o nome da migração. Isso executará todas as migrações anteriores, conforme necessário, até chegar à migração especificada.

## <a name="specify-working-directory"></a>Especificar diretório de trabalho

``` console
Migrate.exe MyApp.exe /startupConfigurationFile="MyApp.exe.config" /startupDirectory="c:\\MyApp"
```

Se o assembly tiver dependências ou ler arquivos relativos ao diretório de trabalho, será necessário definir startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Especificar a configuração de migração a ser usada

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile="..\\web.config"
```

Se você tiver várias classes de configuração de migração, classes herdadas de DbMigrationConfiguration, será necessário especificar qual será usada para essa execução. Isso é especificado fornecendo o segundo parâmetro opcional sem uma opção como acima.

## <a name="provide-connection-string"></a>Fornecer cadeia de conexão

``` console
Migrate.exe BlogDemo.dll /connectionString="Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI" /connectionProviderName="System.Data.SqlClient"
```

Se você quiser especificar uma cadeia de conexão na linha de comando, também deverá fornecer o nome do provedor. Não especificar o nome do provedor causará uma exceção.

## <a name="common-problems"></a>Problemas comuns

| Mensagem de Erro                                                                                                                                                                                                                                                                                                                      | {1&gt;&lt;1} Solução                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exceção sem tratamento: System. IO. FileLoadException: não foi possível carregar o arquivo ou assembly ' EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' ou uma de suas dependências. A definição do manifesto do assembly localizado não corresponde à referência do assembly. (Exceção de HRESULT: 0x80131040)         | Isso normalmente significa que você está executando um aplicativo .NET 4 sem o arquivo reredirect. config. Você precisa copiar o redirect. config para o mesmo local que o Migrate. exe e renomeá-lo como Migrate. exe. config.                                                                                       |
| Exceção sem tratamento: System. IO. FileLoadException: não foi possível carregar o arquivo ou assembly ' EntityFramework, Version = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 ' ou uma de suas dependências. A definição do manifesto do assembly localizado não corresponde à referência do assembly. (Exceção de HRESULT: 0x80131040)          | Essa exceção significa que você está executando um aplicativo .NET 4,5 com o redirect. config copiado para o local Migrate. exe. Se seu aplicativo for .NET 4,5, você não precisará ter o arquivo de configuração com os redirecionamentos dentro do. Exclua o arquivo Migrate. exe. config.                                    |
| ERRO: não é possível atualizar o banco de dados para corresponder ao modelo atual porque há alterações pendentes e a migração automática está desabilitada. Grave as alterações de modelo pendentes em uma migração baseada em código ou habilite a migração automática. Defina DbMigrationsConfiguration. AutomaticMigrationsEnabled como true para habilitar a migração automática. | Esse erro ocorrerá se você estiver executando migrar quando não tiver criado uma migração para lidar com as alterações feitas no modelo e o banco de dados não corresponder ao modelo. A adição de uma propriedade a uma classe de modelo, em seguida, a execução de migrar. exe sem a criação de uma migração para atualizar o banco de dados é um exemplo disso. |
| ERRO: o tipo não foi resolvido para o membro ' System. Data. Entity. Migrations. Design. ToolingFacade + UpdateRunner, EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089 '.                                                                                                                                       | Esse erro pode ser causado pela especificação de um diretório de inicialização incorreto. Deve ser o local de Migrate. exe                                                                                                                                                                                      |
| Exceção sem tratamento: System. NullReferenceException: referência de objeto não definida como uma instância de um objeto. <br/>   em System. Data. Entity. Migrations. console. Program. Main (String [] args)                                                                                                                                             | Isso pode ser causado por não especificar um parâmetro obrigatório para um cenário que você está usando. Por exemplo, especificando uma cadeia de conexão sem especificar o nome do provedor.                                                                                                                        |
| ERRO: mais de um tipo de configuração de migrações foi encontrado no assembly ' ClassLibrary1 '. Especifique o nome do um a ser usado.                                                                                                                                                                                                  | Como o erro diz, há mais de uma classe de configuração no assembly fornecido. Você deve usar a opção/configurationType para especificar qual usar.                                                                                                                                           |
| ERRO: não foi possível carregar o arquivo ou o assembly '&lt;assemblyName&gt;' ou uma de suas dependências. O nome ou a codebase do assembly fornecido era inválido. (Exceção de HRESULT: 0x80131047)                                                                                                                                                    | Isso pode ser causado pela especificação de um nome de assembly incorretamente ou por não ter                                                                                                                                                                                                                          |
| ERRO: não foi possível carregar o arquivo ou o assembly '&lt;assemblyName&gt;' ou uma de suas dependências. Foi feita uma tentativa para carregar um programa com um formato incorreto.                                                                                                                                                                          | Isso acontece se você estiver tentando executar o Migrate. exe em um aplicativo x64. O EF 5,0 e inferior funcionarão apenas no x86.                                                                                                                                                                                |
