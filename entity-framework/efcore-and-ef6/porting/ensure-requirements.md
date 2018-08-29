---
title: Portando de EF6 para EF Core – validar requisitos
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993434"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de portabilidade do EF6 para EF Core: validar os requisitos do aplicativo

Antes de iniciar o processo de portabilidade é importante validar que o EF Core atende aos requisitos de acesso a dados para seu aplicativo.

## <a name="missing-features"></a>Recursos ausentes

Certifique-se de que o EF Core tem todos os recursos que você precisa usar em seu aplicativo. Ver [comparação de recursos](../features.md) para uma comparação detalhada de como o conjunto de recursos no EF Core se compara ao EF6. Se quaisquer recursos necessários estão ausentes, certifique-se de que você pode compensar a falta desses recursos antes de portar para EF Core.

## <a name="behavior-changes"></a>Alterações de comportamento

Esta é uma lista parcial de algumas alterações no comportamento entre o EF6 e EF Core. É importante manter essas práticas como sua porta de seu aplicativo como eles podem alterar a maneira que seu aplicativo se comporta, mas não será exibida como erros de compilação após a troca para o EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento de DbSet.Add/Attach e grafo

No EF6, chamar `DbSet.Add()` em resultados de uma entidade em uma pesquisa recursiva para todas as entidades referenciadas em suas propriedades de navegação. Qualquer entidade que é encontrada e já não é controlada pelo contexto, também é marcadas como adicionadas. `DbSet.Attach()` se comporta da mesma, exceto pelo fato de todas as entidades são marcadas como inalterado.

**O EF Core executa uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**

*  A entidade raiz sempre está no estado solicitado (adicionado `DbSet.Add` e inalterado para `DbSet.Attach`).

*  **Para entidades que são encontradas durante a pesquisa recursiva de propriedades de navegação:**

    *  **Se a chave primária da entidade é geradas pelo repositório**

        * Se a chave primária não está definida como um valor, o estado é definido como adicionado. O valor de chave primária é considerado "não definido" se ele é atribuído o valor de padrão do CLR para o tipo de propriedade (por exemplo, `0` para `int`, `null` para `string`, etc.).

        * Se a chave primária é definida como um valor, o estado é definido como inalterado.

    *  Se a chave primária não é um banco de dados gerado, a entidade será colocada no mesmo estado, como a raiz.

### <a name="code-first-database-initialization"></a>Primeira inicialização do banco de dados de código

**EF6 tem uma quantidade significativa de mágica, que ele executa em torno de selecionar a conexão de banco de dados e inicializar o banco de dados. Algumas dessas regras incluem:**

* Se nenhuma configuração é realizada, o EF6 selecionará um banco de dados SQL Express ou LocalDb.

* Se uma cadeia de caracteres de conexão com o mesmo nome que o contexto é em aplicativos de `App/Web.config` arquivo, essa conexão será usada.

* Se o banco de dados não existir, ele é criado.

* Se nenhuma das tabelas do modelo existir no banco de dados, o esquema para o modelo atual é adicionado ao banco de dados. Se as migrações são habilitadas, eles são usados para criar o banco de dados.

* Se o banco de dados existe e EF6 tivesse criado anteriormente o esquema, o esquema é verificado para compatibilidade com o modelo atual. Uma exceção é lançada se o modelo foi alterado desde que o esquema foi criado.

**O EF Core não executa essa mágica.**

* A conexão de banco de dados deve ser configurado explicitamente no código.

* Nenhuma inicialização será executada. Você deve usar `DbContext.Database.Migrate()` para aplicar as migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar as migrações).

### <a name="code-first-table-naming-convention"></a>Tabela do primeiro código convenção de nomenclatura

EF6 é executado o nome de classe de entidade por meio de um serviço de pluralização para calcular o nome da tabela padrão que a entidade é mapeada para.

EF Core usa o nome da `DbSet` propriedade que a entidade é exposta no contexto derivado. Se a entidade não tiver um `DbSet` propriedade e, em seguida, o nome da classe é usada.
