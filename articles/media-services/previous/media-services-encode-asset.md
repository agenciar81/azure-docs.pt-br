---
title: Visão geral dos codificadores de mídia sob demanda do Azure | Microsoft Docs
description: Os Serviços de Mídia do Azure fornecem várias opções para a codificação de mídia na nuvem. Este artigo fornece uma visão geral dos codificadores de mídia sob demanda do Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: d5d5b8a7328ee82e94d494795617832cb0258667
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79251096"
---
# <a name="overview-of-azure-on-demand-media-encoders"></a>Visão geral dos codificadores de mídia sob demanda do Azure 

> [!NOTE]
> Não estão sendo adicionados novos recursos ou funcionalidades aos Serviços de Mídia v2. <br/>Confira a versão mais recente, [Serviços de Mídia v3](https://docs.microsoft.com/azure/media-services/latest/). Além disso, consulte [diretrizes de migração de v2 para v3](../latest/migrate-from-v2-to-v3.md)

Os Serviços de Mídia do Azure fornecem várias opções para a codificação de mídia na nuvem.

Ao começar a usar os Serviços de Mídia é importante compreender a diferença entre codecs e formatos de arquivo.
Codecs são o software que implementa os algoritmos de compactação/descompactação. Já os formatos de arquivo são contêineres que armazenam o vídeo compactado.

Os Serviços de Mídia fornecem empacotamento dinâmico, o que permite distribuir seu conteúdo codificado em Smooth Streaming ou MP4 com taxa de bits adaptável em formatos de streaming com suporte dos Serviços de Mídia (MPEG DASH, HLS, Smooth Streaming), sem a necessidade de empacotar novamente nesses formatos de streaming.

Quando sua conta dos Serviços de Mídia é criada, um ponto de extremidade de streaming **padrão** é adicionado à sua conta em estado **Parado**. Para iniciar seu conteúdo de streaming e tirar proveito do empacotamento dinâmico e da criptografia dinâmica, o ponto de extremidade de streaming do qual você deseja transmitir o conteúdo deve estar em estado **Executando**. A cobrança para pontos de extremidade de streaming ocorre sempre que o ponto de extremidade está em um estado de **execução** .

Os Serviços de Mídia são compatíveis com os seguintes codificadores sob demanda descritos neste artigo:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Fluxo de trabalho do Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Este artigo fornece uma breve visão geral dos codificadores de mídia sob demanda e fornece links para artigos que oferecem informações mais detalhadas. O tópico também fornece uma comparação entre os codificadores.

Por padrão, cada conta dos Serviços de Mídia pode ter uma tarefa de codificação ativa por vez. Você pode reservar unidades de codificação que permitem ter várias tarefas de codificação em execução simultaneamente, uma para cada unidade reservada de codificação que você comprar. Para saber mais, consulte [Dimensionamento das unidades de codificação](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard

### <a name="how-to-use"></a>Como usar
[Como codificar com o Codificador de Mídia Padrão](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formatos
[Formatos e codecs](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Predefinições
O Codificador de Mídia Padrão é configurado usando um dos codificadores predefinidos descritos [aqui](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Metadados de entrada e saída
Os metadados de entrada dos codificadores estão descritos [aqui](media-services-input-metadata-schema.md).

Os metadados de saída dos codificadores estão descritos [aqui](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Gerar miniaturas
Para obter informações, veja [Como gerar miniaturas usando o Codificador de Mídia Padrão](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Cortar vídeos (recorte)
Para obter informações, veja [Como cortar vídeos usando o Codificador de Mídia Padrão](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Criar sobreposições
Para obter informações, veja [Como criar sobreposições usando o Codificador de Mídia Padrão](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Confira também
[O blog Serviços de Mídia](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Fluxo de trabalho do Media Encoder Premium
### <a name="overview"></a>Visão geral
[Apresentando a codificação Premium nos Serviços de Mídia do Azure](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Como usar
O fluxo de trabalho do Media Encoder Premium é configurado usando fluxos de trabalho complexos. Os arquivos de fluxo de trabalho podem ser criados e atualizados usando a ferramenta [Designer de Fluxo de Trabalho](media-services-workflow-designer.md) .

[Como usar a codificação Premium nos Serviços de Mídia do Azure](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Problemas conhecidos
Se o vídeo de entrada não contiver a legendagem oculta, o ativo de saída ainda conterá um arquivo TTML vazio.

## <a name="media-services-learning-paths"></a>Roteiros de aprendizagem dos Serviços de Mídia
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornecer comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artigos relacionados
* [Executar tarefas avançadas de codificação ao personalizar predefinições do Codificador de Mídia Padrão](media-services-custom-mes-presets-with-dotnet.md)
* [Cotas e limitações](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
