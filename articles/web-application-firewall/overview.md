---
title: Introdução ao Firewall do Aplicativo Web do Azure
description: Este artigo fornece uma visão geral do Azure WAF (Firewall do Aplicativo Web)
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 03/06/2020
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 44bc8db5d8ada0378f8f9d0911ed398ba491d289
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851195"
---
# <a name="what-is-azure-web-application-firewall"></a>O que é o Firewall do Aplicativo Web do Azure?

O WAF (Firewall do Aplicativo Web) fornece proteção centralizada de seus aplicativos Web contra vulnerabilidades e explorações comuns. Os aplicativos Web cada vez mais são alvos de ataques mal-intencionados que exploram vulnerabilidades conhecidas comuns. A injeção de SQL e o script entre sites estão entre os ataques mais comuns.

![Visão geral do WAF](media/overview/wafoverview.png)

A prevenção de tais ataques no código do aplicativo é desafiadora. Isso pode exigir manutenção rigorosa, aplicação de patches e monitoramento em várias camadas da topologia do aplicativo. Um firewall de aplicativo Web centralizado ajuda a tornar o gerenciamento de segurança muito mais simples. Um WAF também oferece aos administradores de aplicativos melhor garantia de proteção contra ameaças e invasões.

Uma solução WAF pode reagir a uma ameaça de segurança mais rapidamente, corrigindo uma vulnerabilidade conhecida de modo central, em vez de proteger cada um dos aplicativos Web individuais.

## <a name="supported-services"></a>Serviços com suporte

O WAF pode ser implantado com o [Gateway de Aplicativo do Azure](../application-gateway/overview.md) e o [Azure Front Door Service](../frontdoor/front-door-overview.md). Os dois serviços são balanceadores de carga de Camada 7 (HTTP/S), mas o Gateway de Aplicativo é um serviço regional, e o Front Door é um serviço global. O WAF tem recursos que são personalizados para cada serviço específico.

Para obter mais informações, confira a visão geral do WAF para cada serviço.

## <a name="next-steps"></a>Próximas etapas

- Para obter mais informações sobre o Firewall do Aplicativo Web no Gateway de Aplicativo, confira [Firewall de Aplicativo Web no Gateway de Aplicativo do Azure](./ag/ag-overview.md).
- Para obter mais informações sobre o firewall do aplicativo Web no Azure Front Door Service, confira [firewall do aplicativo Web no Azure Front Door Service](./afds/afds-overview.md).
