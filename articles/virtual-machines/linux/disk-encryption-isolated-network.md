---
title: Azure Disk Encryption em uma rede isolada
description: Este artigo fornece dicas de solução de problemas para Microsoft Azure a criptografia de disco para VMs do Linux.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 02/27/2020
ms.custom: seodec18
ms.openlocfilehash: aa0dc204a017e2d40eb3952a9ede0755127f8de2
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970670"
---
# <a name="azure-disk-encryption-on-an-isolated-network"></a>Azure Disk Encryption em uma rede isolada

Quando a conectividade estiver restrita por requisitos de proxy, firewall ou NSG (grupo de segurança de rede), a capacidade da extensão para executar as tarefas necessárias poderá ser interrompida. Essa interrupção pode resultar em mensagens de status como "Status da extensão não disponível na VM".

## <a name="package-management"></a>Gerenciamento de pacotes

Azure Disk Encryption depende de vários componentes, que são normalmente instalados como parte da habilitação de ADE, se ainda não estiverem presentes. Quando por trás de um firewall ou de outra forma isolada da Internet, esses pacotes devem ser pré-instalados ou disponíveis localmente.

Aqui estão os pacotes necessários para cada distribuição. Para obter uma lista completa dos tipos de volume e distribuições com suporte, consulte [VMs e sistemas operacionais com suporte](disk-encryption-overview.md#supported-vms-and-operating-systems).

- **Ubuntu 14, 4, 16, 4, 18, 4**: lsscsi, psmisc, at, cryptsetup-bin, Python-in, Python-seis, procps
- **CentOS 7,2-7,7**: lsscsi, psmisc, lvm2, UUID, em, patch, cryptsetup, cryptsetup-recriptografar, pyparted, procps-ng, util-linux
- **CentOS 6,8**: lsscsi, psmisc, lvm2, UUID, at, cryptsetup – recriptografar, pyparted, Python-seis
- **RedHat 7,2-7,7**: lsscsi, psmisc, lvm2, UUID, em, patch, cryptsetup, cryptsetup-recriptografar, procps-ng, util-linux
- **RedHat 6,8**: lsscsi, psmisc, lvm2, UUID, at, patch, cryptsetup – recriptografar
- **openSUSE 42,3, SLES 12-SP4, 12-SP3**: lsscsi, cryptsetup

No Red Hat, quando um proxy é necessário, é preciso garantir que o gerenciador de assinaturas e o yum sejam configurados corretamente. Para saber mais, confira [How to troubleshoot subscription-manager and yum problems](https://access.redhat.com/solutions/189533) (Como solucionar problemas do gerenciador de assinaturas e do yum).  

Quando os pacotes são instalados manualmente, eles também devem ser atualizados manualmente conforme novas versões são lançadas.

## <a name="network-security-groups"></a>Regras de segurança de rede
Qualquer configuração de grupo de segurança de rede aplicada ainda deve permitir que o ponto de extremidade atenda aos pré-requisitos de configuração de rede documentados para criptografia de disco.  Consulte [Azure Disk Encryption: requisitos de rede](disk-encryption-overview.md#networking-requirements)

## <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure Disk Encryption com o Azure AD (versão anterior)

Se você estiver usando [Azure Disk Encryption com o Azure AD (versão anterior)](disk-encryption-overview-aad.md), a [biblioteca de Azure Active Directory](../../active-directory/azuread-dev/active-directory-authentication-libraries.md) precisará ser instalada manualmente para todos os distribuições (além dos pacotes apropriados para o distribuição, conforme [listado acima](#package-management)).

Quando a criptografia é habilitada com [credenciais do Azure AD](disk-encryption-linux-aad.md), a VM de destino deve permitir a conectividade com pontos de extremidade do Azure Active Directory e pontos de extremidade do Key Vault. Os pontos de extremidade de autenticação do Active Directory do Azure atuais são mantidos nas seções 56 e 59 da documentação [Intervalos de endereços IP e URLs do Office 365](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges). As instruções do Key Vault são fornecidas na documentação sobre como [Acessar o Azure Key Vault por trás de um firewall](../../key-vault/key-vault-access-behind-firewall.md).

### <a name="azure-instance-metadata-service"></a>Serviço de metadados de instância do Azure 

A máquina virtual deve ser capaz de acessar o ponto de extremidade do [serviço de metadados de instância do Azure](instance-metadata-service.md) , que usa um endereço IP não roteável conhecido (`169.254.169.254`) que pode ser acessado somente de dentro da VM.  Não há suporte para as configurações de proxy que alteram o tráfego HTTP local para esse endereço (por exemplo, a adição de um cabeçalho X-Forwardd-for).

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- Veja mais etapas para a [solução de problemas de criptografia de disco do Azure](disk-encryption-troubleshooting.md)
- [Criptografia de dados em repouso do Azure](../../security/fundamentals/encryption-atrest.md)
