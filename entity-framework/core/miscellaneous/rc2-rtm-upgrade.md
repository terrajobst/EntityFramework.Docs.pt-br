---
title: Atualização do EF Core 1.0 RC2 para RTM – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998313"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Atualização do EF Core 1.0 RC2 para RTM

Este artigo fornece diretrizes para mover um aplicativo compilado com os pacotes do RC2 para 1.0.0 RTM.

## <a name="package-versions"></a>Versões do pacote

Os nomes dos pacotes de nível superior que você normalmente instalaria em um aplicativo não foram alterada entre RC2 e RTM.

**Você precisará atualizar os pacotes instalados para as versões RTM:**

* Pacotes de tempo de execução (por exemplo, `Microsoft.EntityFrameworkCore.SqlServer`) foi alterado de `1.0.0-rc2-final` para `1.0.0`.

* O `Microsoft.EntityFrameworkCore.Tools` pacote foi alterado de `1.0.0-preview1-final` para `1.0.0-preview2-final`. Observe que as ferramentas ainda pré-lançamento.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migrações existentes, talvez seja necessário maxLength adicionado

No RC2, a definição de coluna em uma migração parecia `table.Column<string>(nullable: true)` e o comprimento da coluna foi pesquisado em alguns metadados armazenamos no code-behind a migração. Na versão RTM, o comprimento agora está incluído no código gerado por scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

As migrações existentes que foram geradas por scaffolding antes de usar o RTM não terá o `maxLength` argumento especificado. Isso significa que o comprimento máximo aceito pelo banco de dados será usado (`nvarchar(max)` no SQL Server). Isso pode ser bom para algumas colunas, mas as colunas que fazem parte de uma chave, chave estrangeira, ou o índice precisa ser atualizado para incluir um comprimento máximo. Por convenção, 450 é o comprimento máximo usado para chaves, chaves estrangeiras e colunas indexadas. Se você configurou um comprimento explicitamente no modelo, você deve usar esse comprimento em vez disso.

**Identidade do ASP.NET**

Essa alteração afeta os projetos que usam o ASP.NET Identity e foram criados a partir de um pré-RTM do modelo de projeto. O modelo de projeto inclui uma migração usada para criar o banco de dados. Essa migração deve ser editada para especificar um comprimento máximo de `256` para as seguintes colunas.

*  **AspNetRoles**

    * Nome

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Falha ao fazer essa alteração resultará na seguinte exceção quando a migração inicial é aplicada a um banco de dados.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Remover "imports" no Project. JSON

Se você estiver direcionando o .NET Core com o RC2, você precisava adicionar `imports` a Project. JSON como uma solução temporária para algumas das dependências do EF Core não dar suporte a .NET Standard. Eles agora podem ser removidos.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> A partir da versão 1.0 RTM, o [SDK do .NET Core](https://www.microsoft.com/net/download/core) não oferece suporte a `project.json` ou desenvolvendo aplicativos .NET Core usando o Visual Studio 2015. Recomendamos que você [migre do project.json para csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Se você estiver usando o Visual Studio, é recomendável atualizar para o [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Adicionar redirecionamentos de associação

A tentativa de executar comandos do EF em resultados de projetos de plataforma Universal do Windows (UWP) no seguinte erro:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Você precisa adicionar manualmente redirecionamentos de associação para o projeto UWP. Crie um arquivo chamado `App.config` no projeto de pasta raiz e adicione redirecionamentos para as versões de assembly correto.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
