---
title: Definindo DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993851"
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
