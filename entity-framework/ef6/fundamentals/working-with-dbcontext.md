---
title: Trabalhando com DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416318"
---
# <a name="working-with-dbcontext"></a>Como trabalhar com DbContext

Para usar Entity Framework para consultar, inserir, atualizar e excluir dados usando objetos .NET, primeiro você precisa [criar um modelo](~/ef6/modeling/index.md) que mapeie as entidades e relações definidas em seu modelo para tabelas em um banco de dados.

Quando você tiver um modelo, a classe primária com a qual seu aplicativo interage é `System.Data.Entity.DbContext` (geralmente conhecida como a classe de contexto). Você pode usar um DbContext associado a um modelo para:
- Criar e executar consultas   
- Materializar resultados da consulta como objetos de entidade
- Controlar alterações feitas nesses objetos
- Persistir alterações no objeto de volta no banco de dados
- Associar objetos na memória a controles da interface do usuário

Esta página fornece algumas diretrizes sobre como gerenciar a classe de contexto.  

## <a name="defining-a-dbcontext-derived-class"></a>Definindo uma classe derivada DbContext  

A maneira recomendada para trabalhar com o contexto é definir uma classe derivada de DbContext e expõe as propriedades DbSet que representam as coleções das entidades especificadas no contexto. Se você estiver trabalhando com o designer do EF, o contexto será gerado para você. Se você estiver trabalhando com Code First, normalmente escreverá o contexto por conta própria.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Depois de ter um contexto, você consultaria, adicionaria (usando os métodos `Add` ou `Attach`) ou removerá as entidades (usando `Remove`) no contexto por meio dessas propriedades. O acesso a uma propriedade `DbSet` em um objeto de contexto representa uma consulta inicial que retorna todas as entidades do tipo especificado. Observe que apenas o acesso a uma propriedade não executará a consulta. Uma consulta é executada quando:  

- Ele é enumerado por uma instrução `foreach`C#() ou `For Each` (Visual Basic).  
- Ele é enumerado por uma operação de coleta, como `ToArray`, `ToDictionary`ou `ToList`.  
- Operadores LINQ, como `First` ou `Any`, são especificados na parte mais externa da consulta.  
- Um dos seguintes métodos é chamado de: o método de extensão `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`e `DbSet<T>.Find`, se uma entidade com a chave especificada não for encontrada já carregada no contexto.  

## <a name="lifetime"></a>Tempo de vida  

O tempo de vida do contexto começa quando a instância é criada e termina quando a instância é descartada ou coletada como lixo. Use **usando** se você quiser que todos os recursos que os controles de contexto sejam descartados no final do bloco. Quando **você usa o, o**compilador cria automaticamente um bloco try/finally e chama Dispose no bloco **finally** .  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Aqui estão algumas diretrizes gerais ao decidir sobre o tempo de vida do contexto:  

- Ao trabalhar com aplicativos Web, use uma instância de contexto por solicitação.  
- Ao trabalhar com Windows Presentation Foundation (WPF) ou Windows Forms, use uma instância de contexto por formulário. Isso permite que você use a funcionalidade de controle de alterações que o contexto fornece.  
- Se a instância de contexto for criada por um contêiner de injeção de dependência, geralmente é responsabilidade do contêiner descartar o contexto.
- Se o contexto for criado no código do aplicativo, lembre-se de descartar o contexto quando ele não for mais necessário.  
- Ao trabalhar com contexto de execução longa, considere o seguinte:  
    - Conforme você carrega mais objetos e suas referências na memória, o consumo de memória do contexto pode aumentar rapidamente. Isso pode causar problemas de desempenho.  
    - O contexto não é thread-safe, portanto, ele não deve ser compartilhado entre vários threads, fazendo o trabalho nele simultaneamente.
    - Se uma exceção fizer com que o contexto esteja em um estado irrecuperável, todo o aplicativo poderá ser encerrado.  
    - As chances de ocorrerem problemas relacionados à simultaneidade aumentam de acordo com o intervalo entre o tempo em que os dados são consultados e atualizados.  

## <a name="connections"></a>conexões  

Por padrão, o contexto gerencia as conexões com o banco de dados. O contexto é aberto e fecha as conexões conforme necessário. Por exemplo, o contexto abre uma conexão para executar uma consulta e fecha a conexão quando todos os conjuntos de resultados são processados.  

Há casos em que você deseja ter mais controle sobre quando a conexão abre e fecha. Por exemplo, ao trabalhar com SQL Server Compact, geralmente é recomendável manter uma conexão aberta separada com o banco de dados durante o tempo de vida do aplicativo para melhorar o desempenho. Você pode gerenciar esse processo manualmente usando a propriedade `Connection`.  
