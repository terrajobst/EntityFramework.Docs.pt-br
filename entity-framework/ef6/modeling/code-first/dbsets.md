---
title: Definindo DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39120194"
---
# <a name="defining-dbsets"></a>Definindo DbSets
Ao desenvolver com o fluxo de trabalho do Code First você definir um DbContext derivado que representa a sessão com o banco de dados e expõe um DbSet para cada tipo em seu modelo. Este tópico aborda as várias maneiras que você pode definir as propriedades DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext com propriedades DbSet  

É o caso comum mostrado nos exemplos de código primeiro ter um DbContext com propriedades públicas de DbSet automática para os tipos de entidade do seu modelo. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Quando usado no modo de Code First, isso irá configurar Blogs e postagens como tipos de entidade, bem como configurar outros tipos pode ser acessados a partir desses. Além disso DbContext automaticamente chamará o setter para cada uma dessas propriedades para definir uma instância do DbSet apropriada.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext com propriedades IDbSet  

Há situações, como quando criar simulações ou falsificações, onde é mais útil declarar as propriedades de conjunto usando uma interface. Em tais casos o IDbSet interface pode ser usado no lugar do DbSet. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Nesse contexto funciona exatamente da mesma maneira como o contexto que usa a classe DbSet para suas propriedades de conjunto.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext com as propriedades definidas somente leitura  

Se você não quiser expor setters públicos para suas propriedades DbSet ou IDbSet em vez disso, você pode criar propriedades somente leitura e criar as instâncias do conjunto por conta própria. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Observe que o DbContext armazena em cache a instância do DbSet retornado do método de conjunto, de modo que cada uma dessas propriedades retornarão a mesma instância sempre que ele é chamado.  

Descoberta de tipos de entidade do Code First funciona da mesma maneira como faz em propriedades com públicos getters e setters.  
