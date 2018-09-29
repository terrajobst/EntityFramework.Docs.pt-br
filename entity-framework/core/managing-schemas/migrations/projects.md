---
title: Migrações com vários projetos – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447138"
---
<a name="using-a-separate-project"></a>Usando um projeto separado
========================
Você pode desejar armazenar suas migrações em um assembly diferente que o outro contendo seu `DbContext`. Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para desenvolvimento e outro para atualizações de versão para versão.

Para fazer isso...

1. Criar uma nova biblioteca de classes.

2. Adicione uma referência ao seu assembly de DbContext.

3. Mova as migrações e arquivos de instantâneo do modelo para a biblioteca de classes.
   > [!TIP]
   > Se você não tiver nenhuma migração existente, gerá-lo no projeto que contém o DbContext e em seguida, movê-lo. Isso é importante porque se o assembly de migrações não contiver uma migração existente, o comando Add-Migration será não é possível localizar o DbContext.

4. Configure o conjunto de migrações:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Adicione uma referência ao seu assembly de migrações do assembly de inicialização.
   * Se isso faz com que uma dependência circular, atualize o caminho de saída da biblioteca de classes:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Se você fez tudo corretamente, você poderá adicionar novas migrações ao projeto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
