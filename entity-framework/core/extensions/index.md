---
title: Ferramentas e Extensões – EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995507"
---
# <a name="ef-core-tools--extensions"></a>Ferramentas e Extensões do EF Core

Ferramentas e extensões oferecem funcionalidade adicional para o Entity Framework Core.

> [!IMPORTANT]  
> As extensões são compostas de diversas fontes e não são mantidas como parte do projeto do Entity Framework Core. Ao considerar uma extensão de terceiros, avalie a qualidade, o licenciamento, a compatibilidade, o suporte etc. para garantir ela cumpra os requisitos.

## <a name="tools"></a>Ferramentas

### <a name="llblgen-pro"></a>LLBLGen Pro

O LLBLGen Pro é uma solução de modelagem de entidade compatível com o Entity Framework e o Entity Framework Core. Com ele, é possível definir facilmente o modelo de entidade e mapeá-lo em seu banco de dados usando as abordagens Model First ou Database First. Assim, você pode começar a escrever consultas imediatamente.

[site](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Desenvolvedor de Entidade Devart

O Desenvolvedor de Entidade é um potente designer de ORM para o ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL. Use as abordagens Model First e Database First para criar seu modelo de ORM e gerar um código C# ou Visual Basic .NET para ele. O Desenvolvedor de Entidade apresenta novas abordagens para a criação de modelos de ORM, aumenta a produtividade e facilita o desenvolvimento de aplicativos de banco de dados.

[site](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Visual Studio 2017+ extensão. É possível fazer engenharia reversa do DbContext e de classes POCO por meio de um banco de dados existente ou projeto de banco de dados SQL Server, bem como visualizar e inspecionar seu DbContext de diversas maneiras.

[Wiki do GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a>Extensões

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Plug-in para o Microsoft.EntityFrameworkCore para oferecer suporte à gravação automática do histórico de alterações de dados.

[Repositório do GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Extensões do Dynamic Linq para Microsoft.EntityFrameworkCore que adicionam suporte assíncrono

 [Repositório do GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Tente capturar algumas boas ou melhores práticas em uma API com suporte para testes – incluindo uma pequena estrutura para verificar se há consultas N+1.

[Repositório do GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Biblioteca de Cache de Segundo Nível. O cache segundo nível é um cache de consulta. Os resultados dos comandos do EF serão armazenados no cache. Esses mesmos comandos recuperarão seus dados do cache em vez de executá-los no banco de dados novamente.

[Repositório do GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Carrega e salva grafos de entidade desanexados inteiros (a entidade com suas entidades filho e listas). Inspirado por [GraphDiff](https://github.com/refactorthis/GraphDiff/). A ideia é também adicionar alguns plug-ins para simplificar algumas tarefas repetitivas, como auditoria e paginação.

[Repositório do GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Recupere a chave primária (incluindo chaves compostas) de qualquer entidade como um dicionário.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Wrappers de extensão reativa para observáveis ativos das entidades do Entity Framework.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Adicione gatilhos às suas entidades com eventos de inserção, atualização e exclusão. Há três eventos para cada um: antes, após e em caso de falha.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Obtenha acesso tipado ao OriginalValue de suas propriedades de entidade. Há suporte para propriedades simples e complexas. Não há suporte para navegação/coleções.

[Repositório do GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

O Geco oferece um gerador de Modelo Reverso com suporte para Pluralização/Singularização e modelos editáveis baseados em cadeias de caracteres interpoladas do C# 6.0 executado no .Net Core. Também conta com um gerador de script Seed com scripts SQL Merge e um executor de script.

[Repositório do GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

O LinqKit.Microsoft.EntityFrameworkCore é um conjunto gratuito de extensões para usuários avançados do LINQ to SQL e do EntityFrameworkCore. Com suporte para Include(...) e IDbAsync.

[Repositório do GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

O NeinLinq.EntityFrameworkCore oferece extensões úteis para provedores LINQ, como o Entity Framework compatível com apenas um subconjunto menor de funções do .NET, reutilização de funções, reescrita de consultas e transformação delas em null-safe e criação de consultas dinâmicas com predicados e seletores traduzíveis.

[Repositório do GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Um plug-in para Microsoft.EntityFrameworkCore para dar suporte a repositório, padrões de unidade de trabalho e vários bancos de dados compatíveis com transação distribuída.

[Repositório do GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Carregamento lento para EF Core 1.1

[Repositório do GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensões do EntityFrameworkCore para operações em massa (Inserir, Atualizar e Excluir).

[Repositório do GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Adiciona a pluralização de tempo de design ao EF Core.

[Repositório do GitHub](https://github.com/bricelam/EFCore.Pluralizer)
