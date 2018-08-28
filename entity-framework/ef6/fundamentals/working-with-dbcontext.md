---
title: Trabalhando com DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998140"
---
# <a name="working-with-dbcontext"></a>Trabalhando com DbContext

Para usar o Entity Framework para consultar, inserir, atualizar e excluir dados usando objetos .NET, você primeiro precisa [criar um modelo](~/ef6/modeling/index.md) que mapeia as entidades e relações que são definidas em seu modelo para tabelas em um banco de dados.

Depois de ter um modelo, a classe principal de seu aplicativo interage com é `System.Data.Entity.DbContext` (também conhecido como a classe de contexto). Você pode usar um DbContext associado a um modelo para:
- Criar e executar consultas   
- Materializar resultados da consulta como objetos de entidade
- Acompanhar as alterações feitas a esses objetos
- Manter alterações de objeto volta no banco de dados
- Associar objetos na memória a controles de interface do usuário

Esta página fornece algumas diretrizes sobre como gerenciar a classe de contexto.  

## <a name="defining-a-dbcontext-derived-class"></a>Definindo uma classe derivada de DbContext  

A maneira recomendada para trabalhar com o contexto é definir uma classe que deriva de DbContext e expõe as propriedades DbSet que representam coleções de entidades especificadas no contexto. Se você estiver trabalhando com o EF Designer, o contexto será gerado para você. Se você estiver trabalhando com o Code First, você normalmente escreverá o contexto por conta própria.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Quando você tiver um contexto, você teria consultar, adicionar (usando `Add` ou `Attach` métodos) ou remover (usando `Remove`) entidades no contexto por meio dessas propriedades. Acessando um `DbSet` propriedade em um objeto de contexto representa uma consulta inicial que retorna todas as entidades do tipo especificado. Observe que apenas acesso a uma propriedade não será executado a consulta. Uma consulta é executada quando:  

- Ele é enumerado por um `foreach` (c#) ou `For Each` declaração (Visual Basic).  
- Ele é enumerado por uma operação de coleção, como `ToArray`, `ToDictionary`, ou `ToList`.  
- Operadores LINQ, como `First` ou `Any` são especificados na parte externa da consulta.  
- Um dos seguintes métodos são chamados: o `Load` método de extensão `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, e `DbSet<T>.Find`, se uma entidade com a chave especificada não for encontrada já carregada no contexto.  

## <a name="lifetime"></a>Tempo de vida  

O tempo de vida do contexto começa quando a instância é criada e termina quando a instância é descartada ou coleta de lixo. Use **usando** se quiser que todos os recursos que o contexto controla sejam descartados no final do bloco. Quando você usa **usando**, o compilador cria automaticamente um bloco try/finally e chamada o descarte na **finalmente** bloco.  

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
- Ao trabalhar com o Windows Presentation Foundation (WPF) ou o Windows Forms, use uma instância de contexto por formulário. Isso permite que você usar a funcionalidade de controle de alterações fornece esse contexto.  
- Se a instância de contexto é criada por um contêiner de injeção de dependência, geralmente, é responsabilidade do contêiner para descartar o contexto.
- Se o contexto é criado no código do aplicativo, lembre-se descartar o contexto quando ele não é mais necessário.  
- Ao trabalhar com o contexto de execução longa, considere o seguinte:  
    - Conforme você carrega mais objetos e suas referências na memória, o consumo de memória do contexto pode aumentar rapidamente. Isso pode causar problemas de desempenho.  
    - O contexto não é thread-safe, portanto ele não deve ser compartilhado entre vários threads trabalhando ao mesmo tempo.
    - Se uma exceção faz com que o contexto de estar em um estado irrecuperável, todo o aplicativo pode encerrar.  
    - As chances de ocorrerem problemas relacionados à simultaneidade aumentam de acordo com o intervalo entre o tempo em que os dados são consultados e atualizados.  

## <a name="connections"></a>Conexões  

Por padrão, o contexto gerencia conexões com o banco de dados. O contexto abre e fecha conexões conforme o necessário. Por exemplo, o contexto abre uma conexão para executar uma consulta e, em seguida, fecha a conexão quando todos os conjuntos de resultados tiverem sido processados.  

Há casos em que você deseja ter mais controle sobre quando a conexão abre e fecha. Por exemplo, ao trabalhar com o SQL Server Compact, muitas vezes convém manter uma conexão aberta separada no banco de dados para o tempo de vida do aplicativo para melhorar o desempenho. Você pode gerenciar esse processo manualmente usando a propriedade `Connection`.  
