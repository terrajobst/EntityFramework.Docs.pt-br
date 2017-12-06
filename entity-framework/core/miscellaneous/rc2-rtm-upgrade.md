---
title: "Atualização do EF Core 1.0 RC2 para RTM - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Atualização do EF Core 1.0 RC2 para RTM

Este artigo fornece diretrizes para mover um aplicativo criado com os pacotes RC2 1.0.0 RTM.

## <a name="package-versions"></a>Versões do pacote

Os nomes dos pacotes de nível superior que normalmente você instala em um aplicativo não foram alterado entre RC2 e RTM.

**Você precisa atualizar os pacotes instalados para as versões RTM:**

* Pacotes de tempo de execução (por exemplo, `Microsoft.EntityFrameworkCore.SqlServer`) alterado de `1.0.0-rc2-final` para `1.0.0`.

* O `Microsoft.EntityFrameworkCore.Tools` pacote alterado de `1.0.0-preview1-final` para `1.0.0-preview2-final`. Observe que ferramentas ainda pré-lançamento.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migrações existentes podem ser necessário maxLength adicionado

No RC2, a definição de coluna em uma migração parecia `table.Column<string>(nullable: true)` e o comprimento da coluna foi pesquisado em alguns metadados armazenamos no code-behind a migração. Na versão RTM, o comprimento agora está incluído no código scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

Quaisquer migrações existentes que foram Scaffold antes de usar RTM não terá o `maxLength` argumento especificado. Isso significa que o comprimento máximo aceito pelo banco de dados será usado (`nvarchar(max)` no SQL Server). Isso pode ser bom para algumas colunas, mas as colunas que fazem parte de uma chave, a chave estrangeira, ou o índice precisa ser atualizado para incluir um comprimento máximo. Por convenção, 450 é o comprimento máximo usado para chaves, chaves estrangeiras e colunas indexadas. Se você configurou um comprimento explicitamente no modelo, você deve usar esse comprimento em vez disso.

**Identidade do ASP.NET**

Essa alteração afeta projetos que usam o ASP.NET Identity e foram criados a partir de pré-RTM do modelo de projeto. O modelo de projeto inclui uma migração usada para criar o banco de dados. Essa migração deve ser editada para especificar um comprimento máximo de `256` para as seguintes colunas.

*  **AspNetRoles**

    * Nome

    * NormalizedName

*  **AspNetUsers**

   * Email

   * NormalizedEmail

   * NormalizedUserName

   * Nome de usuário

Falha ao fazer esta alteração resultará na seguinte exceção quando a migração inicial é aplicada a um banco de dados.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Remover "imports" no Project

Se foram direcionando o .NET Core com RC2, você precisava adicionar `imports` para Project como uma solução temporária para algumas das dependências do núcleo do EF não dá suporte à .NET padrão. Eles agora podem ser removidos.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a>UWP: Adicione redirecionamentos de associação

Ao tentar executar os comandos EF nos resultados de projetos do Windows UWP (plataforma Universal) no seguinte erro:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Você precisa adicionar manualmente os redirecionamentos de associação ao projeto UWP. Crie um arquivo chamado `App.config` no projeto pasta raiz e adicionar redirecionamentos para as versões de assembly correto.

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
