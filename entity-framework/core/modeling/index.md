---
title: Criar e configurar um modelo – EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412771"
---
# <a name="creating-and-configuring-a-model"></a>Criar e configurar um modelo

O Entity Framework usa um conjunto de convenções para criar um modelo com base na forma de suas classes de entidade. Você pode especificar configurações adicionais para complementar e/ou substituir o que foi descoberto por convenção.

Este artigo aborda a configuração que pode ser aplicada a um modelo que tenha como destino qualquer armazenamento de dados e que possa ser aplicado ao ter qualquer banco de dados relacional como destino. Os provedores também podem habilitar configuração específica de um armazenamento de dados em particular. Para mais informações sobre a configuração específica do provedor, consulte a seção [Provedor de Banco de Dados](../providers/index.md) .

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) deste artigo no GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Usar a API fluente para configurar um modelo

Você pode modificar o método `OnModelCreating` no contexto derivado e usar o `ModelBuilder API` para configurar seu modelo. Este é o método mais eficiente de configuração e permite que a configuração seja especificada sem modificação de suas classes de entidade. A configuração da API fluente tem a precedência mais alta e substituirá as anotações de dados e as convenções.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Usar anotações de dados para configurar um modelo

Você também pode aplicar atributos (conhecidos como Anotações de Dados) a classes e propriedades. As anotações de dados substituirão as convenções, mas serão substituídas pela configuração da API Fluente.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
