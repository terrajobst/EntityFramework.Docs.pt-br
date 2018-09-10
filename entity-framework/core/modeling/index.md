---
title: Criar um Modelo – Core EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e4eed480178ce43cbc5ece8db8e584032da7b2b9
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250342"
---
# <a name="creating-and-configuring-a-model"></a>Criar e configurar um modelo

O Entity Framework usa um conjunto de convenções para criar um modelo com base na forma de suas classes de entidade. Você pode especificar configurações adicionais para complementar e/ou substituir o que foi descoberto por convenção.

Este artigo aborda a configuração que pode ser aplicada a um modelo que tenha como destino qualquer armazenamento de dados e que possa ser aplicado ao ter qualquer banco de dados relacional como destino. Os provedores também podem habilitar configuração específica de um armazenamento de dados em particular. Para obter documentação sobre configuração específica do provedor, consulte a seção [Provedores de Banco de Dados](../providers/index.md).

> [!TIP]  
> Você pode exibir o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) deste artigo no GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Usar a API fluente para configurar um modelo

Você pode substituir o método `OnModelCreating` no contexto derivado e usar o `ModelBuilder API` para configurar seu modelo. Este é o método mais eficiente de configuração e permite que a configuração seja especificada sem modificação de suas classes de entidade. A configuração da API fluente tem a precedência mais alta e substituirá as anotações de dados e as convenções.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

## <a name="use-data-annotations-to-configure-a-model"></a>Usar anotações de dados para configurar um modelo

Você também pode aplicar atributos (conhecidos como Anotações de Dados) a classes e propriedades. As anotações de dados substituirão as convenções, mas serão substituídas pela configuração da API Fluente.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
