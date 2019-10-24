---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812140"
---
# <a name="whats-new-in-ef6"></a>Novidades no EF6

É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.
No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.
Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

O tempo de execução do EF 6.3.0 foi lançado para o NuGet em setembro de 2019. O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0. A comunidade também contribuiu com diversos aprimoramentos e correções de bugs. Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes. Estes são alguns dos mais notáveis:

- Suporte para .NET Core 3.0
  - Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1, além do .NET Framework 4.x.
  - Isso significa que o EF 6.3 é multiplataforma e tem suporte em outros sistemas operacionais além do Windows, como Linux e macOS.
  - Os comandos de migrações foram reescritos para executar fora do processo e trabalhar com projetos de estilo SDK.
- Suporte para HierarchyId do SQL Server.
- Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet.
- Adição do utilitário `ef6.exe` para habilitar, adicionar, executar scripts e aplicar migrações de assemblies. Substitui o `migrate.exe`.

### <a name="ef-designer-support"></a>Suporte ao designer do EF

Atualmente, não há suporte para usar o designer do EF diretamente em projetos do .NET Core ou .NET Standard. 

É possível contornar essa limitação adicionando o arquivo EDMX e as classes geradas para as entidades e o DbContext como arquivos vinculados a um projeto do .NET Core 3.0 ou .NET Standard 2.1 na mesma solução.

Os arquivos vinculados seriam semelhantes a este arquivo de projeto:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

Observe que o arquivo EDMX está vinculado à ação de build de EntityDeploy. Essa é uma tarefa especial do MSBuild (agora incluída no pacote do EF 6.3), que adiciona o modelo do EF no assembly de destino como recursos incorporados ou o copia como arquivos na pasta de saída, dependendo da configuração de processamento de artefatos de metadados no EDMX. Para obter mais detalhes sobre como obter essa configuração, confira nosso [Exemplo de EDMX no .NET Core](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Versões anteriores

A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.
