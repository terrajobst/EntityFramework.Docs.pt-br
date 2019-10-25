---
title: Usando um projeto de migrações separado-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812035"
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

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
