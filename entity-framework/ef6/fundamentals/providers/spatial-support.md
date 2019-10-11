---
title: Suporte do provedor para tipos espaciais-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181592"
---
# <a name="provider-support-for-spatial-types"></a>Suporte do provedor para tipos espaciais
Entity Framework dá suporte ao trabalho com dados espaciais por meio das classes DbGeography ou DbGeometry. Essas classes dependem da funcionalidade específica do banco de dados oferecida pelo provedor de Entity Framework. Nem todos os provedores dão suporte a dados espaciais e aqueles que podem ter pré-requisitos adicionais, como a instalação de assemblies de tipo espacial. Mais informações sobre o suporte do provedor para tipos espaciais são fornecidas abaixo.  

Informações adicionais sobre como usar tipos espaciais em um aplicativo podem ser encontradas em dois passo a passos, um para Code First, o outro para Database First ou Model First:  

- [Tipos de dados espaciais no Code First](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Tipos de dados espaciais no designer do EF](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versões do EF que dão suporte a tipos espaciais  

O suporte para tipos espaciais foi introduzido em EF5. No entanto, em tipos espaciais EF5 só há suporte quando o aplicativo é direcionado e executado no .NET 4,5.  

A partir do EF6, os tipos espaciais têm suporte para aplicativos destinados ao .NET 4 e ao .NET 4,5.  

## <a name="ef-providers-that-support-spatial-types"></a>Provedores de EF que dão suporte a tipos espaciais  

### <a name="ef5"></a>EF5  

Os provedores de Entity Framework para EF5 que estamos cientes desse suporte são os tipos espaciais:  

- Provedor de Microsoft SQL Server  
    - Esse provedor é enviado como parte do EF5.  
    - Esse provedor depende de algumas bibliotecas de nível inferior adicionais que talvez precisem ser instaladas – consulte abaixo para obter detalhes.  
- [Devart dotConnect para Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Este é um provedor de terceiros da Devart.  

Se você souber de um provedor EF5 que dá suporte a tipos espaciais, entre em contato e teremos o prazer de adicioná-lo a essa lista.  

### <a name="ef6"></a>EF6  

Os provedores de Entity Framework para EF6 que estamos cientes desse suporte são os tipos espaciais:  

- Provedor de Microsoft SQL Server  
    - Esse provedor é enviado como parte do EF6.  
    - Esse provedor depende de algumas bibliotecas de nível inferior adicionais que talvez precisem ser instaladas – consulte abaixo para obter detalhes.  
- [Devart dotConnect para Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Este é um provedor de terceiros da Devart.  

Se você souber de um provedor EF6 que dá suporte a tipos espaciais, entre em contato e teremos o prazer de adicioná-lo a essa lista.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Pré-requisitos para tipos espaciais com Microsoft SQL Server  

SQL Server suporte espacial depende dos tipos de baixo nível SQL Server específicos, geography e SqlGeometry. Esses tipos residem no assembly Microsoft. SqlServer. Types. dll, e esse assembly não é enviado como parte do EF ou como parte do .NET Framework.  

Quando o Visual Studio é instalado, ele geralmente também instala uma versão do SQL Server, e isso incluirá a instalação do Microsoft. SqlServer. Types. dll.  

Se o SQL Server não estiver instalado no computador em que você deseja usar tipos espaciais, ou se os tipos espaciais tiverem sido excluídos da instalação do SQL Server, você precisará instalá-los manualmente. Os tipos podem ser instalados usando `SQLSysClrTypes.msi`, que é parte do Microsoft SQL Server Feature Pack. Os tipos espaciais são SQL Server específicos à versão, portanto, é recomendável [Pesquisar "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) no centro de download da Microsoft e, em seguida, selecionar e baixar a opção que corresponde à versão do SQL Server que você usará.
