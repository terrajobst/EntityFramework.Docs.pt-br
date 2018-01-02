---
title: "Referência de Linha de Comando – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
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

Se o seu projeto tiver como destino outra estrutura (por exemplo, Universal do Windows ou Xamarin), é recomendável criar um projeto .NET Standard separado e fazer o direcionamento cruzado das estruturas compatíveis.

Para fazer o direcionamento cruzado do .NET Core, por exemplo, clique com o botão direito do mouse no projeto e selecione **Editar \*.csproj**. Atualize a propriedade `TargetFramework` da seguinte maneira. (Observe que o nome da propriedade passa a estar no plural.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Se você estiver usando uma biblioteca de classes .NET Standard, não precisará fazer o direcionamento cruzado caso seu projeto de inicialização tenha como destino o .NET Framework ou o .NET Core.

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
