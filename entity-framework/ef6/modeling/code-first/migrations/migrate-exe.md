---
title: Usando migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: cf6c3a0a256730b24addf1012d6ff53b17035cd4
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459533"
---
# <a name="using-migrateexe"></a>Usando migrate.exe
Migrações do Code First pode ser usadas para atualizar um banco de dados de dentro do visual studio, mas também podem ser executadas por meio de linha de comando ferramenta migrate.exe. Esta página fornecerá uma visão geral rápida sobre como usar migrate.exe para executar migrações em relação a um banco de dados.

> [!NOTE]
> Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos. Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="copy-migrateexe"></a>Copiar migrate.exe

Quando você instala o Entity Framework usando o NuGet migrate.exe estará dentro da pasta de ferramentas do pacote baixado. Na &lt;pasta do projeto&gt;\\pacotes\\EntityFramework.&lt; versão&gt;\\ferramentas

Quando você tiver migrate.exe, em seguida, você precisará copiá-lo para o local do assembly que contém suas migrações.

Se seu aplicativo tem como alvo o .NET 4, e não 4.5, em seguida, você precisará copiar o **Redirect.config** no local bem e renomeá-lo **migrate.exe.config**. Isso é para que migrate.exe obtém os redirecionamentos de associação correto para ser capaz de localizar o assembly do Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Arquivos do .NET 4.5](~/ef6/media/net45files.png) | ![Arquivos do .NET 4.0](~/ef6/media/net40files.png) |

> [!NOTE]
> Migrate.exe não dá suporte a x64 assemblies.

Depois de mover migrate.exe para a pasta correta, em seguida, você poderá usá-lo para executar migrações no banco de dados. Tudo o que o utilitário é projetado para fazer é executar as migrações. Ele não é possível gerar migrações ou criar um script SQL.

## <a name="see-options"></a>Consulte as opções

``` console
Migrate.exe /?
```

As opções acima exibirá a página de ajuda associada com esse utilitário, observe que você precisará ter o EntityFramework no mesmo local que você está executando migrate.exe para que isso funcione.

## <a name="migrate-to-the-latest-migration"></a>Migrar para a migração mais recente

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Quando executar migrate.exe o parâmetro obrigatório somente é o assembly, que é o assembly que contém as migrações que você está tentando executar, mas ele usará a convenção de todas as configurações baseadas no se você não especificar o arquivo de configuração.

## <a name="migrate-to-a-specific-migration"></a>Migrar para uma migração específica

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Se você quiser executar as migrações até uma migração específica, você pode especificar o nome da migração. Isso executará todas as migrações anteriores conforme necessário até que a introdução à migração especificada.

## <a name="specify-working-directory"></a>Especifique o diretório de trabalho

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Se você o assembly tem dependências ou lê arquivos em relação ao diretório de trabalho, em seguida, você precisará definir startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Especifique a configuração da migração para usar

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Se você tiver várias classes de configuração de migração, classes herdadas de DbMigrationConfiguration, em seguida, você precisa especificar qual deve ser usado para essa execução. Isso é especificado, fornecendo o segundo parâmetro opcional sem um comutador como acima.

## <a name="provide-connection-string"></a>Forneça a cadeia de caracteres de conexão

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Se você quiser especificar uma cadeia de caracteres de conexão na linha de comando, em seguida, você também deve fornecer o nome do provedor. Não especificando o nome do provedor fará com que uma exceção.

## <a name="common-problems"></a>Problemas comuns

| Mensagem de erro                                                                                                                                                                                                                                                                                                                      | Solução                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exceção sem tratamento: System.IO.FileLoadException: não foi possível carregar arquivo ou assembly ' EntityFramework, versão = Version=5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' ou uma de suas dependências. Definição do manifesto do assembly localizado não coincide com a referência de assembly. (Exceção de HRESULT: 0x80131040)         | Normalmente, isso significa que você está executando um aplicativo .NET 4 sem o arquivo Redirect.config. Você precisa copiar o Redirect.config no mesmo local como migrate.exe e renomeie-o para migrate.exe.config.                                                                                       |
| Exceção sem tratamento: System.IO.FileLoadException: não foi possível carregar arquivo ou assembly ' EntityFramework, versão = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' ou uma de suas dependências. Definição do manifesto do assembly localizado não coincide com a referência de assembly. (Exceção de HRESULT: 0x80131040)          | Essa exceção significa que você está executando um aplicativo com o Redirect.config copiado para o local de migrate.exe do .NET 4.5. Se seu aplicativo for .NET 4.5, em seguida, você não precisará ter o arquivo de configuração com os redirecionamentos dentro. Exclua o arquivo migrate.exe.config.                                    |
| Erro: Não é possível atualizar o banco de dados de acordo com o modelo atual porque existem alterações pendentes e a migração automática está desabilitada. Gravar as alterações do modelo pendentes em uma migração baseada em código ou habilitar a migração automática. Defina DbMigrationsConfiguration.AutomaticMigrationsEnabled para verdadeiro para habilitar a migração automática. | Esse erro ocorre se migrar de execução quando você não tiver criado uma migração para lidar com as alterações feitas no modelo e o banco de dados não coincide com o modelo. Adicionar uma propriedade a uma classe de modelo, em seguida, executando migrate.exe sem criar uma migração para atualizar o banco de dados é um exemplo disso. |
| Erro: Tipo não for resolvido para o membro ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, versão = Version=5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089'.                                                                                                                                       | Esse erro pode ser causado pela especificação de um diretório de inicialização incorretos. Isso deve ser o local de migrate.exe                                                                                                                                                                                      |
| Exceção sem tratamento: System. NullReferenceException: referência de objeto não definida para uma instância de um objeto. <br/>   no System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Isso pode ser causado por não especificar um parâmetro obrigatório para um cenário que você está usando. Por exemplo, especificando uma cadeia de caracteres de conexão sem especificar o nome do provedor.                                                                                                                        |
| Erro: mais de um tipo de configuração de migrações foi encontrado no assembly 'ClassLibrary1'. Especifique o nome de um para usar.                                                                                                                                                                                                  | Como o erro afirma, há mais de uma classe de configuração no assembly fornecido. Você deve usar a opção de /configurationType para especificar qual deles usar.                                                                                                                                           |
| Erro: Não foi possível carregar arquivo ou assembly '&lt;assemblyName&gt;' ou uma de suas dependências. O assembly determinado nome ou a codebase era inválido. (Exceção de HRESULT: 0x80131047)                                                                                                                                                    | Isso pode ser causado por especificar um nome de assembly incorretamente ou não ter                                                                                                                                                                                                                          |
| Erro: Não foi possível carregar arquivo ou assembly '&lt;assemblyName&gt;' ou uma de suas dependências. Foi feita uma tentativa de carregar um programa com um formato incorreto.                                                                                                                                                                          | Isso acontece se você está tentando executar migrate.exe em x64 aplicativo. EF 5.0 e abaixo funcionará somente em x86.                                                                                                                                                                                |
