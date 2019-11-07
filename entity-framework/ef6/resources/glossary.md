---
title: Glossário de Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656151"
---
# <a name="entity-framework-glossary"></a>Glossário de Entity Framework
## <a name="code-first"></a>Code First
Criar um modelo de Entity Framework usando código. O modelo pode direcionar um banco de dados existente ou um novo banco de dados.

## <a name="context"></a>Contexto
Uma classe que representa uma sessão com o banco de dados, permitindo que você consulte e salve os mesmos. Um contexto deriva da classe DbContext ou ObjectContext.

## <a name="convention-code-first"></a>Convenção (Code First)
Uma regra que Entity Framework usa para inferir a forma do modelo de suas classes.

## <a name="database-first"></a>Database First
Criar um modelo de Entity Framework, usando o designer do EF, que tem como alvo um banco de dados existente.

## <a name="eager-loading"></a>Carregamento adiantado
Um padrão de carregar dados relacionados em que uma consulta para um tipo de entidade também carrega entidades relacionadas como parte da consulta.

## <a name="ef-designer"></a>EF Designer
Um designer visual no Visual Studio que permite criar um modelo de Entity Framework usando caixas e linhas.

## <a name="entity"></a>Entidade
Uma classe ou um objeto que representa dados de aplicativos, como clientes, produtos e pedidos.

## <a name="entity-data-model"></a>Modelo de Dados de Entidade
Um modelo que descreve as entidades e as relações entre elas. O EF usa o EDM para descrever o modelo conceitual no qual os programas de desenvolvedor. O EDM se baseia no modelo de relacionamento de entidade introduzido pela Dr. Peter Chen. O EDM foi originalmente desenvolvido com o principal objetivo de se tornar o modelo de dados comum em um conjunto de tecnologias de desenvolvedor e de servidor da Microsoft. O EDM também é usado como parte do protocolo OData.

## <a name="explicit-loading"></a>Carregamento explícito
Um padrão de carregar dados relacionados em que objetos relacionados são carregados chamando uma API.

## <a name="fluent-api"></a>API fluente
Uma API que pode ser usada para configurar um modelo de Code First.

## <a name="foreign-key-association"></a>Associação de chave estrangeira
Uma associação entre entidades em que uma propriedade que representa a chave estrangeira é incluída na classe da entidade dependente. Por exemplo, Product contém uma propriedade CategoryId.

## <a name="identifying-relationship"></a>Identificando relação
Uma relação em que a chave primária da entidade de segurança é parte da chave primária da entidade dependente. Nesse tipo de relação, a entidade dependente não pode existir sem a entidade de segurança.

## <a name="independent-association"></a>Associação independente
Uma associação entre entidades em que não há nenhuma propriedade representando a chave estrangeira na classe da entidade dependente. Por exemplo, uma classe Product contém uma relação com Category, mas sem Propriedade CategoryId. Entity Framework rastreia o estado da Associação independentemente do estado das entidades nas duas extremidades de associação.

## <a name="lazy-loading"></a>Carregamento lento
Um padrão de carregar dados relacionados em que objetos relacionados são carregados automaticamente quando uma propriedade de navegação é acessada.

## <a name="model-first"></a>Model First
Criar um modelo de Entity Framework, usando o designer do EF, que é usado para criar um novo banco de dados.

## <a name="navigation-property"></a>Propriedade de navegação
Uma propriedade de uma entidade que faz referência a outra entidade. Por exemplo, Product contém uma propriedade de navegação Category e Category contém uma propriedade de navegação Products.

## <a name="poco"></a>POCO
Acrônimo para objeto CLR Plain antigo. Uma classe de usuário simples que não tem nenhuma dependência com nenhuma estrutura. No contexto do EF, uma classe de entidade que não é derivada de EntityObject, implementa qualquer interface ou transporta qualquer atributo definido no EF. Essas classes de entidade que são dissociadas da estrutura de persistência também são consideradas "Persistence que desconhecem".  

## <a name="relationship-inverse"></a>Inverter relação
A extremidade oposta de uma relação, por exemplo, produto. Categoria e categoria. Remessa.

## <a name="self-tracking-entity"></a>Entidade de rastreamento automático
Uma entidade criada com base em um modelo de geração de código que ajuda com o desenvolvimento de N camadas.

## <a name="table-per-concrete-type-tpc"></a>Tipo de tabela por concreto (TPC)
Um método de mapear a herança em que cada tipo não abstrato na hierarquia é mapeado para uma tabela separada no banco de dados.

## <a name="table-per-hierarchy-tph"></a>Tabela por hierarquia (TPH)
Um método de mapear a herança em que todos os tipos na hierarquia são mapeados para a mesma tabela no banco de dados. Uma ou mais colunas discriminadoras são usadas para identificar a qual tipo cada linha está associada.

## <a name="table-per-type-tpt"></a>Tabela por tipo (TPT)
Um método de mapear a herança em que as propriedades comuns de todos os tipos na hierarquia são mapeadas para a mesma tabela no banco de dados, mas as propriedades exclusivas de cada tipo são mapeadas para uma tabela separada.

## <a name="type-discovery"></a>Descoberta de tipo
O processo de identificar os tipos que devem fazer parte de um modelo de Entity Framework.
