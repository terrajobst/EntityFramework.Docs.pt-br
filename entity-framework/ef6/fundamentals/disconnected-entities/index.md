---
title: Trabalhando com entidades desconectadas – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413361"
---
# <a name="working-with-disconnected-entities"></a>Trabalhando com entidades desconectadas
Em um aplicativo baseado no Entity Framework, uma classe de contexto é responsável por detectar alterações aplicadas a entidades controladas. Chamar o método SaveChanges mantém as alterações controladas pelo contexto para o banco de dados. Ao trabalhar com aplicativos de N camadas, os objetos de entidade normalmente são modificados enquanto estão desconectados do contexto, e você deve decidir como rastrear as alterações e relatar essas alterações de volta para o contexto. Este tópico discute as diferentes opções que estão disponíveis ao usar o Entity Framework com entidades desconectadas.   

## <a name="web-service-frameworks"></a>Estruturas de serviço Web

As tecnologias de serviços Web geralmente são compatíveis com padrões que podem ser usados para manter as alterações feitas em objetos desconectados individuais. Por exemplo, o ASP.NET Web API permite que você codifique ações do controlador que podem incluir chamadas para o EF para manter as alterações feitas em um objeto em um banco de dados. Na verdade, as ferramentas da API Web no Visual Studio facilitam a realização do scaffolding de um controlador da API Web do seu modelo do Entity Framework 6. Para obter mais informações, consulte [usando a API Web com o Entity Framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

Historicamente, houve várias outras tecnologias de serviços Web que ofereciam a integração com o Entity Framework, como o [WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) e o [RIA Services](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>APIs do EF de baixo nível

Se você não quiser usar uma solução de N camadas existente ou se desejar personalizar o que acontece dentro de uma ação do controlador em serviços de API Web, o Entity Framework fornece APIs que permitem aplicar as alterações feitas em uma camada desconectada. Para obter mais informações, consulte [Adicionar, Anexar e estado da entidade](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Entidades de rastreamento automático  

O controle de alterações em grafos arbitrários de entidades enquanto estão desconectadas do contexto do EF é um problema difícil. Uma das tentativas de resolvê-lo foi o modelo de geração de código de Entidades de Rastreamento Automático. Esse modelo gera classes de entidade que contêm a lógica para rastrear as alterações feitas em uma camada desconectada, como estado nas entidades em si. Um conjunto de métodos de extensão também é gerado para aplicar essas alterações para um contexto.

Esse modelo pode ser usado com modelos criados usando o EF Designer, mas não pode ser usado com modelos do Code First. Para obter mais informações, consulte [Entidades de rastreamento automático](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Não recomendamos usar o modelo de entidades de rastreamento automático. Ele continuará disponível apenas para dar suporte aos aplicativos existentes. Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](https://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.
