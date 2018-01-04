---
title: "Migrações com vários projetos - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
<a name="using-a-separate-project"></a>Usando um projeto separado
========================
Você pode desejar armazenar suas migrações em um conjunto diferente de uma contendo o `DbContext`. Você também pode usar essa estratégia para manter vários conjuntos de migrações, por exemplo, um para o desenvolvimento e outro para atualizações de versão para versão.

Para fazer isso...

1. Criar uma nova biblioteca de classes.

2. Adicione uma referência ao seu assembly de DbContext.

3. Mova os arquivos de instantâneo de modelo e migrações para a biblioteca de classes.
   * Se você não adicionou nenhum, adicione uma ao projeto DbContext e movê-lo.

4. Configure o assembly de migrações:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Adicione uma referência ao seu assembly de migrações do assembly de inicialização.
   * Se isso faz com que uma dependência circular, atualize o caminho de saída da biblioteca de classes:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Se você fez tudo corretamente, você poderá adicionar novas migrações ao projeto.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
