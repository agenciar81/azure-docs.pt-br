---
title: Validar a conectividade com o Azure Sentinel | Microsoft Docs
description: Valide a conectividade da solução de segurança para garantir que as mensagens CEF sejam encaminhadas para o Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: e224f6d5cfd82dfc6cb1ce107d111ee0e031247b
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588426"
---
# <a name="step-3-validate-connectivity"></a>ETAPA 3: validar a conectividade



Depois de implantar o agente e configurar sua solução de segurança para encaminhar mensagens CEF, use este artigo para entender como verificar a conectividade entre o Azure Sentinel e sua solução de segurança. 

## <a name="how-to-validate-connectivity"></a>Como validar a conectividade

1. Abra Log Analytics para garantir que os logs sejam recebidos usando o esquema CommonSecurityLog.<br> Pode levar até 20 minutos até que os logs comecem a aparecer na Log Analytics. 

1. Antes de executar o script, recomendamos que você envie mensagens de sua solução de segurança para certificar-se de que elas estão sendo encaminhadas para o computador proxy de syslog configurado. 
1. Você deve ter permissões elevadas (sudo) em seu computador. Verifique se você tem o Python em seu computador usando o seguinte comando: `python –version`
1. Execute o script a seguir para verificar a conectividade entre o agente, o Azure Sentinel e sua solução de segurança. Ele verifica se o encaminhamento do daemon está configurado corretamente, escuta as portas corretas e se nada está bloqueando a comunicação entre o daemon e o agente de Log Analytics. O script também envia as mensagens de simulação ' TestCommonEventFormat ' para verificar a conectividade de ponta a ponta. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Próximas etapas
Neste documento, você aprendeu a conectar os dispositivos CEF ao Azure Sentinel. Para saber mais sobre o Azure Sentinel, consulte os seguintes artigos:
- Saiba como [obter visibilidade dos seus dados e possíveis ameaças](quickstart-get-visibility.md).
- Comece a [detectar ameaças com o Azure Sentinel](tutorial-detect-threats.md).
- [Use pastas de trabalho](tutorial-monitor-your-data.md) para monitorar seus dados.

