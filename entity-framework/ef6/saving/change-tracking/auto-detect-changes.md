---
title: Detecção automática de alterações – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416973"
---
# <a name="automatic-detect-changes"></a>Detectar alterações automaticamente
Ao usar a maioria das entidades POCO, a determinação de como uma entidade foi alterada (e, portanto, quais atualizações precisam ser enviadas ao banco de dados) é tratada pelo algoritmo detectar alterações. Detectar alterações funciona detectando as diferenças entre os valores de propriedade atuais da entidade e os valores de propriedade originais que são armazenados em um instantâneo quando a entidade foi consultada ou anexada. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Por padrão, o Entity Framework executa a detecção de alterações automaticamente quando os seguintes métodos são chamados:  

- DbSet.Find  
- DbSet. local  
- DbSet. Add  
- DbSet. AddRange
- DbSet. Remove  
- DbSet. RemoveRange
- DbSet. Attach  
- DbContext.SaveChanges  
- DbContext. getvalidationerrors  
- DbContext.Entry  
- DbChangeTracker. Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Desabilitando a detecção automática de alterações  

Se você estiver acompanhando muitas entidades em seu contexto e chamar um desses métodos muitas vezes em um loop, poderá obter melhorias significativas de desempenho, desativando a detecção de alterações durante o loop. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Não se esqueça de reabilitar a detecção de alterações após o loop — usamos uma tentativa/finally para garantir que ela seja sempre habilitada novamente, mesmo que o código no loop gere uma exceção.  

Uma alternativa para desabilitar e reabilitar é deixar a detecção automática de alterações desativada em todos os momentos e em qualquer contexto de chamada. ChangeTracker. DetectChanges explicitamente ou use os proxies de controle de alterações de maneira atenta. Essas duas opções são avançadas e podem facilmente apresentar Bugs sutis em seu aplicativo, portanto, use-os com cuidado.  

Se você precisar adicionar ou remover muitos objetos de um contexto, considere usar DbSet. AddRange e DbSet. RemoveRange. Esses métodos detectam automaticamente as alterações apenas uma vez depois que as operações adicionar ou remover são concluídas. 
