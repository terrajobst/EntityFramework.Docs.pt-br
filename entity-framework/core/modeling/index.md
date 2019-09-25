---
title: Criar e configurar um modelo – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 5b886226b16b5b1a1f01e6040e58d92ae8678d29
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197311"
---
# <a name="creating-and-configuring-a-model"></a>Criar e configurar um modelo

O Entity Framework usa um conjunto de convenções para criar um modelo com base na forma de suas classes de entidade. Você pode especificar configurações adicionais para complementar e/ou substituir o que foi descoberto por convenção.

Este artigo aborda a configuração que pode ser aplicada a um modelo que tenha como destino qualquer armazenamento de dados e que possa ser aplicado ao ter qualquer banco de dados relacional como destino. Os provedores também podem habilitar configuração específica de um armazenamento de dados em particular. Para mais informações sobre a configuração específica do provedor, consulte a seção [Provedor de Banco de Dados](../providers/index.md) .

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) deste artigo no GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Usar a API fluente para configurar um modelo

Você pode modificar o método `OnModelCreating` no contexto derivado e usar o `ModelBuilder API` para configurar seu modelo. Este é o método mais eficiente de configuração e permite que a configuração seja especificada sem modificação de suas classes de entidade. A configuração da API fluente tem a precedência mais alta e substituirá as anotações de dados e as convenções.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Usar anotações de dados para configurar um modelo

Você também pode aplicar atributos (conhecidos como Anotações de Dados) a classes e propriedades. As anotações de dados substituirão as convenções, mas serão substituídas pela configuração da API Fluente.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
