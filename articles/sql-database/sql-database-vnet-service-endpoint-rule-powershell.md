---
title: PowerShell para pontos de extremidade de VNet e regras para bancos de dados individuais e em pool
description: Fornece scripts do PowerShell para criar e gerenciar pontos de extremidade de serviço virtual para o banco de dados SQL do Azure e o Azure Synapse.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, vanto
ms.date: 03/12/2019
tags: azure-synapse
ms.openlocfilehash: f61403ef50af209fdc6e811191d31ccc83f8da73
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191853"
---
# <a name="powershell--create-a-virtual-service-endpoint-and-vnet-rule-for-sql"></a>PowerShell: criar um ponto de extremidade de Serviço Virtual e uma regra VNet para SQL

*As regras de rede virtual* são um recurso de segurança de firewall que controla se o servidor de banco de dados para seus bancos de dado único e pool elástico no [banco de dados SQL](sql-database-technical-overview.md) do Azure ou em seus bancos de dados no [Azure Synapse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) aceita comunicações enviadas de sub-redes específicas em redes virtuais.

> [!IMPORTANT]
> Este artigo se aplica ao SQL Server do Azure e ao banco de dados SQL e data warehouse no Azure Synapse que são criados no SQL Server do Azure. Para simplificar, o banco de dados SQL é usado ao fazer referência ao banco de dados SQL e ao Azure Synapse. Este artigo *não* se aplica a uma implantação de **instância gerenciada** no Banco de Dados SQL do Azure porque não tem um ponto de extremidade de serviço associado.

Este artigo fornece e explica um script do PowerShell que realiza as seguintes ações:

1. Cria um *ponto de extremidade do Serviço virtual* do Microsoft Azure em sua sub-rede.
2. Adiciona o ponto de extremidade ao firewall do seu servidor de Banco de dados SQL do Azure, para criar uma *regra da rede virtual*.

Suas motivações para criar uma regra são explicadas em: [pontos de extremidade de serviço virtual para o banco de dados SQL do Azure][sql-db-vnet-service-endpoint-rule-overview-735r].

> [!TIP]
> Se tudo de que você precisa é avaliar ou adicionar o ponto de extremidade do Serviço virtual *nome do tipo* para o Banco de dados SQL do Microsoft Azure para sua sub-rede, passe adiante até nosso [script do PowerShell mais direto](#a-verify-subnet-is-endpoint-ps-100).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> O módulo Azure Resource Manager do PowerShell ainda tem suporte do banco de dados SQL do Azure, mas todo o desenvolvimento futuro é para o módulo AZ. Sql. Para esses cmdlets, consulte [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Os argumentos para os comandos no módulo AZ e nos módulos AzureRm são substancialmente idênticos.

## <a name="major-cmdlets"></a>Cmdlets principais

Este artigo enfatiza o cmdlet **New-AzSqlServerVirtualNetworkRule** que adiciona o ponto de extremidade de sub-rede à lista de controle de acesso (ACL) do seu servidor de banco de dados SQL do Azure, criando uma regra.

A lista a seguir mostra a sequência de outros cmdlets *principais* que você deve executar para se preparar para a chamada para **New-AzSqlServerVirtualNetworkRule**. Neste artigo, essas chamadas ocorrem no [script 3 "Regra da rede virtual"](#a-script-30):

1. [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig): cria um objeto de sub-rede.
2. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork): cria sua rede virtual, fornecendo-a à sub-rede.
3. [Set-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetworkSubnetConfig): atribui um ponto de extremidade de serviço virtual à sua sub-rede.
4. [Set-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/Set-azVirtualNetwork): mantém atualizações feitas em sua rede virtual.
5. [New-AzSqlServerVirtualNetworkRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlservervirtualnetworkrule): depois que a sub-rede for um ponto de extremidade, o adicionará sua sub-rede como uma regra de rede virtual, na ACL do seu servidor de banco de dados SQL do Azure.
   - Esse cmdlet oferece o parâmetro **-IgnoreMissingVNetServiceEndpoint**, iniciando no Módulo do PowerShell do Azure RM versão 5.1.1.

## <a name="prerequisites-for-running-powershell"></a>Pré-requisitos para execução do PowerShell

- Você já pode fazer logon no Azure, por exemplo, por meio do [portal do Azure][http-azure-portal-link-ref-477t].
- Você já pode executar scripts do PowerShell.

> [!NOTE]
> Verifique se os pontos de extremidade de serviço estão ativados para a Vnet/Sub-rede que você pretende adicionar ao Servidor, caso contrário a criação da Regra de Firewall de VNet falhará.

## <a name="one-script-divided-into-four-chunks"></a>Um script dividido em quatro partes

Nosso script do PowerShell de demonstração é dividido em uma sequência de scripts menores. A divisão facilita o aprendizado e fornece flexibilidade. Os scripts devem ser executados em sua sequência indicada. Se você não tiver tempo para executar os scripts, o resultado real do nosso teste será exibido após o script 4.

<a name="a-script-10" />

### <a name="script-1-variables"></a>Script 1: variáveis

Este primeiro script do PowerShell atribui valores a variáveis. Os scripts subsequentes dependem dessas variáveis.

> [!IMPORTANT]
> Antes de executar esse script, você pode editar os valores, se desejar. Por exemplo, se você já tiver um grupo de recursos, será possível editar o nome do grupo de recursos como o valor atribuído.
>
> O nome da sua assinatura deve ser editado no script.

### <a name="powershell-script-1-source-code"></a>Código-fonte do script 1 do PowerShell

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.
$SubscriptionName = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName $SubscriptionName;

$ResourceGroupName = 'RG-YourNameHere';
$Region            = 'westcentralus';

$VNetName            = 'myVNet';
$SubnetName          = 'mySubnet';
$VNetAddressPrefix   = '10.1.0.0/16';
$SubnetAddressPrefix = '10.1.1.0/24';
$VNetRuleName        = 'myFirstVNetRule-ForAcl';

$SqlDbServerName         = 'mysqldbserver-forvnet';
$SqlDbAdminLoginName     = 'ServerAdmin';
$SqlDbAdminLoginPassword = 'ChangeYourAdminPassword1';

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Official type name.

Write-Host 'Completed script 1, the "Variables".';
```

<a name="a-script-20" />

### <a name="script-2-prerequisites"></a>Script 2: pré-requisitos

Esse script prepara para o próximo script, no qual ocorre a ação de ponto de extremidade. Esse script cria para você os seguintes itens listados, mas apenas se eles ainda não existirem. Se você tiver certeza de que esses itens já existem, você poderá ignorar o script 2:

- Grupo de recursos do Azure
- Servidor de banco de dados SQL do Azure

### <a name="powershell-script-2-source-code"></a>Código-fonte do script 2 do PowerShell

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists.";

$gottenResourceGroup = $null;

$gottenResourceGroup = Get-AzResourceGroup `
  -Name        $ResourceGroupName `
  -ErrorAction SilentlyContinue;

if ($null -eq $gottenResourceGroup)
{
    Write-Host "Creating your missing Resource Group - $ResourceGroupName.";

    $gottenResourceGroup = New-AzResourceGroup `
      -Name $ResourceGroupName `
      -Location $Region;

    $gottenResourceGroup;
}
else { Write-Host "Good, your Resource Group already exists - $ResourceGroupName."; }

$gottenResourceGroup = $null;

###########################################################
## Ensure your Azure SQL Database server already exists. ##
###########################################################

Write-Host "Check whether your Azure SQL Database server already exists.";

$sqlDbServer = $null;

$sqlDbServer = Get-AzSqlServer `
  -ResourceGroupName $ResourceGroupName `
  -ServerName        $SqlDbServerName `
  -ErrorAction       SilentlyContinue;

if ($null -eq $sqlDbServer) {
    Write-Host "Creating the missing Azure SQL Database server - $SqlDbServerName.";
    Write-Host "Gather the credentials necessary to next create an Azure SQL Database server.";

    $sqlAdministratorCredentials = New-Object `
      -TypeName     System.Management.Automation.PSCredential `
      -ArgumentList `
        $SqlDbAdminLoginName, `
        $(ConvertTo-SecureString `
            -String      $SqlDbAdminLoginPassword `
            -AsPlainText `
            -Force `
         );

    if ($null -eq $sqlAdministratorCredentials) {
        Write-Host "ERROR, unable to create SQL administrator credentials.  Now ending.";
        return;
    }

    Write-Host "Create your Azure SQL Database server.";

    $sqlDbServer = New-AzSqlServer `
      -ResourceGroupName $ResourceGroupName `
      -ServerName        $SqlDbServerName `
      -Location          $Region `
      -SqlAdministratorCredentials $sqlAdministratorCredentials;

    $sqlDbServer;
}
else {
    Write-Host "Good, your Azure SQL Database server already exists - $SqlDbServerName."; 
}

$sqlAdministratorCredentials = $null;
$sqlDbServer                 = $null;

Write-Host 'Completed script 2, the "Prerequisites".';
```

<a name="a-script-30" />

## <a name="script-3-create-an-endpoint-and-a-rule"></a>Script de 3: criar um ponto de extremidade e uma regra

Esse script cria uma rede virtual com uma sub-rede. Em seguida, o script atribui o tipo de ponto de extremidade **Microsoft.Sql** à sua sub-rede. Por fim, o script adiciona sua sub-rede à lista de controle de acesso (ACL) do seu servidor de Banco de dados SQL do Microsoft Azure, criando assim uma regra.

### <a name="powershell-script-3-source-code"></a>Código-fonte do script 3 do PowerShell

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network.";

$subnet = New-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Create a virtual network '$VNetName'." `
  "  Give the subnet to the virtual network that we created.";

$vnet = New-AzVirtualNetwork `
  -Name              $VNetName `
  -AddressPrefix     $VNetAddressPrefix `
  -Subnet            $subnet `
  -ResourceGroupName $ResourceGroupName `
  -Location          $Region;

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet.";

$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

Write-Host "Persist the updates made to the virtual network > subnet.";

$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

$vnet.Subnets[0].ServiceEndpoints;  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

$subnet = Get-AzVirtualNetworkSubnetConfig `
  -Name           $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Add the subnet .Id as a rule, into the ACLs for your Azure SQL Database server.";

$vnetRuleObject1 = New-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -VirtualNetworkSubnetId $subnet.Id;

$vnetRuleObject1;

Write-Host "Verify that the rule is in the SQL DB ACL.";

$vnetRuleObject2 = Get-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName;

$vnetRuleObject2;

Write-Host 'Completed script 3, the "Virtual-Network-Rule".';
```

<a name="a-script-40" />

## <a name="script-4-clean-up"></a>Script 4: limpar

Esse script final exclui os recursos que os scripts anteriores criaram para a demonstração. No entanto, o script solicita a confirmação antes de excluir o seguinte:

- Servidor de banco de dados SQL do Azure
- Grupo de recursos do Azure

Você pode executar o script 4 a qualquer momento após a conclusão do script 1.

### <a name="powershell-script-4-source-code"></a>Código-fonte do script 4 do PowerShell

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL DB ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL DB ACL.";

Remove-AzSqlServerVirtualNetworkRule `
  -ResourceGroupName      $ResourceGroupName `
  -ServerName             $SqlDbServerName `
  -VirtualNetworkRuleName $VNetRuleName `
  -ErrorAction            SilentlyContinue;

Write-Host "Delete the endpoint from the subnet.";

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

Remove-AzVirtualNetworkSubnetConfig `
  -Name $SubnetName `
  -VirtualNetwork $vnet;

Write-Host "Delete the virtual network (thus also deletes the subnet).";

Remove-AzVirtualNetwork `
  -Name              $VNetName `
  -ResourceGroupName $ResourceGroupName `
  -ErrorAction       SilentlyContinue;

###########################################################
##   Clean-up phase B:  Conditional deletes.             ##
##                                                       ##
##   These might have already existed, so user might     ##
##   want to keep.                                       ##
##                                                       ##
##   1. Azure SQL Database server                        ##
##   2. Azure resource group                             ##
###########################################################

$yesno = Read-Host 'CAUTION !: Do you want to DELETE your Azure SQL Database server AND your Resource Group?  [yes/no]';
if ('yes' -eq $yesno) {
    Write-Host "Remove the Azure SQL DB server.";

    Remove-AzSqlServer `
      -ServerName        $SqlDbServerName `
      -ResourceGroupName $ResourceGroupName `
      -ErrorAction       SilentlyContinue;

    Write-Host "Remove the Azure Resource Group.";

    Remove-AzResourceGroup `
      -Name        $ResourceGroupName `
      -ErrorAction SilentlyContinue;
}
else {
    Write-Host "Skipped over the DELETE of SQL Database and resource group.";
}

Write-Host 'Completed script 4, the "Clean-Up".';
```

<a name="a-actual-output" />

<a name="a-verify-subnet-is-endpoint-ps-100" />

## <a name="verify-your-subnet-is-an-endpoint"></a>Verificar se sua sub-rede é um ponto de extremidade

Você pode ter uma sub-rede que já foi atribuída ao nome de tipo **Microsoft.Sql**, o que significa que já é um ponto de extremidade do Serviço virtual. Você pode usar o [portal do Azure][http-azure-portal-link-ref-477t] para criar uma regra de rede virtual a partir do ponto de extremidade.

Ou, talvez você não tenha certeza se a sua sub-rede tem o nome do tipo **Microsoft.Sql**. É possível executar o seguinte script do PowerShell para executar estas ações:

1. Confira se sua sub-rede tem o nome do tipo **Microsoft.Sql**.
2. Opcionalmente, atribua o nome do tipo se ele estiver ausente.
    - O script solicitará que você *confirme*, antes de aplicar o nome do tipo ausente.

### <a name="phases-of-the-script"></a>Fases do script

Estas são as fases do script do PowerShell:

1. Faça logon em sua conta do Azure, o que é necessário apenas uma vez por sessão de PS.  Atribua variáveis.
2. Pesquise pela sua rede virtual e, em seguida, pela sua sub-rede.
3. Sua sub-rede está marcada como tipo de servidor de ponto de extremidade **Microsoft.Sql**?
4. Adicione um ponto de extremidade do Serviço virtual do nome do tipo **Microsoft.Sql**, em sua sub-rede.

> [!IMPORTANT]
> Antes de executar esse script, você deve editar os valores atribuídos a variáveis $-, na parte superior do script.

### <a name="direct-powershell-source-code"></a>Código-fonte do PowerShell Direct

Esse script do PowerShell não atualiza nada, a menos que você responda sim se o PowerShell solicitar sua confirmação. O script pode adicionar o nome do tipo **Microsoft.Sql** à sua sub-rede. Mas o script tenta adicionar somente se sua sub-rede não tiver o nome do tipo.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.
$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]';
if ('yes' -eq $yesno) { Connect-AzAccount; }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName  = 'yourSubscriptionName';
Select-AzSubscription -SubscriptionName "$SubscriptionName";

$ResourceGroupName   = 'yourRGName';
$VNetName            = 'yourVNetName';
$SubnetName          = 'yourSubnetName';
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.'; # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql';  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.
# Search for the virtual network.
$vnet = $null;
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Name              $VNetName;

if ($vnet -eq $null) {
    Write-Host "Caution: No virtual network found by the name '$VNetName'.";
    Return;
}

$subnet = $null;
for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $subnet = $vnet.Subnets[$nn];
    if ($subnet.Name -eq $SubnetName)
    { break; }
    $subnet = $null;
}

if ($subnet -eq $null) {
    Write-Host "Caution: No subnet found by the name '$SubnetName'";
    Return;
}

### 3. Is your subnet tagged as 'Microsoft.Sql' endpoint server type?
$endpointMsSql = $null;
for ($nn=0; $nn -lt $subnet.ServiceEndpoints.Count; $nn++) {
    $endpointMsSql = $subnet.ServiceEndpoints[$nn];
    if ($endpointMsSql.Service -eq $ServiceEndpointTypeName_SqlDb) {
        $endpointMsSql;
        break;
    }
    $endpointMsSql = $null;
}

if ($endpointMsSql -ne $null) {
    Write-Host "Good: Subnet found, and is already tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";
    Return;
}
else {
    Write-Host "Caution: Subnet found, but not yet tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'.";

    # Ask the user for confirmation.
    $yesno = Read-Host 'Do you want the PS script to apply the endpoint type name to your subnet?  [yes/no]';
    if ('no' -eq $yesno) { Return; }
}

### 4. Add a Virtual Service endpoint of type name 'Microsoft.Sql', on your subnet.
$vnet = Set-AzVirtualNetworkSubnetConfig `
  -Name            $SubnetName `
  -AddressPrefix   $SubnetAddressPrefix `
  -VirtualNetwork  $vnet `
  -ServiceEndpoint $ServiceEndpointTypeName_SqlDb;

# Persist the subnet update.
$vnet = Set-AzVirtualNetwork `
  -VirtualNetwork $vnet;

for ($nn=0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $vnet.Subnets[0].ServiceEndpoints; }  # Display.
```

<!-- Link references: -->
[sql-db-vnet-service-endpoint-rule-overview-735r]: sql-database-vnet-service-endpoint-rule-overview.md
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
