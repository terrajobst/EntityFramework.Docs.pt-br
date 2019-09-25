---
title: Portando de EF6 para EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 42e40ce769a67a987883027e1807ec7eaeb4ad7a
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198034"
---
# <a name="porting-from-ef6-to-ef-core"></a>Portando de EF6 para EF Core

Devido às mudanças fundamentais no EF Core, não recomendamos tentar mover um aplicativo EF6 para o EF Core, a menos que você tenha um motivo convincente para fazer essa alteração.
Você deve entender a mudança de EF6 para EF Core como portabilidade, em vez de uma upgrade.

> [!IMPORTANT]
> Antes de você começar o processo de portabilidade, é importante validar se o EF Core atende aos requisitos de acesso a dados do seu aplicativo.

## <a name="missing-features"></a>Recursos ausentes

Verifique se o EF Core possui todos os recursos que você precisa usar em seu aplicativo. Confira [Comparação de recursos](xref:efcore-and-ef6/index) para obter uma comparação detalhada entre o conjunto de recursos do EF Core e do EF6. Se algum dos recursos obrigatórios estiver ausente, garanta que você possa compensar a falta desses recursos antes da portabilidade para o EF Core.

## <a name="behavior-changes"></a>Alterações de comportamento

Esta é uma lista parcial de algumas mudanças no comportamento entre o EF6 e o EF Core. É importante tê-las em mente ao portar seu aplicativo, já que elas podem mudar a forma como seu aplicativo se comporta, mas não aparecerão como erros de compilação após a troca para o EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamento de DbSet.Add/Attach e graph

No EF6, chamar `DbSet.Add()` em uma entidade resulta em uma pesquisa recursiva de todas as entidades referenciadas em suas propriedades de navegação. Quaisquer entidades encontradas e que ainda não são rastreadas pelo contexto também são marcadas como adicionadas. O `DbSet.Attach()` se comporta da mesma forma, mas todas as entidades são marcadas como inalteradas.

**O EF Core realiza uma pesquisa recursiva semelhante, mas com algumas regras ligeiramente diferentes.**

*  A entidade raiz está sempre no estado solicitado (adicionado para `DbSet.Add` e inalterado para `DbSet.Attach`).

*  **Para entidades encontradas durante a pesquisa recursiva de propriedades de navegação:**

    *  **Se a chave primária da entidade for gerada pelo repositório**

        * Se a chave primária não for definida com um valor, o estado será definido como adicionado. O valor da chave primária será considerado "não definido" se for atribuído a ele o valor padrão de CLR do tipo de propriedade (por exemplo, `0` para `int`, `null` para `string` etc.).

        * Se a chave primária for definida com um valor, o estado será definido como inalterado.

    *  Se a chave primária não for gerada pelo banco de dados, a entidade será colocada no mesmo estado que a raiz.

### <a name="code-first-database-initialization"></a>Inicialização de banco de dados Code First

**O EF6 realiza uma quantidade significativa de mágica ao selecionar a conexão de banco de dados e inicializar o banco de dados. Essas regras incluem:**

* Se nenhuma configuração for realizada, o EF6 selecionará um banco de dados no SQL Express ou LocalDb.

* Se uma cadeia de conexão com o mesmo nome que o contexto estiver no arquivo `App/Web.config` dos aplicativos, essa conexão será usada.

* Se o banco de dados não existir, ele será criado.

* Se nenhuma das tabelas do modelo existir no banco de dados, o esquema do modelo atual será adicionado ao banco de dados. Se as migrações estiverem habilitadas, elas serão usadas para criar o banco de dados.

* Se o banco de dados existir e o EF6 tiver criado o esquema anteriormente, o esquema será verificado quanto à compatibilidade com o modelo atual. Uma exceção será lançada se o modelo tiver mudado desde a criação do esquema.

**O EF Core não faz nenhuma parte dessa mágica.**

* A conexão de banco de dados precisa ser configurada explicitamente no código.

* Nenhuma inicialização é realizada. Você deve usar `DbContext.Database.Migrate()` para aplicar migrações (ou `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` para criar/excluir o banco de dados sem usar migrações).

### <a name="code-first-table-naming-convention"></a>Convenção de nomenclatura de tabela Code First

O EF6 executa o nome de classe da entidade por meio de um serviço de pluralização para calcular o nome de tabela padrão para a qual a entidade é mapeada.

O EF Core usa o nome da propriedade `DbSet` à qual a entidade é exposta no contexto derivado. Se a entidade não tiver uma propriedade `DbSet`, o nome de classe será usado.
