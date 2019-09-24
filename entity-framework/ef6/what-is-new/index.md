---
title: Novidades – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149126"
---
# <a name="whats-new-in-ef6"></a>Novidades no EF6

É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.
No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.
Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

O tempo de execução do EF 6.3.0 foi lançado para o NuGet em setembro de 2019. O principal objetivo desse lançamento é facilitar a migração de aplicativos existentes que usam o EF 6 para o .NET Core 3.0. A comunidade também contribuiu com diversos aprimoramentos e correções de bugs. Consulte os problemas fechados em cada [marco](https://github.com/aspnet/EntityFramework6/milestones?state=closed) do 6.3.0 para obter detalhes. Estes são alguns dos mais notáveis:

- Suporte para .NET Core 3.0
  - Agora, o pacote do EntityFramework é destinado ao .NET Standard 2.1 além do .NET Framework 4.x
  - Os comandos de migração foram re-escritos para execução fora do processo e para funcionarem com projetos de estilo SDK
- Suporte para HierarchyId do SQL Server
- Melhoria da compatibilidade com PackageReference do Roslyn e do NuGet
- Adição de ef6.exe para habilitar, adicionar, executar scripts e aplicar migrações de assemblies. Ele substitui migrate.exe

## <a name="past-releases"></a>Versões anteriores

A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.
