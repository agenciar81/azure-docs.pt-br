---
title: Soluções VMware do Azure (AVS) – escolha uma solução de balanceamento de carga para nuvens privadas de AVS
description: Descreve as opções de balanceamento de carga implantando um aplicativo em uma nuvem privada de AVS
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 405bc9d95b8d82e2181e2fb828d6bcc00c8c4639
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014871"
---
# <a name="choose-a-load-balancing-solution-for-avs-private-clouds"></a>Escolher uma solução de balanceamento de carga para nuvens privadas da AVS

Ao implantar um aplicativo em uma nuvem privada de AVS, você pode escolher qualquer uma das várias opções para o balanceamento de carga.

Você pode escolher um balanceador de carga baseado em software ou virtual em sua nuvem privada da AVS ou até mesmo usar o Azure L7 Load Balancer em execução na sua assinatura do Azure para front-end suas VMs de camada da Web em execução na nuvem privada da AVS. Aqui, listamos algumas opções:

## <a name="virtual-load-balancers"></a>Balanceadores de carga virtuais

Você pode implantar dispositivos de balanceador de carga virtual em seu ambiente VMware por meio da interface do vCenter e configurá-los para front-end do tráfego do aplicativo.

Alguns fornecedores populares são: NginX: http://nginx.org/en/docs/http/load_balancing.html F5-BigIP-Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC: https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Balanceador de carga L7 do Azure

Ao usar Aplicativo Azure Gateway como um balanceador de carga L7 para seu aplicativo em execução em uma nuvem privada de AVS, você não precisa gerenciar o software do balanceador de carga. O software do balanceador de carga é gerenciado pelo Azure. Todas as VMs da camada da Web na nuvem privada da AVS usam endereços IP privados e não exigem regras NAT adicionais ou endereços IPs públicos para resolver nomes. As VMs da camada da Web se comunicam com o gateway de Aplicativo Azure por uma conexão privada, de baixa latência e de largura de banda alta.

Para saber mais sobre como configurar essa solução, consulte o guia de solução sobre como usar Aplicativo Azure Gateway como um balanceador de carga L7.

## <a name="azure-internal-load-balancer"></a>Balanceador de carga interno do Azure

Se você optar por executar seu aplicativo em uma implantação híbrida em que a camada de front-end da Web está sendo executada em uma vNet do Azure em sua assinatura do Azure e a camada de BD do aplicativo estiver sendo executada em VMs VMware na nuvem privada da AVS, você poderá usar o Azure Load balancer interno (L 4 balanceador de carga) na frente de suas VMs de camada de BD para o gerenciamento de tráfego.

Para saber mais, consulte a documentação de [Load balancer interno](../load-balancer/concepts-limitations.md#internalloadbalancer) do Azure.

## <a name="global-server-load-balancer"></a>Balanceador de carga do servidor global

Se você estiver procurando um balanceador de carga baseado em DNS, poderá usar soluções de terceiros disponíveis no Azure Marketplace ou ir com a solução nativa do Azure.

O Gerenciador de tráfego do Azure é um balanceador de carga de tráfego baseado em DNS que permite distribuir o tráfego de forma ideal para serviços em regiões globais do Azure e locais, fornecendo alta disponibilidade e capacidade de resposta. Para saber mais, consulte a documentação do [Gerenciador de tráfego](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) do Azure.
