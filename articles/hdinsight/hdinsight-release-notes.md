---
title: Notas da versão mais recente do Microsoft Azure HDInsight
description: Notas da versão mais recente do Microsoft Azure HDInsight. Obtenha dicas e detalhes de desenvolvimento para Hadoop, Spark, R Server, Hive e muito mais.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/13/2020
ms.openlocfilehash: b83828c3c78913598c103730e11222969fe1fddb
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79370161"
---
# <a name="release-notes"></a>Notas de versão

Este artigo fornece informações sobre as atualizações de versão **mais recentes** do Azure HDInsight. Para obter informações sobre versões anteriores, consulte o [Arquivo morto de notas de versão do HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumo

O Azure HDInsight é um dos serviços mais populares entre clientes empresariais para análise de software livre no Azure.

## <a name="release-date-01092020"></a>Data de lançamento: 01/09/2020

Esta versão se aplica tanto ao HDInsight 3,6 quanto ao 4,0. A versão do HDInsight é disponibilizada para todas as regiões durante vários dias. A data de lançamento aqui indica a data de lançamento da primeira região. Se você não vir as alterações abaixo, aguarde até que a liberação esteja ativa em sua região em vários dias.

> [!IMPORTANT]  
> O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para saber mais, confira o [artigo sobre controle de versão do HDInsight](hdinsight-component-versioning.md).

## <a name="new-features"></a>Novos recursos
### <a name="tls-12-enforcement"></a>Imposição do TLS 1.2
Os protocolos TLS e SSL são protocolos criptográficos que fornecem segurança de comunicações em uma rede de computadores. Saiba mais sobre o [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). O HDInsight usa o TLS 1,2 em pontos de extremidade HTTPs públicos, mas o TLS 1,1 ainda tem suporte para compatibilidade com versões anteriores. 

Com esta versão, os clientes podem optar pelo TLS 1,2 apenas para todas as conexões por meio do ponto de extremidade do cluster público. Para dar suporte a isso, a nova propriedade **minSupportedTlsVersion** é introduzida e pode ser especificada durante a criação do cluster. Se a propriedade não estiver definida, o cluster ainda dará suporte a TLS 1,0, 1,1 e 1,2, que é o mesmo comportamento de hoje. Os clientes podem definir o valor dessa propriedade como "1,2", o que significa que o cluster só oferece suporte a TLS 1,2 e superior. Para obter mais informações, consulte [planejar uma rede virtual – segurança da camada de transporte](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#transport-layer-security).

### <a name="bring-your-own-key-for-disk-encryption"></a>Traga sua própria chave para a criptografia de disco
Todos os discos gerenciados no HDInsight são protegidos com o SSE (Criptografia do Serviço de Armazenamento) do Azure. Por padrão, os dados nesses discos são criptografados por chaves gerenciadas pela Microsoft. A partir desta versão, você pode Bring Your Own Key (BYOK) para a criptografia de disco e gerenciá-lo usando Azure Key Vault. A criptografia BYOK é uma configuração de uma etapa durante a criação do cluster sem custo adicional. Basta registrar o HDInsight como uma identidade gerenciada com Azure Key Vault e adicionar a chave de criptografia ao criar o cluster. Para obter mais informações, consulte [criptografia de disco de chave gerenciada pelo cliente](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

## <a name="deprecation"></a>Reprovação
Nenhuma substituição para esta versão. Para se preparar para [as futuras substituições](#upcoming-changes), confira alterações futuras.

## <a name="behavior-changes"></a>Alterações de comportamento
Nenhuma alteração de comportamento para esta versão. Para se preparar para as alterações futuras, confira [alterações futuras](#upcoming-changes).

## <a name="upcoming-changes"></a>Alterações futuras
As seguintes alterações ocorrerão em versões futuras. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Uma VM de 4 núcleos mínima é necessária para o nó de cabeçalho 
Uma VM de 4 núcleos mínima é necessária para o nó de cabeçalho para garantir a alta disponibilidade e a confiabilidade dos clusters HDInsight. A partir de 6 de abril de 2020, os clientes podem escolher apenas 4 núcleos ou acima da VM como nó principal para os novos clusters HDInsight. Os clusters existentes continuarão a ser executados conforme o esperado. 

### <a name="esp-spark-cluster-node-size-change"></a>Alteração do tamanho do nó do cluster do Spark do ESP 
Na próxima versão, o tamanho mínimo de nó permitido para o cluster do Spark do ESP será alterado para Standard_D13_V2. As VMs da série a podem causar problemas de cluster ESP devido à capacidade relativamente baixa de CPU e memória. As VMs da série A serão preteridas para a criação de novos clusters ESP.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Migrando para conjuntos de dimensionamento de máquinas virtuais do Azure
O HDInsight agora usa máquinas virtuais do Azure para provisionar o cluster. Na próxima versão, o HDInsight usará os conjuntos de dimensionamento de máquinas virtuais do Azure em vez disso. Veja mais sobre os conjuntos de dimensionamento de máquinas virtuais do Azure.

### <a name="hbase-20-to-21"></a>HBase 2,0 a 2,1
Na próxima versão do HDInsight 4,0, a versão do HBase será atualizada da versão 2,0 para a 2,1.

## <a name="bug-fixes"></a>Correções de bug
O HDInsight continua a tornar as melhorias de desempenho e confiabilidade do cluster. 

## <a name="component-version-change"></a>Alteração de versão do componente
Nenhuma alteração de versão de componente para esta versão. Você pode encontrar as versões de componente atuais para o HDInsight 4,0 Ad HDInsight 3,6 aqui.
