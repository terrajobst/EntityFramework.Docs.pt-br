---
title: Portando de EF6 para requisitos de validação de EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565352"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de portar de EF6 para EF Core: Validar os requisitos do aplicativo

Antes de iniciar o processo de portabilidade, é importante validar que EF Core atende aos requisitos de acesso de dados para seu aplicativo.

## <a name="missing-features"></a>Recursos ausentes

Verifique se EF Core tem todos os recursos que você precisa usar em seu aplicativo. Consulte [comparação de recursos](../features.md) para obter uma comparação detalhada de como o conjunto de recursos no EF Core se compara com EF6. Se algum recurso necessário estiver ausente, verifique se você pode compensar a falta desses recursos antes de portar para EF Core.

## <a name="behavior-changes"></a>Alterações de comportamento

Esta é uma lista não exaustiva de algumas alterações no comportamento entre EF6 e EF Core. É importante ter isso em mente como sua porta de seu aplicativo, pois eles podem mudar a forma com que seu aplicativo se comporta, mas não aparecerá como erros de compilação após a troca para EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet. Adicionar/anexar e comportamento de grafo

No EF6, a `DbSet.Add()` chamada em uma entidade resulta em uma pesquisa recursiva de todas as entidades referenciadas em suas propriedades de navegação. Todas as entidades que são encontradas e ainda não são rastreadas pelo contexto também são marcadas como adicionadas. `DbSet.Attach()`comporta-se, exceto que todas as entidades são marcadas como inalteradas.

**EF Core executa uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**

*  A entidade raiz sempre está no estado solicitado (adicionada para `DbSet.Add` e inalterada para `DbSet.Attach`).

*  **Para entidades que são encontradas durante a pesquisa recursiva de propriedades de navegação:**

    *  **Se a chave primária da entidade for gerada pelo armazenamento**

        * Se a chave primária não estiver definida como um valor, o estado será definido como adicionado. O valor da chave primária será considerado "não definido" se for atribuído o valor padrão do CLR para o tipo de propriedade (por `0` exemplo `int`, `null` para `string`, para, etc.).

        * Se a chave primária for definida como um valor, o estado será definido como inalterado.

    *  Se a chave primária não for gerada pelo banco de dados, a entidade será colocada no mesmo estado que a raiz.

### <a name="code-first-database-initialization"></a>Inicialização do banco de dados Code First

**O EF6 tem uma quantidade significativa de mágica que ele realiza em relação à seleção da conexão de banco de dados e à inicialização do banco de dados. Algumas dessas regras incluem:**

* Se nenhuma configuração for executada, o EF6 selecionará um banco de dados no SQL Express ou LocalDb.

* Se uma cadeia de conexão com o mesmo nome que o contexto estiver no arquivo `App/Web.config` de aplicativos, essa conexão será usada.

* Se o banco de dados não existir, ele será criado.

* Se nenhuma das tabelas do modelo existir no banco de dados, o esquema para o modelo atual será adicionado ao banco de dados. Se as migrações estiverem habilitadas, elas serão usadas para criar o banco de dados.

* Se o banco de dados existir e EF6 tiver criado o esquema anteriormente, o esquema será verificado quanto à compatibilidade com o modelo atual. Uma exceção será gerada se o modelo tiver sido alterado desde que o esquema foi criado.

**EF Core não realiza nenhuma dessas mágicas.**

* A conexão de banco de dados deve ser explicitamente configurada no código.

* Nenhuma inicialização é executada. Você deve usar `DbContext.Database.Migrate()` para aplicar migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar as migrações).

### <a name="code-first-table-naming-convention"></a>Code First Convenção de nomenclatura de tabela

EF6 executa o nome da classe de entidade por meio de um serviço de pluralização para calcular o nome de tabela padrão ao qual a entidade está mapeada.

EF Core usa o nome da `DbSet` Propriedade na qual a entidade é exposta no contexto derivado. Se a entidade não tiver uma `DbSet` Propriedade, o nome da classe será usado.
