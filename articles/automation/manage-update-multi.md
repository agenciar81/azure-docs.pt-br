---
title: Gerenciar atualizações para várias máquinas virtuais do Azure
description: Este artigo descreve como gerenciar atualizações para máquinas virtuais do Azure e não Azure.
services: automation
ms.subservice: update-management
ms.date: 01/16/2020
ms.topic: conceptual
ms.openlocfilehash: de7171d3807540ae7d5f09c3a877031631248e49
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76168044"
---
# <a name="manage-updates-for-multiple-machines"></a>Gerenciar atualizações de vários computadores

É possível usar a solução Gerenciamento de Atualizações para gerenciar atualizações e patches para as máquinas virtuais do Windows e Linux. A partir da sua conta da [Automação do Azure](automation-offering-get-started.md), é possível:

- Integrar máquinas virtuais
- Avaliar o status das atualizações disponíveis
- Agendar a instalação de atualizações necessárias
- Examinar os resultados de implantação para verificar se as atualizações foram aplicadas com êxito em todas as máquinas virtuais nas quais o Gerenciamento de Atualizações está habilitado

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Gerenciamento de Atualizações, você precisa de:

- Uma máquina virtual ou um computador com um dos sistemas operacionais com suporte instalado.

- Acesso a um repositório de atualizações para VMs do Linux integradas à solução.

## <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis

O Gerenciamento de Atualizações dá suporte aos seguintes sistemas operacionais:

|Sistema operacional  |Observações  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Suporta apenas avaliações de atualização.         |
|Windows Server 2008 R2 SP1 e posterior     |Windows PowerShell 4.0 ou posterior é necessário. ([Baixar WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1é recomendado para maior confiabilidade. ([Baixar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) e 7 (x64)      | |
|Red Hat Enterprise 6 (x86/x64) e 7 (x64)     | |
|SUSE Linux Enterprise Server 11 (x86/x64) e 12 (x64)     | |
|Ubuntu 14.04 LTS, 16.04 LTS e 18.04 LTS (x86/x64)      | |

> [!NOTE]
> Para prevenir que atualizações sejam aplicadas fora de uma janela de manutenção no Ubuntu, reconfigure o pacote de atualização automática para desabilitar as atualizações automáticas. Para obter mais informações, consulte o [tópico de Atualizações automáticas no Guia do servidor Ubuntu](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

## <a name="enable-update-management-for-azure-virtual-machines"></a>Habilitar o Gerenciamento de Atualizações para máquinas virtuais do Azure

No portal do Azure, abra a conta de Automação e selecione **Gerenciamento de Atualizações**.

Selecione **Adicionar VMs do Azure**.

![Adicione tab Azure VM](./media/manage-update-multi/update-onboard-vm.png)

Selecione uma máquina virtual a ser carregada.

Em **Habilitar Gerenciamento de Atualizações**, selecione **Habilitar** para integrar a máquina virtual.

![Caixa de diálogo Habilitar Gerenciamento de Atualizações](./media/manage-update-multi/update-enable.png)

Quando a integração é concluída, o Gerenciamento de Atualizações é habilitado para a máquina virtual.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Habilitar o Gerenciamento de Atualizações para computadores e máquinas virtuais que não são Azure

O agente de Log Analytics para Windows e Linux precisa ser instalado nas VMs que estão em execução na sua rede corporativa ou em outro ambiente de nuvem para habilitá-las com Gerenciamento de Atualizações. Para saber os requisitos do sistema e os métodos com suporte para implantar o agente em computadores hospedados fora do Azure, consulte [visão geral do agente de log Analytics](../azure-monitor/platform/log-analytics-agent.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Exibir computadores conectados à sua conta de Automação

Após habilitar o Gerenciamento de Atualizações para as máquinas, você poderá exibir as informações da máquina, selecionando **Computadores**. É possível exibir as informações sobre *nome do computador*, *status de conformidade*, *ambiente*, *tipo de SO*, *atualizações críticas e de segurança instaladas*, *outras atualizações instaladas* e *preparação para atualização do agente* dos computadores.

  ![Guia Exibir computadores](./media/manage-update-multi/update-computers-tab.png)

Computadores que foram habilitados recentemente para o Gerenciamento de Atualizações podem ainda não terem sido avaliados. O status do estado de conformidade para esses computadores é **Não avaliado**. Aqui, é apresentada uma lista de possíveis valores para o estado de conformidade:

- **Compatível**: computadores com todas as atualizações críticas ou de segurança.

- **Não compatível**: computadores que não têm pelo menos uma atualização crítica ou de segurança.

- **Não avaliado**: os dados da avaliação de atualização não foram recebidos do computador dentro do período de tempo esperado. Para computadores Linux, o tempo de espera esperado é na última hora. Para computadores Windows, o período de tempo esperado é nas últimas 12 horas.

Para exibir o status do agente, selecione o link na coluna **PREPARAÇÃO PARA ATUALIZAÇÃO DO AGENTE**. Selecionar essa opção abre o painel **Hybrid Worker** e exibe o status do Hybrid Worker. A imagem a seguir mostra um exemplo de um agente que não foi conectado ao Gerenciamento de Atualizações por um longo período de tempo:

![Guia Exibir computadores](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Exibir uma avaliação de atualização

Depois que o Gerenciamento de Atualizações for habilitado, o painel **Gerenciamento de Atualizações** será aberto. Você pode ver uma lista de atualizações ausentes na guia **Atualizações ausentes**.

## <a name="collect-data"></a>Coletar dados

Os agentes instalados em máquinas virtuais e computadores coletam dados sobre atualizações. Os agentes enviam os dados para o Gerenciamento de Atualizações do Azure.

### <a name="supported-agents"></a>Agentes com suporte

A tabela a seguir descreve as fontes conectadas às quais essa solução dá suporte:

| Fonte conectada | Com suporte | Description |
| --- | --- | --- |
| Agentes do Windows |Sim |O Gerenciamento de Atualizações coleta informações sobre atualizações do sistema de agentes do Windows e, em seguida, inicia a instalação das atualizações necessárias. |
| Agentes do Linux |Sim |O Gerenciamento de Atualizações coleta informações sobre atualizações do sistema de agentes para Linux e, em seguida, inicia a instalação das atualizações necessárias nas distribuições com suporte. |
| Grupo de gerenciamento do Operations Manager |Sim |O Gerenciamento de Atualizações coleta informações sobre atualizações do sistema de agentes em um grupo de gerenciamento conectado. |
| Conta de Armazenamento do Azure |Não |O Armazenamento do Microsoft Azure não inclui informações sobre atualizações do sistema. |

### <a name="collection-frequency"></a>Frequência de coleta

Depois que um computador conclui uma verificação de conformidade de atualização, o agente encaminha as informações em massa para Azure Monitor logs. Em um computador Windows, a verificação de conformidade é executada a cada 12 horas por padrão.

Além do agendamento da verificação, a verificação de conformidade de atualizações será iniciada em 15 minutos se o MMA estiver sendo reiniciado antes da instalação da atualização e após a instalação da atualização.

Para um computador Linux, a verificação de conformidade é executada a cada hora por padrão. Se o agente MMA for reiniciado, uma verificação de conformidade é iniciada dentro de 15 minutos.

Pode demorar entre 30 minutos e 6 horas para o painel exibir os dados atualizados dos computadores gerenciados.

## <a name="schedule-an-update-deployment"></a>Agendar uma implantação de atualização

Para instalar atualizações, agende uma implantação que alinhe-se com a agenda de liberação e período de serviço. Você pode escolher quais tipos de atualização deseja incluir na implantação. Por exemplo, você pode incluir atualizações críticas ou de segurança e excluir pacotes cumulativos de atualizações.

>[!NOTE]
>Quando você agenda uma implantação de atualização, ela cria um recurso de [agendamento](shared-resources/schedules.md) vinculado ao runbook **Patch-MicrosoftOMSComputers** que processa a implantação de atualização nos computadores de destino. Se você excluir o recurso de agendamento no portal do Azure ou usando o PowerShell depois de criar a implantação, ele interromperá a implantação de atualização agendada e apresentará um erro quando você tentar reconfigurá-la no portal. Você só pode excluir o recurso de agendamento excluindo o agendamento de implantação correspondente.
>

Para agendar uma nova implantação de atualização para uma ou mais máquinas virtuais, em **Gerenciamento de Atualizações**, selecione **Agendar implantação de atualizações**.

No painel **Nova implantação de atualização**, especifique as seguintes informações:

- **Nome**: insira um nome exclusivo para identificar a implantação de atualizações.
- **Sistema operacional**: selecione **Windows** ou **Linux**.
- **Grupos a serem atualizados**: defina uma consulta com base em uma combinação de assinatura, grupos de recursos, locais e marcas para criar um grupo dinâmico de VMs do Azure para incluir em sua implantação. Para VMs não Azure, as pesquisas salvas são usadas para criar um grupo dinâmico para incluir em sua implantação. Para saber mais, consulte [grupos dinâmicos](automation-update-management-groups.md).
- **Máquinas para atualizar**: Selecione uma pesquisa salva, um grupo importado ou selecione Máquinas, para escolher as máquinas que você deseja atualizar.

   >[!NOTE]
   >A seleção da opção de pesquisa salva não retorna identidades de computador, apenas seus nomes. Se você tiver várias VMs com o mesmo nome em vários grupos de recursos, elas serão retornadas nos resultados. O uso da opção **grupos a serem atualizados** é recomendado para garantir que você inclua VMs exclusivas que correspondam aos seus critérios.

   Se você escolher **Machines**, a prontidão da máquina é mostrada na coluna **UPDATE AGENT READINESS**. É possível ver o estado de integridade do computador antes de agendar a implantação de atualização. Para saber mais sobre os diferentes métodos de criação de grupos de computadores nos logs do Azure Monitor, veja [Grupos de computadores nos logs do Azure Monitor](../azure-monitor/platform/computer-groups.md)

  ![Painel da nova implantação de atualizações](./media/manage-update-multi/update-select-computers.png)

- **Classificação da atualização**: selecione os tipos de software para incluir na implantação de atualização. Para obter uma descrição dos tipos de classificação, consulte [Classificações de atualização](automation-view-update-assessments.md#update-classifications). Os tipos de classificação são:
  - Atualizações críticas
  - Atualizações de segurança
  - Pacotes cumulativos de atualização
  - Feature packs
  - Service packs
  - Atualizações de definição
  - Ferramentas
  - Atualizações

- **Atualizações a serem incluídas/excluídas** – Isso abre a página **Incluir/Excluir**. As atualizações a serem incluídas ou excluídas estão em guias separadas. Para obter informações adicionais sobre como a inclusão é tratada, consulte [agendar uma implantação de atualização](automation-tutorial-update-management.md#schedule-an-update-deployment).

> [!NOTE]
> É importante saber que as exclusões substituem as inclusões. Por exemplo, se você definir uma regra de exclusão de `*`, nenhuma correção ou pacote será instalado, pois todos serão excluídos. Correções excluídas ainda são mostradas como ausentes do computador. Para computadores Linux, se um pacote estiver incluído, mas tiver um pacote dependente excluído, o pacote não será instalado.

> [!NOTE]
> Você não pode especificar atualizações que foram substituídas para inclusão na implantação da atualização.
>

- **Configurações de agenda**: você pode aceitar a data e hora padrão, que é de 30 minutos após a hora atual. Também é possível especificar uma hora diferente.

   Você também pode especificar se a implantação ocorre uma única vez ou em um agendamento recorrente. Para configurar um agendamento recorrente, em **Recorrente**, selecione **Recorrência**.

   ![Caixa de diálogo Configurações de agendamento](./media/manage-update-multi/update-set-schedule.png)

- **Pré-scripts + pós-scripts**: selecione os scripts a serem executados antes e após sua implantação. Para saber mais, consulte [Gerenciar pré e pós-scripts](pre-post-scripts.md).
- **Janela de manutenção (minutos)** : especifique o período de tempo em que deseja que a implantação de atualização ocorra. Essa configuração ajuda a garantir que as alterações sejam executadas dentro das janelas de serviço definidas.

- **Reinicialize o controle** -essa configuração determina como as reinicializações são tratadas para a implantação de atualização.

   |Opção|Description|
   |---|---|
   |Reinicialização, se necessário| **(Padrão)** Se necessário, uma reinicialização será iniciada se a janela de manutenção permitir.|
   |Sempre reinicializar|Uma reinicialização for iniciada, independentemente se é necessário. |
   |Nunca reinicializar|Independentemente de uma reinicialização ser ou não necessária, as reinicializações são suprimidas.|
   |Somente reinicialização - não instalará as atualizações|Essa opção ignora a instalação de atualizações e apenas inicia uma reinicialização.|

Ao terminar de configurar o agendamento, selecione o botão **Criar** para retornar ao painel de status. A tabela **Agendado** mostra o agendamento de implantação que você criou.

> [!NOTE]
> O Gerenciamento de Atualizações dá suporte à implantação de atualizações próprias e patches baixados previamente. Isso requer alterações nos sistemas a corrigir, consulte [suporte próprio e de pré-download](automation-configure-windows-update.md#pre-download-updates) para saber como definir essas configurações em seus sistemas.

## <a name="view-results-of-an-update-deployment"></a>Exibir resultados de uma implantação de atualização

Após o início da implantação agendada, você pode ver o status dessa implantação na guia **Implantações de atualização** em **Gerenciamento de Atualizações**.

Se a implantação estiver em execução no momento, seu status será **Em andamento**. Depois que a implementação for concluída com êxito, o status será alterado para **Êxito**.

Se uma ou mais atualizações falharem na implantação, o status será **Falhou parcialmente**.

![Status da implantação da atualização](./media/manage-update-multi/update-view-results.png)

Para ver o painel de uma implantação de atualização, selecione a implantação concluída.

O painel **Resultados da atualização** mostra o número total de atualizações e os resultados da implantação para a máquina virtual. A tabela à direita fornece uma análise detalhada de cada atualização e os resultados da instalação. Os resultados de instalação podem ser um dos seguintes valores:

- **Nenhuma tentativa**: a atualização não foi instalada pois não havia tempo suficiente disponível com base na janela de manutenção definida.
- **Êxito**: a atualização foi bem-sucedida.
- **Falha**: a atualização falhou.

Para ver todas as entradas de log que a implantação criou, selecione **Todos os logs**.

Para ver o fluxo de trabalho do runbook que gerencia a implantação de atualização na máquina virtual de destino, selecione o bloco de saída.

Para ver informações detalhadas sobre quaisquer erros da implantação, selecione **Erros**.

## <a name="next-steps"></a>Próximos passos

Para saber mais sobre o Gerenciamento de Atualizações, incluindo logs, saída e erros, consulte [Solução Gerenciamento de Atualizações no Azure](../operations-management-suite/oms-solution-update-management.md).

