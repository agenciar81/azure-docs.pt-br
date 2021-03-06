---
title: Alteração in-loco da edição do SQL Server
description: Saiba como alterar a edição de sua máquina virtual SQL Server no Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/14/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: bd9622d3e34198402bc45e5d855d05b0c8e35eb8
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77605451"
---
# <a name="in-place-change-of-sql-server-edition-on-azure-vm"></a>Alteração in-loco da edição do SQL Server na VM do Azure

Este artigo descreve como alterar a edição do SQL Server em uma máquina virtual do Windows no Azure. 

A edição do SQL Server é determinada pela chave do produto (Product Key) e é especificada durante o processo de instalação usando a mídia de instalação do. A edição determina quais [recursos](/sql/sql-server/editions-and-components-of-sql-server-2017) estão disponíveis no produto SQL Server. Você pode alterar a edição do SQL Server com a mídia de instalação e fazer o downgrade para reduzir o custo ou atualizar para habilitar mais recursos.

Depois que a edição do SQL Server tiver sido alterada internamente para a VM SQL Server, você deverá atualizar a propriedade de edição de SQL Server no portal do Azure para fins de cobrança. 

## <a name="prerequisites"></a>Prerequisites

Para fazer uma alteração in-loco da edição do SQL Server, você precisará do seguinte: 

- Uma [assinatura do Azure](https://azure.microsoft.com/free/).
- Uma [vm SQL Server no Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) registrada com o [provedor de recursos de VM do SQL](virtual-machines-windows-sql-register-with-resource-provider.md).
- Instale a mídia com a **edição desejada** do SQL Server. Os clientes que têm o [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) podem obter a mídia de instalação do [centro de licenciamento por volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Os clientes que não têm o Software Assurance podem usar a mídia de instalação de um Azure Marketplace SQL Server imagem de VM que tem sua edição desejada (normalmente localizada em `C:\SQLServerFull`). 


## <a name="upgrade-an-edition"></a>Atualizar uma edição

> [!WARNING]
> Atualizar a edição do SQL Server reiniciará o serviço para SQL Server, juntamente com quaisquer serviços associados, como Analysis Services e R Services. 

Para atualizar a edição do SQL Server, obtenha a mídia de instalação do SQL Server para a edição desejada do SQL Server e, em seguida, faça o seguinte:

1. Abra o setup. exe da mídia de instalação do SQL Server. 
1. Vá para **manutenção** e escolha a opção de **atualização de edição** . 

   ![Seleção para atualizar a edição do SQL Server](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Selecione **Avançar** até chegar à página **pronto para atualizar a edição** e, em seguida, selecione **Atualizar**. A janela de instalação pode parar de responder por alguns minutos, enquanto a alteração está sendo efetivada. Uma página **completa** confirmará que a atualização de edição foi concluída. 

Depois que a edição do SQL Server for atualizada, modifique a propriedade de edição da máquina virtual SQL Server no portal do Azure. Isso atualizará os metadados e a cobrança associados a essa VM.

## <a name="downgrade-an-edition"></a>Fazer downgrade de uma edição

Para fazer downgrade da edição do SQL Server, você precisa desinstalar completamente o SQL Server e reinstalá-lo novamente com a mídia de instalação da edição desejada. 

> [!WARNING]
> A desinstalação de SQL Server pode incorrer em tempo de inatividade adicional. 

Você pode fazer downgrade da edição do SQL Server seguindo estas etapas:

1. Faça backup de todos os bancos de dados, incluindo os bancos de dados do sistema. 
1. Mover bancos de dados do sistema (Mestre, modelo e msdb) para um novo local. 
1. Desinstale completamente SQL Server e todos os serviços associados. 
1. Reinicie a máquina virtual. 
1. Instale SQL Server usando a mídia com a edição desejada do SQL Server.
1. Instale os service packs e as atualizações cumulativas mais recentes.  
1. Substitua os novos bancos de dados do sistema que foram criados durante a instalação com os bancos de dados do sistema que você moveu anteriormente para um local diferente. 

Depois que a edição do SQL Server for rebaixada, modifique a propriedade de edição da máquina virtual SQL Server no portal do Azure. Isso atualizará os metadados e a cobrança associados a essa VM.

## <a name="change-edition-in-portal"></a>Alterar edição no portal 

Depois de alterar a edição do SQL Server usando a mídia de instalação e tiver registrado sua VM do SQL Server com o [provedor de recursos da VM do SQL](virtual-machines-windows-sql-register-with-resource-provider.md), você poderá usar o portal do Azure para modificar a propriedade de edição da VM SQL Server para fins de cobrança. Para fazer isso, siga estas etapas: 

1. Entre no [portal do Azure](https://portal.azure.com). 
1. Vá para o recurso de máquina virtual SQL Server. 
1. Em **configurações**, selecione **Configurar**. Em seguida, selecione a edição desejada do SQL Server na lista suspensa em **edição**. 

   ![Alterar metadados de edição](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Examine o aviso que diz que você deve alterar a edição de SQL Server primeiro e que a propriedade de edição deve corresponder à edição de SQL Server. 
1. Selecione **aplicar** para aplicar as alterações de metadados de edição. 


## <a name="remarks"></a>Comentários

- A propriedade de edição para a VM SQL Server deve corresponder à edição da instância de SQL Server instalada para todas as máquinas virtuais de SQL Server, incluindo os tipos de licenças pré-pago e traga sua própria licença.
- Se você descartar o recurso de VM SQL Server, voltará para a configuração de edição embutida em código da imagem.
- A capacidade de alterar a edição é um recurso do provedor de recursos de VM do SQL. A implantação de uma imagem do Azure Marketplace por meio do portal do Azure registra automaticamente uma VM SQL Server com o provedor de recursos. No entanto, os clientes que estiverem instalando automaticamente SQL Server precisarão [registrar manualmente sua VM SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md).
- Adicionar uma VM SQL Server a um conjunto de disponibilidade requer a recriação da VM. Todas as VMs adicionadas a um conjunto de disponibilidade voltarão para a edição padrão e a edição precisará ser modificada novamente.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte os seguintes artigos: 

* [Visão geral do SQL Server em uma VM do Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Perguntas frequentes para SQL Server em uma VM do Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Diretrizes de preços para SQL Server em uma VM do Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de versão para SQL Server em uma VM do Windows](virtual-machines-windows-sql-server-iaas-release-notes.md)


