---
title: incluir arquivo
description: incluir arquivo
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 284aad7dd5b51268b1c8ff8a02f4489d6f1cd3d9
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279389"
---
Definições de configuração para [Funções Duráveis](../articles/azure-functions/durable-functions-overview.md).

### <a name="durable-functions-1x"></a>Durable Functions 1. x

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Completed", "Failed", "Terminated"]
  }
}
```

### <a name="durable-functions-2-0-host-json"></a>Durable Functions 2. x

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "storageProvider": {
      "connectionStringName": "AzureWebJobsStorage",
      "controlQueueBatchSize": 32,
      "controlQueueBufferThreshold": 256,
      "controlQueueVisibilityTimeout": "00:05:00",
      "maxQueuePollingInterval": "00:00:30",
      "partitionCount": 4,
      "trackingStoreConnectionStringName": "TrackingStorage",
      "trackingStoreNamePrefix": "DurableTask",
      "workItemQueueVisibilityTimeout": "00:05:00",
    },
    "tracing": {
      "traceInputsAndOutputs": false,
      "traceReplayEvents": false,
    },
    "notifications": {
      "eventGrid": {
        "topicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
        "keySettingName": "EventGridKey",
        "publishRetryCount": 3,
        "publishRetryInterval": "00:00:30",
        "publishEventTypes": [
          "Started",
          "Pending",
          "Failed",
          "Terminated"
        ]
      }
    },
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "extendedSessionsEnabled": false,
    "extendedSessionIdleTimeoutInSeconds": 30,
    "useGracefulShutdown": false
  }
}
```

Nomes de hubs de tarefas devem começar com uma letra e devem ser compostos somente por letras e números. Se não for especificado, o nome do hub de tarefas padrão de um aplicativo de funções será **DurableFunctionsHub**. Para obter mais informações, consulte [Hubs de tarefas](../articles/azure-functions/durable-functions-task-hubs.md).

|Propriedade  |Padrão | Description |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Nomes alternativos para [hub de tarefas](../articles/azure-functions/durable-functions-task-hubs.md) podem ser usados para isolar vários aplicativos de Funções Duráveis uns dos outros, mesmo se eles estiverem usando o mesmo back-end de armazenamento.|
|controlQueueBatchSize|32|O número de mensagens para efetuar pull da fila de controle por vez.|
|controlQueueBufferThreshold|256|O número de mensagens de fila de controle que podem ser armazenadas em buffer na memória por vez; nesse ponto, o Dispatcher aguardará antes de retirar a fila de qualquer mensagem adicional.|
|partitionCount |4|A contagem de partição para a fila de controle. Pode ser um número inteiro positivo entre 1 e 16.|
|controlQueueVisibilityTimeout |5 minutos|O limite de tempo de visibilidade das mensagens de remoção da fila de controle.|
|workItemQueueVisibilityTimeout |5 minutos|O limite de tempo de visibilidade das mensagens de remoção da fila de item de trabalho.|
|maxConcurrentActivityFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções de atividade que podem ser processadas simultaneamente em uma única instância de host.|
|maxConcurrentOrchestratorFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções do orquestrador que podem ser processadas simultaneamente em uma única instância do host.|
|maxQueuePollingInterval|30 segundos|O intervalo de sondagem de fila de item de trabalho e de controle máximo no formato *hh: mm: SS* . Valores mais altos podem resultar em latências de processamento de mensagens mais altas. Valores mais baixos podem resultar em custos de armazenamento maiores devido a maiores transações de armazenamento.|
|azureStorageConnectionStringName |AzureWebJobsStorage|O nome da configuração de aplicativo que tem a cadeia de conexão do Armazenamento do Azure usada para gerenciar os recursos subjacentes do Armazenamento do Azure.|
|trackingStoreConnectionStringName||O nome de uma cadeia de conexão a ser usada para as tabelas de histórico e instâncias. Se não for especificado, a conexão `azureStorageConnectionStringName` será usada.|
|trackingStoreNamePrefix||O prefixo a ser usado para as tabelas de histórico e instâncias quando `trackingStoreConnectionStringName` for especificado. Se não estiver definido, o valor de prefixo padrão será `DurableTask`. Se `trackingStoreConnectionStringName` não for especificado, as tabelas de histórico e instâncias usarão o valor de `hubName` como seu prefixo e qualquer configuração para `trackingStoreNamePrefix` será ignorada.|
|traceInputsAndOutputs |false|Um valor que indica se as entradas e saídas de chamadas de função sertão rastreadas. O comportamento padrão durante o rastreamento de eventos de execução de função é incluir o número de bytes nas entradas e saídas serializadas para chamadas de função. Esse comportamento fornece informações mínimas sobre o que as entradas e saídas parecem sem inflar os logs ou expor inadvertidamente informações confidenciais. A definição dessa propriedade como true faz com que o log de função padrão registre todo o conteúdo de entradas e saídas da função.|
|logReplayEvents|false|Um valor que indica se é necessário gravar eventos de reprodução de orquestração para o Application Insights.|
|eventGridTopicEndpoint ||A URL de um ponto de extremidade de tópico personalizado da Grade de Eventos do Azure. Quando essa propriedade é definida, os eventos de notificação do ciclo de vida da orquestração são publicados nesse ponto de extremidade. Esta propriedade dá suporte à resolução de Configurações do Aplicativo.|
|eventGridKeySettingName ||O nome da configuração de aplicativo que contém a chave usada para autenticar com o tópico personalizado da Grade de Eventos do Azure em `EventGridTopicEndpoint`.|
|eventGridPublishRetryCount|0|O número de novas tentativas se a publicação no Tópico de Grade de Eventos falha.|
|eventGridPublishRetryInterval|5 minutos|A Grade de Eventos publica o intervalo de repetição no formato *hh:mm:ss*.|
|eventGridPublishEventTypes||Uma lista de tipos de eventos a serem publicados na grade de eventos. Se não for especificado, todos os tipos de evento serão publicados. Os valores permitidos incluem `Started`, `Completed`, `Failed``Terminated`.|
|useGracefulShutdown|false|Apresentação Habilite o desligamento normal para reduzir a chance de desligamentos de host falharem em execuções de função em processo.|

Muitas dessas configurações são para otimizar o desempenho. Para obter mais informações, consulte [Desempenho e escala](../articles/azure-functions/durable-functions-perf-and-scale.md).
