---
title: Trabalhando com proxies-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419335"
---
# <a name="working-with-proxies"></a>Trabalhando com proxies
Ao criar instâncias de tipos de entidade POCO, Entity Framework muitas vezes cria instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Esse proxy substitui algumas propriedades virtuais da entidade para inserir ganchos para executar ações automaticamente quando a propriedade é acessada. Por exemplo, esse mecanismo é usado para dar suporte ao carregamento lento de relações. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="disabling-proxy-creation"></a>Desabilitando a criação de proxy  

Às vezes, é útil impedir que Entity Framework criem instâncias de proxy. Por exemplo, a serialização de instâncias não proxy é consideravelmente mais fácil do que a serialização de instâncias de proxy. A criação de proxy pode ser desativada desmarcando o sinalizador ProxyCreationEnabled. Um local que você pode fazer é no construtor do seu contexto. Por exemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Observe que o EF não criará proxies para tipos em que não há nada para o proxy fazer. Isso significa que você também pode evitar proxies tendo tipos lacrados e/ou não ter nenhuma propriedade virtual.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Criando explicitamente uma instância de um proxy  

Uma instância de proxy não será criada se você criar uma instância de uma entidade usando o novo operador. Isso pode não ser um problema, mas se você precisar criar uma instância de proxy (por exemplo, para que o carregamento lento ou o controle de alterações de proxy funcione), você pode fazer isso usando o método Create de DbSet. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

A versão genérica de Create pode ser usada se você quiser criar uma instância de um tipo de entidade derivada. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Observe que o método Create não adiciona ou anexa a entidade criada ao contexto.  

Observe que o método Create criará apenas uma instância do próprio tipo de entidade se a criação de um tipo de proxy para a entidade não tiver nenhum valor porque ela não faria nada. Por exemplo, se o tipo de entidade for sealed e/ou não tiver nenhuma propriedade virtual, Create criará apenas uma instância do tipo de entidade.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Obtendo o tipo de entidade real por meio de um tipo de proxy  

Os tipos de proxy têm nomes parecidos com este:  

System. Data. Entity. DynamicProxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Você pode encontrar o tipo de entidade para esse tipo de proxy usando o método GetObjectType do ObjectContext. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Observe que, se o tipo passado para GetObjectType for uma instância de um tipo de entidade que não seja um tipo de proxy, o tipo de entidade ainda será retornado. Isso significa que você sempre pode usar esse método para obter o tipo de entidade real sem nenhuma outra verificação para ver se o tipo é um tipo de proxy ou não.  
