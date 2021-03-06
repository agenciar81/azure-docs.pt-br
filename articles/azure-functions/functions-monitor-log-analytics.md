---
title: Azure Functions de monitoramento com logs de Azure Monitor
description: Saiba como usar os logs de Azure Monitor com Azure Functions para monitorar as execuções de função.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: cshoe
ms.openlocfilehash: 13c72a1cf8a0dd4a1124e51b9ceee04ae04bf261
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649867"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Azure Functions de monitoramento com logs de Azure Monitor

O Azure Functions oferece uma integração com [Azure monitor logs](../azure-monitor/platform/data-platform-logs.md) para monitorar funções. Este artigo mostra como configurar Azure Functions para enviar logs gerados pelo sistema e pelo usuário a logs do Azure Monitor.

Os logs de Azure Monitor fornecem a capacidade de consolidar logs de diferentes recursos no mesmo espaço de trabalho, onde podem ser analisados com [consultas](../azure-monitor/log-query/log-query-overview.md) para recuperar, consolidar e analisar rapidamente os dados coletados.  É possível criar e testar consultas usando o [Log Analytics](../azure-monitor/log-query/portals.md) no portal do Azure e, em seguida, analisar diretamente os dados usando essas ferramentas ou salvar consultas para uso com [visualizações](../azure-monitor/visualizations.md) ou [regras de alerta](../azure-monitor/platform/alerts-overview.md).

O Azure Monitor usa uma versão da [linguagem de consulta Kusto](/azure/kusto/query/) usada pelo Azure Data Explorer que é adequada para consultas de log simples, mas também inclui funcionalidades avançadas como agregações, junções e análises inteligentes. É possível aprender a linguagem de consulta rapidamente por meio de [várias lições](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> A integração com os logs de Azure Monitor está atualmente em visualização pública para aplicativos de funções em execução no consumo do Windows, Premium e planos de hospedagem dedicados.

## <a name="setting-up"></a>Configurando

Na seção **monitoramento** , selecione **configurações de diagnóstico** e clique em **Adicionar configuração de diagnóstico**.

![Adicionar uma configuração de diagnóstico](media/functions-monitor-log-analytics/diagnostic-settings-add.png)

Na página **configurações de diagnóstico** , escolha **Enviar para log Analytics**e, em seguida, selecione o espaço de trabalho log Analytics. Em **log** , escolha **FunctionAppLogs**, essa tabela contém os logs desejados.

![Adicionar uma configuração de diagnóstico](media/functions-monitor-log-analytics/choose-table.png)

## <a name="user-generated-logs"></a>Logs gerados pelo usuário

Para gerar logs personalizados, você pode usar a instrução de log específica, dependendo do seu idioma, aqui estão os trechos de código de exemplo:


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Consultando os logs

Para consultar os logs gerados, vá para o espaço de trabalho Log Analytics que você configurou para enviar os logs de função e clique em **logs**.

![Janela de consulta na LA espaço de trabalho](media/functions-monitor-log-analytics/querying.png)

Azure Functions grava todos os logs na tabela **FunctionAppLogs** , aqui estão algumas consultas de exemplo.

### <a name="all-logs"></a>Todos os logs

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="a-specific-function-logs"></a>Logs de função específicos

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Exceções

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Próximas etapas

- Examinar a [visão geral de Azure Functions](functions-overview.md)
- Saiba mais sobre [os logs de Azure monitor](../azure-monitor/platform/data-platform-logs.md)
- Saiba mais sobre a [linguagem de consulta](../azure-monitor/log-query/get-started-queries.md).
