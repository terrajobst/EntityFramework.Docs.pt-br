---
title: Usando um projeto de migrações separado-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824415"
---
# <a name="using-a-separate-migrations-project"></a>Usando um projeto de migrações separado

Talvez você queira armazenar suas migrações em um assembly diferente daquele que contém seu `DbContext`. Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de lançamento em lançamento.

Para fazer isso...

1. Criar uma nova biblioteca de classes.

2. Adicione uma referência ao seu assembly DbContext.

3. Mova os arquivos de instantâneos de modelo e migrações para a biblioteca de classes.
   > [!TIP]
   > Se você não tiver migrações existentes, gere uma no projeto que contém DbContext e, em seguida, mova-a.
   > Isso é importante porque, se o assembly de migrações não contiver uma migração existente, o comando Add-Migration não conseguirá localizar o DbContext.

4. Configure o assembly de migrações:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Adicione uma referência ao assembly de migrações do assembly de inicialização.
   * Se isso causar uma dependência circular, atualize o caminho de saída da biblioteca de classes:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Se você fez tudo corretamente, deve ser capaz de adicionar novas migrações ao projeto.

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI do .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
