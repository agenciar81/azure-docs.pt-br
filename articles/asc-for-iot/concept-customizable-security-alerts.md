---
title: Guia de alerta de segurança personalizável para a central de segurança do Azure para IoT | Microsoft Docs
description: Saiba mais sobre os alertas de segurança personalizáveis e a correção recomendada usando a central de segurança do Azure para recursos e serviços de IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2020
ms.author: mlottner
ms.openlocfilehash: 32d79267559480de8ec91e5e66196bfd57e01cf1
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78299565"
---
# <a name="azure-security-center-for-iot-security-alerts"></a>Central de segurança do Azure para alertas de segurança de IoT

A central de segurança do Azure para IoT analisa continuamente sua solução de IoT usando a análise avançada e a inteligência contra ameaças para alertá-lo sobre atividades mal-intencionadas.

Incentivamos você a criar alertas personalizados com base em seu conhecimento do comportamento de dispositivo esperado para garantir que os alertas atuem como os indicadores mais eficientes de comprometimento potencial em sua implantação e cenário de organização exclusivos. 

A lista a seguir da central de segurança do Azure para alertas de IoT é definida por você com base no comportamento esperado do Hub IoT e/ou do dispositivo. Para obter mais detalhes sobre como personalizar cada alerta, consulte [criar alertas personalizados](quickstart-create-custom-alerts.md).

## <a name="iot-hub-alerts-available-for-customization"></a>Alertas do Hub IoT disponíveis para personalização 



| Severity | Nome do alerta | Fonte de dados | Descrição | Correção sugerida|
|---|---|---|---|---|
| Baixo      | Alerta personalizado – o número de mensagens da nuvem para o dispositivo no protocolo AMQP está fora do intervalo permitido          | Hub IoT     | O número de mensagens de nuvem para dispositivo (protocolo AMQP) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.||
| Baixo      | Alerta personalizado – o número de mensagens da nuvem para o dispositivo rejeitadas no protocolo AMQP está fora do intervalo permitido | Hub IoT     | O número de mensagens de nuvem para dispositivo (protocolo AMQP) rejeitadas pelo dispositivo em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.||
| Baixo      | Alerta personalizado – o número de mensagens do dispositivo para a nuvem no protocolo AMQP está fora do intervalo permitido      | Hub IoT     | O número de mensagens do dispositivo para a nuvem (protocolo AMQP) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|   |
| Baixo      | Alerta personalizado – o número de invocações de método direto está fora do intervalo permitido | Hub IoT     | O número de invocações de método direto em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.||
| Baixo      | Alerta personalizado – o número de uploads de arquivos está fora do intervalo permitido | Hub IoT     | O número de uploads de arquivo em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.| |
| Baixo      | Alerta personalizado – o número de mensagens da nuvem para o dispositivo no protocolo HTTP está fora do intervalo permitido | Hub IoT     | A quantidade de mensagens da nuvem para o dispositivo (protocolo HTTP) em uma janela de tempo não está no intervalo configurado permitido                                  |
| Baixo      | Alerta Personalizado – o número de mensagens da nuvem para o dispositivo rejeitadas no protocolo HTTP está fora do intervalo permitido | Hub IoT     | O número de mensagens da nuvem para o dispositivo (protocolo HTTP) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido. |
| Baixo      | Alerta personalizado – o número de mensagens do dispositivo para a nuvem no protocolo HTTP está fora do intervalo permitido | Hub IoT| O número de mensagens do dispositivo para a nuvem (protocolo HTTP) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|    |
| Baixo      | Alerta personalizado – o número de mensagens da nuvem para o dispositivo no protocolo MQTT está fora do intervalo permitido | Hub IoT     | O número de mensagens da nuvem para o dispositivo (protocolo MQTT) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|   |
| Baixo      | Alerta personalizado – o número de mensagens rejeitadas da nuvem para o dispositivo no protocolo MQTT está fora do intervalo permitido | Hub IoT     | O número de mensagens de nuvem para dispositivo (protocolo MQTT) rejeitadas pelo dispositivo em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido. |
| Baixo      | Alerta personalizado – o número de mensagens do dispositivo para a nuvem no protocolo MQTT está fora do intervalo permitido          | Hub IoT     | O número de mensagens do dispositivo para a nuvem (protocolo MQTT) em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|
| Baixo      | Alerta personalizado – o número de limpezas de filas de comando está fora do intervalo permitido                               | Hub IoT     | O número de limpezas de fila de comando em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.||
| Baixo      | Alerta personalizado – o número de atualizações de módulos gêmeos está fora do intervalo permitido                                       | Hub IoT     | O número de atualizações de módulo gêmeo em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|
| Baixo      | Alerta personalizado – o número de operações não autorizadas está fora do intervalo permitido  | Hub IoT     | O número de operações não autorizadas em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|
|

## <a name="agent-alerts-available-for-customization"></a>Alertas de agente disponíveis para personalização 

| Severity | Nome do alerta | Fonte de dados | Descrição | Correção sugerida|
|---|---|---|---|---|
| Baixo      | Alerta poersonalizado – o número de conexões ativas está fora do intervalo permitido  | Agente do       | O número de conexões ativas em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido.|  Investigue os logs do dispositivo. Saiba onde a conexão foi originada e determine se ela é benigna ou mal-intencionada. Se for mal-intencionada, remova o possível malware e entenda a origem. Se for benigna, adicione a origem à lista de conexões permitidas.  |
| Baixo      | Alerta personalizado – uma conexão de saída foi criada para um IP que não é permitido                             | Agente do       | Uma conexão de saída foi criada para um IP que está fora da sua lista de IPs permitidos. |Investigue os logs do dispositivo. Saiba onde a conexão foi originada e determine se ela é benigna ou mal-intencionada. Se for mal-intencionada, remova o possível malware e entenda a origem. Se for benigna, adicione a origem à lista de IPs permitidos.                        |
| Baixo      | Alerta personalizado – o número de logons locais com falha está fora do intervalo permitido                               | Agente do       | O número de logons locais com falha em uma janela de tempo específica está fora do intervalo atualmente configurado e permitido. |   |
| Baixo      | Alerta personalizado – logon de um usuário que não está na lista de usuários permitidos | Agente do       | Um usuário local fora da lista de usuários permitidos, conectado ao dispositivo.|  Se você estiver salvando dados brutos, navegue até sua conta da análise de logs e use os dados para investigar o dispositivo, identifique a origem e, em seguida, corrija a lista de permissões/bloqueios para essas configurações. Se você não estiver salvando dados brutos no momento, vá para o dispositivo e corrija a lista de permissões/bloqueios para essas configurações.|
| Baixo      | Alerta personalizado – um processo não permitido foi executado | Agente do       | Um processo não permitido foi executado no dispositivo. |Se você estiver salvando dados brutos, navegue até sua conta da análise de logs e use os dados para investigar o dispositivo, identifique a origem e, em seguida, corrija a lista de permissões/bloqueios para essas configurações. Se você não estiver salvando dados brutos no momento, vá para o dispositivo e corrija a lista de permissões/bloqueios para essas configurações.  |
|

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- Saiba como [Personalizar um alerta](quickstart-create-custom-alerts.md)
- [Visão geral](overview.md) da central de segurança do Azure para serviços de IOT
- Saiba como [acessar seus dados de segurança](how-to-security-data-access.md)
- Saiba mais sobre [a investigação de um dispositivo](how-to-investigate-device.md)