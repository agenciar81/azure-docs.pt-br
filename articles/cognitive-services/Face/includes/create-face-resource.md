---
title: Suporte a contêiner
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 3886777b283af35e84683480a59097584b537fea
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73522280"
---
## <a name="create-an-face-resource"></a>Criar um recurso facial

1. Entre no [Portal do Azure](https://portal.azure.com)
1. Clique em [criar recurso **facial** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)
1. Insira todas as configurações necessárias:

    |Configuração|Valor|
    |--|--|
    |Nome|Nome desejado (2-64 caracteres)|
    |Assinatura|Selecione a assinatura apropriada|
    |Localização|Selecione qualquer local próximo e disponível|
    |Camada de preços|`F0`-o tipo de preço mínimo|
    |Grupo de recursos|Selecionar um grupo de recursos disponível|

1. Clique em **criar** e aguarde até que o recurso seja criado. Depois de criado, navegue até a página de recursos
1. Coletar `endpoint` configuradas e uma chave de API:

    |Guia de recursos no portal|Configuração|Valor|
    |--|--|--|
    |**Visão geral**|Ponto de extremidade|Copie o ponto de extremidade. Ele é semelhante a `https://face.cognitiveservices.azure.com/face/v1.0`|
    |**Chaves**|Chave de API|Cópia 1 das duas chaves. É uma cadeia de caracteres de caractere alfanumérico 32 sem espaços ou traços, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
