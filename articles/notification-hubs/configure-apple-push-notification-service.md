---
title: Configurar Apple Push Notification Service nos hubs de notificação do Azure | Microsoft Docs
description: Saiba como configurar um hub de notificação do Azure com configurações de Apple Push Notification Service (APNS).
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 116af5192236045ec50409f65a9687ffec5223b5
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406129"
---
# <a name="configure-apple-push-notification-service-apns-settings-for-a-notification-hub-in-the-azure-portal"></a>Definir configurações de Apple Push Notification Service (APNS) para um hub de notificação no portal do Azure
Este artigo mostra como definir as configurações de Apple Push Notification Service (APNS) para um hub de notificação do Azure usando o portal do Azure. 

## <a name="prerequisites"></a>pré-requisitos
Se você ainda não criou um hub de notificação, crie um agora. Para obter mais informações, consulte [Criar um hub de notificação do Azure no portal do Azure](create-notification-hub-portal.md). 

## <a name="configure-apple-push-notification-service"></a>Configurar Apple Push Notification Service

O procedimento a seguir fornece as etapas para definir as configurações de Apple Push Notification Service (APNS) para um hub de notificação:

1. Na portal do Azure, na página **Hub de notificação** , selecione **Apple (APNS)** no menu à esquerda.

1. Para o **Modo de Autenticação**, selecione **Certificado** ou **Token**.

   a. Se você selecionar **Certificado**:
   * Selecione o ícone de arquivo, e em seguida, o arquivo *.p12* que você deseja carregar.
   * Digite uma senha.
   * Selecione o modo **Sandbox**. Ou, para enviar notificação por push aos usuários que compraram seu aplicativo da loja, selecione o modo **Produção**.

     ![Captura de tela de uma configuração de certificado APN no portal do Azure](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   b. Se você selecionar **Token**:

   * Insira os valores de **Identificação da chave**, **ID do pacote**, **ID da equipe** e **Token**.
   * Selecione o modo **Sandbox**. Ou, para enviar notificação por push aos usuários que compraram seu aplicativo da loja, selecione o modo **Produção**.

     ![Captura de tela de uma configuração de token APNS no portal do Azure](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Próximas etapas
Para obter um tutorial de instruções passo a passo para enviar notificações para dispositivos iOS, consulte o seguinte artigo: [notificações por push para dispositivos IOS usando hubs de notificação e APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
