---
title: Automático detectar alterações - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490981"
---
# <a name="automatic-detect-changes"></a>Automático detectar alterações
Ao usar a maioria das entidades POCO a determinação de como uma entidade foi alterada (e, portanto, quais atualizações precisam ser enviados para o banco de dados) é tratada pelo algoritmo detectar alterações. Detecte works alterações Detectando as diferenças entre os valores de propriedade atuais da entidade e os valores de propriedade originais que são armazenados em um instantâneo quando a entidade foi consultada ou anexada. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Por padrão, o Entity Framework executa detectar alterações automaticamente quando são chamados de métodos a seguir:  

- DbSet.Find  
- DbSet  
- DbSet  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Desabilitar a detecção automática de alterações  

Se você estiver rastreando um lote de entidades no seu contexto e chamar um desses métodos muitas vezes em um loop, você pode obter melhorias significativas de desempenho desativando a detecção de alterações para a duração do loop. Por exemplo:  

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

Não se esqueça de habilitar novamente a detecção de alterações após o loop, usamos um try/finally para garantir que ele está sempre habilitado novamente, mesmo se o código no loop lançará uma exceção.  

Uma alternativa para desabilitar e reabilitar é deixar a detecção automática de alterações desativado em todos os momentos e o contexto de chamada. ChangeTracker.DetectChanges explicitamente ou use proxies de controle de alterações cuidadosamente. Ambas as opções avançadas e pode facilmente introduzir bugs sutis em seu aplicativo então usá-las com cuidado.  

Se você precisar adicionar ou remover muitos objetos de um contexto, considere usar DbSet.AddRange e DbSet.RemoveRange. Esse método detecta automaticamente as alterações em apenas uma vez após a conclusão de operações de adicionar ou remover. 
