---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: commercial marketplace
documentationcenter: partner-center-commercial-marketplace
author: qianw211
manager: evansma
editor: ''
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/16/2019
ms.author: v-qiwe
ms.custom: include file
ms.openlocfilehash: 059a2691c8e7905295a65daf262338ece65247d1
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812669"
---
Ao publicar sua oferta no Marketplace por meio do Partner Center, você precisará conectar sua oferta ao seu sistema de gerenciamento de relacionamento com o cliente (CRM) para que possa receber informações de contato do cliente imediatamente depois que um cliente expressar interesse ou implanta seu produto.

1. **Selecione um destino de cliente potencial no qual você deseja que enviemos leads do cliente**. Há suporte para os seguintes sistemas CRM:

    * [Dynamics 365](../commercial-marketplace-lead-management-instructions-dynamics.md) para compromisso com o cliente
    * [Marketo](../commercial-marketplace-lead-management-instructions-marketo.md)
    * [Salesforce](../commercial-marketplace-lead-management-instructions-salesforce.md)

    Se o sistema CRM não tiver suporte explícito na lista acima, você terá as opções a seguir que permitem armazenar os dados de Lead do cliente e, em seguida, poderá exportar ou importar esses dados para o sistema CRM.

    * [Tabela do Azure](../commercial-marketplace-lead-management-instructions-azure-table.md)
    * [Ponto de extremidade https](../commercial-marketplace-lead-management-instructions-https.md)

2. Leia a documentação vinculada acima ao destino do cliente potencial selecionado para ver como configurar o destino do cliente potencial para receber clientes potenciais de sua oferta do Marketplace. 
3. Conecte sua oferta ao destino do cliente potencial ao publicar a oferta no Marketplace no Partner Center. Consulte a documentação vinculada acima para saber como fazer isso.
4. Confirme se a conexão com o destino do cliente potencial está configurada corretamente. Depois de configurar o destino de Lead corretamente e clicar em publicar em sua oferta no Partner Center, validaremos a conexão e enviaremos a você um líder de teste. Quando você exibe a oferta antes de entrar no ar, pode também testar a conexão do lead tentando adquirir a oferta no ambiente de visualização. 
5. Certifique-se de que a conexão com o destino do cliente potencial permaneça atualizada para que você não perca nenhum cliente potencial, portanto, atualize essas conexões sempre que algo tiver mudado no final.
