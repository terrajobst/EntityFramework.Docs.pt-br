---
title: Definindo DbSets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419090"
---
# <a name="defining-dbsets"></a>Definindo DbSets
Ao desenvolver com o fluxo de trabalho de Code First, você define um DbContext derivado que representa a sessão com o banco de dados e expõe um DbSet para cada tipo em seu modelo. Este tópico aborda as várias maneiras pelas quais você pode definir as propriedades DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext com propriedades DbSet  

O caso comum mostrado em Code First exemplos é ter um DbContext com propriedades públicas automáticas de DbSet para os tipos de entidade do seu modelo. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Quando usado no modo de Code First, isso irá configurar Blogs e postagens como tipos de entidade, bem como configurar outros tipos acessíveis a partir deles. Além disso, o DbContext chamará automaticamente o setter para cada uma dessas propriedades para definir uma instância do DbSet apropriado.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext com propriedades IDbSet  

Há situações, como ao criar simulações ou falsificações, em que é mais útil declarar as propriedades definidas usando uma interface. Nesses casos, a interface IDbSet pode ser usada no lugar de DbSet. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Esse contexto funciona exatamente da mesma forma que o contexto que usa a classe DbSet para suas propriedades set.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext com propriedades de conjunto somente leitura  

Se você não deseja expor os setters públicos para suas propriedades DbSet ou IDbSet, você pode criar propriedades somente leitura e criar as instâncias definidas por conta própria. Por exemplo:  

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

Observe que DbContext armazena em cache a instância de DbSet retornada do método set para que cada uma dessas propriedades retorne a mesma instância toda vez que for chamada.  

A descoberta de tipos de entidade para Code First funciona da mesma maneira aqui para propriedades com getters e setters públicos.  
