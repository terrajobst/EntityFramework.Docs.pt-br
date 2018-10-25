---
title: Versões do Visual Studio - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022243"
---
# <a name="visual-studio-releases"></a>Versões do Visual Studio

É recomendável usar sempre a versão mais recente do Visual Studio porque ele contém as ferramentas mais recentes do .NET, o NuGet e o Entity Framework.
Na verdade, vários exemplos e explicações passo a passo na documentação do Entity Framework pressupõem que você está usando uma versão recente do Visual Studio.

É possível, no entanto, para usar versões mais antigas do Visual Studio com diferentes versões do Entity Framework desde que você levar em conta algumas diferenças:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 e mais recente

- Esta versão do Visual Studio inclui a versão mais recente das ferramentas de Entity Framework e o tempo de execução do EF 6.2 e não requer etapas adicionais de configuração.
Ver [What's New](~/ef6/what-is-new/index.md) para obter mais detalhes sobre essas versões.
- Adicionar o Entity Framework para novos projetos usando as ferramentas do EF adicionará automaticamente o pacote NuGet do EF 6.2.
Manualmente, você pode instalar ou atualizar para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância do SQL Server disponível com esta versão do Visual Studio é uma instância do LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de caracteres de conexão que você deve usar é "(localdb)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou clique duas vezes invertidas "\\\\" ao especificar uma cadeia de caracteres de conexão no código c#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 para Visual Studio 2017 15.6

- Essas versões do Visual Studio incluem ferramentas do Entity Framework e o tempo de execução 6.1.3.
Ver [últimas versões](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar o Entity Framework para novos projetos usando as ferramentas do EF adicionará automaticamente o EF 6.1.3 pacote do NuGet.
Manualmente, você pode instalar ou atualizar para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância do SQL Server disponível com esta versão do Visual Studio é uma instância do LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de caracteres de conexão que você deve usar é "(localdb)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou clique duas vezes invertidas "\\\\" ao especificar uma cadeia de caracteres de conexão no código c#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Esta versão do Visual Studio inclui e a versão mais antiga do tempo de execução e ferramentas do Entity Framework.
É recomendável que você atualize para o Entity Framework Tools 6.1.3, usando [o instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponíveis no Microsoft Download Center.
Ver [últimas versões](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar o Entity Framework para novos projetos usando as ferramentas do EF atualizadas adicionará automaticamente o EF 6.1.3 pacote do NuGet.
Manualmente, você pode instalar ou atualizar para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância do SQL Server disponível com esta versão do Visual Studio é uma instância do LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de caracteres de conexão que você deve usar é "(localdb)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou clique duas vezes invertidas "\\\\" ao especificar uma cadeia de caracteres de conexão no código c#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Esta versão do Visual Studio inclui e a versão mais antiga do tempo de execução e ferramentas do Entity Framework.
É recomendável que você atualize para o Entity Framework Tools 6.1.3, usando [o instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponíveis no Microsoft Download Center.
Ver [últimas versões](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar o Entity Framework para novos projetos usando as ferramentas do EF atualizadas adicionará automaticamente o EF 6.1.3 pacote do NuGet.
Manualmente, você pode instalar ou atualizar para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância do SQL Server disponível com esta versão do Visual Studio é uma instância do LocalDB chamada v11.0.
A seção de servidor da cadeia de caracteres de conexão que você deve usar é "(localdb)\\v11.0".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou clique duas vezes invertidas "\\\\" ao especificar uma cadeia de caracteres de conexão no código c#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- A versão do Entity Framework Tools disponível com esta versão do Visual Studio não é compatível com o tempo de execução do Entity Framework 6 e não pode ser atualizada.
- Por padrão, as ferramentas do Entity Framework adicionará o Entity Framework 4.0 para seus projetos.
Para criar aplicativos usando as versões mais recentes do EF, você primeiro precisará instalar o [extensão do Gerenciador de pacotes NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Por padrão, todos os de geração de código na versão do ferramentas do EF baseia-se em EntityObject e Entity Framework 4.
Recomendamos que você alterne a geração de código deve ser baseado em DbContext e o Entity Framework 5, instalando os modelos de geração de código do DbContext para [c#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) ou [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Depois de instalar as extensões do Gerenciador de pacotes NuGet, você pode instalar manualmente ou atualize para qualquer pacote NuGet do EF disponível online e usam o EF6 Code First, que não exige que um designer.
- Por padrão, a instância do SQL Server disponível com esta versão do Visual Studio é o SQL Server Express denominada SQLEXPRESS.
A seção de servidor da cadeia de caracteres de conexão que você deve usar é ". \\SQLEXPRESS ".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou clique duas vezes invertidas "\\\\" ao especificar uma cadeia de caracteres de conexão no código c#.
