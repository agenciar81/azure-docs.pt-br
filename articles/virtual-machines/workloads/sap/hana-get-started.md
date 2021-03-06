---
title: 'Início rápido: a instalação manual de instância única SAP HANA em máquinas virtuais do Azure | Microsoft Docs'
description: Guia de início rápido para a instalação manual do SAP HANA de instância única em máquinas virtuais do Azure
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: hermannd
ms.openlocfilehash: 0090ffe977dee3e493d726c9eb4d151bcbeb503f
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617245"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-virtual-machines"></a>Início rápido: instalação manual de SAP HANA de instância única em máquinas virtuais do Azure
## <a name="introduction"></a>Introdução
Este guia ajuda você a configurar um SAP HANA de instância única em máquinas virtuais do Azure quando você instala o SAP NetWeaver 7,5 e o SAP HANA 1,0 SP12 manualmente. O foco deste guia é sobre como implantar SAP HANA no Azure. Ele não substitui a documentação do SAP. 

> [!NOTE]
> Esta guia descreve as implantações do SAP HANA em VMs do Azure. Para obter informações sobre como implantar SAP HANA em instâncias grandes do HANA, consulte [usar o SAP em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Prerequisites
Este guia pressupõe que você esteja familiarizado com esses conceitos básicos de IaaS (infraestrutura como serviço) como:
 * Como implantar máquinas virtuais (VMs) ou redes virtuais por meio do portal do Azure ou do PowerShell.
 * A CLI (interface de linha de comando) entre plataformas do Azure, que inclui a opção de usar modelos de JavaScript Object Notation (JSON).

Este guia também pressupõe que você esteja familiarizado com:
* SAP HANA e SAP NetWeaver e como instalá-los no local.
* Como instalar e operar SAP HANA e instâncias de aplicativo SAP no Azure.
* Os conceitos e procedimentos a seguir:
   * Planejando a implantação do SAP no Azure, que inclui o planejamento de rede virtual do Azure e o uso do armazenamento do Azure. Consulte [SAP NetWeaver em máquinas virtuais do Azure – guia de planejamento e implementação](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Princípios de implantação e maneiras de implantar VMs no Azure. Confira [Implantação de máquinas virtuais do Azure para SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Alta disponibilidade para SAP NetWeaver ABAP SAP central Services (ASCS), SAP central Services (SCS) e servidor de replicação de enfileiramento (ERS) no Azure. Confira [Alta disponibilidade do SAP NetWeaver em VMs do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Detalhes sobre como melhorar a eficiência em uma instalação de vários SID do ASCS/SCS no Azure. Confira [Como criar uma configuração vários SID do SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Os princípios da execução do SAP NetWeaver com base em VMs controladas por Linux no Azure. Consulte [executar o SAP NetWeaver em VMs Microsoft Azure SuSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Este guia fornece configurações específicas para Linux em VMs do Azure. Ele também fornece informações sobre como anexar corretamente discos de armazenamento do Azure a VMs do Linux.

Os tipos de VM do Azure que podem ser usados para cenários de produção são listados na [documentação do SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Para cenários de não produção, uma variedade maior de tipos de VM nativos do Azure está disponível.
Para obter mais informações sobre a configuração e as operações da VM, consulte [SAP Hana configurações de infraestrutura e operações no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
Para SAP HANA alta disponibilidade, consulte [SAP Hana alta disponibilidade para máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

Se você quiser obter uma instância SAP HANA ou um sistema S/4HANA ou BW/4HANA implantado rapidamente, considere usar a [SAP Cloud Appliance library](https://cal.sap.com). Você pode encontrar a documentação sobre como implantar um sistema S/4HANA por meio da SAP Cloud Appliance Library no Azure, por exemplo, neste [guia](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Tudo o que você precisa é de uma assinatura do Azure e de um usuário SAP que possa ser registrado com a SAP Cloud Appliance library.

## <a name="additional-resources"></a>Recursos adicionais
### <a name="sap-hana-backup"></a>Backup do SAP HANA
Para obter informações sobre como fazer backup de bancos de dados do SAP HANA em VMs do Azure, consulte:
* [Guia de backup para SAP Hana em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide).
* [SAP Hana o backup do Azure no nível do arquivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level).
* [SAP Hana backup com base em instantâneos de armazenamento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots).

### <a name="sap-cloud-appliance-library"></a>Biblioteca de dispositivo de nuvem do SAP
Para obter informações sobre como usar a SAP Cloud Appliance Library para implantar S/4HANA ou BW/4HANA, consulte [implantar SAP S/4HANA ou BW/4HANA em Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>Sistemas operacionais SAP HANA com suporte
Para obter informações sobre sistemas operacionais com suporte SAP HANA, consulte [SAP Note 2235581-SAP Hana: sistemas operacionais com suporte](https://launchpad.support.sap.com/#/notes/2235581/E). As VMs do Azure oferecem suporte a apenas um subconjunto desses sistemas operacionais. Os seguintes sistemas operacionais têm suporte para implantar o SAP HANA no Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Para obter documentação SAP adicional sobre o SAP HANA e sistemas operacionais Linux diferentes, confira:

* [Observação sap 171356: software SAP no Linux: informações gerais](https://launchpad.support.sap.com/#/notes/1984787).
* [Observação do SAP 1944799: diretrizes de SAP Hana para a instalação do sistema operacional SLES](http://service.sap.com/sap/support/notes/1944799).
* [Observação SAP 2205917: configurações de so recomendadas do SAP Hana DB para SLES 12 para aplicativos SAP](https://launchpad.support.sap.com/#/notes/2205917/E).
* [Observação SAP 1391070: soluções de UUID do Linux](https://launchpad.support.sap.com/#/notes/1391070).
* [Nota SAP 2009879: diretrizes de SAP Hana para o sistema operacional Red Hat Enterprise Linux (RHEL)](https://launchpad.support.sap.com/#/notes/2009879).
* [Observação SAP 2292690: SAP Hana DB: configurações de so recomendadas para RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E).

### <a name="sap-monitoring-in-azure"></a>Monitoramento SAP no Azure
Para obter informações sobre o monitoramento do SAP no Azure:

* A [Nota sap 2191498](https://launchpad.support.sap.com/#/notes/2191498/E) aborda o monitoramento aprimorado do SAP com VMs do Linux no Azure. 
* A [Nota SAP 1102124](https://launchpad.support.sap.com/#/notes/1102124/E) discute informações sobre o SAPOSCOL no Linux. 
* A [Observação da sap 2178632](https://launchpad.support.sap.com/#/notes/2178632/E) aborda as principais métricas de monitoramento para SAP em Microsoft Azure.

### <a name="azure-vm-types"></a>Tipos de VM do Azure
Os tipos de VM do Azure e os cenários de carga de trabalho com suporte SAP usados com SAP HANA estão documentados em [plataformas de IaaS certificadas pelo SAP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Os tipos de VM do Azure que são certificados pelo SAP para SAP NetWeaver ou pela camada de aplicativo S/4HANA estão documentados na [Observação SAP 1928533: aplicativos SAP no Azure: produtos com suporte e tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533/E).

> [!NOTE]
> A integração SAP-Linux-Azure tem suporte apenas no Azure Resource Manager e não no modelo de implantação clássico. 

## <a name="manual-installation-of-sap-hana"></a>Instalação manual do SAP HANA

> [!IMPORTANT]
> Certifique-se de que o sistema operacional selecionado é certificado pelo SAP para SAP HANA nos tipos específicos de VM que você usa. A lista de tipos de VM certificados SAP HANA e as versões de sistema operacional para esses tipos de VM podem ser pesquisadas em [SAP Hana plataformas IaaS certificadas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Certifique-se de clicar nos detalhes do tipo de VM listado para obter a lista completa de versões de sistema operacional com suporte SAP HANA para o tipo de VM específico. Para o exemplo deste documento, usamos uma versão do sistema operacional SUSE Linux Enterprise Server (SLES) que não tem suporte do SAP para SAP HANA em VMs da série M.
>

Esse guia descreve duas maneiras diferentes de instalar o SAP HANA manualmente em VMs do Azure:

* Use o SWPM (Gerenciador de provisionamento de software) do SAP como parte de uma instalação distribuída do NetWeaver na etapa "instalar instância de banco de dados".
* Use a ferramenta SAP HANA Database Lifecycle Manager (HDBLCM) e, em seguida, instale o NetWeaver.

Você também pode usar o SWPM para instalar todos os componentes, como SAP HANA, o servidor de aplicativos SAP e a instância ASCS, em uma única VM. As etapas são descritas neste [SAP Hana comunicado de blog](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Essa opção não está descrita neste guia de início rápido, mas os problemas que você deve considerar são os mesmos.

Antes de iniciar uma instalação, recomendamos que você leia a seção "preparar VMs do Azure para instalação manual do SAP HANA" mais adiante neste guia. Isso pode ajudar a evitar vários erros básicos que podem ocorrer quando você usa apenas uma configuração de VM do Azure padrão.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Principais etapas de instalação do SAP HANA ao usar o SAP SWPM
Esta seção lista as etapas principais para uma instalação manual do SAP HANA de instância única com o uso do SWPM da SAP para executar uma instalação distribuída do SAP NetWeaver 7.5. As etapas individuais são explicadas em mais detalhes nas capturas de tela neste documento.

1. Como criar uma rede virtual do Azure que inclui as duas VMs de teste.
2. Implante as duas VMs do Azure com sistemas operacionais de acordo com o modelo de Azure Resource Manager. Este exemplo usa SUSE Linux Enterprise Server e SLES para aplicativos SAP 12 SP1. 
3. Anexe dois discos de armazenamento Standard ou Premium do Azure, por exemplo, discos de 75 GB ou 500 GB, à VM do servidor de aplicativos.
4. Anexe discos de armazenamento premium à VM do servidor de HANA DB. Para obter mais informações, consulte a seção "configuração de disco" mais adiante neste guia.
5. Dependendo dos requisitos de tamanho ou taxa de transferência, anexe vários discos. Em seguida, crie volumes distribuídos. Use o LVM (gerenciamento de volume lógico) ou uma ferramenta de administração de vários dispositivos (mdadm) no nível do sistema operacional dentro da VM.
6. Como criar sistemas de arquivos XFS nos discos anexados/volumes lógicos.
7. Montar os novos sistemas de arquivos XFS no nível do sistema operacional. Use um sistema de arquivos para todos os softwares SAP. Use o sistema de arquivos para o diretório /sapmnt e backups, por exemplo. No servidor de banco de dados do SAP HANA, montar os sistemas de arquivos XFS nos discos de armazenamento premium como /hana e /usr/sap. Esse processo é necessário para impedir que o sistema de arquivos raiz seja preenchido. O sistema de arquivos raiz não é grande em VMs do Azure do Linux. 
8. Insira os endereços IP locais das VMs de teste no arquivo /etc/hosts.
9. Insira o parâmetro **nofail** no arquivo /etc/fstab.
10. Defina parâmetros de kernel do Linux de acordo com a versão do sistema operacional Linux que você usa. Para obter mais informações, consulte as notas SAP que discutem o HANA e a seção "parâmetros de kernel" neste guia.
11. Adicionar espaço de troca.
12. Como opção, instalar uma área de trabalho gráfica nas VMs de teste. Caso contrário, usar uma instalação remota SAPinst.
13. Baixe o software SAP no marketplace do serviço SAP.
14. Instalar a instância ASCS do SAP na VM do servidor de aplicativo.
15. Compartilhar o diretório /sapmnt entre as VMs de teste usando NFS. A VM do servidor de aplicativo é o servidor NFS.
16. Instale a instância do banco de dados, que inclui o HANA, usando SWPM na VM do servidor de BD.
17. Instalar o PAS (servidor de aplicativo principal) na VM do servidor de aplicativo.
18. Inicie o Console de gerenciamento do SAP (SAP MC). Conecte-se com o SAP GUI ou HANA Studio, por exemplo.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Principais etapas de instalação do SAP HANA ao usar HDBLCM
Esta seção lista as etapas principais para uma instalação manual do SAP HANA de instância única com o uso do SAP HDBLCM para executar uma instalação distribuída do SAP NetWeaver 7.5. As etapas individuais são explicadas em mais detalhes nas capturas de tela em toda esta guia.

1. Como criar uma rede virtual do Azure que inclui as duas VMs de teste.
2. Implante duas VMs do Azure com sistemas operacionais de acordo com o modelo de Azure Resource Manager. Este exemplo usa SLES e SLES para aplicativos SAP 12 SP1.
3. Anexe dois discos de armazenamento Standard ou Premium do Azure, por exemplo, discos de 75 GB ou 500 GB, à VM do servidor de aplicativos.
4. Anexe discos de armazenamento premium à VM do servidor de HANA DB. Para obter mais informações, consulte a seção "configuração de disco" mais adiante neste guia.
5. Dependendo dos requisitos de tamanho ou taxa de transferência, anexe vários discos. Crie volumes distribuídos usando o gerenciamento de volume lógico ou uma ferramenta mdadm no nível do sistema operacional dentro da VM.
6. Como criar sistemas de arquivos XFS nos discos anexados/volumes lógicos.
7. Montar os novos sistemas de arquivos XFS no nível do sistema operacional. Use um sistema de arquivos para todos os softwares SAP. Use o sistema de arquivos para o diretório /sapmnt e backups, por exemplo. No servidor de banco de dados do SAP HANA, montar os sistemas de arquivos XFS nos discos de armazenamento premium como /hana e /usr/sap. Esse processo é necessário para ajudar a impedir que o sistema de arquivos raiz seja preenchido. O sistema de arquivos raiz não é grande em VMs do Azure do Linux.
8. Insira os endereços IP locais das VMs de teste no arquivo /etc/hosts.
9. Insira o parâmetro **nofail** no arquivo /etc/fstab.
10. Defina parâmetros de kernel de acordo com a versão do sistema operacional Linux que você usa. Para obter mais informações, consulte as notas SAP que discutem o HANA e a seção "parâmetros de kernel" neste guia.
11. Adicionar espaço de troca.
12. Como opção, instalar uma área de trabalho gráfica nas VMs de teste. Caso contrário, usar uma instalação remota SAPinst.
13. Baixe o software SAP no marketplace do serviço SAP.
14. Crie um grupo, "sapsys", com o ID de grupo 1001 na VM do servidor de banco de dados do HANA.
15. Instale SAP HANA na VM do servidor de banco de dados usando o HANA Database Lifecycle Manager.
16. Instalar a instância ASCS do SAP na VM do servidor de aplicativo.
17. Compartilhar o diretório /sapmnt entre as VMs de teste usando NFS. A VM do servidor de aplicativo é o servidor NFS.
18. Instale a instância do banco de dados, que inclui o HANA, usando SWPM na VM do servidor de BD HANA.
19. Instale o servidor de aplicativos primário na VM do servidor de aplicativos.
20. Inicie o SAP MC. Conecte-se por meio do SAP GUI ou HANA Studio.

## <a name="prepare-azure-vms-for-a-manual-installation-of-sap-hana"></a>Preparar VMs do Azure para instalação manual do SAP HANA
Esta seção contém os seguintes tópicos:

* Atualizações do sistema operacional
* Configuração de disco
* Parâmetros de kernel
* Sistemas de arquivos
* O arquivo /etc/hosts
* O arquivo /etc/fstab

### <a name="os-updates"></a>Atualizações do sistema operacional
Verifique se há atualizações e correções do SO Linux antes de instalar o software adicional. Ao instalar um patch, você pode evitar uma chamada para o suporte técnico.

Certifique-se de usar:
* SUSE Linux Enterprise Server para aplicativos SAP.
* Red Hat Enterprise Linux para aplicativos SAP ou Red Hat Enterprise Linux para o SAP HANA. 

Se você ainda não tiver feito isso, registre a implantação do sistema operacional com sua assinatura do Linux do fornecedor do Linux. O SUSE tem imagens do sistema operacional para aplicativos SAP que já incluem serviços e que são registrados automaticamente.

Veja um exemplo de como verificar se há patches disponíveis para o SUSE Linux usando o comando **zypper** :

 `sudo zypper list-patches`

Dependendo do tipo de defeito, os patches são classificados por categoria e gravidade. Os valores comumente usados para a categoria são: 
- Segurança
- Recomendadas
- Opcional
- Recurso
- Document
- YaST

Valores comumente usados para gravidade são:

- Crítico
- Importante
- Moderado
- Baixo
- Não Especificado

O comando **zypper** procura somente as atualizações que os seus pacotes instalados precisam. Por exemplo, você pode usar este comando:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Você pode adicionar o parâmetro `--dry-run` para testar a atualização sem realmente atualizar o sistema.


### <a name="disk-setup"></a>Configuração de disco
O sistema de arquivos- raiz em uma VM do Linux no Azure tem limite de tamanho. Portanto, você precisa anexar espaço em disco adicional a uma VM do Azure para executar o SAP. Para VMs do Azure do servidor de aplicativos SAP, o uso de discos de armazenamento standard do Azure pode ser suficiente. Para SAP HANA VMs do Azure DBMS, o uso de discos de armazenamento Premium do Azure para implementações de produção e não produtos é obrigatório.

Com base nos [requisitos de armazenamento de TDI SAP Hana](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), a seguinte configuração de armazenamento Premium do Azure é sugerida: 

| SKU da VM | RAM |  hana/data e hana/log <br /> distribuído com LVM ou mdadm | /hana/shared | /root volume | /usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

Na configuração de disco sugerida, o volume de dados do HANA e o volume de log são colocados no mesmo conjunto de discos de armazenamento Premium do Azure que são distribuídos com LVM ou mdadm. Não é necessário definir nenhum nível de redundância RAID porque o armazenamento Premium do Azure mantém três imagens dos discos para redundância. 

Para configurar o armazenamento suficiente, consulte [requisitos de armazenamento de SAP Hana TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) e [Guia de instalação e atualização do SAP Hana Server](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Considere também os diferentes volumes de taxa de transferência de VHD (disco rígido virtual) dos diferentes discos de armazenamento Premium do Azure, conforme documentado em [armazenamento Premium de alto desempenho e em discos gerenciados para VMs](../../windows/disks-types.md). 

Você pode adicionar mais discos de armazenamento Premium às VMs do DBMS do HANA para armazenar backups de banco de dados ou de log de transações.

Para obter mais informações sobre as duas ferramentas principais usadas para configurar a distribuição, consulte:

* [Configure o RAID de software no Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Configurar o LVM em uma VM Linux no Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para obter mais informações sobre como anexar discos a VMs do Azure que executam o Linux como um sistema operacional convidado, consulte [Adicionar um disco a uma VM do Linux](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Com o SSDs Premium do Azure, você pode definir modos de cache de disco. Para o conjunto distribuído que contém/Hana/data e/Hana/log, desabilite o cache de disco. Para os outros volumes, ou seja, discos, defina o modo de cache como **ReadOnly**.

Para encontrar modelos JSON de exemplo a serem usados para criar VMs, consulte os [modelos de início rápido do Azure](https://github.com/Azure/azure-quickstart-templates).
O modelo vm-simple-sles é um modelo básico. Ele inclui uma seção de armazenamento com um disco de dados de 100 GB adicionais. Use este modelo como base. Você pode adaptar o modelo em sua configuração específica.

> [!NOTE]
> É importante anexar o disco de armazenamento do Azure usando um UUID, conforme documentado em [executar o SAP NetWeaver em VMs Microsoft Azure SuSE Linux](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

No ambiente de teste, dois discos de armazenamento standard do Azure são anexados à VM do servidor de aplicativos SAP, conforme mostrado na captura de tela a seguir. Um disco armazena todo o software SAP, como NetWeaver 7,5, SAP GUI e SAP HANA, para instalação. O segundo disco garante que espaço livre suficiente esteja disponível para requisitos adicionais. Por exemplo, os dados de backup e teste e o diretório/sapmnt, ou seja, perfis SAP, precisam ser compartilhados entre todas as VMs que pertencem à mesma estrutura SAP.

![A janela de discos do servidor de aplicativos SAP HANA exibe dois discos de dados e seus tamanhos](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Parâmetros de kernel
SAP HANA requer configurações específicas do kernel do Linux. Essas configurações de kernel não fazem parte das imagens padrão da galeria do Azure e devem ser definidas manualmente. Dependendo se você usar SUSE ou Red Hat, os parâmetros podem ser diferentes. As notas SAP listadas anteriormente fornecem informações sobre esses parâmetros. Nas capturas de tela mostradas, foi usado o SUSE Linux 12 SP1. 

SLES for SAP Applications 12-disponibilidade geral e SLES for SAP Applications 12 SP1 têm uma nova ferramenta, **ajustada-ADM**, que substitui a antiga ferramenta de **sapconf** . Um perfil especial do SAP HANA está disponível para o **tuned-adm**. Para ajustar o sistema para SAP HANA, insira o seguinte perfil como um usuário raiz:

   `tuned-adm profile sap-hana`

Para saber mais informações sobre o **tuned-adm**, confira a [documentação do SUSE sobre o tuned-adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

Na captura de tela a seguir, você pode ver como **ajustado-ADM** alterou os valores `transparent_hugepage` e `numa_balancing`, de acordo com as configurações de SAP Hana necessárias:

![A ferramenta tuned-adm altera valores de acordo com as configurações obrigatórias do SAP HANA](./media/hana-get-started/image005.jpg)

Para tornar as configurações de kernel SAP HANA permanentes, use **grub2** no SLES 12. Para obter mais informações sobre o **Grub2**, consulte a seção [estrutura do arquivo de configuração](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) da documentação do SUSE.

A captura de tela a seguir mostra como as configurações de kernel foram alteradas no arquivo de configuração e compiladas com o uso do **grub2-mkconfig**:

![Configurações de kernel alteradas no arquivo de configuração e compiladas usando grub2 mkconfig](./media/hana-get-started/image006.jpg)

Outra opção seria alterar as configurações usando o YaST e as configurações do **Carregador de Inicialização** > **Parâmetros de Kernel**:

![A guia de configurações Parâmetros de Kernel no carregador de inicialização do YaST](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Sistemas de arquivos
A captura de tela a seguir mostra dois sistemas de arquivos que foram criados na VM do servidor de aplicativo SAP usando os dois discos de armazenamento padrão do Azure anexados. Ambos os sistemas de arquivos são do tipo XFS e são montados em/sapdata e/sapsoftware.

Não é obrigatório estruturar os sistemas de arquivos dessa maneira. Você tem outras opções para estruturar o espaço em disco. O mais importante é evitar que o sistema de arquivo raiz fique sem espaço livre.

![Dois sistemas de arquivos criados na VM do servidor de aplicativos SAP](./media/hana-get-started/image008.jpg)

Para a VM do SAP HANA DB, durante uma instalação de banco de dados, quando você usa o SAPinst com SWPM e a opção de instalação **típica** , tudo é instalado em/Hana e/usr/SAP. O local padrão para o backup de log do SAP HANA é /usr/sap. Novamente, é importante impedir que o sistema de arquivos raiz fique sem espaço de armazenamento. Verifique se há espaço livre suficiente em/Hana e/usr/SAP antes de instalar SAP HANA usando SWPM.

Para obter uma descrição do layout padrão do sistema de arquivos de SAP HANA, consulte o [Guia de instalação e atualização do SAP Hana Server](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Outros sistemas de arquivos criados na VM do servidor de aplicativos SAP](./media/hana-get-started/image009.jpg)

Quando você instala o SAP NetWeaver em uma imagem SLES/SLES padrão para aplicativos SAP 12 da galeria do Azure, uma mensagem é exibida dizendo que não há espaço de permuta, conforme mostrado na captura de tela a seguir. Para ignorar essa mensagem, você pode adicionar um arquivo de permuta manualmente usando **dd**, **mkswap** e **swapon**. Para saber como fazer isso, pesquise "adicionando um arquivo de permuta manualmente" na seção [usando o particionador YaST](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) da documentação do SUSE.

Outra opção é configurar o espaço de troca por meio do agente de VM do Linux. Para saber mais, confira o [Guia do usuário do agente Linux para o Azure](../../extensions/agent-linux.md).

![Mensagem pop-up avisando que não há espaço de permuta suficiente](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>O arquivo /etc/hosts
Antes de começar a instalar o SAP, não se esqueça de incluir os nomes de host e endereços IP das VMs SAP no arquivo /etc/hosts. Implante todas as VMs SAP em uma rede virtual do Azure. Em seguida, use os endereços IP internos, conforme mostrado aqui:

![Nomes de host e endereços IP das VMs SAP listados no arquivo /etc/hosts](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>O arquivo /etc/fstab

É útil adicionar o parâmetro **nofail** ao arquivo fstab. Dessa forma, se algo der errado com os discos, a VM não deixará de responder no processo de inicialização. Mas, lembre-se de que o espaço em disco adicional pode não estar disponível e processos podem preencher o sistema de arquivos raiz. Se /hana estiver ausente, o SAP HANA não será iniciado.

![Adicione o parâmetro nofail ao arquivo fstab](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Área de trabalho GNOME gráfica no SLES 12/SLES para Aplicativos SAP 12
Esta seção explica como:

* Instale o desktop GNOME e o xrdp no SLES 12/SLES para aplicativos SAP 12.
* Execute o SAP MC baseado em Java usando o Firefox no SLES 12/SLES para aplicativos SAP 12.

Você também pode usar alternativas como Xterminal ou VNC, que não são descritas neste guia.

### <a name="install-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Instalar a área de trabalho GNOME e o xrdp no SLES 12/SLES para aplicativos SAP 12
Se você tiver um plano de fundo do Windows, poderá usar facilmente uma área de trabalho gráfica diretamente nas VMs do SAP Linux para executar o Firefox, o SAPinst, o SAP GUI, o SAP MC ou o HANA Studio. Em seguida, você pode se conectar à VM por meio do protocolo RDP (RDP) de um computador com Windows. Dependendo de suas políticas de empresa sobre a adição de GUIs a sistemas de produção e não-produtos baseados em Linux, talvez você queira instalar o GNOME no servidor. Siga estas etapas para instalar a área de trabalho GNOME em uma VM SLES 12/SLES para aplicativos SAP 12 do Azure.

1. Instale o desktop GNOME digitando o seguinte comando, por exemplo, em uma janela de reinicialização:

   `zypper in -t pattern gnome-basic`

2. Instale o xrdp para permitir uma conexão com a VM por meio de RDP:

   `zypper in xrdp`

3. Edite /etc/sysconfig/windowmanager e defina o gerenciador de janelas padrão como GNOME:

   `DEFAULT_WM="gnome"`

4. Execute o comando **chkconfig** para ter certeza de que o xrdp será iniciado automaticamente após a reinicialização:

   `chkconfig -level 3 xrdp on`

5. Se você tiver um problema com a conexão RDP, tente reiniciar, por exemplo, de uma janela de reinicialização:

   `/etc/xrdp/xrdp.sh restart`

6. Se uma reinicialização de xrdp mencionada na etapa anterior não funcionar, procure um arquivo .pid:

   `check /var/run` 

   Procure `xrdp.pid`. Se você achar, remova-o e tentar reiniciar novamente.

### <a name="start-sap-mc"></a>Iniciar o SAP MC
Depois de instalar a área de trabalho GNOME, inicie o SAP MC gráfico baseado em Java do Firefox. Se ele for executado em uma VM do Azure SLES 12/SLES para aplicativos SAP 12, ele poderá exibir um erro. O erro ocorre devido ao plug-in de navegador Java ausente.

A URL para iniciar o SAP MC é `<server>:5<instance_number>13`.

Para obter mais informações, consulte [iniciando o console de gerenciamento SAP baseado na Web](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

A captura de tela a seguir mostra a mensagem de erro exibida quando o plug-in do navegador Java está ausente:

![Mensagem de erro indicando a falta do plug-in de navegador Java](./media/hana-get-started/image013.jpg)

Uma maneira de resolver o problema é simplesmente instalar o plug-in ausente usando o YaST, como mostra a captura de tela a seguir:

![Usando o YaST para instalar o plug-in ausente](./media/hana-get-started/image014.jpg)

Ao inserir novamente a URL do console de gerenciamento do SAP, você será solicitado a ativar o plug-in:

![Caixa de diálogo solicitando a ativação do plug-in](./media/hana-get-started/image015.jpg)

Você também poderá receber uma mensagem de erro sobre um arquivo ausente, javafx.properties. Ele se relaciona com o requisito do Oracle Java 1,8 para SAP GUI 7,4. Para obter mais informações, consulte [SAP Note 2059429](https://launchpad.support.sap.com/#/notes/2059424).
A versão do IBM Java e o pacote OpenJDK que é fornecido com o SLES/SLES para aplicativos SAP 12 não incluem o arquivo JavaFX. Properties necessário. A solução é baixar e instalar o Java SE 8 da Oracle.

Para obter informações sobre um problema semelhante com openjdk no openSUSE, confira a discussão [Leap SAPGui 7.4 Java para openSUSE 42.1](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Instalação manual do SAP HANA: SWPM
A série de capturas de tela nesta seção mostra as principais etapas de como instalar o SAP NetWeaver 7,5 e SAP HANA SP12 ao usar o SWPM com SAPinst. Como parte de uma instalação do NetWeaver 7,5, o SWPM também pode instalar o banco de dados do HANA como uma única instância.

Em um ambiente de teste de exemplo, instalamos um servidor de aplicativo de ABAP (programação de aplicativos de negócios) avançado. Conforme mostrado na captura de tela a seguir, usamos a opção **Distributed System** para instalar as instâncias de servidor de aplicativo ASCS e Primary em uma VM do Azure. Nós usamos SAP HANA como o sistema de banco de dados em outra VM do Azure.

![Instâncias ASCS e de servidor de aplicativos principal instaladas usando a opção Sistema Distribuído](./media/hana-get-started/image012.jpg)

Depois que a instância ASCS é instalada na VM do servidor de aplicativos, ela é identificada por um ícone verde no console de gerenciamento do SAP. O diretório/sapmnt, que inclui o diretório de perfil SAP, deve ser compartilhado com a VM do servidor do SAP HANA DB. A etapa de instalação do banco de dados precisa acessar essas informações. A melhor maneira de fornecer acesso é usando NFS, que pode ser configurado com YaST.

![Console de gerenciamento SAP mostrando a instância de ASCS instalada na VM do servidor de aplicativos usando um ícone verde](./media/hana-get-started/image016.jpg)

Na VM do servidor de aplicativos, o diretório/sapmnt é compartilhado via NFS usando as opções **RW** e **no_root_squash** . Os padrões são **ro** e **root_squash**, o que pode levar a problemas durante a instalação da instância de banco de dados.

![Compartilhamento do diretório /sapmnt por meio de NFS usando as opções rw e no_root_squash](./media/hana-get-started/image017b.jpg)

Como mostra a próxima captura de tela, o compartilhamento/sapmnt da VM do servidor de aplicativos deve ser configurado na VM do servidor do SAP HANA DB usando o **cliente NFS** e o YaST:

![O compartilhamento/sapmnt configurado usando o cliente NFS](./media/hana-get-started/image018b.jpg)

Para executar uma instalação distribuída do NetWeaver 7,5, ou seja, uma **instância de banco de dados**, entre na VM do servidor do SAP Hana DB e inicie o SWPM:

![Instalação de uma instância de banco de dados entrando na VM do servidor de banco de dados do SAP HANA e iniciando o SWPM](./media/hana-get-started/image019.jpg)

Depois de selecionar a instalação **típica** e o caminho para a mídia de instalação, insira um SID de banco de usuários, o nome do host, o número da instância e a senha do administrador do sistema do BD:

![A página de entrada do administrador de sistema de banco de dados do SAP HANA](./media/hana-get-started/image035b.jpg)

Insira a senha para o esquema DBACOCKPIT:

![A caixa de entrada de senha para o esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Insira uma pergunta para a senha do esquema SAPABAP1:

![Insira uma pergunta para a senha do esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Depois que a tarefa é concluída, uma marca de seleção verde é exibida ao lado de cada fase do processo de instalação do BD. A mensagem "execução de... A instância do banco de dados foi concluída "é exibida.

![Janela de tarefa concluída com mensagem de confirmação](./media/hana-get-started/image023.jpg)

Após a instalação bem-sucedida, o console de gerenciamento do SAP também mostra a instância do banco de forma com um ícone verde. Ele exibe a lista completa de processos de SAP HANA, como hdbindexserver e hdbcompileserver.

![Janela do SAP Management Console com lista de processos do SAP HANA](./media/hana-get-started/image024.jpg)

A captura de tela a seguir mostra as partes da estrutura de arquivos no diretório /hana/shared criado pelo SWPM durante a instalação do HANA. Como não há nenhuma opção para especificar um caminho diferente, é importante montar espaço em disco adicional no diretório/Hana antes da instalação do SAP HANA usando o SWPM. Esta etapa impede que o sistema de arquivos raiz fique sem espaço livre.

![A estrutura de arquivos do diretório /hana/shared criada durante a instalação do HANA](./media/hana-get-started/image025.jpg)

Esta captura de tela mostra a estrutura de arquivos do diretório /usr/sap:

![A estrutura de arquivo de diretório /usr/sap](./media/hana-get-started/image026.jpg)

A última etapa da instalação da ABAP distribuída é a instância do servidor de aplicativos principal:

![Instalação da ABAP mostrando a instância do servidor de aplicativos principal como a última etapa](./media/hana-get-started/image027b.jpg)

Depois que a instância do servidor de aplicativos primário e a GUI do SAP estiverem instaladas, use a transação do **DBA cockpit** para confirmar se a instalação do SAP Hana foi concluída corretamente:

![Janela DBA Cockpit confirmando a instalação bem-sucedida](./media/hana-get-started/image028b.jpg)

Como etapa final, talvez você queira primeiro instalar o HANA Studio na VM do servidor de aplicativos SAP. Em seguida, conecte-se à instância de SAP HANA que está em execução na VM do servidor de BD.

![Instalando o SAP HANA Studio na VM do servidor de aplicativos SAP](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Instalação manual do SAP HANA: HDBLCM
Além de instalar o SAP HANA como parte de uma instalação distribuída usando SWPM, primeiro, você pode instalar o HANA autônomo usando HDBLCM. Em seguida, você pode instalar o SAP NetWeaver 7.5, por exemplo. As capturas de tela nesta seção mostram como esse processo funciona.

Para obter mais informações sobre a ferramenta HANA HDBLCM, confira:

* [Escolha a SAP Hana HDBLCM correta para a sua tarefa](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm).
* [SAP Hana ferramentas de gerenciamento do ciclo de vida](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm).
* [Guia de instalação e atualização do SAP Hana Server](https://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf).

Você deseja evitar problemas com uma configuração de ID de grupo padrão para o `\<HANA SID\>adm user`, que é criado pela ferramenta HDBLCM. Antes de instalar SAP HANA via HDBLCM, defina um novo grupo chamado `sapsys` usando a ID de grupo `1001`:

![Grupo novo "sapsys" definido usando a ID de grupo 1001](./media/hana-get-started/image030.jpg)

Quando você inicia o HDBLCM pela primeira vez, um menu iniciar simples é exibido. Selecione o item 1, **instalar novo sistema**:

![A opção "Instalar o novo sistema" na janela de início do HDBLCM](./media/hana-get-started/image031.jpg)

A captura de tela a seguir exibe todas as opções essenciais que você selecionou anteriormente.

> [!IMPORTANT]
> Os diretórios que são nomeados para volumes de dados e log do HANA e o caminho de instalação, que é/Hana/Shared neste exemplo, e/usr/SAP não devem fazer parte do sistema de arquivos raiz. Esses diretórios pertencem aos discos de dados do Azure que foram anexados à VM. Para obter mais informações, consulte a seção "configuração de disco". 

Essa abordagem ajuda a evitar que o sistema de arquivos raiz fique sem espaço. Observe que o administrador do sistema HANA tem a ID de usuário `1005` e faz parte do grupo de `sapsys`, com ID `1001`, que foi definido antes da instalação.

![Lista de todos os principais componentes do SAP HANA selecionados anteriormente](./media/hana-get-started/image032.jpg)

Verifique os detalhes do `\<HANA SID\>adm user` no diretório/etc/passwd Procure `azdadm`, conforme mostrado na seguinte captura de tela:

![HANA \<HANA SID\> detalhes do usuário de administração listados no diretório /etc/passwd](./media/hana-get-started/image033.jpg)

Depois de instalar o SAP HANA usando HDBLCM, é possível observar a estrutura de arquivos no SAP HANA Studio, conforme mostrado na seguinte captura de tela. O esquema SAPABAP1, que inclui, por exemplo, todas as tabelas do SAP NetWeaver, ainda não está disponível.

![Estrutura de arquivos do SAP HANA no SAP HANA Studio](./media/hana-get-started/image034.jpg)

Após a instalação do SAP HANA, você poderá instalar o SAP NetWeaver nele. Conforme mostrado na captura de tela a seguir, a instalação foi executada como uma instalação distribuída usando SWPM. Esse processo é descrito na seção anterior. Ao instalar a instância do banco de dados usando o SWPM, você insere os mesmos dados usando HDBLCM. Por exemplo, insira o nome do host, o SID do HANA e o número da instância. O SWPM usa a instalação do HANA existente e adiciona mais esquemas.

![Uma instalação distribuída realizada pelo SWPM](./media/hana-get-started/image035b.jpg)

A captura de tela a seguir mostra a etapa de instalação do SWPM onde você insere dados sobre o esquema DBACOCKPIT:

![A etapa de instalação do SWPM onde são inseridos os dados do esquema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Insira dados sobre o esquema SAPABAP1:

![Inserir dados sobre o esquema SAPABAP1](./media/hana-get-started/image037b.jpg)

Depois que a instalação da instância do banco de dados SWPM for concluída, você poderá ver o esquema SAPABAP1 no SAP HANA Studio:

![O esquema SAPABAP1 no SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Por fim, após a conclusão das instalações do servidor de aplicativos SAP e da GUI SAP, verifique a instância do banco de BD do HANA usando a transação do **DBA cockpit** :

![A instância de BD do HANA verificada com transação DBA Cockpit](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>Downloads de software SAP
Você pode baixar o software do Marketplace do serviço SAP como mostram as capturas de tela a seguir.

Baixar o NetWeaver 7.5 para Linux/HANA:

 ![Janela de instalação e atualização do serviço SAP para baixar o NetWeaver 7,5](./media/hana-get-started/image001.jpg)

Baixar o HANA SP12 Platform Edition:

 ![Janela de instalação e atualização do serviço SAP para baixar o HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)

