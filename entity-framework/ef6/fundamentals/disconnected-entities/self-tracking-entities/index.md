---
title: Entidades de rastreamento automático – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181722"
---
# <a name="self-tracking-entities"></a>Entidades de rastreamento automático

> [!IMPORTANT]
> Não recomendamos usar o modelo de entidades de rastreamento automático. Ele continuará disponível apenas para dar suporte aos aplicativos existentes. Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](https://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.

Em um aplicativo baseado no Entity Framework, um contexto é responsável por controlar as alterações nos objetos. Você, em seguida, usa o método SaveChanges para manter as alterações no banco de dados. Ao trabalhar com aplicativos de N camadas, os objetos de entidade normalmente são desconectados do contexto, e você deve decidir como rastrear as alterações e relatar essas alterações de volta para o contexto. As STEs (Entidades de Rastreamento Automático) podem ajudá-lo a rastrear as alterações em qualquer camada e repetir essas alterações em um contexto para serem salvas.  

Use as STEs somente se o contexto não estiver disponível em uma camada em que as alterações para o grafo do objeto são feitas. Se o contexto estiver disponível, não será necessário usar as STEs porque o contexto se encarregará de controlar as alterações.  

Este item de modelo gera dois arquivos .tt (modelo de texto):  

- O arquivo **\<nome do modelo\>.tt** gera os tipos de entidade e uma classe auxiliar que contém a lógica do controle de alterações que é usada por entidades e os métodos de extensão que permitem definir o estado de autocontrole em entidades de rastreamento automático.  
- O arquivo **\<nome do modelo\>.Context.tt** gera um contexto derivado e uma classe de extensão que contém métodos **ApplyChanges** para as classes **ObjectContext** e **ObjectSet**. Esses métodos examinam as informações de controle de alterações que estão contidas no grafo das entidades de rastreamento automático para inferir o conjunto de operações que devem ser executadas para salvar as alterações no banco de dados.  

## <a name="get-started"></a>Introdução  

Para começar, visite a página [Passo a passo de Entidades de Rastreamento Automático](walkthrough.md).  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Considerações funcionais ao trabalhar com entidades de rastreamento automático  
> [!IMPORTANT]
> Não recomendamos usar o modelo de entidades de rastreamento automático. Ele continuará disponível apenas para dar suporte aos aplicativos existentes. Se o seu aplicativo exigir o trabalho com grafos de entidades desconectados, considere outras alternativas como [Entidades Rastreáveis](https://trackableentities.github.io/), que são uma tecnologia semelhante às Entidades de Rastreamento Automático desenvolvidas mais ativamente pela comunidade, ou escreva um código personalizado usando as APIs de controle de alterações de baixo nível.

Considere o seguinte ao trabalhar com entidades de rastreamento automático:  

- Certifique-se de que seu projeto de cliente tenha uma referência ao assembly que contém os tipos de entidade. Se você adicionar somente a referência de serviço ao projeto do cliente, ele usará os tipos de proxy do WCF e não os tipos de entidade de rastreamento automático reais. Isso significa que você não terá os recursos de notificação automatizada que gerenciam o rastreamento das entidades no cliente. Se intencionalmente você não desejar incluir os tipos de entidade, será necessário definir manualmente as informações de controle de alterações no cliente para que as alterações sejam enviadas de volta para o serviço.  
- Chamadas para a operação de serviço devem ser sem estado e criar uma nova instância do contexto de objeto. Também recomendamos que você crie o contexto de objeto em um bloco **using**.  
- Quando você envia o grafo que foi modificado no cliente para o serviço e, em seguida, pretende continuar trabalhando com o mesmo grafo no cliente, precisa iterar manualmente o grafo e chamar o método **AcceptChanges** em cada objeto para redefinir o rastreador de alterações.  

    > Se os objetos no seu grafo contiverem propriedades com valores gerados pelo banco de dados (por exemplo, valores de simultaneidade ou identidade), o Entity Framework substituirá os valores dessas propriedades pelos valores gerados pelo banco de dados após o método **SaveChanges** ser chamado. Você pode implementar sua operação de serviço para retornar objetos salvos ou uma lista de valores de propriedade gerada para os objetos de volta ao cliente. O cliente precisará substituir as instâncias de objeto ou os valores de propriedade do objeto pelos objetos ou valores de propriedade retornados da operação de serviço.  
- Mesclar grafos de várias solicitações de serviço pode introduzir objetos com valores de chave duplicados no grafo resultante. O Entity Framework não remove os objetos com chaves duplicadas quando você chama o método **ApplyChanges**, mas gera uma exceção. Para evitar ter grafos com valores de chave duplicados, siga um dos padrões descritos no blog a seguir: [Self-Tracking Entities: ApplyChanges and duplicate entities](https://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409) (Entidades de Rastreamento Automático: ApplyChanges e entidades duplicadas).  
- Quando você altera a relação entre objetos definindo a propriedade de chave estrangeira, a propriedade de navegação de referência é definida como null e não é sincronizada com a entidade de segurança apropriada no cliente. Depois que o grafo é anexado ao contexto de objeto (por exemplo, depois de chamar o método **ApplyChanges**), as propriedades de chave estrangeira e as propriedades de navegação são sincronizadas.  

    > Não ter uma propriedade de navegação de referência sincronizada com o objeto de entidade de segurança apropriada poderia ser um problema se você tivesse especificado a exclusão em cascata na relação da chave estrangeira. Se você excluir a entidade de segurança, a exclusão não será propagada para os objetos dependentes. Se você tiver exclusões em cascata especificadas, use as propriedades de navegação para alterar a relação em vez de configurar a propriedade de chave estrangeira.  
- As entidades de rastreamento automático não estão habilitadas para executar o carregamento lento.  
- A serialização binária e a serialização para objetos de gerenciamento de estado do ASP.NET não são compatíveis com as entidades de rastreamento automático. No entanto, você pode personalizar o modelo para adicionar a compatibilidade à serialização binária. Para obter mais informações, consulte [Usando a serialização binária e ViewState com entidades de rastreamento automático](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Considerações sobre segurança  

As considerações de segurança a seguir devem ser levadas em conta ao trabalhar com entidades de rastreamento automático:  

- Um serviço não deve confiar em solicitações para recuperar ou atualizar dados de um cliente não confiável ou por meio de um canal não confiável. Um cliente deve ser autenticado: um envelope de mensagem ou canal seguro deve ser usado. Solicitações de clientes para atualizar ou recuperar dados devem ser validadas para garantir que elas estão em conformidade com as alterações esperadas e legítimas para o cenário dado.  
- Evite usar informações confidenciais, como chaves de entidade (por exemplo, números do seguro social). Isso reduz a possibilidade de serializar inadvertidamente informações confidenciais nos grafos de entidade de rastreamento automático para um cliente que não é totalmente confiável. Com associações independentes, a chave original de uma entidade que está relacionada à que está sendo serializada pode ser enviada para o cliente também.  
- Para evitar a propagação de mensagens de exceção que contêm dados confidenciais para a camada do cliente, chamadas para **ApplyChanges** e **SaveChanges** na camada do servidor devem ser encapsuladas no código de tratamento de exceção.  
