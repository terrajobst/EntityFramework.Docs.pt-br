---
title: Propriedades da sombra – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781177"
---
# <a name="shadow-properties"></a>Propriedades de sombra

Propriedades de sombra são propriedades que não são definidas em sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo de EF Core. O valor e o estado dessas propriedades são mantidos puramente no controlador de alterações. As propriedades de sombra são úteis quando há dados no banco que não devem ser expostos nos tipos de entidade mapeada.

## <a name="foreign-key-shadow-properties"></a>Propriedades da sombra da chave estrangeira

As propriedades de sombra são usadas com mais frequência para propriedades de chave estrangeira, em que a relação entre duas entidades é representada por um valor de chave estrangeira no banco de dados, mas a relação é gerenciada nos tipos de entidade usando propriedades de navegação entre a entidade digita. Por convenção, o EF introduzirá uma propriedade de sombra quando uma relação for descoberta, mas nenhuma propriedade de chave estrangeira for encontrada na classe de entidade dependente.

A propriedade será nomeada `<navigation property name><principal key property name>` (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomenclatura). Se o nome da propriedade da chave principal incluir o nome da propriedade de navegação, o nome será apenas `<principal key property name>`. Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança será usado em seu lugar.

Por exemplo, a listagem de código a seguir resultará em uma propriedade de sombra `BlogId` introduzida na entidade `Post`:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Configurando propriedades de sombra

Você pode usar a API Fluent para configurar propriedades de sombra. Depois de ter chamado a sobrecarga de cadeia de caracteres de `Property`, você pode encadear qualquer uma das chamadas de configuração para outras propriedades. No exemplo a seguir, como `Blog` não tem nenhuma propriedade CLR chamada `LastUpdated`, uma propriedade Shadow é criada:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

Se o nome fornecido ao método de `Property` corresponder ao nome de uma propriedade existente (uma propriedade de sombra ou uma definida na classe de entidade), o código configurará essa propriedade existente em vez de introduzir uma nova propriedade de sombra.

## <a name="accessing-shadow-properties"></a>Acessando Propriedades de sombra

Os valores de propriedade de sombra podem ser obtidos e alterados por meio da API `ChangeTracker`:

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

As propriedades de sombra podem ser referenciadas em consultas LINQ por meio do método estático `EF.Property`:

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
