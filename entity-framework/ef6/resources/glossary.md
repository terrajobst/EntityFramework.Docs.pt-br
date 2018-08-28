---
title: Glossário do Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 4ad56c4d655e004d97c3537707fa6b13c7acf88e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997713"
---
# <a name="entity-framework-glossary"></a>Glossário do Entity Framework
## <a name="code-first"></a>Code First
Criando um modelo do Entity Framework usando código. O modelo pode ter como destino e o banco de dados existente ou um novo banco de dados.

## <a name="context"></a>Contexto
Uma classe que representa uma sessão com o banco de dados, permitindo que você consultar e salvar os dados. Um contexto deriva da classe DbContext ou ObjectContext.

## <a name="convention-code-first"></a>Convenção (Code First)
Uma regra que usa o Entity Framework para inferir a forma do modelo para você de suas classes.

## <a name="database-first"></a>Primeiro banco de dados
Criando um modelo do Entity Framework, usando o Designer de EF, que tem como alvo um banco de dados existente.

## <a name="eager-loading"></a>Carregamento adiantado
Um padrão de carregamento de dados relacionados em que uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.

## <a name="ef-designer"></a>EF Designer
Um designer visual no Visual Studio que permite que você crie um modelo do Entity Framework usando as caixas e linhas.

## <a name="entity"></a>Entidade
Uma classe ou objeto que representa os dados de aplicativo, como clientes, produtos e pedidos.

## <a name="entity-data-model"></a>Modelo de Dados de Entidade
Um modelo que descreve entidades e as relações entre eles. O EF usa EDM para descrever o modelo conceitual em relação ao qual os programas do desenvolvedor. EDM se baseia no modelo de relação de entidade, introduzido pela recuperação de desastre. Peter Chen. O EDM foi desenvolvido originalmente com o principal objetivo de se tornar o modelo de dados comum de um conjunto de tecnologias de desenvolvedor e o servidor da Microsoft. O EDM também é usado como parte do protocolo OData.

## <a name="explicit-loading"></a>Carregamento explícito
Um padrão de carregamento de dados relacionados em que os objetos relacionados são carregados, chamando uma API.

## <a name="fluent-api"></a>API fluente
Uma API que pode ser usada para configurar um modelo Code First.

## <a name="foreign-key-association"></a>Associação de chave estrangeira
Uma associação entre entidades em que uma propriedade que representa a chave estrangeira está incluída na classe de entidade dependente. Por exemplo, o produto contém uma propriedade CategoryId.

## <a name="identifying-relationship"></a>Relação de identificação
Uma relação em que a chave primária da entidade de segurança é parte da chave primária da entidade dependente. Nesse tipo de relação, a entidade dependente não pode existir sem a entidade de segurança.

## <a name="independent-association"></a>Associação independente
Uma associação entre entidades em que não há nenhuma propriedade que representa a chave estrangeira na classe de entidade dependente. Por exemplo, uma classe Product contém uma relação de categoria, mas nenhuma propriedade CategoryId. Entity Framework controla o estado da associação independentemente do estado das entidades nas extremidades de associação de dois.

## <a name="lazy-loading"></a>Carregamento lento
Um padrão de carregamento de dados relacionados em que os objetos relacionados são carregados automaticamente quando uma propriedade de navegação é acessada.

## <a name="model-first"></a>Model First
Criar um modelo do Entity Framework, usando o Designer de EF, o que, em seguida, é usado para criar um novo banco de dados.

## <a name="navigation-property"></a>Propriedade de navegação
Uma propriedade de uma entidade que faz referência a outra entidade. Por exemplo, o produto contém uma propriedade de navegação de categoria e categoria contém uma propriedade de navegação de produtos.

## <a name="poco"></a>POCO
Acrônimo para objeto Plain Old CLR. Uma classe de usuário simples que não tem nenhuma dependência com qualquer estrutura. No contexto do EF, um uma classe de entidade que não é derivado de EntityObject, implementar quaisquer interfaces ou executa qualquer atributo definido no EF. Essas classes de entidade que são separados da estrutura de persistência também devem ser "com ignorância de persistência".  

## <a name="relationship-inverse"></a>Inverso da relação
A extremidade oposta de uma relação, por exemplo, o produto. Categoria e categoria. Produto.

## <a name="self-tracking-entity"></a>Entidade de autocontrole
Entidade criada a partir de um modelo de geração de código que ajuda com o desenvolvimento de N camadas.

## <a name="table-per-concrete-type-tpc"></a>Tipo de tabela por concreto (TPC)
Um método de mapeamento de herança em que cada tipo de não-abstrata na hierarquia é mapeado para separar a tabela no banco de dados.

## <a name="table-per-hierarchy-tph"></a>Tabela por hierarquia (TPH)
Um método de mapeamento de herança em que todos os tipos na hierarquia são mapeados para a mesma tabela no banco de dados. Um discriminador coluna (s) é usado para identificar qual tipo cada linha está associado.

## <a name="table-per-type-tpt"></a>Tabela por tipo (TPT)
Um método de mapeamento de herança em que as propriedades comuns de todos os tipos na hierarquia são mapeadas para a mesma tabela no banco de dados, mas propriedades exclusivas para cada tipo são mapeadas para uma tabela separada.

## <a name="type-discovery"></a>Descoberta do tipo
O processo de identificar os tipos que devem fazer parte de um modelo do Entity Framework.
