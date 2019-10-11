---
title: Atualizando do EF Core 1,0 RC2 para RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181358"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Atualizando do EF Core 1,0 RC2 para RTM

Este artigo fornece diretrizes para mover um aplicativo criado com os pacotes RC2 para o 1.0.0 RTM.

## <a name="package-versions"></a>Versões do pacote

Os nomes dos pacotes de nível superior que você normalmente instalaria em um aplicativo não foram alterados entre o RC2 e o RTM.

**Você precisa atualizar os pacotes instalados para as versões RTM:**

* Pacotes de tempo de execução (por exemplo, `Microsoft.EntityFrameworkCore.SqlServer`) alterados de `1.0.0-rc2-final` para `1.0.0`.

* O pacote `Microsoft.EntityFrameworkCore.Tools` mudou de `1.0.0-preview1-final` para `1.0.0-preview2-final`. Observe que as ferramentas ainda são pré-lançamento.

## <a name="existing-migrations-may-need-maxlength-added"></a>As migrações existentes podem precisar de maxLength adicionado

No RC2, a definição de coluna em uma migração ficou como `table.Column<string>(nullable: true)` e o comprimento da coluna foi pesquisado em alguns metadados que armazenamos no código por trás da migração. No RTM, o comprimento agora está incluído no código com Scaffold `table.Column<string>(maxLength: 450, nullable: true)`.

Todas as migrações existentes que foram com scaffolddas antes de usar a versão RTM não terão o argumento `maxLength` especificado. Isso significa que o comprimento máximo com suporte do banco de dados será usado (`nvarchar(max)` em SQL Server). Isso pode ser adequado para algumas colunas, mas as colunas que fazem parte de uma chave, chave estrangeira ou índice precisam ser atualizadas para incluir um comprimento máximo. Por convenção, 450 é o comprimento máximo usado para chaves, chaves estrangeiras e colunas indexadas. Se você tiver configurado explicitamente um comprimento no modelo, deverá usar esse comprimento em vez disso.

**ASP.NET Identity**

Essa alteração afeta os projetos que usam ASP.NET Identity e foram criados a partir de um modelo de projeto anterior ao RTM. O modelo de projeto inclui uma migração usada para criar o banco de dados. Essa migração deve ser editada para especificar um comprimento máximo de `256` para as colunas a seguir.

*  **AspNetRoles**

    * Nome

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * UserName

A falha ao fazer essa alteração resultará na seguinte exceção quando a migração inicial for aplicada a um banco de dados.

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Remover "Imports" em Project. JSON

Se você estiver visando o .NET Core com o RC2, precisaria adicionar `imports` ao Project. JSON como uma solução alternativa temporária para algumas das dependências de EF Core que não dão suporte a .NET Standard. Agora eles podem ser removidos.

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
> A partir da versão 1,0 RTM, o [SDK do .NET Core](https://www.microsoft.com/net/download/core) não oferece mais suporte a `project.json` ou a desenvolver aplicativos .NET Core usando o Visual Studio 2015. Recomendamos que você [migre do project.json para csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Se você estiver usando o Visual Studio, recomendamos que atualize para o [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Adicionar redirecionamentos de associação

A tentativa de executar comandos do EF em projetos Plataforma Universal do Windows (UWP) resulta no seguinte erro:

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

Você precisa adicionar manualmente os redirecionamentos de associação ao projeto UWP. Crie um arquivo chamado `App.config` na pasta raiz do projeto e adicione redirecionamentos às versões corretas do assembly.

```xml
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
