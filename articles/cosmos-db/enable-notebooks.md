---
title: Habilitar blocos de anotações na conta de Azure Cosmos DB (versão prévia)
description: Os blocos de anotações internos do Azure Cosmos DB permitem que você analise e visualize seus dados de dentro do Portal. Este artigo descreve como habilitar esse recurso para contas do cosmos.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/22/2019
ms.author: dech
ms.openlocfilehash: dcec310db43baa513b2d574d03f3f35dee3f773b
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76768011"
---
# <a name="enable-notebooks-for-azure-cosmos-db-accounts-preview"></a>Habilitar blocos de anotações para contas de Azure Cosmos DB (versão prévia)

> [!IMPORTANT]
> Os blocos de anotações internos para Azure Cosmos DB estão disponíveis atualmente nas seguintes regiões do Azure: leste da Austrália, leste dos EUA, leste dos EUA 2, Europa Setentrional, Sul EUA Central, Sudeste Asiático, Sul do Reino Unido, Europa Ocidental e oeste dos EUA 2. Para usar blocos de anotações, [crie uma nova conta com blocos de anotações](#enable-notebooks-in-a-new-cosmos-account) ou [habilite blocos de anotações em uma conta existente](#enable-notebooks-in-an-existing-cosmos-account) em uma dessas regiões.

Os blocos de anotações internos do Jupyter no Azure Cosmos DB permitem que você analise e visualize os dados do portal do Azure. Este artigo descreve como habilitar esse recurso para sua conta do Azure Cosmos DB.

## <a name="enable-notebooks-in-a-new-cosmos-account"></a>Habilitar blocos de anotações em uma nova conta do cosmos
1. Faça logon no [Portal do Azure](https://portal.azure.com/).
1. Selecione **Criar um recurso** > **Bancos de dados** > **Azure Cosmos DB**.
1. Na página **criar conta de Azure Cosmos DB** , selecione **notebooks**. 
 
    ![Opção selecionar blocos de anotações na folha Azure Cosmos DB criar](media/enable-notebooks/create-new-account-with-notebooks.png)
1. Selecione **Examinar + criar**. Você pode ignorar a opção de **rede** e **marcas** . 
1. Examine as configurações da conta e selecione **Criar**. São necessários alguns minutos para criar a conta. Aguarde até que a página do portal exiba **Sua implantação está concluída**. 

    ![O painel Notificações do portal do Azure](media/enable-notebooks/create-new-account-with-notebooks-complete.png)
1. Selecione **Ir para recurso** para ir para a página da conta do Azure Cosmos DB. 

    ![A página da conta do Azure Cosmos DB](../../includes/media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-3.png)

1. Navegue até o painel de **Data Explorer** . Agora você deve ver seu espaço de trabalho de blocos de anotações.

    ![Espaço de trabalho novo Azure Cosmos DB blocos de anotações](media/enable-notebooks/new-notebooks-workspace.png)

## <a name="enable-notebooks-in-an-existing-cosmos-account"></a>Habilitar blocos de anotações em uma conta existente do cosmos
Você também pode habilitar blocos de anotações em contas existentes. Esta etapa precisa ser feita apenas uma vez por conta.

1. Navegue até o painel de **Data Explorer** em sua conta do cosmos.
1. Selecione **habilitar blocos de anotações**.

    ![Criar um novo espaço de trabalho de blocos de anotações no Data Explorer](media/enable-notebooks/enable-notebooks-workspace.png)
1. Isso solicitará que você crie um novo espaço de trabalho de blocos de anotações. Selecione **concluir configuração.**
1. Sua conta agora está habilitada para usar blocos de anotações!

## <a name="create-and-run-your-first-notebook"></a>Criar e executar seu primeiro bloco de anotações

Para verificar se você pode usar blocos de anotações, selecione um dos blocos de anotações em blocos de anotações de exemplo. Isso salvará uma cópia do bloco de anotações no espaço de trabalho e o abrirá.

Neste exemplo, usaremos **gettingstarted. ipynb**. 

![Exibir o GettingStarted. ipynb Notebook](media/enable-notebooks/select-getting-started-notebook.png)

Para executar o bloco de anotações:
1. Selecione a primeira célula de código que contém o código Python. 
1. Selecione **executar** para executar a célula. Você também pode usar **Shift + Enter** para executar a célula.
1. Atualize o painel de recursos para ver o banco de dados e o contêiner que foram criados.

    ![Executar o bloco de anotações de introdução](media/enable-notebooks/run-first-notebook-cell.png)

Você também pode selecionar **novo bloco de anotações** para criar um novo bloco de anotações ou carregar um arquivo de bloco de anotações (. ipynb) existente selecionando **carregar arquivo** no menu **meus blocos de anotações** . 

![Criar ou carregar um novo bloco de anotações](media/enable-notebooks/create-or-upload-new-notebook.png)

## <a name="next-steps"></a>Próximos passos

- Saiba mais sobre os benefícios do [Azure Cosmos DB notebooks Jupyter](cosmosdb-jupyter-notebooks.md)
