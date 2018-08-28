---
title: Referência de Linha de Comando – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 757d6562f5d3bcd4f026def02f208f5827786873
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997064"
---
<a name="entity-framework-core-tools"></a>Ferramentas do Entity Framework Core
===========================
As Ferramentas do Entity Framework Core ajudam durante o desenvolvimento de aplicativos EF Core. Eles são usados principalmente para o scaffold de DbContext e tipos de entidade fazendo engenharia reversa do esquema de um banco de dados e para gerenciar as Migrações.

As [Ferramentas PMC (Console do Gerenciador de Pacotes) do EF Core][1] oferece uma experiência superior no Visual Studio. Execute-as usando o [PMC (Console do Gerenciador de Pacotes)][2] do NuGet. Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.

As [Ferramentas de Linha de Comando do EF Core .NET][3] são uma extensão das [Ferramentas da CLI (Interface de Linha de Comando) do .NET Core][4] que são de multiplaforma e podem ser executadas fora do Visual Studio. Essas ferramentas exigem um projeto de SDK do .NET Core (com `Sdk="Microsoft.NET.Sdk"` ou semelhantes no arquivo de projeto).

Ambas as ferramentas expõem a mesma funcionalidade. Se você estiver desenvolvendo no Visual Studio, é recomendável usar as Ferramentas de PMC, pois elas oferecem uma experiência mais integrada.

<a name="frameworks"></a>Estruturas
----------
As ferramentas são compatíveis com projetos voltados para o .NET Framework ou o .NET Core.

Se você quiser usar uma biblioteca de classes, considere usar uma biblioteca de classes .NET Framework ou .NET Core, se possível. Assim, ocorrerão menos problemas com as ferramentas do .NET. Se, em vez disso, você quiser utilizar uma biblioteca de classes do .NET Standard, será necessário usar um projeto de inicialização que direcione para o .NET Framework ou .NET Core. Desse modo, as ferramentas terão uma plataforma de destino concreta, na qual poderão carregar a biblioteca de classes. Esse projeto de inicialização pode ser fictício, sem nenhum código real – ele só é necessário para fornecer um destino para as ferramentas.

Se o projeto direcionar para outra estrutura (por exemplo, Plataforma Universal do Windows ou Xamarin), será necessário criar uma biblioteca de classes .NET Standard separada. Nesse caso, siga as diretrizes acima para criar um projeto de inicialização que possa ser usado pelas ferramentas.

<a name="startup-and-target-projects"></a>Inicialização e projetos de destino
---------------------------
Sempre que você invoca um comando, existem dois projetos envolvidos: o projeto de destino e o projeto de inicialização.

O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).

O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.

O projeto de destino e o projeto de inicialização podem ser o mesmo.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
