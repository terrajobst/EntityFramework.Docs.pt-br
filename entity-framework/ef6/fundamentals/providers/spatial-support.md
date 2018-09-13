---
title: Suporte do provedor para tipos espaciais – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489746"
---
# <a name="provider-support-for-spatial-types"></a>Suporte do provedor para tipos espaciais
Entity Framework dá suporte ao trabalho com dados espaciais pelas classes DbGeography ou DbGeometry. Essas classes dependem da funcionalidade de banco de dados específicos oferecida pelo provedor de Entity Framework. Nem todos os provedores dão suporte a dados espaciais e os que podem ter os pré-requisitos adicionais, como a instalação de assemblies de tipo espacial. Para obter mais informações sobre o suporte do provedor para tipos espaciais são fornecidas abaixo.  

Informações adicionais sobre como usar tipos espaciais em um aplicativo podem ser encontradas em duas instruções passo a passo, um para o Code First, outro para Model First ou Database First:  

- [Tipos de dados espaciais no código pela primeira vez](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Tipos de dados espaciais no EF Designer](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versões EF que dão suporte a tipos espaciais  

Suporte para tipos espaciais foi introduzido no EF5. No entanto, no EF5 tipos espaciais só têm suporte quando o aplicativo tem como alvo e é executado no .NET 4.5.  

Começando com o EF6 tipos espaciais têm suporte para aplicativos destinados ao .NET 4 e .NET 4.5.  

## <a name="ef-providers-that-support-spatial-types"></a>Provedores do EF que dão suporte a tipos espaciais  

### <a name="ef5"></a>EF5  

Os provedores de Entity Framework para EF5 que estamos cientes de que deem suporte a tipos espaciais são:  

- Provedor do Microsoft SQL Server  
    - Esse provedor é fornecido como parte do EF5.  
    - Esse provedor depende de algumas bibliotecas de nível baixo adicionais que talvez precise ser instalado — consulte abaixo para obter detalhes.  
- [Devart dotConnect para Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Esse é um provedor de terceiros do Devart.  

Se você souber de um provedor do EF5 que dá suporte a tipos espaciais, em seguida, por favor, entrar em contato e ficaremos felizes de adicioná-lo a essa lista.  

### <a name="ef6"></a>EF6  

Os provedores de Entity Framework para o EF6 que estamos cientes de que deem suporte a tipos espaciais são:  

- Provedor do Microsoft SQL Server  
    - Esse provedor é fornecido como parte do EF6.  
    - Esse provedor depende de algumas bibliotecas de nível baixo adicionais que talvez precise ser instalado — consulte abaixo para obter detalhes.  
- [Devart dotConnect para Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Esse é um provedor de terceiros do Devart.  

Se você souber de um provedor do EF6 que dá suporte a tipos espaciais, em seguida, por favor, entrar em contato e ficaremos felizes de adicioná-lo a essa lista.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Pré-requisitos para tipos espaciais com o Microsoft SQL Server  

Suporte espacial do SQL Server depende de baixo nível, tipos específicos do SQL Server SqlGeography e SqlGeometry. Esses tipos de ativos no assembly Types e esse assembly não é enviado como parte do EF ou como parte do .NET Framework.  

Quando o Visual Studio está instalado ele geralmente também instalará uma versão do SQL Server, e isso inclui a instalação do Types.  

Se o SQL Server não está instalado no computador em que você deseja usar tipos espaciais, ou se os tipos espaciais foram excluídos da instalação do SQL Server, você precisará instalá-los manualmente. Os tipos podem ser instalados usando `SQLSysClrTypes.msi`, que faz parte do Feature Pack do Microsoft SQL Server. Tipos espaciais são específicos da versão, do SQL Server, portanto, recomendamos [pesquisar "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) no Microsoft Download Center, em seguida, selecione e baixe a opção que corresponde à versão do SQL Server, você usará.
