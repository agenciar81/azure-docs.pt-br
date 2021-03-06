---
title: Soluções VMware do Azure (AVS)-configurar a criptografia vSAN para a nuvem privada da AVS
description: Descreve como configurar o recurso de criptografia de software vSAN para que sua nuvem privada de AVS possa trabalhar com um servidor de gerenciamento de chaves em execução em sua rede virtual do Azure.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 056c05701a3915610fb17a7e8c04feb743e38286
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020634"
---
# <a name="configure-vsan-encryption-for-avs-private-cloud"></a>Configurar a criptografia vSAN para nuvem privada da AVS

Você pode configurar o recurso de criptografia de software vSAN para que sua nuvem privada de AVS possa trabalhar com um servidor de gerenciamento de chaves em execução em sua rede virtual do Azure.

O VMware requer o uso de uma ferramenta KMS (servidor de gerenciamento de chaves) de terceiros compatível com KMIP 1,1 ao usar a criptografia vSAN. Você pode aproveitar qualquer KMS com suporte que seja certificado pelo VMware e que esteja disponível para o Azure.

Este guia descreve como usar o HyTrust keycontrol KMS em execução em uma rede virtual do Azure. Uma abordagem semelhante pode ser usada para qualquer outra solução KMS certificada de terceiros para vSAN.

Essa solução KMS exige que você:

* Instale, configure e gerencie uma ferramenta KMS de terceiros certificada pela VMware em sua rede virtual do Azure.
* Forneça suas próprias licenças para a ferramenta KMS.
* Configure e gerencie a criptografia vSAN em sua nuvem privada de AVS usando a ferramenta KMS de terceiros em execução em sua rede virtual do Azure.

## <a name="kms-deployment-scenario"></a>Cenário de implantação do KMS

O cluster de servidor KMS é executado em sua rede virtual do Azure e pode ser acessado por IP do vCenter privado de nuvem de AVS sobre a conexão do Azure ExpressRoute configurada.

![.. cluster/media/KMS na rede virtual do Azure](media/vsan-kms-cluster.png)

## <a name="how-to-deploy-the-solution"></a>Como implantar a solução

O processo de implantação tem as seguintes etapas:

1. [Verificar se os pré-requisitos foram atendidos](#verify-prerequisites-are-met)
2. [Portal da AVS: obter informações de emparelhamento do ExpressRoute](#avs-portal-obtain-expressroute-peering-information)
3. [Portal do Azure: conectar sua rede virtual à nuvem privada da AVS](#azure-portal-connect-your-virtual-network-to-the-avs-private-cloud)
4. [Portal do Azure: implantar um cluster de keycontrol HyTrust em sua rede virtual](#azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network)
5. [HyTrust WebUI: configurar o servidor KMIP](#hytrust-webui-configure-the-kmip-server)
6. [interface do usuário do vCenter: configurar a criptografia vSAN para usar o cluster KMS em sua rede virtual do Azure](#vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network)

### <a name="verify-prerequisites-are-met"></a>Verificar se os pré-requisitos foram atendidos

Verifique o seguinte antes da implantação:

* O fornecedor, a ferramenta e a versão do KMS selecionados estão na lista de compatibilidade do vSAN.
* O fornecedor selecionado dá suporte a uma versão da ferramenta a ser executada no Azure.
* A versão do Azure da ferramenta KMS é compatível com KMIP 1,1.
* Um Azure Resource Manager e uma rede virtual já foram criados.
* Uma nuvem privada de AVS já foi criada.

### <a name="avs-portal-obtain-expressroute-peering-information"></a>Portal da AVS: obter informações de emparelhamento do ExpressRoute

Para continuar a instalação, você precisa da chave de autorização e do URI do circuito par para o ExpressRoute mais acesso à sua assinatura do Azure. Essas informações estão disponíveis na página conexão de rede virtual no portal da AVS. Para obter instruções, consulte [Configurar uma conexão de rede virtual para a nuvem privada da AVS](virtual-network-connection.md). Se você tiver problemas para obter as informações, abra uma [solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="azure-portal-connect-your-virtual-network-to-the-avs-private-cloud"></a>Portal do Azure: conectar sua rede virtual à nuvem privada da AVS

1. Crie um gateway de rede virtual para sua rede virtual seguindo as instruções em [configurar um gateway de rede virtual para o ExpressRoute usando o portal do Azure](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).
2. Vincule sua rede virtual ao circuito de ExpressRoute do AVS seguindo as instruções em [conectar uma rede virtual a um circuito do expressroute usando o portal](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
3. Use as informações de circuito da AVS do ExpressRoute recebidas no seu email de boas-vindas de AVS para vincular sua rede virtual ao circuito do ExpressRoute do AVS no Azure.
4. Insira a chave de autorização e o URI do circuito de par, dê um nome à conexão e clique em **OK**.

![Forneça o URI do circuito de par do CS ao criar a rede virtual](media/vsan-azureportal01.png) 

### <a name="azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network"></a>Portal do Azure: implantar um cluster de keycontrol do HyTrust no Azure Resource Manager em sua rede virtual

Para implantar um cluster de keycontrol do HyTrust no Azure Resource Manager em sua rede virtual, execute as seguintes tarefas. Consulte a [documentação do HyTrust](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0) para obter detalhes.

1. Crie um grupo de segurança de rede do Azure (NSG-hytrust) com as regras de entrada especificadas seguindo as instruções na documentação do HyTrust.
2. Gere um par de chaves SSH no Azure.
3. Implante o nó de keycontrol inicial da imagem no Azure Marketplace. Use a chave pública do par de chaves que foi gerada e selecione **NSG-hytrust** como o grupo de segurança de rede para o nó de controle de chave.
4. Converta o endereço IP privado do keycontrol em um endereço IP estático.
5. SSH para a VM de controle de chave usando seu endereço IP público e a chave privada do par de chaves mencionado anteriormente.
6. Quando solicitado no Shell SSH, selecione `No` para definir o nó como o nó de keycontrol inicial.
7. Adicione nós de controle de informações adicionais repetindo as etapas 3-5 deste procedimento e selecionando `Yes` quando for solicitado a adicionar a um cluster existente.

### <a name="hytrust-webui-configure-the-kmip-server"></a>HyTrust WebUI: configurar o servidor KMIP

Acesse https://*Public-IP*, em que *Public-IP* é o endereço IP público da VM do nó de keycontrol. Siga estas etapas da [documentação do HyTrust](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0).

1. [Configurando um servidor KMIP](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2)
2. [Criando um pacote de certificado para criptografia do VMware](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3)

### <a name="vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network"></a>interface do usuário do vCenter: configurar a criptografia vSAN para usar o cluster KMS em sua rede virtual do Azure

Siga as instruções de HyTrust para [criar um cluster kms no vCenter](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4).

![Adicionar detalhes do cluster KMS no vCenter](media/vsan-config01.png)

No vCenter, vá para **Cluster > configurar** e selecione a opção **geral** para vSAN. Habilite a criptografia e selecione o cluster KMS que foi adicionado anteriormente ao vCenter.

![Habilitar a criptografia vSAN e configurar o cluster KMS no vCenter](media/vsan-config02.png)

## <a name="references"></a>Referências

### <a name="azure"></a>Azure

[Configurar um gateway de rede virtual para o ExpressRoute usando o portal do Azure](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)

[Conectar uma rede virtual a um circuito ExpressRoute usando o portal](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)

### <a name="hytrust"></a>HyTrust

[HyTrust DataControl e Microsoft Azure](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0)

[Configurando um servidor KMPI](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2)

[Criando um pacote de certificado para criptografia do VMware](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3)

[Criando o cluster KMS no vSphere](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4)
