---
title: Migrações – Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136204"
---
# <a name="migrations"></a>Migrações

Um modelo de dados muda durante o desenvolvimento e fica fora de sincronia com o banco de dados. Você pode descartar o banco de dados e permitir que o EF crie um novo que corresponda ao modelo, mas esse procedimento resulta na perda de dados. O recurso de migrações no EF Core oferece uma maneira de atualizar de forma incremental o esquema de banco de dados para mantê-lo em sincronia com o modelo de dados do aplicativo, preservando os dados existentes no banco de dados.

As migrações incluem ferramentas de linha de comando e APIs que ajudam com as seguintes tarefas:

* [Criar uma migração](#create-a-migration). Gerar um código que pode atualizar o banco de dados para sincronizá-lo com um conjunto de alterações do modelo.
* [Atualizar o banco de dados](#update-the-database). Aplicar migrações pendentes para atualizar o esquema de banco de dados.
* [Personalizar o código de migração](#customize-migration-code). Às vezes, o código gerado precisa ser modificado ou complementado.
* [Remover uma migração](#remove-a-migration). Excluir o código gerado.
* [Reverter uma migração](#revert-a-migration). Desfazer as alterações do banco de dados.
* [Gerar scripts SQL](#generate-sql-scripts). Talvez seja necessário um script para atualizar um banco de dados de produção ou para solucionar problemas de código de migração.
* [Aplicar migrações em runtime](#apply-migrations-at-runtime). Quando as atualizações de tempo de design e a execução de scripts não forem as melhores opções, chame o método `Migrate()`.

> [!TIP]
> Se `DbContext` está em um assembly diferente do projeto de inicialização, você pode especificar explicitamente o destino e os projetos de inicialização nas [ferramentas do Console do Gerenciador de Pacotes](xref:core/miscellaneous/cli/powershell#target-and-startup-project) ou nas [ferramentas da CLI do .NET Core](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).

## <a name="install-the-tools"></a>Instalar as ferramentas

Instale as [ferramentas de linha de comando](xref:core/miscellaneous/cli/index):

* Para o Visual Studio, recomendamos as [ferramentas do Console do Gerenciador de Pacotes](xref:core/miscellaneous/cli/powershell).
* Para outros ambientes de desenvolvimento, escolha as [ferramentas de CLI do .NET Core](xref:core/miscellaneous/cli/dotnet).

## <a name="create-a-migration"></a>Criar uma migração

Depois de ter [definido seu modelo inicial](xref:core/modeling/index), é hora de criar o banco de dados. Execute o comando a seguir para adicionar uma migração inicial.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

Três arquivos são adicionados ao seu projeto no diretório **Migrações**:

* **XXXXXXXXXXXXXX_InitialCreate.cs**– o arquivo principal de migrações. Contém as operações necessárias para aplicar a migração (em `Up()`) e revertê-la (em `Down()`).
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs**– o arquivo de metadados de migrações. Contém informações usadas pelo EF.
* **MyContextModelSnapshot.cs**– um instantâneo do seu modelo atual. Usado para determinar o que mudou ao adicionar a próxima migração.

O carimbo de data/hora no nome de arquivo ajuda a mantê-lo organizado por ordem cronológica para que você possa ver o andamento das alterações.

> [!TIP]
> Você é livre para mover os arquivos de Migrações e alterar o namespace deles. Novas migrações são criadas como irmãs da última migração.

## <a name="update-the-database"></a>Atualizar o banco de dados

Em seguida, aplique a migração ao banco de dados para criar o esquema.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>Personalizar o código de migração

Após fazer alterações no modelo do EF Core, o esquema de banco de dados pode ficar fora de sincronia. Para atualizá-las, adicione outra migração. O nome da migração pode ser usado como uma mensagem de confirmação em um sistema de controle de versão. Por exemplo, é possível escolher um nome como *AddProductReviews* caso a alteração seja uma nova classe de entidade para revisões.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

Depois que o scaffolding for aplicado à migração (codificado para ela), examine a precisão do código e adicione, remova ou modifique quaisquer operações necessárias para a correta aplicação.

Por exemplo, uma migração poderia conter as seguintes operações:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Embora essas operações tornem o esquema de banco de dados compatível, elas não preservam os nomes de cliente existentes. Para torná-las melhor, regrave-as da seguinte maneira.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> O processo de scaffolding da migração avisa quando uma operação puder resultar em perda de dados (como o descarte de uma coluna). Caso veja esse aviso, não deixe de examinar a precisão do código de migrações.

Aplique a migração ao banco de dados usando o comando apropriado.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>Migrações vazias

Às vezes é útil adicionar uma migração sem fazer nenhuma alteração ao modelo. Neste caso, adicionar uma nova migração cria arquivos de código com classes vazias. Você pode personalizar essa migração para executar operações que não estejam diretamente relacionadas ao modelo EF Core. Alguns elementos que talvez você queira gerenciar assim são:

* Pesquisa de texto completo
* Funções
* Procedimentos armazenados
* Gatilhos
* Exibições

## <a name="remove-a-migration"></a>Remover uma migração

Às vezes, você adiciona uma migração e percebe que precisa fazer alterações adicionais ao modelo do EF Core antes de aplicá-lo. Para remover a última migração, use este comando.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

Após remover a migração, você poderá fazer as alterações adicionais ao modelo e adicioná-la novamente.

## <a name="revert-a-migration"></a>Reverter uma migração

Se você já tiver aplicado uma migração (ou várias migrações) no banco de dados, mas precisar revertê-la, poderá usar o mesmo comando usado para aplicar as migrações, mas especificando o nome da migração para a qual você deseja reverter.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>Gerar scripts SQL

Ao depurar suas migrações ou implantá-las em um banco de dados de produção, é útil gerar um script SQL. O script então pode ser verificado em mais detalhes quanto à precisão e ajustado para atender às necessidades de um banco de dados de produção. O script também pode ser usado em conjunto com uma tecnologia de implantação. O comando básico é o seguinte.

### <a name="net-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Uso básico
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>Com From (To implícito)
Isso gerará um script SQL dessa migração para a migração mais recente.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>Com From e To
Isso gerará um script SQL da migração `from` para a migração `to` especificada.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
É possível usar um `from` mais recente que o `to` para gerar um script de reversão. *Anote os possíveis cenários de perda de dados.*

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Uso básico
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>Com From (To implícito)
Isso gerará um script SQL dessa migração para a migração mais recente.
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>Com From e To
Isso gerará um script SQL da migração `from` para a migração `to` especificada.
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
É possível usar um `from` mais recente que o `to` para gerar um script de reversão. *Anote os possíveis cenários de perda de dados.*

***

Há várias opções para este comando.

A migração **de** deve ser a última migração aplicada ao banco de dados antes de executar o script. Se nenhuma migração tiver sido aplicada, especifique `0` (esse é o padrão).

A migração **para** é a última migração que será aplicada ao banco de dados após a execução do script. O padrão é a última migração em seu projeto.

Um script **idempotente** pode ser gerado como opção. Esse script aplicará migrações somente se elas ainda não tiverem sido aplicadas ao banco de dados. Isso é útil se você não souber exatamente qual foi a última migração aplicada ao banco de dados ou estiver implantando em vários bancos de dados que podem estar, cada um, em uma migração diferente.

## <a name="apply-migrations-at-runtime"></a>Aplicar migrações em runtime

Alguns aplicativos talvez queiram aplicar migrações no runtime durante a inicialização ou a primeira execução. Faça isso usando o método `Migrate()`.

Este método é criado sobre o serviço `IMigrator`, que pode ser usado em cenários mais avançados. Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` para acessá-lo.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * Esta abordagem não é para todo mundo. Embora seja ótimo para aplicativos com um banco de dados local, a maioria dos aplicativos exigirá uma estratégia de implantação mais robusta, como gerar scripts SQL.
> * Não chame `EnsureCreated()` antes de `Migrate()`. O `EnsureCreated()` ignora as Migrações para criar o esquema e causa falha no `Migrate()`.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte <xref:core/miscellaneous/cli/index>.
