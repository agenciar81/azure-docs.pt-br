---
title: Restrições de nomenclatura de recursos
description: Mostra as regras e restrições para nomear recursos do Azure.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 7b6a9e16ba28a2a0e5e4e181dc5650d2110eab88
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79273911"
---
# <a name="naming-rules-and-restrictions-for-azure-resources"></a>Regras de nomenclatura e restrições para recursos do Azure

Este artigo resume as regras de nomenclatura e as restrições dos recursos do Azure. Para obter recomendações sobre como nomear recursos, consulte [pronto: convenções de nomenclatura e marcação recomendadas](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).

Os nomes de recursos não diferenciam maiúsculas de minúsculas, a menos que sejam especificamente indicados na coluna caracteres válidos.

Nas tabelas a seguir, o termo alfanumérico refere-se a:

* **a** a **z** (letras minúsculas)
* **A** a **Z** (letras maiúsculas)
* **0** a **9** (números)

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | grupo de recursos | 3-63 | Letras minúsculas e números.<br><br>Comece com uma letra minúscula. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | serviço | global | 1-50 | Alfanuméricos.<br><br>Comece com a letra. |
> | serviço/APIs | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/problemas | api | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/problemas/anexos | lo | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/problemas/comentários | lo | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/operações | api | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/operações/marcas | operação | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/versões | api | 1-80 | Alfanuméricos, sublinhados e hifens.<br><br>Comece e termine com caracteres alfanuméricos ou sublinhados. |
> | serviços/APIs/esquemas | api | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/tagDescriptions | api | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/APIs/marcas | api | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/API – conjuntos de versão | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/authorizationServers | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/back-ends | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/certificados | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/diagnóstico | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/grupos | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/grupos/usuários | group | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/identityProviders | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/agentes | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/notificações | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/notificações/recipientEmails | notificação | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/openidConnectProviders | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/políticas | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviços/produtos | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/produtos/APIs | product | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/produtos/grupos | product | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/produtos/marcas | product | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/Propriedades | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/assinaturas | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/marcas | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/modelos | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |
> | serviço/usuários | serviço | 1-256 | Não é possível usar:<br> `*#&+:<>?` |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | configurationStores | grupo de recursos | 5 a 50 | Alfanuméricos, sublinhados e hifens. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | locks | escopo da atribuição | 1-90 | Alfanuméricos, pontos, sublinhados, hifens e parênteses.<br><br>Não pode terminar no período. |
> | policyassignments | escopo da atribuição | 1-128 nome para exibição<br><br>1-260 nome do recurso | O nome para exibição pode conter qualquer caractere.<br><br>O nome do recurso não pode incluir `%` e não pode terminar com o período ou o espaço. |
> | PolicyDefinitions | escopo da definição | 1-128 nome para exibição<br><br>1-260 nome do recurso | O nome para exibição pode conter qualquer caractere.<br><br>O nome do recurso não pode incluir `%` e não pode terminar com o período ou o espaço. |
> | policySetDefinitions | escopo da definição | 1-128 nome para exibição<br><br>1-260 nome do recurso | O nome para exibição pode conter qualquer caractere.<br><br>O nome do recurso não pode incluir `%` e não pode terminar com o período ou o espaço.  |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | automationaccounts | grupo de recursos | 6-50 | Alfanuméricos e hifens.<br><br>Comece com letra e termine com alfanumérico. |
> | automationAccounts/certificados | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço.  |
> | automationAccounts/conexões | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço. |
> | automationAccounts/credenciais | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço. |
> | automationAccounts/runbooks | conta de automação | 1-63 | Alfanuméricos, sublinhados e hifens.<br><br>Comece com a letra.  |
> | automationAccounts/agendas | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço. |
> | automationAccounts/variáveis | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço. |
> | automationAccounts/inspetores | conta de automação | 1-63 |  Alfanuméricos, sublinhados e hifens.<br><br>Comece com a letra. |
> | automationAccounts/WebHooks | conta de automação | 1-128 | Não é possível usar:<br> `<>*%&:\?.+/` <br><br>Não é possível terminar com espaço. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | batchAccounts | Região | 3-24 | Letras minúsculas e números. |
> | batchAccounts/aplicativos | conta do lote | 1-64 | Alfanuméricos, sublinhados e hifens. |
> | batchAccounts/certificados | conta do lote | 5-45 | Alfanuméricos, sublinhados e hifens. |
> | batchAccounts/pools | conta do lote | 1-64 | Alfanuméricos, sublinhados e hifens. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | blockchainMembers | global | 2-20 | Letras minúsculas e números.<br><br>Comece com uma letra minúscula. |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | botServices | global | 2-64 |  Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. |
> | botServices/canais | serviço de bot | 2-64 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. |
> | botServices/conexões | serviço de bot | 2-64 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. |
> | enterpriseChannels | grupo de recursos | 2-64 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | Redis | global | 1-63 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. Hifens consecutivos não são permitidos. |
> | Redis/firewallRules | Redis | 1-256 | Alfanuméricos |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | perfis | grupo de recursos | 1-260 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |
> | perfis/pontos de extremidade | global | 1-50 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | certificateOrders | grupo de recursos | 3-30 | Alfanuméricos. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | grupo de recursos | 2-64 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | availabilitySets | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Terminar com alfanumérico ou sublinhado. |
> | diskEncryptionSets | grupo de recursos | 1-80 | Caracteres alfanuméricos e sublinhados. |
> | disks | grupo de recursos | 1-80 | Caracteres alfanuméricos e sublinhados. |
> | galleries | grupo de recursos | 1-80 | Alfanuméricos e pontos.<br><br>Comece e termine com alfanumérico. |
> | galerias/aplicativos | clip | 1-80 | Alfanuméricos, hifens e pontos.<br><br>Comece e termine com alfanumérico. |
> | galerias/aplicativos/versões | aplicativo | Inteiro de 32 bits | Números e pontos. |
> | galerias/imagens | clip | 1-80 | Alfanuméricos, hifens e pontos.<br><br>Comece e termine com alfanumérico. |
> | galerias/imagens/versões | image | Inteiro de 32 bits | Números e pontos. |
> | images | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Terminar com alfanumérico ou sublinhado. |
> | snapshots | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Terminar com alfanumérico ou sublinhado. |
> | virtualMachines | grupo de recursos | 1-15 (Windows)<br>1-64 (Linux)<br><br>Consulte a observação abaixo. | Não é possível usar:<br> `\/""[]:|<>+=;,?*@&`<br><br>Não é possível começar com sublinhado. Não é possível terminar com ponto ou hífen. |
> | virtualMachineScaleSets | grupo de recursos | 1-15 (Windows)<br>1-64 (Linux)<br><br>Consulte a observação abaixo. | Não é possível usar:<br> `\/""[]:|<>+=;,?*@&`<br><br>Não é possível começar com sublinhado. Não é possível terminar com ponto ou hífen. |

> [!NOTE]
> As máquinas virtuais do Azure têm dois nomes distintos: nome do recurso e nome do host. Quando você cria uma máquina virtual no portal, o mesmo valor é usado para ambos os nomes. As restrições na tabela anterior são para o nome do host. O nome real do recurso pode ter até 64 caracteres.

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | containerGroups | grupo de recursos | 1-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. Hifens consecutivos não são permitidos. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | registries | global | 5 a 50 | Alfanuméricos. |
> | registros/buildTasks | Registro | 5 a 50 | Alfanuméricos. |
> | registros/buildTasks/etapas | tarefa de build | 5 a 50 | Alfanuméricos. |
> | registros/replicações | Registro | 5 a 50 | Alfanuméricos. |
> | registros/scopeMaps | Registro | 5 a 50 | Alfanuméricos, hifens e sublinhados. |
> | registros/tarefas | Registro | 5 a 50 | Alfanuméricos, hifens e sublinhados. |
> | registros/tokens | Registro | 5 a 50 | Alfanuméricos, hifens e sublinhados. |
> | registros/WebHooks | Registro | 5 a 50 | Alfanuméricos. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | managedClusters | grupo de recursos | 1-63 | Alfanuméricos, sublinhados e hifens.<br><br>Comece e termine com alfanumérico. |
> | openShiftManagedClusters | grupo de recursos | 1-30 | Alfanuméricos. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | hubs | grupo de recursos | 1-64 | Alfanuméricos.<br><br>Comece com a letra.  |
> | hubs/authorizationPolicies | 82801ER | 1-50 | Alfanuméricos, sublinhados e pontos.<br><br>Comece e termine com alfanumérico. |
> | hubs/conectores | 82801ER | 1-128 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/conectores/mapeamentos | conector | 1-128 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/interações | 82801ER | 1-128 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/KPI | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/links | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/previsões | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/perfis | 82801ER | 1-128 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/relationshipLinks | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/relações | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/roleAssignments | 82801ER | 1-128 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |
> | hubs/exibições | 82801ER | 1-512 | Caracteres alfanuméricos e sublinhados.<br><br>Comece com a letra. |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | associações | grupo de recursos | 1-180 | Não é possível usar:<br>`%&\\?/`<br><br>Não é possível terminar com período ou espaço. |
> | resourceProviders | grupo de recursos | 3-64 | Não é possível usar:<br>`%&\\?/`<br><br>Não é possível terminar com período ou espaço. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | jobs | grupo de recursos | 3-24 | Alfanuméricos, hifens, sublinhados e pontos. |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaces | grupo de recursos | 3-30 | Alfanuméricos, sublinhados e hifens |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | factories | global | 3-63 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |
> | fábricas/fluxos de fluxo de os | fábrica | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |
> | fábricas/conjuntos de | fábrica | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |
> | fábricas/integrationRuntimes | fábrica | 3-63 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |
> | fábricas/linksservices | fábrica | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |
> | fábricas/pipelines | fábrica | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |
> | fábricas/gatilhos | fábrica | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |
> | fábricas/gatilhos/rerunTriggers | gatilho | 1-260 | Não é possível usar:<br>`<>*#.%&:\\+?/`<br><br>Comece com alfanumérico. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Letras minúsculas e números. |
> | contas/computePolicies | account | 3-60 | Alfanuméricos, hifens e sublinhados. |
> | contas/dataLakeStoreAccounts | account | 3-24 | Letras minúsculas e números. |
> | contas/firewallRules | account | 3-50 | Alfanuméricos, hifens e sublinhados. |
> | contas/storageAccounts | account | 3-60 | Alfanuméricos, hifens e sublinhados. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | global | 3-24 | Letras minúsculas e números. |
> | contas/firewallRules | account | 3-50 | Alfanuméricos, hifens e sublinhados. |
> | contas/virtualNetworkRules | account | 3-50 | Alfanuméricos, hifens e sublinhados. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | services | grupo de recursos | 2-62 | Alfanuméricos, hifens, pontos e sublinhados.<br><br>Comece com alfanumérico. |
> | serviços/projetos | serviço | 2-57 | Alfanuméricos, hifens, pontos e sublinhados.<br><br>Comece com alfanumérico. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, hifens e números.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servidores/bancos de dados | servers | 1-63 | Alfanuméricos e hifens. |
> | servidores/firewallRules | servers | 1-128 | Alfanuméricos, hifens e sublinhados. |
> | servidores/virtualNetworkRules | servers | 1-128 | Alfanuméricos e hifens. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, hifens e números.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servidores/bancos de dados | servers | 1-63 | Alfanuméricos e hifens. |
> | servidores/firewallRules | servers | 1-128 | Alfanuméricos, hifens e sublinhados. |
> | servidores/virtualNetworkRules | servers | 1-128 | Alfanuméricos e hifens. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | servers | global | 3-63 | Letras minúsculas, hifens e números.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servidores/bancos de dados | servers | 1-63 | Alfanuméricos e hifens. |
> | servidores/firewallRules | servers | 1-128 | Alfanuméricos, hifens e sublinhados. |
> | servidores/virtualNetworkRules | servers | 1-128 | Alfanuméricos e hifens. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | IotHubs | global | 3-50 | Alfanuméricos e hifens.<br><br>Não é possível terminar com hífen. |
> | IotHubs/certificados | Hub IoT | 1-64 | Alfanuméricos, hifens, pontos e sublinhados. |
> | IotHubs/eventHubEndpoints/ConsumerGroups | eventHubEndpoints | 1-50 | Alfanuméricos, hifens, pontos e sublinhados. |
> | provisioningServices | grupo de recursos | 3-64 | Alfanuméricos e hifens.<br><br>Terminar com alfanumérico. |
> | provisioningServices/certificados | provisioningServices | 1-64 | Alfanuméricos, hifens, pontos e sublinhados. |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | labs | grupo de recursos | 1-50 | Alfanuméricos, sublinhados e hifens. |
> | laboratórios/customimages | laboratórios | 1-80 | Alfanuméricos, sublinhados, hifens e parênteses. |
> | laboratórios/fórmulas | laboratórios | 1-80 | Alfanuméricos, sublinhados, hifens e parênteses. |
> | laboratórios/VirtualMachines | laboratórios | 1-15 (Windows)<br>1-64 (Linux) | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. Não pode ser todos os números. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | databaseAccounts | global | 3-31 | Letras minúsculas, números e hifens.<br><br>Comece com letra minúscula ou número. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | domains | grupo de recursos | 3-50 | Alfanuméricos e hifens. |
> | domínios/tópicos | domínio | 3-50 | Alfanuméricos e hifens. |
> | eventSubscriptions | grupo de recursos | 3-64 | Alfanuméricos e hifens. |
> | topics | grupo de recursos | 3-50 | Alfanuméricos e hifens. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | grupo de recursos | 6-50 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com letra ou número. |
> | namespaces | global | 6-50 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com letra ou número. |
> | namespaces/AuthorizationRules | namespace | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com letra ou número. |
> | namespaces/disasterRecoveryConfigs | namespace | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com letra ou número. |
> | namespaces/Eventhubs | namespace | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com letra ou número. |
> | namespaces/Eventhubs/authorizationRules | Hub de eventos | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com letra ou número. |
> | namespaces/Eventhubs/consumergroups | Hub de eventos | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com letra ou número. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | global | 3-59 | Alfanuméricos e hifens<br><br>Comece e termine com letra ou número. |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | jobs | grupo de recursos | 2-64 | Alfanuméricos e hifens.<br><br>Comece com a letra. |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | IoTApps | global | 2-63 | Letras minúsculas, números e hifens.<br><br>Comece com letra minúscula ou número. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | vaults | global | 3-24 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com letra ou dígito. Não pode conter hifens consecutivos. |
> | cofres/segredos | Cofre | 1-127 | Alfanuméricos e hifens. |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | global | 4-22 | Letras minúsculas e números.<br><br>Comece com a letra. |
> | /clusters/bancos de dados | cluster | 1-260 | Alfanuméricos, hifens, espaços e pontos. |
> | /clusters/bancos de dados/conexões | Banco de Dados | 1-40 | Alfanuméricos, hifens, espaços e pontos. |
> | /clusters/bancos de dados/eventhubconnections | Banco de Dados | 1-40 | Alfanuméricos, hifens, espaços e pontos. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | integrationAccounts | grupo de recursos | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts/assemblies | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts / batchConfigurations | conta de integração | 1-20 | Alfanuméricos. |
> | integrationAccounts/certificados | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts/Maps | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts/parceiros | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts / rosettanetprocessconfigurations | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts/esquemas | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationAccounts/sessões | conta de integração | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |
> | integrationServiceEnvironments | grupo de recursos | 1-80 | Alfanuméricos, hifens, pontos e sublinhados. |
> | integrationServiceEnvironments / managedApis | ambiente do serviço de integração | 1-80 | Alfanuméricos, hifens, pontos e sublinhados. |
> | workflows | grupo de recursos | 1-80 | Alfanuméricos, hifens, sublinhados, pontos e parênteses. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | commitmentPlans | grupo de recursos | 1-260 | Não é possível usar:<br>`<>*%&:?+/\\`<br><br>Não é possível terminar com um espaço. |
> | webServices | grupo de recursos | 1-260 | Não é possível usar:<br>`<>*%&:?+/\\`<br><br>Não é possível terminar com um espaço. |
> | workspaces | grupo de recursos | 1-260 | Não é possível usar:<br>`<>*%&:?+/\\`<br><br>Não é possível terminar com um espaço. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaces | grupo de recursos | 3-33 | Alfanuméricos e hifens. |
> | espaços de trabalho/computações | workspace | 2-16 | Alfanuméricos e hifens. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | userAssignedIdentities | grupo de recursos | 3-128 | Alfanuméricos, hifens e sublinhados<br><br>Comece com letra ou número. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | accounts | grupo de recursos | 1-98 (para nome do grupo de recursos e nome da conta) | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | mediaservices | grupo de recursos | 3-24 | Letras minúsculas e números. |
> | mediaservices/liveEvents | Serviço de mídia | 1-32 | Alfanuméricos e hifens.<br><br>Comece com alfanumérico. |
> | mediaservices/liveEvents/liveOutputs | Evento ao vivo | 1-256 | Alfanuméricos e hifens.<br><br>Comece com alfanumérico. |
> | mediaservices/streamingEndpoints | Serviço de mídia | 1-24 | Alfanuméricos e hifens.<br><br>Comece com alfanumérico. |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | applicationGateways | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | applicationSecurityGroups | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | azureFirewalls | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Terminar com alfanumérico ou sublinhado. |
> | bastionHosts | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | connections | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | dnsZones | grupo de recursos | 1-63 caracteres<br><br>2 a 34 rótulos<br><br>Cada rótulo é um conjunto de caracteres separados por um ponto. Por exemplo, **contoso.com** tem 2 rótulos. | Cada rótulo pode conter alfanuméricos, sublinhados e hifens.<br><br>Cada rótulo é separado por um ponto. |
> | expressRouteCircuits | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | firewallPolicies | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | firewallPolicies/grupo | Política de firewall | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | frontDoors | global | 5-64 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |
> | loadBalancers | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | Balanceadores de carga/inboundNatRules | balanceador de carga | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | virtualNetworkGateways | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | networkInterfaces | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | networkSecurityGroups | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | networkSecurityGroups/securityRules | grupo de segurança de rede | 1-80 |  Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | networkWatchers | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | privateDnsZones | grupo de recursos | 1-63 caracteres<br><br>2 a 34 rótulos<br><br>Cada rótulo é um conjunto de caracteres separados por um ponto. Por exemplo, **contoso.com** tem 2 rótulos. | Cada rótulo pode conter alfanuméricos, sublinhados e hifens.<br><br>Cada rótulo é separado por um ponto. |
> | privateDnsZones / virtualNetworkLinks | zona DNS privada | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | publicIPAddresses | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | publicIPPrefixes | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | routeFilters | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | routeFilters / routeFilterRules | filtro de rota | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | routeTables | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | routeTables/rotas | route table | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | serviceEndpointPolicies | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | trafficmanagerprofiles | global | 1-63 | Alfanuméricos, hifens e pontos.<br><br>Comece e termine com alfanumérico. |
> | virtualNetworkGateways | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | virtualNetworks | grupo de recursos | 2-64 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | virtualnetworks/sub-redes | rede virtual | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | virtualNetworks/virtualNetworkPeerings | rede virtual | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | virtualWans | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | vpnGateways | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | vpnGateways / vpnConnections | gateway de VPN | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |
> | vpnSites | grupo de recursos | 1-80 | Alfanuméricos, sublinhados, pontos e hifens.<br><br>Comece com alfanumérico. Fim alfanumérico ou sublinhado. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | namespaces | global | 6-50 | Alfanuméricos e hifens<br><br>Comece e termine com alfanumérico. |
> | namespaces/AuthorizationRules | namespace | 1-256 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Iniciar alfanumérico. |
> | namespaces/notificationHubs | namespace | 1-260 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Iniciar alfanumérico. |
> | namespaces/notificationHubs/AuthorizationRules | Hub de notificação | 1-256 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Iniciar alfanumérico. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | grupo de recursos | 4-63 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |
> | workspaces | grupo de recursos | 4-63 | Alfanuméricos e hifens.<br><br>Comece e termine com alfanumérico. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | dashboards | grupo de recursos | 3-160 | Alfanuméricos e hifens.<br><br>Para usar caracteres restritos, adicione uma marca denominada **Hidden-título** com o nome do painel que você deseja usar. O portal exibe esse nome ao mostrar o painel. |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | workspaceCollections | region | 3-63 | Alfanuméricos e hifens.<br><br>Não é possível começar com hífen. Não é possível usar hifens consecutivos. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | capacities | region | 3-63 | Letras minúsculas ou números<br><br>Comece com uma letra minúscula. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | vaults | grupo de recursos | 2-50 | Alfanuméricos e hifens.<br><br>Comece com a letra. |
> | cofres/backupPolicies | cofre | 3-150 | Alfanuméricos e hifens.<br><br>Comece com a letra. Não é possível terminar com hífen. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | namespaces | global | 6-50 | Alfanuméricos e hifens.<br><br>Comece com uma letra. Terminar com uma letra ou número. |
> | namespaces/AuthorizationRules | namespace | 1-50 |  Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alfanumérico. |
> | namespaces/HybridConnections | namespace | 1-260 | Alfanuméricos, pontos, hifens, sublinhados e barras.<br><br>Comece e termine com alfanumérico. |
> | namespaces/HybridConnections/authorizationRules | conexão híbrida | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alfanumérico. |
> | namespaces/WcfRelays | namespace | 1-260 | Alfanuméricos, pontos, hifens, sublinhados e barras.<br><br>Comece e termine com alfanumérico. |
> | namespaces/WcfRelays/authorizationRules | Retransmissão do WCF | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alfanumérico. |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | deployments | grupo de recursos | 1-64 | Alfanuméricos, sublinhados, parênteses, hifens e pontos. |
> | resourcegroups | subscription | 1-90 | Alfanuméricos, sublinhados, parênteses, hifens, pontos e caracteres Unicode que correspondem à [documentação do Regex](/rest/api/resources/resourcegroups/createorupdate).<br><br>Não é possível terminar com período. |
> | tagNames | recurso | 1-512 | Não é possível usar:<br>`<>%&\?/` |
> | tagNames/tagValues | nome da marca | 1-256 | Todos os caracteres. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | namespaces | global | 6-50 | Alfanuméricos e hifens.<br><br>Comece com uma letra. Terminar com uma letra ou número.<br><br>Para obter mais informações, consulte [criar namespace](/rest/api/servicebus/create-namespace). |
> | namespaces/AuthorizationRules | namespace | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alphnumeric. |
> | namespaces/disasterRecoveryConfigs | global | 6-50 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com alfanumérico. |
> | namespaces/migrationConfigurations | namespace |  | Sempre deve ser **$Default**. |
> | namespaces/filas | namespace | 1-260 | Alfanuméricos, pontos, hifens, sublinhados e barras.<br><br>Comece e termine com alfanumérico. |
> | namespaces/filas/authorizationRules | fila | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alphnumeric. |
> | namespaces/tópicos | namespace | 1-260 | Alfanuméricos, pontos, hifens, sublinhados e barras.<br><br>Comece e termine com alfanumérico. |
> | namespaces/tópicos/authorizationRules | topic | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alphnumeric. |
> | namespaces/tópicos/assinaturas | topic | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alphnumeric. |
> | namespaces/tópicos/assinaturas/regras | subscription | 1-50 | Alfanuméricos, pontos, hifens e sublinhados.<br><br>Comece e termine com alphnumeric. |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | clusters | region | 4-23 | Letras minúsculas, números e hifens.<br><br>Comece com uma letra minúscula. Terminar com letra minúscula ou número. |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | signalR | global | 3-63 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com letra ou número.  |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | managedInstances | global | 1-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servers | global | 1-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servidores/bancos de dados | Servidor | 1-128 | Não é possível usar:<br>`<>*%&:\/?`<br><br>Não é possível terminar com período ou espaço. |
> | servidores/bancos de dados/syncGroups | Banco de Dados | 1-150 | Alfanuméricos, hifens e sublinhados. |
> | servidores/elasticPools | Servidor | 1-128 | Não é possível usar:<br>`<>*%&:\/?`<br><br>Não é possível terminar com período ou espaço. |
> | servidores/failoverGroups | global | 1-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. |
> | servidores/firewallRules | Servidor | 1-128 | Não é possível usar:<br>`<>*%&:;\/?`<br><br>Não é possível terminar com período. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | storageAccounts | global | 3-24 | Letras minúsculas e números. |
> | storageAccounts/blobservices | do Azure |  | Deve ser `default`. |
> | storageAccounts/blobservices/contêineres | do Azure | 3-63 | Letras minúsculas, números e hifens.<br><br>Comece com letra minúscula ou número. Não é possível usar hifens consecutivos. |
> | storageAccounts/fileservices | do Azure |  | Deve ser `default`. |
> | storageAccounts/fileservices/compartilhamentos | do Azure | 3-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. Não é possível usar hifens consecutivos. |
> | storageAccounts/managementPolicies | do Azure |  | Deve ser `default`. |
> | blob | contêiner | 1-1024 | Qualquer caractere de URL, diferencia maiúsculas de minúsculas |
> | fila | do Azure | 3-63 | Letras minúsculas, números e hifens.<br><br>Não é possível iniciar ou terminar com hífen. Não é possível usar hifens consecutivos. |
> | tabela | do Azure | 3-63 | Alfanuméricos.<br><br>Comece com a letra. |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | storageSyncServices | grupo de recursos | 1-260 | Alfanuméricos, espaços, pontos, hifens e sublinhados.<br><br>Não é possível terminar com período ou espaço. |
> | storageSyncServices / syncGroups | serviço de sincronização de armazenamento | 1-260 | Alfanuméricos, espaços, pontos, hifens e sublinhados.<br><br>Não é possível terminar com período ou espaço. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | managers | grupo de recursos | 2-50 | Alfanuméricos e hifens.<br><br>Comece com a letra. Terminar com alfanumérico. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | streamingjobs | grupo de recursos | 3-63 | Alfanuméricos, hifens e sublinhados. |
> | streamingjobs/funções | trabalho de streaming | 3-63 | Alfanuméricos, hifens e sublinhados. |
> | streamingjobs/entradas | trabalho de streaming | 3-63 | Alfanuméricos, hifens e sublinhados. |
> | streamingjobs/saídas | trabalho de streaming | 3-63 | Alfanuméricos, hifens e sublinhados. |
> | streamingjobs/transformações | trabalho de streaming | 3-63 | Alfanuméricos, hifens e sublinhados. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | environments | grupo de recursos | 1-90 | Não é possível usar:<br>`'<>%&:\?/#` |
> | ambientes/accessPolicies | environment | 1-90 | Não é possível usar:<br> `'<>%&:\?/#` |
> | ambientes/eventSources | environment | 1-90 | Não é possível usar:<br>`'<>%&:\?/#` |
> | ambientes/referenceDataSets | environment | 3-63 | Alfanuméricos |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Entidade | Escopo | Comprimento | Caracteres válidos |
> | --- | --- | --- | --- |
> | serverfarms | grupo de recursos | 1-40 | Alfanuméricos e hifens. |
> | sites | global | 2-60 | Contém caracteres alfanuméricos e hifens.<br><br>Não é possível iniciar ou terminar com hífen. |
> | sites/Slots | site | 2-59 | Alfanuméricos e hifens. |

## <a name="next-steps"></a>Próximas etapas

Para obter recomendações sobre como nomear recursos, consulte [pronto: convenções de nomenclatura e marcação recomendadas](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).
