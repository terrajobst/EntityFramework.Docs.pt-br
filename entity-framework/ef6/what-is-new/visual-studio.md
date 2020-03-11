---
title: Versões do Visual Studio-EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416936"
---
# <a name="visual-studio-releases"></a>Lançamentos do Visual Studio

Recomendamos sempre usar a versão mais recente do Visual Studio porque ela contém as ferramentas mais recentes para .NET, NuGet e Entity Framework.
Na verdade, os vários exemplos e instruções passo a passos na documentação Entity Framework pressupõem que você esteja usando uma versão recente do Visual Studio.

No entanto, é possível usar versões mais antigas do Visual Studio com diferentes versões do Entity Framework contanto que você considere algumas diferenças:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 e mais recente

- Esta versão do Visual Studio inclui a versão mais recente do Entity Framework Tools e o tempo de execução do EF 6,2 e não requer etapas de configuração adicionais.
Veja [as novidades para](~/ef6/what-is-new/index.md) obter mais detalhes sobre essas versões.
- Adicionar Entity Framework a novos projetos usando as ferramentas do EF adicionará automaticamente o pacote NuGet 6,2 do EF.
Você pode instalar ou atualizar manualmente para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância de SQL Server disponível com esta versão do Visual Studio é uma instância de LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de conexão que você deve usar é "(LocalDB)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou barras invertidas duplas "\\\\" C# ao especificar uma cadeia de conexão no código.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 para Visual Studio 2017 15,6

- Essas versões do Visual Studio incluem Entity Framework ferramentas e tempo de execução 6.1.3.
Consulte [versões anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar Entity Framework a novos projetos usando as ferramentas do EF adicionará automaticamente o pacote NuGet do EF 6.1.3.
Você pode instalar ou atualizar manualmente para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância de SQL Server disponível com esta versão do Visual Studio é uma instância de LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de conexão que você deve usar é "(LocalDB)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou barras invertidas duplas "\\\\" C# ao especificar uma cadeia de conexão no código.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Esta versão do Visual Studio inclui e versão mais antiga do Entity Framework ferramentas e tempo de execução.
É recomendável que você atualize para o Entity Framework Tools 6.1.3, usando [o instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponível no centro de download da Microsoft.
Consulte [versões anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar Entity Framework a novos projetos usando as ferramentas do EF atualizadas adicionará automaticamente o pacote NuGet do EF 6.1.3.
Você pode instalar ou atualizar manualmente para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância de SQL Server disponível com esta versão do Visual Studio é uma instância de LocalDB chamada MSSQLLocalDB.
A seção de servidor da cadeia de conexão que você deve usar é "(LocalDB)\\MSSQLLocalDB".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou barras invertidas duplas "\\\\" C# ao especificar uma cadeia de conexão no código.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Esta versão do Visual Studio inclui e versão mais antiga do Entity Framework ferramentas e tempo de execução.
É recomendável que você atualize para o Entity Framework Tools 6.1.3, usando [o instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponível no centro de download da Microsoft.
Consulte [versões anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obter mais detalhes sobre essas versões.
- Adicionar Entity Framework a novos projetos usando as ferramentas do EF atualizadas adicionará automaticamente o pacote NuGet do EF 6.1.3.
Você pode instalar ou atualizar manualmente para qualquer pacote NuGet do EF disponível online.
- Por padrão, a instância de SQL Server disponível com esta versão do Visual Studio é uma instância de LocalDB chamada v 11.0.
A seção de servidor da cadeia de conexão que você deve usar é "(LocalDB)\\v 11.0".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou barras invertidas duplas "\\\\" C# ao especificar uma cadeia de conexão no código.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- A versão do Entity Framework Tools disponível com esta versão do Visual Studio não é compatível com o tempo de execução do Entity Framework 6 e não pode ser atualizada.
- Por padrão, as ferramentas de Entity Framework adicionarão Entity Framework 4,0 a seus projetos.
Para criar aplicativos usando qualquer versão mais recente do EF, primeiro será necessário instalar a [extensão do Gerenciador de pacotes NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Por padrão, toda geração de código na versão das ferramentas do EF é baseada em EntityObject e Entity Framework 4.
Recomendamos que você alterne a geração de código para basear-se em DbContext e Entity Framework 5, instalando os modelos de [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) geração de código DbContext para ou [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Depois de instalar as extensões do Gerenciador de pacotes NuGet, você pode instalar ou atualizar manualmente para qualquer pacote NuGet do EF disponível online e usar o EF6 com Code First, que não requer um designer.
- Por padrão, a instância de SQL Server disponível com esta versão do Visual Studio é SQL Server Express chamada SQLExpress.
A seção de servidor da cadeia de conexão que você deve usar é ".\\SQLExpress ".
Lembre-se de usar uma cadeia de caracteres textual prefixada com `@` ou barras invertidas duplas "\\\\" C# ao especificar uma cadeia de conexão no código.
