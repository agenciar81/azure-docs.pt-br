---
title: Consultar logs de Gerenciamento de Atualizações do Azure
description: Este artigo descreve como consultar os logs para Gerenciamento de Atualizações em seu espaço de trabalho do Log Analytics.
services: automation
ms.subservice: update-management
ms.date: 03/11/2020
ms.topic: conceptual
ms.openlocfilehash: f31168d47f31d8e740c95cb3d9e449f473cc78dc
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79216841"
---
# <a name="query-update-records-for-update-management-in-azure-monitor-logs"></a>Registros de atualização de consulta para Gerenciamento de Atualizações em logs de Azure Monitor

Além dos detalhes fornecidos na solução de Gerenciamento de Atualizações, você pode pesquisar nos logs armazenados em seu espaço de trabalho do Log Analytics. Na página solução, no painel esquerdo, selecione **logs**. A página **pesquisa de logs** é aberta.

Você também pode aprender como personalizar as consultas ou usá-las de diferentes clientes e muito mais visitando: [log Analytics a documentação da API de pesquisa](https://dev.loganalytics.io/).

## <a name="update-records"></a>Registros de atualização

Registros que são coletados por Gerenciamento de Atualizações para VMs Windows e Linux e os tipos de dados que aparecem nos resultados da pesquisa de log. As seções a seguir descrevem esses registros.

### <a name="required-updates"></a>Atualizações necessárias

É criado um registro com um tipo de `RequiredUpdate` que representa as atualizações exigidas por um computador. Esses registros têm as propriedades descritas na tabela a seguir:

| Propriedade | DESCRIÇÃO | 
|----------|-------------|
| Computador | Nome de domínio totalmente qualificado da máquina de relatórios. |
| KBID | ID do artigo da base de dados de conhecimento para o Windows Update. |
| ManagementGroupName | Nome do Operations Manager grupo de gerenciamento ou Log Analytics espaço de trabalho. | 
| Produto | Os produtos aos quais a atualização é aplicável. | 
| PublishDate | A data em que a atualização está pronta para ser baixada e instalada a partir de Windows Update. |
| Servidor | | 
| SourceHealthServiceId | Identificador exclusivo que representa o Log Analytics a ID do agente do Windows. |
| SourceSystem | *OperationsManager* | 
| TenantId | Identificador exclusivo que representa a instância de sua organização do Azure Active Directory. | 
| TimeGenerated | Data e hora de criação do registro. | 
| Type | *Atualização* | 
| UpdateClassification | Indica o tipo de atualizações que podem ser aplicadas. Para Windows:<br> *Atualizações críticas*<br> *Atualizações de segurança*<br> *Pacotes cumulativos de atualizações*<br> *Pacotes de recursos*<br> *Service Packs*<br> *Atualizações de definições*<br> *Ferramentas*<br> *Atualizações*. Para Linux:<br> *Atualizações críticas e de segurança*<br> *Outros* |
| UpdateSeverity | Classificação de severidade para a vulnerabilidade. Os valores são:<br> *Crítico*<br> *Importante*<br> *Media*<br> *Pequena* |
| UpdateTitle | O título da atualização.|

### <a name="update"></a>Atualizar

É criado um registro com um tipo de `Update` que representa as atualizações disponíveis e seu status de instalação para um computador. Esses registros têm as propriedades descritas na tabela a seguir:

| Propriedade | DESCRIÇÃO | 
|----------|-------------|
| Aprovação | Aplica-se somente ao sistema operacional Windows. O valor é *Microsoft Update*. |
| Aprovado | *Verdadeiro* ou *falso* |
| classificação | *Atualizações* |
| Computador | Nome de domínio totalmente qualificado da máquina de relatórios. |
| ComputerEnvironment | *Azure* ou *não Azure*. |
| MSRCBulletinID | Número da ID do boletim de segurança | 
| MSRCSeverity | Classificação de severidade para a vulnerabilidade. Os valores são:<br> *Crítico*<br> *Importante*<br> *Media*<br> *Pequena* |  
| KBID | ID do artigo da base de dados de conhecimento para o Windows Update. |
| ManagementGroupName | Nome do Operations Manager grupo de gerenciamento ou Log Analytics espaço de trabalho. |
| UpdateID | Identificador exclusivo da atualização de software. |
| RevisionNumber | O número de revisão de uma revisão específica de uma atualização. |
| Opcional | *Verdadeiro* ou *falso* | 
| RebootBehavior | O comportamento de reinicialização após instalar/desinstalar uma atualização. |
| _ResourceId | Identificador exclusivo do recurso ao qual o registro está associado. |
| Type | *Atualização* |
| VMUUID | Identificador exclusivo para a máquina virtual. |
| MG | Identificador exclusivo para o grupo de gerenciamento ou Log Analytics espaço de trabalho. | 
| TenantId | Identificador exclusivo que representa a instância de sua organização do Azure Active Directory. | 
| SourceSystem | *OperationsManager* | 
| TimeGenerated | Data e hora de criação do registro. | 
| SourceComputerId | Identificador exclusivo que representa o computador de origem. | 
| Title | O título da atualização. |
| PublishedDate (UTC) | A data em que a atualização está pronta para ser baixada e instalada a partir de Windows Update.  |
| UpdateState | O estado atual da atualização. | 
| Produto | Os produtos aos quais a atualização é aplicável. |
| SubscriptionId | Identificador exclusivo da assinatura do Azure. | 
| ResourceGroup | Nome do grupo de recursos do qual o recurso é membro. | 
| ResourceProvider | Especifica o provedor de recursos. | 
| Recurso | Nome do recurso. | 
| ResourceType | Nome do tipo de recurso. | 

### <a name="update-agent"></a>Atualizar Agente

É criado um registro com um tipo de `UpdateAgent` que fornece detalhes do agente de atualização no computador. Esses registros têm as propriedades descritas na tabela a seguir:

| Propriedade | DESCRIÇÃO | 
|----------|-------------|
| AgeofOldestMissingRequiredUpdate | | 
| AutomaticUpdateEnabled | | 
| Computador | Nome de domínio totalmente qualificado da máquina de relatórios. |
| DaySinceLastUpdateBucket | | 
| ManagementGroupName | Nome do Operations Manager grupo de gerenciamento ou Log Analytics espaço de trabalho. |
| OSVersion | A versão do sistema operacional. |
| Servidor | |
| SourceHealthServiceId | Identificador exclusivo que representa o Log Analytics a ID do agente do Windows. |
| SourceSystem | *OperationsManager* | 
| TenantId | Identificador exclusivo que representa a instância de sua organização do Azure Active Directory. |
| TimeGenerated | Data e hora de criação do registro. |
| Type | *Atualização* | 
| WindowsUpdateAgentVersion | Versão do agente de Windows Update. |
| WSUSServer | Mostra erros se o agente de Windows Update tiver um problema para auxiliar na solução de problemas. |

### <a name="update-deployment-status"></a>Atualizar status da implantação 

É criado um registro com um tipo de `UpdateRunProgress` que fornece o status de implantação de atualização de uma implantação agendada por computador. Esses registros têm as propriedades descritas na tabela a seguir:

| Propriedade | DESCRIÇÃO | 
|----------|-------------|
| Computador | Nome de domínio totalmente qualificado da máquina de relatórios. |
| ComputerEnvironment | *Azure* ou *não Azure*. | 
| CorrelationId | Identificador exclusivo da execução do trabalho de runbook para a atualização. |
| EndTime | A hora em que o processo de sincronização terminou. | 
| ErrorResult | Windows Update código de erro gerado se uma atualização não for instalada. | 
| InstallationStatus | Os Estados de instalação possíveis de uma atualização no computador cliente, *em andamento*, com *êxito*, *falharam parcialmente*. |
| KBID | ID do artigo da base de dados de conhecimento para o Windows Update. | 
| ManagementGroupName | Nome do Operations Manager grupo de gerenciamento ou Log Analytics espaço de trabalho. |
| OSType | Especifica o tipo de sistema operacional, *Windows* ou *Linux*. | 
| Produto | Os produtos aos quais a atualização é aplicável. |
| Recurso | Nome do recurso. | 
| ResourceId | Identificador exclusivo do recurso ao qual o registro está associado. |
| ResourceProvider | Especifica o provedor de recursos. | 
| ResourceType | Nome do tipo de recurso. | 
| SourceComputerId | Identificador exclusivo que representa o computador de origem. | 
| SourceSystem | *OperationsManager* |
| StartTime | Hora em que a atualização está agendada para ser instalada. |
| SubscriptionId | Identificador exclusivo da assinatura do Azure. | 
| SucceededOnRetry | Mostra quando a execução da atualização falhou na primeira tentativa e a operação atual é uma tentativa de repetição. |
| TimeGenerated | Data e hora de criação do registro. |
| Title | O título da atualização. |
| Type | *UpdateRunProgress* |
| UpdateId | Identificador exclusivo da atualização de software. |
| VMUUID | Identificador exclusivo para a máquina virtual. |
| _ResourceId | Identificador exclusivo do recurso ao qual o registro está associado. |

### <a name="update-summary"></a>Resumo da Atualização 

É criado um registro com um tipo de `UpdateSummary` que fornece o resumo da atualização por computador. Esses registros têm as propriedades descritas na tabela a seguir:

| Propriedade | DESCRIÇÃO | 
|----------|-------------|
| Computador | Nome de domínio totalmente qualificado da máquina de relatórios. |
| ComputerEnvironment | *Azure* ou *não Azure*. | 
| CriticalUpdatesMissing | Número de atualizações críticas ausentes aplicáveis. | 
| ManagementGroupName | Nome do Operations Manager grupo de gerenciamento ou Log Analytics espaço de trabalho. |
| NETRuntimeVersion | Versão do .NET Framework instalada no computador com Windows. |
| OldestMissingSecurityUpdateBucket | Os valores são:<br> *Recente*<br> *30 dias atrás*<br> *60 dias atrás*<br> *Velho* | 
| OldestMissingSecurityUpdateInDays | Número total de dias para a atualização mais antiga detectada, conforme aplicável, que não foi instalado. |
| OsVersion | A versão do sistema operacional. |
| OtherUpdatesMissing | Contagem de atualizações detectadas ausentes. |
| Recurso |  Nome do recurso. | 
| ResourceGroup | Nome do grupo de recursos do qual o recurso é membro. |
| ResourceId | Identificador exclusivo do recurso ao qual o registro está associado. |
| ResourceProvider | Especifica o provedor de recursos. |
| ResourceType | Nome do tipo de recurso. |
| RestartPending | *True* ou *False*. |
| SecurityUpdatesMissing | Contagem de atualizações de segurança ausentes aplicáveis.| 
| SourceComputerId | Identificador exclusivo para a máquina virtual. |
| SourceSystem | *OpsManager* | 
| SubscriptionId | Identificador exclusivo da assinatura do Azure. |
| TimeGenerated | Data e hora de criação do registro. |
| TotalUpdatesMissing | Número total de atualizações ausentes aplicáveis. | 
| Type | *UpdateSummary* |
| VMUUID | Identificador exclusivo para a máquina virtual. |
| WindowsUpdateAgentVersion | Versão do agente de Windows Update. |
| WindowsUpdateSetting | Mostra o status do agente de Windows Update. Os valores possíveis são:<br> *Instalação agendada*<br> *Notificar antes da instalação*<br> Erro retornado do agente do WUA não íntegro. | 
| WSUSServer | Mostra erros se o agente de Windows Update tiver um problema para auxiliar na solução de problemas. |
| _ResourceId | Identificador exclusivo do recurso ao qual o registro está associado. |

## <a name="sample-queries"></a>Consultas de exemplo

As seções a seguir fornecem exemplos de consultas de log para registros de atualização que são coletados para Gerenciamento de Atualizações.

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Confirmar se computadores não Azure estão integrados

Para confirmar se os computadores conectados diretamente estão se comunicando com os logs de Azure Monitor, execute uma das pesquisas de log a seguir.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Em um computador com Windows, você pode examinar as seguintes informações para verificar a conectividade do agente com os logs de Azure Monitor:

1. No painel de controle, abra o **Microsoft Monitoring Agent**. Na guia **Log Analytics do Azure**, o agente exibe a seguinte mensagem: **O Microsoft Monitoring Agent se conectou com êxito ao Log Analytics**.
2. Abra o Log de Eventos do Windows. Navegue até **Logs de Aplicativos e Serviços\Operations Manager** e procure as IDs de Evento 3000 e 5002 do **Conector de Serviço** de origem. Esses eventos indicam que o computador foi registrado com o espaço de trabalho do Log Analytics e está recebendo a configuração.

Se o agente não puder se comunicar com os logs de Azure Monitor e o agente estiver configurado para se comunicar com a Internet por meio de um servidor proxy ou firewall, confirme se o firewall ou o servidor proxy está configurado corretamente. Para saber como verificar se o firewall ou o servidor proxy está configurado corretamente, veja [Configuração de rede para agente do Windows](../azure-monitor/platform/agent-windows.md) ou [Configuração de rede para agente do Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Se os seus sistemas Linux estiverem configurados para se comunicar com um proxy ou com o Log Analytics Gateway e você estiver integrando essa solução, atualize as permissões *proxy.conf* para conceder ao grupo omiuser permissão de leitura no arquivo usando os seguintes comandos:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Agentes do Linux recém-adicionados mostram um status de **Atualizado** após ter sido realizada uma avaliação. Esse processo pode levar até seis horas.

Para confirmar que um grupo de gerenciamento de Operations Manager está se comunicando com os logs de Azure Monitor, consulte [validar Operations Manager integração com os logs de Azure monitor](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

### <a name="single-azure-vm-assessment-queries-windows"></a>Consultas de Avaliação de VM única do Azure (Windows)

Substitua o valor VMUUID pelo GUID da VM da máquina virtual que você está consultando. Você pode encontrar o VMUUID que deve ser usado executando a seguinte consulta nos logs de Azure Monitor: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Resumo das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Lista das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

### <a name="single-azure-vm-assessment-queries-linux"></a>Consultas de avaliação de VM única do Azure (Linux)

Para alguns distribuições do Linux, há uma incompatibilidade de [endian](https://en.wikipedia.org/wiki/Endianness) com o valor de VMUUID que vem de Azure Resource Manager e o que é armazenado em logs de Azure monitor. A consulta a seguir procura uma correspondência em qualquer endianness. Substitua os valores VMUUID pelo formato big-endian e little-endian do GUID para retornar corretamente os resultados. Você pode encontrar o VMUUID que deve ser usado executando a seguinte consulta nos logs de Azure Monitor: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

#### <a name="missing-updates-summary"></a>Resumo das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="missing-updates-list"></a>Lista das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

### <a name="multi-vm-assessment-queries"></a>Consultas de avaliação de várias VMs

#### <a name="computers-summary"></a>Resumo de computadores

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

#### <a name="missing-updates-summary"></a>Resumo das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

#### <a name="computers-list"></a>Lista de computadores

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

#### <a name="missing-updates-list"></a>Lista das atualizações ausentes

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="next-steps"></a>Próximas etapas

* Use pesquisas de log em [logs de Azure monitor](../log-analytics/log-analytics-log-searches.md) para exibir dados de atualização detalhados.
* [Criar alertas](automation-tutorial-update-management.md#configure-alerts) para status de implantação de atualização.
