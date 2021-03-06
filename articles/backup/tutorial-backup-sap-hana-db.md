---
title: Tutorial – fazer backup de bancos de dados do SAP HANA em VMs do Azure
description: Neste tutorial, saiba o backup de bancos de dados SAP HANA executados em uma VM do Azure pode ser realizado no cofre dos Serviços de Recuperação do Backup do Azure.
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: bb84f6b362adf7c190f3300e6e3f1bc572153151
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75753983"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Tutorial: Fazer backup de bancos de dados do SAP HANA em uma VM do Azure

Este tutorial mostra a você como fazer backup de bancos de dados do SAP HANA em execução em VMs do Azure em um cofre dos Serviços de Recuperação do Backup do Azure. Neste artigo, você aprenderá a:

> [!div class="checklist"]
>
> * Criar e configurar um cofre
> * Descobrir bancos de dados
> * Configurar backups

[Aqui](sap-hana-backup-support-matrix.md#scenario-support) estão todos os cenários aos quais damos suporte no momento.

## <a name="onboard-to-the-public-preview"></a>Integrar-se à versão prévia pública

Integre à versão prévia pública da seguinte maneira:

* No portal, registre sua ID da assinatura para o provedor de serviço dos Serviços de Recuperação [seguindo este artigo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).

* Para o PowerShell, execute este cmdlet. Ele deve ser concluído como "Registrado".

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="prerequisites"></a>Prerequisites

Antes de configurar backups, verifique se você fez o seguinte:

1. Na VM que executa o banco de dados SAP HANA, instale e habilite pacotes de driver ODBC do pacote/mídia oficial do SLES usando o zypper, da seguinte maneira:

```bash
sudo zypper update
sudo zypper install unixODBC
```

>[!NOTE]
> Se você não estiver atualizando os repositórios, verifique se a versão de unixODBC é 2.3.4 ou superior. Para saber a versão do uniXODBC, execute `odbcinst -j` como raiz
>

2. Permita a conectividade da VM com a Internet para que ela possa acessar o Azure, conforme descrito no [procedimento abaixo](#set-up-network-connectivity).

3. Execute o script de pré-registro na máquina virtual em que o HANA está instalado como um usuário raiz. [Este script](https://aka.ms/scriptforpermsonhana) definirá as [permissões corretas](#setting-up-permissions).

## <a name="set-up-network-connectivity"></a>Configurar a conectividade de rede

Para todas as operações, a VM do SAP HANA exige a conectividade com endereços IP públicos do Azure. As operações de VM (descoberta de banco de dados, configuração de backups, agendamento de backups, restauração de pontos de recuperação etc.) falham sem conectividade com endereços IP públicos do Azure.

Estabeleça a conectividade usando uma das seguintes opções:

### <a name="allow-the-azure-datacenter-ip-ranges"></a>Permitir os intervalos de IP do datacenter do Azure

Essa opção permite os [intervalos de IP](https://www.microsoft.com/download/details.aspx?id=41653) no arquivo baixado. Para acessar um NSG (grupo de segurança de rede), use o cmdlet Set-AzureNetworkSecurityRule. Se a lista de destinatários confiáveis só incluir IPs específicos da região, você também precisará atualizar a lista de destinatários confiáveis quanto à marca de serviço do Azure AD (Azure Active Directory) para habilitar a autenticação.

### <a name="allow-access-using-nsg-tags"></a>Permitir acesso usando marcas NSG

Se você usar um NSG para restringir a conectividade, deverá usar a marca de serviço AzureBackup para permitir o acesso de saída ao Backup do Azure. Além disso, você deverá permitir a conectividade para autenticação e transferência de dados usando [regras](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) para o Azure AD e o Armazenamento do Azure. Isso pode ser feito no portal do Azure ou por meio do PowerShell.

Para criar uma regra usando o portal:

  1. Em **Todos os Serviços**, acesse **Grupos de segurança de rede** e selecione o grupo de segurança de rede.
  2. Selecione **Regras de segurança de saída** em **Configurações**.
  3. Selecione **Adicionar**. Insira todos os detalhes necessários para criar uma regra, conforme descrito em [Configurações da regra de segurança](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings). Verifique se a opção **Destino** está definida como **Marca de Serviço** e se **Marca de serviço de destino** está definida como **AzureBackup**.
  4. Clique em **Adicionar** para salvar a regra de segurança de saída recém-criada.

Para criar uma regra usando o PowerShell:

 1. Adicione as credenciais de conta do Azure e atualize as nuvens nacionais<br/>
      `Add-AzureRmAccount`<br/>

 2. Selecione a assinatura do NSG<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. Selecione o NSG<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Adicione a permissão da regra de saída à marca de serviço do Backup do Azure<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Adicione a permissão da regra de saída à marca de serviço do Armazenamento<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. Adicione a permissão da regra de saída à marca de serviço AzureActiveDirectory<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. Salve o NSG<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Permitir o acesso usando marcas do Firewall do Azure**. Se estiver usando o Firewall do Azure, crie uma regra de aplicativo usando a marca de [FQDN](https://docs.microsoft.com/azure/firewall/fqdn-tags) do AzureBackup. Isso permite o acesso de saída ao Backup do Azure.

**Implantar um servidor proxy HTTP para rotear o tráfego**. Quando você faz backup de um banco de dados do SAP HANA em uma VM do Azure, a extensão de backup na VM usa as APIs HTTPS para enviar comandos de gerenciamento ao Backup do Azure e dados ao Armazenamento do Azure. A extensão de backup também usa o Azure AD para autenticação. Roteie o tráfego de extensão de backup para esses três serviços por meio do proxy HTTP. As extensões são o único componente que está configurado para acesso à Internet pública.

As opções de conectividade incluem as seguintes vantagens e desvantagens:

**Opção** | **Vantagens** | **Desvantagens**
--- | --- | ---
Permitir intervalos de IP | Sem custos adicionais | É complexa de ser gerenciada porque os intervalos de endereços IP mudam com o tempo <br/><br/> Fornece acesso a todo o Azure, não somente ao Armazenamento do Azure
Usar marcas de serviço do NSG | Mais fácil de ser gerenciada, pois as alterações de intervalo são mescladas automaticamente <br/><br/> Sem custos adicionais <br/><br/> | Pode ser usada somente com NSGs <br/><br/> Fornece acesso a todo o serviço
Usar marcas de FQDN do Firewall do Azure | Mais fácil de ser gerenciada, pois os FQDNs necessários são gerenciados automaticamente | Pode ser usada somente com o Firewall do Azure
Usar um proxy HTTP | É permitido o controle granular no proxy das URLs de armazenamento <br/><br/> Único ponto de acesso à Internet para VMs <br/><br/> Não está sujeita a alterações de endereços IP do Azure | Custos adicionais para execução de uma VM com o software de proxy

## <a name="setting-up-permissions"></a>Configurando as permissões

O script de pré-registro executa as seguintes ações:

1. Cria AZUREWLBACKUPHANAUSER no sistema HANA e adiciona estas funções e permissões necessárias:
   * ADMIN DO BANCO DE DADOS: para criar novos BDs durante a restauração.
   * LEITURA DO CATÁLOGO: para ler o catálogo de backup.
   * SAP_INTERNAL_HANA_SUPPORT: para acessar algumas tabelas privadas.
2. Adiciona uma chave a Hdbuserstore para o plug-in do HANA lidar com todas as operações (consultas de banco de dados, operações de restauração, configuração e execução de backup).

Para confirmar a criação da chave, execute o comando HDBSQL no computador HANA com credenciais de SIDADM:

```hdbsql
hdbuserstore list
```

A saída do comando deve exibir a chave {SID}{DBNAME}, com o usuário mostrado como AZUREWLBACKUPHANAUSER.

>[!NOTE]
> Verifique se você tem um conjunto exclusivo de arquivos SSFS em /usr/sap/{SID}/home/.hdb/. Deve haver apenas uma pasta neste caminho.
>

## <a name="create-a-recovery-service-vault"></a>Criar um cofre do Serviço de Recuperação

Um cofre dos Serviços de Recuperação é uma entidade que armazena os backups e os pontos de recuperação criados ao longo do tempo. O cofre dos Serviços de Recuperação também contém as políticas de backup associadas às máquinas virtuais protegidas.

Para criar um cofre de Serviços de Recuperação:

1. Entre na sua assinatura no [Portal do Azure](https://portal.azure.com/).

2. No menu esquerdo, selecione **Todos os serviços**

![Selecionar Todos os serviços](./media/tutorial-backup-sap-hana-db/all-services.png)

3. Na caixa de diálogo **Todos os serviços**, insira **Serviços de Recuperação**. A lista de recurso filtra de acordo com sua entrada. Na lista de recursos, selecione **Cofres dos Serviços de Recuperação**.

![Selecione Cofres de Serviços de Recuperação](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. No painel de cofres dos **Serviços de Recuperação**, selecione **Adicionar**.

![Adicionar um cofre dos Serviços de Recuperação](./media/tutorial-backup-sap-hana-db/add-vault.png)

A caixa de diálogo **Cofre dos Serviços de Recuperação** é aberta. Fornecer valores para **Nome, Assinatura, Grupo de recursos** e **Localização**

![Criar cofre de Serviços de Recuperação](./media/tutorial-backup-sap-hana-db/create-vault.png)

* **Name**: O nome é usado para identificar o cofre dos Serviços de Recuperação e precisa ser exclusivo para a assinatura do Azure. Especifique um nome que tenha pelo menos dois, mas não mais de 50 caracteres. O nome deve começar com uma letra e consistir apenas em letras, números e hifens. Para este tutorial, usamos o nome **SAPHanaVault**.
* **Assinatura**: Escolha a assinatura a ser usada. Se você for um membro de apenas uma assinatura, verá esse nome. Se você não tem certeza de qual assinatura usar, utilize a assinatura padrão (sugerida). Só haverá múltiplas opções se a sua conta corporativa ou de estudante estiver associada a várias assinaturas do Azure. Aqui, usamos a **assinatura do laboratório de solução do SAP HANA**.
* **Grupo de recursos**: Use um grupo de recursos existente ou crie um novo. Aqui, usamos **SAPHANADemo**.<br>
Para ver a lista de grupos de recursos disponíveis em sua assinatura, selecione **Usar existente** e, em seguida, selecione um recurso na caixa de listagem suspensa. Para criar um novo grupo de recursos, selecione **Criar novo** e insira o nome. Para obter informações completas sobre grupos de recursos, confira [Visão geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
* **Localização**: Selecione a região geográfica para o cofre. O cofre precisa estar na mesma região que a máquina virtual que executa o SAP HANA. Nós usamos **Leste dos EUA 2**.

5. Selecione **Examinar + criar**.

   ![Selecione Examinar + Criar](./media/tutorial-backup-sap-hana-db/review-create.png)

O cofre dos Serviços de Recuperação agora está criado.

## <a name="discover-the-databases"></a>Descobrir os bancos de dados

1. No cofre, em **Introdução**, clique em **Backup**. Em **Em que local sua carga de trabalho é executada?** , selecione **SAP HANA em VM do Azure**.
2. Clique em **Iniciar Descoberta**. Isso inicia a descoberta de VMs do Linux desprotegidas na região do cofre. Você verá a VM do Azure que deseja proteger.
3. Em **Selecionar Máquinas Virtuais**, clique no link para baixar o script que fornece permissões para que o serviço de Backup do Azure acesse as VMs SAP HANA para descoberta de banco de dados.
4. Execute o script na VM que hospeda os bancos de dados do SAP HANA dos quais você deseja fazer backup.
5. Depois de executar o script na VM, em **Selecionar Máquinas Virtuais**, selecione a VM. Em seguida, clique em **Descobrir BDs**.
6. O Backup do Azure descobre todos os bancos de dados do SAP HANA na VM. Durante a descoberta, o Backup do Azure registra a VM com o cofre e instala uma extensão na VM. Nenhum agente é instalado no banco de dados.

   ![Descobrir os bancos de dados](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Configurar o backup

Agora que os bancos de dados dos quais desejamos fazer backup foram descobertos, vamos habilitar o backup.

1. Clique em **Configurar Backup**.

![Configurar o backup](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. Em **Selecionar os itens para fazer backup**, selecione um ou mais bancos de dados que você deseja proteger e clique em **OK**.

![Selecionar itens para fazer backup](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. Em **Política de Backup > Escolher política de backup**, crie uma política de backup para os bancos de dados, de acordo com as instruções na próxima seção.

![Escolher política de backup](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. Depois de criar a política, no **menu Backup**, clique em **Habilitar o backup**.

   ![Clique em Habilitar o backup](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Acompanhe o progresso da configuração de backup na área de **Notificações** do portal.

## <a name="creating-a-backup-policy"></a>Criar uma política de backup

Uma política de backup define quando os backups são feitos e por quanto tempo eles são mantidos.

* Uma política é criada no nível do cofre.
* Vários cofres podem usar a mesma política de backup, mas você deve aplicar a política de backup a cada cofre.

Especifique as configurações de política da seguinte maneira:

1. Em **Nome da política**, insira um nome para a nova política. Nesse caso, digite **SAPHANA**.

![Inserir um nome para a nova política](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. Em **Política de backup completo**, selecione uma **Frequência de Backup**. Você pode escolher **Diário** ou **Semanal**. Para este tutorial, escolhemos o backup **Diário**.

![Selecionar uma frequência de backup](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. Em **Período de Retenção**, defina as configurações de retenção para o backup completo.
   * Por padrão, todas as opções são selecionadas. Desmarque os limites de período de retenção que você não deseja usar e marque os que você deseja.
   * O período de retenção mínimo para qualquer tipo de backup (completo/diferencial/log) é de sete dias.
   * Os pontos de recuperação são marcados para retenção com base em seu intervalo de retenção. Por exemplo, se você selecionar um backup completo diário, apenas um backup completo será disparado a cada dia.
   * O backup para um dia específico é marcado e mantido com base na configuração e no período de retenção semanal.
   * Os intervalos de retenção mensal e anual comportam-se de maneira semelhante.
4. No menu de **Política de Backup Completo**, clique em **OK** para aceitar as configurações.
5. Em seguida, selecione **Backup Diferencial** para adicionar uma política diferencial.
6. Em **Política de Backup Diferencial**, selecione **Habilitar** para abrir os controles de retenção e frequência. Habilitamos um backup diferencial a cada **Domingo** às **02:00**, que é mantido por **30 dias**.

   ![Política de backup diferencial](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

>[!NOTE]
>Atualmente, backups incrementais não são compatíveis.
>

7. Clique em **OK** para salvar a política e retornar para o menu principal da **Política de backup**.
8. Selecione **Backup de Log** para adicionar uma política de backup de log transacional.
   * **Backup de Log** é definido por padrão como **Habilitar**. Isso não pode ser desabilitado, pois o SAP HANA gerencia todos os backups de log.
   * Definimos **2 horas** como o agendamento de backup e **15 dias** de período de retenção.

    ![Política de backup de log](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

>[!NOTE]
> Os backups de log só começam a fluir depois que um backup completo bem-sucedido é concluído.
>

9. Clique em **OK** para salvar a política e retornar para o menu principal da **Política de backup**.
10. Depois de terminar de definir a política de backup, clique em **OK**.

Você configurou com êxito os backups para seus bancos de dados do SAP HANA.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [executar backups sob demanda em bancos de dados do SAP HANA em execução em VMs do Azure](backup-azure-sap-hana-database.md#run-an-on-demand-backup)
* Saiba como [restaurar bancos de dados do SAP HANA em execução em VMs do Azure](sap-hana-db-restore.md)
* Saiba como [gerenciar bancos de dados do SAP HANA cujo backup é feito usando o Backup do Azure](sap-hana-db-manage.md)
* Saiba como [solucionar problemas comuns ao fazer backup de bancos de dados do SAP HANA](backup-azure-sap-hana-database-troubleshoot.md)
