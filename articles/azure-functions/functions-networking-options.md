---
title: Opções de rede Azure Functions
description: Uma visão geral de todas as opções de rede disponíveis no Azure Functions.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: d8c3357325eadefec7bb97faba5d600e9c6793a9
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79276706"
---
# <a name="azure-functions-networking-options"></a>Opções de rede Azure Functions

Este artigo descreve os recursos de rede disponíveis nas opções de hospedagem para Azure Functions. Todas as seguintes opções de rede oferecem a você uma capacidade de acessar recursos sem usar endereços roteáveis pela Internet ou para restringir o acesso à Internet a um aplicativo de funções.

Os modelos de hospedagem têm níveis diferentes de isolamento de rede disponíveis. Escolher a correta ajudará você a atender aos seus requisitos de isolamento de rede.

Você pode hospedar aplicativos de funções de duas maneiras:

* Há um conjunto de opções de plano que são executadas em uma infraestrutura multilocatário, com vários níveis de conectividade de rede virtual e opções de dimensionamento:
    * O [plano de consumo](functions-scale.md#consumption-plan), que é dimensionado dinamicamente em resposta à carga e oferece opções de isolamento de rede mínimas.
    * O [plano Premium](functions-scale.md#premium-plan), que também é dimensionado dinamicamente, ao mesmo tempo que oferece isolamento de rede mais abrangente.
    * O [plano do serviço de aplicativo](functions-scale.md#app-service-plan)do Azure, que opera em uma escala fixa e oferece isolamento de rede semelhante ao plano Premium.
* Você pode executar funções em um [ambiente do serviço de aplicativo](../app-service/environment/intro.md). Esse método implanta sua função em sua rede virtual e oferece controle e isolamento de rede total.

## <a name="matrix-of-networking-features"></a>Matriz de recursos de rede

|                |[Plano de consumo](functions-scale.md#consumption-plan)|[Plano Premium](functions-scale.md#premium-plan)|[Plano do Serviço de Aplicativo](functions-scale.md#app-service-plan)|[Ambiente do Serviço de Aplicativo](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Restrições de IP de entrada & acesso ao site privado](#inbound-ip-restrictions)|✅Sim|✅Sim|✅Sim|✅Sim|
|[Integração de rede virtual](#virtual-network-integration)|❌não|✅Sim (regional)|✅Sim (regional e gateway)|✅Sim|
|[Gatilhos de rede virtual (não HTTP)](#virtual-network-triggers-non-http)|❌não| ✅Sim |✅Sim|✅Sim|
|[Conexões híbridas](#hybrid-connections) (somente Windows)|❌não|✅Sim|✅Sim|✅Sim|
|[Restrições de IP de saída](#outbound-ip-restrictions)|❌não| ✅Sim|✅Sim|✅Sim|

## <a name="inbound-ip-restrictions"></a>Restrições de IP de entrada

Você pode usar restrições de IP para definir uma lista ordenada de prioridade de endereços IP que têm acesso permitido ou negado ao seu aplicativo. A lista pode incluir endereços IPv4 e IPv6. Quando há uma ou mais entradas, um "negar tudo" implícito existe no final da lista. As restrições de IP funcionam com todas as opções de Hospedagem de função.

> [!NOTE]
> Com as restrições de rede em vigor, você pode usar o editor do portal somente de dentro de sua rede virtual ou quando você colocou o endereço IP do computador que está usando para acessar o portal do Azure na lista de destinatários seguros. No entanto, você ainda pode acessar quaisquer recursos na guia **recursos da plataforma** de qualquer computador.

Para saber mais, consulte [Azure app restrições de acesso estático do serviço](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Acesso a site particular

Acesso ao site privado refere-se a tornar seu aplicativo acessível somente de uma rede privada, como uma rede virtual do Azure.

* O acesso ao site privado está disponível nos planos [Premium](./functions-premium-plan.md), [consumo](functions-scale.md#consumption-plan)e [serviço de aplicativo](functions-scale.md#app-service-plan) quando os pontos de extremidade de serviço são configurados.
    * Os pontos de extremidade de serviço podem ser configurados por aplicativo em **recursos da plataforma** > **rede** > **Configurar restrições de acesso** > **Adicionar regra**. As redes virtuais agora podem ser selecionadas como um tipo de regra.
    * Para obter mais informações, consulte [pontos de extremidade de serviço de rede virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Tenha em mente que, com os pontos de extremidade de serviço, sua função ainda tem acesso total de saída à Internet, mesmo com a integração de rede virtual configurada.
* O acesso ao site privado também está disponível em um Ambiente do Serviço de Aplicativo configurado com um balanceador de carga interno (ILB). Para obter mais informações, consulte [criar e usar um balanceador de carga interno com um ambiente do serviço de aplicativo](../app-service/environment/create-ilb-ase.md).

Para saber como configurar o acesso ao site privado, consulte [estabelecer Azure Functions acesso ao site privado](functions-create-private-site-access.md).

## <a name="virtual-network-integration"></a>Integração de rede virtual

A integração de rede virtual permite que seu aplicativo de funções acesse recursos dentro de uma rede virtual. O Azure Functions dá suporte a dois tipos de integração de rede virtual:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

A integração de rede virtual no Azure Functions usa a infraestrutura compartilhada com aplicativos Web do serviço de aplicativo. Para saber mais sobre os dois tipos de integração de rede virtual, consulte:

* [Integração de rede virtual regional](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Integração de rede virtual do gateway necessária](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Para saber como configurar a integração de rede virtual, consulte [integrar um aplicativo de funções a uma rede virtual do Azure](functions-create-vnet.md).

## <a name="regional-virtual-network-integration"></a>Integração de rede virtual regional

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connecting-to-service-endpoint-secured-resources"></a>Conectando a recursos protegidos do ponto de extremidade de serviço

Para fornecer um nível mais alto de segurança, você pode restringir vários serviços do Azure a uma rede virtual usando pontos de extremidade de serviço. Em seguida, você deve integrar seu aplicativo de funções com essa rede virtual para acessar o recurso. Essa configuração tem suporte em todos os planos que dão suporte à integração de rede virtual.

[Saiba mais sobre pontos de extremidade de serviço de rede virtual.](../virtual-network/virtual-network-service-endpoints-overview.md)

## <a name="restricting-your-storage-account-to-a-virtual-network"></a>Restringindo sua conta de armazenamento a uma rede virtual

Ao criar um aplicativo de funções, você deve criar ou vincular a uma conta de armazenamento do Azure de uso geral que dá suporte ao armazenamento de BLOBs, filas e tabelas. No momento, não é possível usar nenhuma restrição de rede virtual nesta conta. Se você configurar um ponto de extremidade de serviço de rede virtual na conta de armazenamento que você está usando para seu aplicativo de funções, isso interromperá seu aplicativo.

[Saiba mais sobre os requisitos da conta de armazenamento.](./functions-create-function-app-portal.md#storage-account-requirements)

## <a name="using-key-vault-references"></a>Usando referências de Key Vault 

Key Vault referências permitem que você use segredos de Azure Key Vault em seu aplicativo Azure Functions sem a necessidade de nenhuma alteração de código. Azure Key Vault é um serviço que fornece gerenciamento de segredos centralizado, com controle total sobre políticas de acesso e histórico de auditoria.

No momento, [Key Vault referências](../app-service/app-service-key-vault-references.md) não funcionarão se o Key Vault estiver protegido com pontos de extremidade de serviço. Para se conectar a um Key Vault usando a integração de rede virtual, você precisará chamar o Key Vault no código do aplicativo.

## <a name="virtual-network-triggers-non-http"></a>Gatilhos de rede virtual (não HTTP)

No momento, você pode usar funções de gatilho não HTTP de dentro de uma rede virtual de uma das duas maneiras: 
+ Execute seu aplicativo de funções em um plano Premium e habilite o suporte ao gatilho de rede virtual.
+ Execute seu aplicativo de funções em um plano do serviço de aplicativo ou Ambiente do Serviço de Aplicativo.

### <a name="premium-plan-with-virtual-network-triggers"></a>Plano Premium com gatilhos de rede virtual

Ao executar em um plano Premium, você pode conectar funções de gatilho não HTTP a serviços em execução dentro de uma rede virtual. Para fazer isso, você deve habilitar o suporte ao gatilho de rede virtual para seu aplicativo de funções. A configuração de **suporte do gatilho de rede virtual** é encontrada no [portal do Azure](https://portal.azure.com) em configurações do **aplicativo de funções**.

![VNETToggle](media/functions-networking-options/virtual-network-trigger-toggle.png)

Você também pode habilitar gatilhos de rede virtual usando o seguinte comando de CLI do Azure:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Os gatilhos de rede virtual têm suporte na versão 2. x e superior do tempo de execução do functions. Há suporte para os seguintes tipos de gatilho não HTTP.

| Extensão | Versão Mínima |
|-----------|---------| 
|[Microsoft. Azure. webjobs. Extensions. Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 ou superior |
|[Microsoft. Azure. webjobs. Extensions. EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 ou superior|
|[Microsoft. Azure. webjobs. Extensions. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 ou superior|
|[Microsoft. Azure. webjobs. Extensions. CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 ou superior|
|[Microsoft. Azure. webjobs. Extensions. DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 ou superior|

> [!IMPORTANT]
> Ao habilitar o suporte de gatilho de rede virtual, somente os tipos de gatilho acima são dimensionados dinamicamente com seu aplicativo. Você ainda pode usar gatilhos não listados acima, no entanto, eles não são dimensionados além de sua contagem de instâncias previamente passiva. Consulte [gatilhos e associações](./functions-triggers-bindings.md#supported-bindings) para obter a lista completa de gatilhos.

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>Plano e Ambiente do Serviço de Aplicativo do serviço de aplicativo com gatilhos de rede virtual

Quando seu aplicativo de funções é executado em um plano do serviço de aplicativo ou em um Ambiente do Serviço de Aplicativo, você pode usar funções de gatilho não HTTP. Para que suas funções sejam disparadas corretamente, você deve estar conectado a uma rede virtual com acesso ao recurso definido na conexão do gatilho. 

Por exemplo, suponha que você deseja configurar Azure Cosmos DB para aceitar o tráfego somente de uma rede virtual. Nesse caso, você deve implantar seu aplicativo de funções em um plano do serviço de aplicativo que fornece integração de rede virtual com essa rede virtual. Isso permite que uma função seja disparada por esse Azure Cosmos DB recurso. 

## <a name="hybrid-connections"></a>Conexões Híbridas

[Conexões híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) é um recurso da retransmissão do Azure que você pode usar para acessar recursos do aplicativo em outras redes. Ele fornece acesso de seu aplicativo para um ponto de extremidade do aplicativo. Você não pode usá-lo para acessar seu aplicativo. Conexões Híbridas está disponível para funções em execução no Windows em todos, exceto no plano de consumo.

Conforme usado em Azure Functions, cada conexão híbrida se correlaciona com uma única combinação de host e porta de TCP. Isso significa que o ponto de extremidade da conexão híbrida pode estar em qualquer sistema operacional e qualquer aplicativo, desde que você esteja acessando uma porta de escuta TCP. O recurso Conexões Híbridas não conhece ou se preocupa com o que é o protocolo de aplicativo ou o que você está acessando. Ele apenas fornece acesso à rede.

Para saber mais, confira a [documentação do serviço de aplicativo para conexões híbridas](../app-service/app-service-hybrid-connections.md). Essas mesmas etapas de configuração oferecem suporte a Azure Functions.

>[!IMPORTANT]
> Só há suporte para Conexões Híbridas em planos do Windows. Não há suporte para Linux

## <a name="outbound-ip-restrictions"></a>Restrições de IP de saída

As restrições de IP de saída estão disponíveis em um plano Premium, plano do serviço de aplicativo ou Ambiente do Serviço de Aplicativo. Você pode configurar as restrições de saída para a rede virtual em que sua Ambiente do Serviço de Aplicativo está implantada.

Quando você integra um aplicativo de funções em um plano Premium ou um plano do serviço de aplicativo com uma rede virtual, o aplicativo ainda pode fazer chamadas de saída para a Internet por padrão. Ao adicionar uma configuração de aplicativo `WEBSITE_VNET_ROUTE_ALL=1`, você força todo o tráfego de saída a ser enviado para sua rede virtual, em que as regras do grupo de segurança de rede podem ser usadas para restringir o tráfego.

## <a name="troubleshooting"></a>solução de problemas 

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre rede e Azure Functions:

* [Siga o tutorial sobre como começar com a integração de rede virtual](./functions-create-vnet.md)
* [Leia as perguntas frequentes sobre rede de funções](./functions-networking-faq.md)
* [Saiba mais sobre a integração de rede virtual com o serviço de aplicativo/funções](../app-service/web-sites-integrate-with-vnet.md)
* [Saiba mais sobre as redes virtuais no Azure](../virtual-network/virtual-networks-overview.md)
* [Habilitar mais recursos de rede e controle com ambientes de serviço de aplicativo](../app-service/environment/intro.md)
* [Conectar-se a recursos locais individuais sem alterações de firewall usando Conexões Híbridas](../app-service/app-service-hybrid-connections.md)
