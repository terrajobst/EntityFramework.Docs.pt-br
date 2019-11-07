---
title: Chaves primárias-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656045"
---
# <a name="primary-keys"></a>Chaves primárias

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma restrição PRIMARY KEY é introduzida para a chave de cada tipo de entidade.

## <a name="conventions"></a>Convenções

Por convenção, a chave primária no banco de dados será nomeada `PK_<type name>`.

## <a name="data-annotations"></a>Anotações de dados

Nenhum aspecto específico do banco de dados relacional de uma chave primária pode ser configurado usando as anotações.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome da restrição de chave primária no banco de dados.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
