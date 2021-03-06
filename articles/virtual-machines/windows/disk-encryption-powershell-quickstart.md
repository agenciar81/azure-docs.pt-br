---
title: Criar e criptografar uma VM do Windows com o Azure PowerShell
description: Neste início rápido, você aprenderá a usar o Azure PowerShell para criar e criptografar uma máquina virtual do Windows
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: b5e5b44742c85591b913b94e622c76a2aba111ce
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72245706"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-in-azure-with-powershell"></a>Início Rápido: Criar e criptografar uma máquina virtual do Windows no Azure com o PowerShell

O módulo do Azure PowerShell é usado para criar e gerenciar recursos do Azure da linha de comando do PowerShell ou em scripts. Este início rápido mostra como usar o módulo do Azure PowerShell para criar uma máquina virtual (VM) do Windows, criar um Key Vault para o armazenamento de chaves de criptografia e criptografar a VM. 

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Um grupo de recursos é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Crie a máquina virtual do Azure com [New-AzVM](/powershell/module/az.compute/new-azvm). Você deve fornecer credenciais para o cmdlet. 

```powershell
$cred = Get-Credential 

New-AzVM -Name MyVm -Credential $cred -ResourceGroupName MyResourceGroup -Image win2016datacenter -Size Standard_D2S_V3
```

Levará alguns minutos para que sua VM seja implantada. 

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Criar um Cofre de chaves configurado para chaves de criptografia

A criptografia de disco do Azure armazena sua chave de criptografia em um Azure Key Vault. Crie um Key Vault com [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault). Para habilitar o Key Vault para armazenar chaves de criptografia, use o parâmetro -EnabledForDiskEncryption.

> [!Important]
> Cada Key Vault deve ter um nome exclusivo. O exemplo a seguir cria um Key Vault chamado *myKV*, mas você deve colocar um nome diferente.

```powershell
New-AzKeyvault -name MyKV -ResourceGroupName myResourceGroup -Location EastUS -EnabledForDiskEncryption
```

## <a name="encrypt-the-virtual-machine"></a>Criptografar a máquina virtual

Criptografe sua VM com [Set-AzVmDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension). 

Set-AzVmDiskEncryptionExtension requer alguns valores do seu objeto Key Vault. Você pode obter esses valores, transmitindo o nome exclusivo do Key Vault para [Get-AzKeyvault](/powershell/module/az.keyvault/get-azkeyvault).

```powershell
$KeyVault = Get-AzKeyVault -VaultName MyKV -ResourceGroupName MyResourceGroup

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName MyVM -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId
```

Após alguns minutos, o processo retornará o seguinte:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```

Você pode verificar o processo de criptografia executando [Get-AzVmDiskEncryptionStatus](/powershell/module/az.compute/Get-AzVMDiskEncryptionStatus).

```powershell
Get-AzVmDiskEncryptionStatus -VMName MyVM -ResourceGroupName MyResourceGroup
```

Quando a criptografia estiver ativada, você verá o seguinte na saída retornada:

```
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : NoDiskFound
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : Provisioning succeeded
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando não forem mais necessários, você poderá usar o cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para remover o grupo de recursos, a VM e todos os recursos relacionados:

```powershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Próximas etapas

Neste guia de início rápido, você criou uma máquina virtual, um Key Vault habilitado para chaves de criptografia e criptografou a VM.  Avance para o próximo artigo a fim de saber mais sobre os pré-requisitos do Azure Disk Encryption para VMs IaaS.

> [!div class="nextstepaction"]
> [Visão geral do Azure Disk Encryption](disk-encryption-overview.md)