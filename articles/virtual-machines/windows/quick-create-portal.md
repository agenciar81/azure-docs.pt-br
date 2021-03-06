---
title: Início Rápido – Criar uma VM Windows no portal do Azure
description: Neste início rápido, você aprende a usar o portal do Azure para criar uma máquina virtual Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/05/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 002d374f5be606688121ef4a3952383567c43e85
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78363478"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Início Rápido: Crie uma máquina virtual do Windows no Portal do Azure

As máquinas virtuais (VM) do Azure podem ser criadas por meio do Portal do Azure. Esse método fornece uma interface de usuário baseada em navegador para criar as VMS seus recursos relacionados. Este início rápido mostra como usar portal do Azure para implantar uma VM (máquina virtual) no Azure que executa o Windows Server 2019. Para ver a VM em ação, você habilita o protocolo RDP na VM e instala o servidor Web do IIS.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em https://portal.azure.com.

## <a name="create-virtual-machine"></a>Criar máquina virtual

1. Digite **máquinas virtuais** na pesquisa.
1. Em **Serviços**, selecione **Máquinas virtuais**.
1. Na página **Máquinas Virtuais**, selecione **Adicionar**. 
1. Na guia **Básico**, em **Detalhes do projeto**, verifique se a assinatura correta está selecionada e, em seguida, escolha **Criar** grupo de recursos. Digite *myResourceGroup* para o nome. 

    ![Criar um grupo de recursos para sua VM](./media/quick-create-portal/project-details.png)

1. Em **Detalhes da instância**, digite *myVM* para o **Nome da máquina virtual** e escolha *Leste dos EUA* para a **Região** e, em seguida, escolha *Windows Server 2019 Datacenter* para **Imagem**. Deixe os outros padrões.

    ![Seção de detalhes da instância](./media/quick-create-portal/instance-details.png)

1. Em **Conta de administrador**, forneça um nome de usuário, como *azureuser* e uma senha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](faq.md#what-are-the-password-requirements-when-creating-a-vm).

    ![Insira seu nome de usuário e sua senha](./media/quick-create-portal/administrator-account.png)

1. Em **Regras de porta de entrada**, escolha **Permitir portas selecionadas** e, em seguida, selecione **RDP (3389)** e **HTTP (80)** na lista suspensa.

    ![Abrir portas para RDP e HTTP](./media/quick-create-portal/inbound-port-rules.png)

1. Deixe os padrões restantes e, em seguida, selecione o botão **Examinar + criar** na parte inferior da página.

    ![Examinar e criar](./media/quick-create-portal/review-create.png)


## <a name="connect-to-virtual-machine"></a>Conectar-se à máquina virtual

Inicie uma conexão da área de trabalho remota para a máquina virtual. Estas instruções ensinam a se conectar aàsua VM de um computador com Windows. Em um Mac, você precisa de um cliente RDP, como este [Cliente de Área de Trabalho Remota](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) da Mac App Store.

1. Clique no botão **Conectar** na página de visão geral de sua máquina virtual. 

    ![Conecte-se a uma VM do Azure no portal](./media/quick-create-portal/portal-quick-start-9.png)
    
2. Na página **Conectar-se à máquina virtual**, mantenha as opções padrão para se conectar por endereço IP pela porta 3389 e clique em **Baixar arquivo RDP**.

2. Abra o arquivo RDP baixado e clique em **Conectar** quando solicitado. 

3. Na janela **Segurança do Windows**, selecione **Mais opções** e **Usar uma conta diferente**. Digite o nome do usuário como **localhost**\\*nomedeusuário*, insira a senha que você criou para a máquina virtual e, sem seguida, clique em **OK**.

4. Você pode receber um aviso do certificado durante o processo de logon. Clique em **Sim** ou em **Continuar** para criar a conexão.

## <a name="install-web-server"></a>Instalar servidor Web

Para ver a VM em ação, instale o servidor Web do IIS. Abra um prompt do PowerShell na VM e execute o seguinte comando:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Quando terminar, feche a conexão RDP com a VM.


## <a name="view-the-iis-welcome-page"></a>Exibir a página de boas-vindas do IIS

No portal, selecione a VM e, na visão geral da VM, use o botão **clique para copiar** à direita do endereço IP para copiá-lo e cole-o em uma guia do navegador. A página de boas-vindas do IIS padrão será aberta e deverá ter a seguinte aparência:

![Site do IIS padrão](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Limpar os recursos

Quando o grupo de recursos, a máquina virtual e todos os recursos relacionados não forem mais necessários, exclua-os. 

Selecione o grupo de recursos da máquina virtual e, em seguida, selecione **Excluir**. Confirme o nome do grupo de recursos terminar de excluir os recursos.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste início rápido, você implantou uma máquina virtual simples, abriu uma porta de rede para o tráfego da Web e instalou um servidor Web básico. Para saber mais sobre máquinas virtuais do Azure, continue o tutorial para VMs do Windows.

> [!div class="nextstepaction"]
> [Tutoriais de máquina virtual do Windows Azure](./tutorial-manage-vm.md)