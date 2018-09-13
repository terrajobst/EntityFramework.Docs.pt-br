---
title: Estudos de caso para o Entity Framework - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
ms.openlocfilehash: d7982a3f89ac1e57b48039d828f287adf6dc5068
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490866"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Estudos de caso da Microsoft para o Entity Framework
Os estudos de caso nesta página destacar alguns projetos de produção do mundo real que tem empregado o Entity Framework.
> [!NOTE]
> As versões detalhadas esses estudos de caso não estão mais disponíveis no site da Microsoft. Portanto, os links foram removidos.

## <a name="epicor"></a>Epicor
Epicor é uma empresa de software global grandes (com mais de 400 desenvolvedores) que desenvolve soluções de planejamento de recursos empresariais (ERP) para empresas em mais de 150 países.
Seu principal produto Epicor 9, baseia-se em uma SOA (arquitetura) usando o .NET Framework.
Enfrentam várias solicitações de cliente para fornecer suporte para consulta integrada à linguagem (LINQ) e também desejam reduzir a carga em seus servidores SQL de back-end, a equipe decidiu atualizar para o Visual Studio 2010 e o .NET Framework 4.0.
Usando o Entity Framework 4.0, eles conseguiram atingir essas metas e simplificar significativamente o desenvolvimento e manutenção.
Em particular, suporte de T4 avançado do Entity Framework permitiu assumir controle total do seu código gerado e automaticamente compilar em economia de desempenho de recursos, como consultas pré-compilação compiladas e armazenamento em cache.

> "Realizamos alguns testes de desempenho recentemente com o código existente e fomos capazes de reduzir as solicitações para o SQL Server, 90 por cento.
Isso é devido ao ADO.NET Entity Framework 4." – Erik Johnson, Vice-presidente, pesquisa de produto  

## <a name="veracity-solutions"></a>Soluções de veracidade
Ter adquirido um sistema de software de planejamento de eventos que seria difícil de manter e estender a longo prazo, soluções de veracidade usado Visual Studio 2010 reescrevê-lo como um poderoso e fácil de usar Rich Internet Application criados sobre o Silverlight 4.
Usando serviços .NET RIA, eles conseguiram criar rapidamente uma camada de serviço sobre o Entity Framework que evitou a duplicação de código e permitida para validação comum e lógica de autenticação entre camadas.  

> "Nós foram vendidas no Entity Framework quando ele foi introduzido pela primeira vez e o Entity Framework 4 provou para ser ainda melhor.
Ferramentas é aprimorada e é mais fácil manipular os arquivos. edmx que definem o modelo conceitual, o modelo de armazenamento e o mapeamento entre esses modelos... Com o Entity Framework, posso obter essa camada de acesso a dados trabalhando em um dia — e desenvolvê-la durante o processo ao longo.
O Entity Framework é nossa camada de acesso a dados de fato; Não sei por que alguém não usá-lo." – Desenvolvedor sênior Joe McBride

## <a name="nec-display-solutions-of-america"></a>Soluções de exibição NEC da América
NEC queria no mercado para publicidade digital com base no local, com uma solução para se beneficiar de anunciantes e proprietários de rede e aumentar a sua própria receita.
Para fazer isso, ele lançou um par de aplicativos da web que automatizam os processos manuais necessários em uma campanha de anúncio tradicional.
Os sites foram criados usando ASP.NET, Silverlight 3, AJAX e WCF, juntamente com o Entity Framework na camada de acesso a dados para se comunicar com o SQL Server 2008.

> "Com o SQL Server, acreditamos que poderíamos obter a taxa de transferência que precisávamos para servir anunciantes e redes com as informações em tempo real e a confiabilidade para ajudar a garantir que as informações em nossos aplicativos de missão crítica sempre estaria disponíveis"-Mike Corcoran, Diretor de IT

## <a name="darwin-dimensions"></a>Dimensões de Darwin
A equipe de Darwin usando uma tecnologia de ampla da Microsoft, propus a criar Evolver – um portal de avatar online que os consumidores pode usar para criar os avatares impressionantes e realistas para uso em jogos, animações e páginas de rede sociais.
Com os benefícios de produtividade do Entity Framework e efetuar pull nos componentes, como o Windows Workflow Foundation (WF) e o Windows Server AppFabric (cache de um aplicativo altamente escalonável de na memória), a equipe foi capaz de entregar um produto incrível em 35% menos tempo de desenvolvimento.
Apesar dos membros da equipe dividem em vários países, a equipe de acordo com um processo de desenvolvimento do agile com lançamentos semanais.

 > "Nós não tentar criar a tecnologia para a tecnologia. Como um tipo de inicialização, é crucial que podemos aproveitar a tecnologia que economiza tempo e dinheiro.
 .NET era a preferida para o desenvolvimento rápido e econômico." – Zachary Olsen, arquiteto  

## <a name="silverware"></a>Talheres de prata
Com mais de 15 anos de experiência no desenvolvimento de soluções de ponto de venda (PDV) para grupos de restaurante pequeno e médio porte, a equipe de desenvolvimento em talheres de prata se preparam para aprimorar seus produtos com mais recursos de nível empresarial para atrair maiores cadeias de restaurante.
Usando a versão mais recente das ferramentas de desenvolvimento da Microsoft, eles conseguiram criar a nova solução quatro vezes mais rápida do que antes.
Principais recursos novos, como LINQ e o Entity Framework facilitou mover de Crystal Reports para SQL Server 2008 e SQL Server Reporting Services (SSRS) para seu armazenamento de dados e necessidades de relatórios.

> "Gerenciamento de dados eficaz é fundamental para o sucesso da talheres de prata – e isso é por isso, decidimos adotar a relatórios SQL." -Nicholas Romanidis, diretor de IT / engenharia de Software
