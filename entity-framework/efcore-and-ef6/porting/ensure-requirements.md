---
title: Portando de EF6 EF núcleo - validar requisitos
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052856"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de portabilidade de EF6 EF núcleo: validar requisitos do aplicativo

Antes de iniciar o processo de portabilidade é importante validar que o EF Core atende os requisitos de acesso de dados para seu aplicativo.

## <a name="missing-features"></a>Falta de recursos

Certifique-se de núcleo EF tem todos os recursos que você precisa usar em seu aplicativo. Consulte [comparação de recursos](../features.md) para uma comparação detalhada de como o conjunto de recursos no núcleo do EF compara com EF6. Se todos os recursos necessários estão ausentes, certifique-se de que você pode compensar a falta desses recursos antes de portar EF Core.

## <a name="behavior-changes"></a>Alterações de comportamento

Esta é uma lista parcial de algumas alterações no comportamento entre EF6 e EF Core. É importante mantê-las em mente como a porta de seu aplicativo como eles podem alterar a maneira que seu aplicativo se comporta, mas não será exibida como erros de compilação após a troca para EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento de DbSet.Add/Attach e gráfico

Em EF6, chamar `DbSet.Add()` em resultados de uma entidade em uma pesquisa recursiva para todas as entidades referenciadas em suas propriedades de navegação. As entidades que são encontradas e já não são controladas pelo contexto, também são marcado como adicionado. `DbSet.Attach()`se comporta da mesma, exceto pelo fato de todas as entidades são marcadas como inalterado.

**EF Core executa uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**

*  A entidade de raiz está sempre no estado solicitado (adicionado para `DbSet.Add` e permanecem inalterados para `DbSet.Attach`).

*  **Para entidades que são encontrados durante a pesquisa recursiva de propriedades de navegação:**

    *  **Se a chave primária da entidade é gerado pelo repositório**

        * Se a chave primária não está definida como um valor, o estado é definido como adicionado. O valor de chave primária é considerado "não configurado" se ele é atribuído o valor padrão CLR para o tipo de propriedade (ou seja, `0` para `int`, `null` para `string`, etc.).

        * Se a chave primária é definida como um valor, o estado é definido como inalterado.

    *  Se a chave primária não é um banco de dados gerado, a entidade é colocada no mesmo estado como a raiz.

### <a name="code-first-database-initialization"></a>Primeira inicialização de banco de dados de código

**EF6 tem uma quantidade significativa de magic executa em torno de selecionar a conexão de banco de dados e a inicialização de banco de dados. Algumas dessas regras incluem:**

* Se nenhuma configuração é realizada, EF6 selecionará um banco de dados SQL Express ou LocalDb.

* Se for uma cadeia de caracteres de conexão com o mesmo nome que o contexto dos aplicativos `App/Web.config` arquivo, essa conexão será usada.

* Se o banco de dados não existir, ele será criado.

* Se nenhuma das tabelas do modelo existe no banco de dados, o esquema para o modelo atual é adicionado ao banco de dados. Se as migrações são habilitadas, eles são usados para criar o banco de dados.

* Se o banco de dados existe e EF6 já tiver criado o esquema, o esquema é verificado para compatibilidade com o modelo atual. Uma exceção é gerada se o modelo foi alterado desde que o esquema foi criado.

**Núcleo EF não executa essa mágica.**

* A conexão de banco de dados deve ser configurado explicitamente no código.

* Nenhuma inicialização é executada. Você deve usar `DbContext.Database.Migrate()` para aplicar as migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem o uso de migrações).

### <a name="code-first-table-naming-convention"></a>Tabela de convenção de nomenclatura de primeiro do código

EF6 executa o nome da classe de entidade por meio de um serviço pluralização para calcular o nome da tabela padrão que a entidade é mapeada para.

Núcleo EF usa o nome do `DbSet` propriedade que a entidade é exposta no contexto de derivada. Se a entidade não tem um `DbSet` propriedade e, em seguida, o nome da classe é usada.
