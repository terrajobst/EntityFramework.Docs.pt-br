---
title: Selecionando Entity Framework versão de tempo de execução para modelos do EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418143"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Selecionando Entity Framework versão de tempo de execução para modelos de designer do EF
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

A partir do EF6, a tela a seguir foi adicionada ao designer do EF para permitir que você selecione a versão do tempo de execução que você deseja direcionar ao criar um modelo. A tela será exibida quando a versão mais recente do Entity Framework ainda não estiver instalada no projeto. Se a versão mais recente já estiver instalada, ela será usada por padrão.

![Na](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Direcionando EF6. x

Você pode escolher EF6 na tela ' escolher sua versão ' para adicionar o tempo de execução do EF6 ao seu projeto. Depois de adicionar o EF6, você deixará de ver esta tela no projeto atual.

O EF6 será desabilitado se você já tiver uma versão mais antiga do EF instalada (já que não é possível direcionar várias versões do tempo de execução do mesmo projeto). Se a opção EF6 não estiver habilitada aqui, siga estas etapas para atualizar seu projeto para EF6:

1.  Clique com o botão direito do mouse em seu projeto no Gerenciador de Soluções e selecione **gerenciar pacotes NuGet...**
2.  Selecionar **atualizações**
3.  Selecione o **EntityFramework** (verifique se ele vai atualizá-lo para a versão desejada)
4.  Clique em **Atualizar**

 

## <a name="targeting-ef5x"></a>Direcionando EF5. x

Você pode escolher EF5 na tela ' escolher sua versão ' para adicionar o tempo de execução do EF5 ao seu projeto. Depois de adicionar o EF5, você ainda verá a tela com a opção EF6 desabilitada.

Se você tiver uma versão EF4. x do tempo de execução já instalada, verá essa versão do EF listada na tela em vez de EF5. Nessa situação, você pode atualizar para o EF5 usando as seguintes etapas:

1.  Selecione **ferramentas –&gt; Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**
2.  Executar **install-Package EntityFramework-Version 5.0.0**

 

## <a name="targeting-ef4x"></a>Direcionando EF4. x

Você pode instalar o tempo de execução do EF4. x em seu projeto usando as seguintes etapas:

1.  Selecione **ferramentas –&gt; Gerenciador de pacotes de biblioteca-&gt; console do Gerenciador de pacotes**
2.  Executar **install-Package EntityFramework-versão 4.3.0**
