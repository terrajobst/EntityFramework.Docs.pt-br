---
title: Novidades – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271438"
---
# <a name="whats-new-in-ef6"></a>Novidades no EF6

É altamente recomendável que você use a versão mais recente lançada do Entity Framework para garantir os recursos mais recentes e maior estabilidade.
No entanto, sabemos que talvez você precise usar uma versão anterior, ou talvez queira fazer experiências com novos aprimoramentos no pré-lançamento mais recente.
Para instalar versões específicas do EF, consulte [Obter o Entity Framework](~/ef6/fundamentals/install.md).

Esta página documenta os recursos que são incluídos em cada nova versão.

## <a name="recent-releases"></a>Versões recentes

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Atualização de ferramentas do EF no Visual Studio 2017 15.7

Em maio de 2018, lançamos uma versão atualizada das ferramentas do EF como parte do Visual Studio 2017 15.7.
Ela inclui melhorias para alguns pontos de problemas comuns:

- Correções para vários bugs de acessibilidade da interface do usuário
- Solução alternativa para a regressão de desempenho do SQL Server ao gerar modelos de bancos de dados existentes [#4](https://github.com/aspnet/entityframework6/issues/4)
- Suporte para atualizar modelos maiores no SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Outra melhoria nessa nova versão das ferramentas do EF é a instalação do tempo de execução do EF 6.2 quando um modelo é criado em um novo projeto. Com versões mais antigas do Visual Studio, é possível usar o tempo de execução do EF 6.2 (bem como qualquer versão anterior do EF) instalando a versão correspondente do pacote NuGet.

### <a name="ef-62-runtime"></a>Tempo de execução do EF 6.2

O tempo de execução do EF 6.2 foi lançado para o NuGet em outubro de 2017.
Graças, em grande parte, aos esforços de nossa comunidade de colaboradores de software livre, o EF 6.2 inclui várias [correções de bugs](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) e [aprimoramentos de produtos](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Aqui está uma lista resumida das alterações mais importantes que afetam o tempo de execução do EF 6.2:

- Redução do tempo de inicialização carregando modelos do Code First concluídos de um cache persistente [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- API fluente para definir índices [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() para habilitar a gravação de consultas LINQ que são traduzidas como LIKE no SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Agora Migrate.exe é compatível com a opção -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- Agora o EF6 pode trabalhar com valores de chave gerados por uma sequência no SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Atualização da lista de erros transitórios para a estratégia de execução do SQL Azure [n#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Bug: A repetição das consultas ou dos comandos SQL falhou em "O SqlParameter já está contido em outro SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Bug: A avaliação do DbQuery.ToString() atinge frequentemente o tempo limite no depurador [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Versões futuras

Para obter informações sobre a versão futura do EF6, examine nosso [roteiro](roadmap.md).

## <a name="past-releases"></a>Versões anteriores

A página [Versões Anteriores](past-releases.md) contém um arquivo morto de todas as versões anteriores do EF e os principais recursos que foram introduzidos em cada versão.
