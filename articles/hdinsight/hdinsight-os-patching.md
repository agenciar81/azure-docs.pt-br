---
title: Configurar agendamento de aplicação de patch de so para clusters do Azure HDInsight
description: Saiba como configurar o agendamento de aplicação de patch no SO para clusters HDInsight baseados em Linux.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: f8e694f658d6e9de04c92001214ecd5c32ff7753
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206853"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Configurar o agendamento de aplicação de patch do so para clusters HDInsight baseados em Linux

> [!IMPORTANT]
> As imagens do Ubuntu são disponibilizadas para a nova criação de cluster do Azure HDInsight dentro de três meses após a publicação. A partir de janeiro de 2019, os clusters em execução não são automaticamente corrigidos. Os clientes devem usar ações de script ou outros mecanismos para aplicar o patch de um cluster em execução. Clusters recém-criados sempre terão as atualizações mais recentes disponíveis, incluindo os mais recentes patches de segurança.

O HDInsight fornece suporte para que você execute tarefas comuns em seu cluster, como instalação de patches do sistema operacional, atualizações de segurança e nós de reinicialização. Essas tarefas são realizadas usando os dois scripts a seguir que podem ser executados como [ações de script](hdinsight-hadoop-customize-cluster-linux.md)e configurados com parâmetros:

- `schedule-reboots.sh`-faça uma reinicialização imediata ou agende uma reinicialização nos nós do cluster.
- `install-updates-schedule-reboots.sh`-instalar todas as atualizações, apenas as atualizações de kernel + segurança ou apenas as atualizações de kernel.

> [!NOTE]  
> As ações de script não aplicarão atualizações automaticamente para todos os ciclos de atualização futuros. Execute os scripts sempre que novas atualizações devem ser aplicadas para instalar as atualizações e reinicie a VM.

## <a name="preparation"></a>Preparação

Patch em um ambiente de não produção representativo antes da implantação na produção. Desenvolva um plano para testar adequadamente seu sistema antes de sua aplicação de patch real.

De tempos em tempos, de uma sessão SSH com o cluster, você pode receber uma mensagem informando que uma atualização está disponível. A mensagem pode ser semelhante a:

```
New release '18.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade it
```

A aplicação de patch é opcional e a seu critério.

## <a name="restart-nodes"></a>Reinicializar nós
  
A agenda de script [– reinicializa](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh), define o tipo de reinicialização que será executada nos computadores no cluster. Ao enviar a ação de script, defina-a para aplicar em todos os três tipos de nó: nó de cabeçalho, nó de trabalho e Zookeeper. Se o script não for aplicado a um tipo de nó, as VMs desse tipo de nó não serão atualizadas ou reiniciadas.

O `schedule-reboots script` aceita um parâmetro numérico:

| Parâmetro | Valores aceitos | Definição |
| --- | --- | --- |
| Tipo de reinicialização a ser executada | 1 ou 2 | Um valor de 1 Habilita A reinicialização da agenda (agendada em 12-24 horas). Um valor de 2 habilita A reinicialização imediata (em 5 minutos). Se nenhum parâmetro for fornecido, o padrão será 1. |  

## <a name="install-updates-and-restart-nodes"></a>Instalar atualizações e reiniciar nós

O script [install-updates-Schedule-reboots.sh](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) fornece opções para instalar diferentes tipos de atualizações e reiniciar a VM.

O script de `install-updates-schedule-reboots` aceita dois parâmetros numéricos, conforme descrito na tabela a seguir:

| Parâmetro | Valores aceitos | Definição |
| --- | --- | --- |
| Tipo de atualizações a serem instaladas | 0, 1 ou 2 | O valor 0 instala apenas as atualizações do kernel. Um valor de 1 instala todas as atualizações e 2 instala apenas as atualizações de kernel + segurança. Se nenhum parâmetro for fornecido, o padrão será 0. |
| Tipo de reinicialização a ser executada | 0, 1 ou 2 | Um valor de 0 desabilita A reinicialização. Um valor de 1 Habilita A reinicialização da agenda e 2 habilita a reinicialização imediata. Se nenhum parâmetro for fornecido, o padrão será 0. O usuário deve alterar o parâmetro de entrada 1 para o parâmetro de entrada 2. |

> [!NOTE]
> Você deve marcar um script como persistente depois de aplicá-lo a um cluster existente. Caso contrário, quaisquer nós novos criado por meio de operações de dimensionamento usarão o agendamento de aplicação de patch padrão. Se você aplicar o script como parte do processo de criação do cluster, ele será persistido automaticamente.

## <a name="next-steps"></a>Próximas etapas

Para obter etapas específicas sobre como usar as ações de script, consulte as seções a seguir em [Personalizar clusters HDInsight baseados em Linux usando a ação de script](hdinsight-hadoop-customize-cluster-linux.md):

- [Usar uma ação de script durante a criação do cluster](hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)
- [Aplicar uma ação de script em um cluster em execução](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
