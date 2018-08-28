---
title: Trabalhar com proxies - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997197"
---
# <a name="working-with-proxies"></a>Trabalhar com proxies
Ao criar instâncias de tipos de entidade POCO, o Entity Framework geralmente cria instâncias de um tipo derivado gerado dinamicamente que atua como um proxy para a entidade. Esse proxy substitui alguns virtuais propriedades da entidade para inserir os ganchos para executar as ações automaticamente quando a propriedade é acessada. Por exemplo, esse mecanismo é usado para dar suporte a carregamento lento de relações. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="disabling-proxy-creation"></a>Desabilitando a criação de proxy  

Às vezes é útil para evitar que o Entity Framework desde a criação de instâncias de proxy. Por exemplo, serializar instâncias de proxy não é consideravelmente mais fácil do que serializar instâncias do proxy. Criação de proxy pode ser desativada desmarcando o sinalizador ProxyCreationEnabled. Um único lugar, você pode fazer isso é no construtor de seu contexto. Por exemplo:  

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

Observe que o EF não criará proxies para os tipos em que não há nada para fazer o proxy. Isso significa que você também pode evitar proxies tendo tipos que são lacrados e/ou não têm nenhuma propriedade virtual.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Criar explicitamente uma instância de um proxy  

Uma instância do proxy não será criada se você criar uma instância de uma entidade usando o operador new. Isso pode não ser um problema, mas se você precisar criar uma instância do proxy (por exemplo, de modo que funcionarão lento carregamento ou proxy de controle de alterações), em seguida, você pode fazer isso usando o método Create da DbSet. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

A versão genérica de criação pode ser usada se você quiser criar uma instância de um tipo de entidade derivada. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Observe que o método Create não adicionar ou anexar a entidade criada ao contexto.  

Observe que o método Create apenas criar uma instância do tipo de entidade própria se a criação de um tipo de proxy para a entidade não teria nenhum valor porque ele não faria nada. Por exemplo, se o tipo de entidade é selado e/ou não tem nenhuma propriedade virtual, em seguida, criar apenas criará uma instância do tipo de entidade.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Obtendo o tipo de entidade real de um tipo de proxy  

Tipos de proxy têm nomes que algo parecido com isto:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Você pode encontrar o tipo de entidade para este tipo de proxy usando o método GetObjectType de ObjectContext. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Observe que, se o tipo passado para GetObjectType é uma instância de um tipo de entidade que não é um tipo de proxy, em seguida, o tipo de entidade ainda é retornada. Isso significa que você sempre pode usar esse método para obter o tipo de entidade real sem qualquer outra verificação para ver se o tipo for um tipo de proxy ou não.  
