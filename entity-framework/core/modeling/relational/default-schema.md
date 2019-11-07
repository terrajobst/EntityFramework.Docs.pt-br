---
title: Esquema padrão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655974"
---
# <a name="default-schema"></a>Esquema padrão

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O esquema padrão é o esquema de banco de dados no qual os objetos serão criados se um esquema não estiver configurado explicitamente para esse objeto.

## <a name="conventions"></a>Convenções

Por convenção, o provedor de banco de dados escolherá o esquema padrão mais apropriado. Por exemplo, Microsoft SQL Server usará o esquema de `dbo` e o SQLite não usará um esquema (pois não há suporte para esquemas no SQLite).

## <a name="data-annotations"></a>Anotações de dados

Não é possível definir o esquema padrão usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar um esquema padrão.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
