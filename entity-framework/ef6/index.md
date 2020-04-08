---
title: Visão geral do Entity Framework 6 – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412751"
---
# <a name="entity-framework-6"></a>Entity Framework 6
O EF6 (Entity Framework 6) é um O/RM (mapeador relacional de objetos) testado para .NET com muitos anos de desenvolvimento e estabilização de recursos.

Como um O/RM, o EF6 reduz a incompatibilidade de impedância entre os mundos relacionais e orientado a objeto, permitindo que desenvolvedores escrevam aplicativos que interagem com os dados armazenados em bancos de dados relacionais usando objetos .NET fortemente tipados que representam o domínio do aplicativo e eliminando a necessidade de uma grande parte do acesso aos dados “canalizando” o código que normalmente eles precisariam escrever.

O EF6 implementa vários recursos populares de O/RM:
- Mapeamento de classes de entidade [POCO](xref:ef6/resources/glossary#poco) que não dependem de nenhum tipo de EF
- Controle de alterações automático
- Resolução de identidade e a unidade de trabalho
- Carregamento adiantado, lento e explícito
- Conversão das consultas fortemente tipadas usando [LINQ (consulta integrada à linguagem)](https://aka.ms/AA6hsvu)
- Recursos avançados de mapeamento, incluindo compatibilidade com:
  - Relações um para um, um para muitos e muitos para muitos
  - Herança (tabela por hierarquia, tabela por tipo e a tabela por classe concreta)
  - Tipos complexos
  - Procedimentos armazenados
- Um designer visual para criar modelos de entidade.
- Uma experiência de "Code First" para criar modelos de entidade escrevendo código.
- Os modelos podem ser gerados de bancos de dados existentes e editados manualmente ou podem ser criados do zero e usados para gerar novos bancos de dados.
- Integração com os modelos de aplicativos do .NET Framework, incluindo ASP.NET e por meio de associação de dados, com o WPF e o WinForms.
- Conectividade de banco de dados com base no ADO.NET e vários [provedores](xref:ef6/fundamentals/providers/index) disponíveis para se conectar a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 etc.

## <a name="should-i-use-ef6-or-ef-core"></a>Devo usar o EF6 ou o EF Core?

O EF Core é uma versão mais moderna, leve e extensível do Entity Framework que tem recursos e benefícios muito semelhantes ao EF6.
O EF Core é uma reformulação completa e contém muitos recursos novos que não estão disponíveis no EF6, embora ele ainda não tenha alguns dos recursos mais avançados de mapeamento do EF6.
Considere usar o EF Core em novos aplicativos se o conjunto de recursos corresponder aos seus requisitos.
[Comparar o EF Core e o EF6](xref:efcore-and-ef6/index) examina esta escolha mais detalhadamente.

## <a name="get-started"></a>[Introdução](xref:ef6/get-started)

Adicione o pacote NuGet EntityFramework ao seu projeto ou instale o [Entity Framework Tools para Visual Studio](https://aka.ms/AA6i8c5). Em seguida, assista a vídeos e leia tutoriais e a documentação avançada para saber como aproveitar o EF6 ao máximo.

## <a name="past-entity-framework-versions"></a>Versões anteriores do Entity Framework

Esta é a documentação para a versão mais recente do Entity Framework 6, embora a maior parte dela também se aplique às versões anteriores.
Confira as [Novidades](xref:ef6/what-is-new/index) e as [Versões anteriores](xref:ef6/what-is-new/past-releases) para encontrar uma lista completa das versões do EF e os recursos introduzidos.
