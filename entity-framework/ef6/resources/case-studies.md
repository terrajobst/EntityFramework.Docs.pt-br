---
title: Estudos de caso para Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417077"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Estudos de caso da Microsoft para Entity Framework
Os estudos de caso nesta página destacam alguns projetos de produção do mundo real que empregaram Entity Framework.
> [!NOTE]
> As versões detalhadas desses estudos de caso não estão mais disponíveis no site da Microsoft. Portanto, os links foram removidos.

## <a name="epicor"></a>Epicor
O Epicor é uma grande empresa de software global (com mais de 400 desenvolvedores) que desenvolve soluções de ERP (planejamento de recursos empresariais) para empresas em mais de 150 países.
Seu principal produto, Epicor 9, é baseado em uma SOA (arquitetura orientada a serviços) usando o .NET Framework.
Enfrentando inúmeras solicitações de clientes para fornecer suporte para LINQ (consulta integrada à linguagem) e também quer reduzir a carga em seus servidores SQL back-end, a equipe decidiu atualizar para o Visual Studio 2010 e o .NET Framework 4,0.
Usando o Entity Framework 4,0, eles conseguiram atingir essas metas e também simplificam bastante o desenvolvimento e a manutenção.
Em particular, o suporte de T4 avançado da Entity Framework permitia que eles adotassem controle total de seu código gerado e criaremos automaticamente recursos de economia de desempenho, como consultas e cache pré-compilados.

> "Nós conduzimos alguns testes de desempenho recentemente com o código existente, e pudemos reduzir as solicitações para SQL Server em 90%.
Isso ocorre devido ao ADO.NET Entity Framework 4. " – Erik Johnson, Vice-Presidente, pesquisa de produtos  

## <a name="veracity-solutions"></a>Soluções de veracidade
Tendo adquirido um sistema de software de planejamento de eventos que seria difícil de manter e estender em longo prazo, as soluções veracidade usavam o Visual Studio 2010 para reescrevê-lo como um aplicativo avançado de Internet sofisticado e fácil de usar criado no Silverlight 4.
Usando os serviços RIA do .NET, eles podiam criar rapidamente uma camada de serviço sobre o Entity Framework que evitava a duplicação de código e permitia a lógica de validação e autenticação comum entre camadas.  

> "Nós foram vendidos na Entity Framework quando ele foi introduzido pela primeira vez, e o Entity Framework 4 provou ser ainda melhor.
As ferramentas são aprimoradas e é mais fácil manipular os arquivos. edmx que definem o modelo conceitual, o modelo de armazenamento e o mapeamento entre esses modelos... Com o Entity Framework, posso obter essa camada de acesso a dados funcionando em um dia — e compilá-la enquanto eu vou.
A Entity Framework é nossa camada de acesso de dados de fato; Não sei por que ninguém o usaria. " – Joe McBride, desenvolvedor sênior

## <a name="nec-display-solutions-of-america"></a>Soluções de exibição NEC da América
NEC queria entrar no mercado para anúncios baseados em lugares digitais com uma solução para beneficiar os anunciantes e os proprietários da rede e aumentar suas próprias receitas.
Para fazer isso, ele lançou um par de aplicativos Web que automatizam os processos manuais necessários em uma campanha de anúncio tradicional.
Os sites foram criados usando o ASP.NET, o Silverlight 3, o AJAX e o WCF, junto com o Entity Framework na camada de acesso a dados para se comunicar com SQL Server 2008.

> "Com SQL Server, achamos que poderíamos obter a taxa de transferência necessária para atender anunciantes e redes com informações em tempo real e a confiabilidade para ajudar a garantir que as informações em nossos aplicativos críticos sempre estarão disponíveis"-Mike Corcoran, Diretor de ti

## <a name="darwin-dimensions"></a>Dimensões Darwin
Usando uma ampla gama de tecnologias da Microsoft, a equipe, em Darwin, foi definida para criar um desenvolvedor – um portal de avatar online que os consumidores podiam usar para criar avatars incríveis e realistas para uso em jogos, animações e páginas de rede social.
Com os benefícios de produtividade do Entity Framework e a extração de componentes como o Windows Workflow Foundation (WF) e o Windows Server AppFabric (um cache de aplicativos altamente escalonável na memória), a equipe foi capaz de entregar um produto incrível em 35% menos tempo de desenvolvimento.
Apesar de terem os membros da equipe divididos em vários países, a equipe segue um processo de desenvolvimento ágil com versões semanais.

 > "Tentamos não criar tecnologia para fins de tecnologia. Como uma inicialização, é crucial que possamos aproveitar a tecnologia que economiza tempo e dinheiro.
 O .NET foi a opção de desenvolvimento rápido e econômico. " – Zachary Olsen, arquiteto  

## <a name="silverware"></a>Silvering
Com mais de 15 anos de experiência no desenvolvimento de soluções de POS (ponto de venda) para grupos de restaurante pequenos e médios, a equipe de desenvolvimento da Silvering definiu para aprimorar seus produtos com recursos de nível corporativo para atrair maiores cadeias de restaurante.
Usando a versão mais recente das ferramentas de desenvolvimento da Microsoft, eles conseguiram criar a nova solução quatro vezes mais rápida do que antes.
Os principais novos recursos como o LINQ e o Entity Framework facilitaram a migração do Crystal Reports para SQL Server 2008 e SQL Server Reporting Services (SSRS) para suas necessidades de armazenamento e relatórios de dados.

> "O gerenciamento de dados efetivo é fundamental para o sucesso do Silvering – e é por isso que decidimos adotar os relatórios SQL." -Nicholas Romanidis, diretor de engenharia de ti/software
