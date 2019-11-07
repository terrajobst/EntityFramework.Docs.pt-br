---
title: Propriedades da sombra – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655959"
---
# <a name="shadow-properties"></a>Propriedades de sombra

Propriedades de sombra são propriedades que não são definidas em sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo de EF Core. O valor e o estado dessas propriedades são mantidos puramente no controlador de alterações.

As propriedades de sombra são úteis quando há dados no banco que não devem ser expostos nos tipos de entidade mapeada. Eles são usados com mais frequência em Propriedades de chave estrangeira, em que a relação entre duas entidades é representada por um valor de chave estrangeira no banco de dados, mas a relação é gerenciada nos tipos de entidade usando propriedades de navegação entre os tipos de entidade.

Os valores de propriedade de sombra podem ser obtidos e alterados por meio da API `ChangeTracker`.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

As propriedades de sombra podem ser referenciadas em consultas LINQ por meio do método estático `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenções

As propriedades de sombra podem ser criadas por convenção quando uma relação é descoberta, mas nenhuma propriedade de chave estrangeira é encontrada na classe de entidade dependente. Nesse caso, uma propriedade de chave estrangeira de sombra será introduzida. A propriedade de chave estrangeira de sombra será nomeada `<navigation property name><principal key property name>` (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomenclatura). Se o nome da propriedade da chave principal incluir o nome da propriedade de navegação, o nome será apenas `<principal key property name>`. Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança será usado em seu lugar.

Por exemplo, a listagem de código a seguir resultará em uma propriedade de sombra `BlogId` introduzida na entidade `Post`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Anotações de dados

Não é possível criar propriedades de sombra com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar propriedades de sombra. Depois de ter chamado a sobrecarga de cadeia de caracteres de `Property` você pode encadear qualquer uma das chamadas de configuração para outras propriedades.

Se o nome fornecido ao método de `Property` corresponder ao nome de uma propriedade existente (uma propriedade de sombra ou uma definida na classe de entidade), o código configurará essa propriedade existente em vez de introduzir uma nova propriedade de sombra.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
