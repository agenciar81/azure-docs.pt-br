---
title: Conceitos de serviço no Serviço de Provisionamento de Dispositivos no Hub IoT do Azure | Microsoft Docs
description: Descreve os conceitos de provisionamento de serviço específicos para dispositivos com o DPS (serviço de provisionamento de dispositivos) e o Hub IoT
author: nberdy
ms.author: nberdy
ms.date: 09/18/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: f42502ac4db12a060af5906243d3f8e7584c5df3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79285208"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Conceitos do Serviço de Provisionamento de Dispositivos no Hub IoT

O Serviço de Provisionamento de Dispositivos no Hub IoT é um serviço auxiliar do Hub IoT que você usa para configurar o provisionamento de dispositivo sem interação para um Hub IoT especificado. Com o Serviço de Provisionamento de Dispositivos, é possível [provisionar automaticamente](concepts-auto-provisioning.md) milhões de dispositivos de maneira segura e escalonável.

O provisionamento de dispositivos é um processo de duas partes. A primeira parte é estabelecer a conexão inicial entre o dispositivo e a solução de IoT, *registrando* o dispositivo. A segunda parte é aplicar a *configuração* apropriada ao dispositivo com base nos requisitos específicos da solução. Quando as duas etapas forem concluídas, o dispositivo será totalmente *provisionado*. O Serviço de Provisionamento de Dispositivos automatiza as etapas para fornecer uma experiência perfeita de provisionamento para o dispositivo.

Este artigo fornece uma visão geral dos conceitos de provisionamento mais aplicáveis para gerenciar o *serviço*. Este artigo é muito relevante para todas as personas envolvidas na [etapa de configuração de nuvem](about-iot-dps.md#cloud-setup-step) da preparação de um dispositivo para implantação.

## <a name="service-operations-endpoint"></a>Ponto de extremidade das operações de serviço

O ponto de extremidade das operações de serviço é o ponto de extremidade para gerenciar as configurações de serviço e manter a lista de registro. Esse ponto de extremidade é usado somente pelo administrador de serviços. Ele não é usado pelos dispositivos.

## <a name="device-provisioning-endpoint"></a>Ponto de extremidade de provisionamento do dispositivo

O ponto de extremidade de provisionamento de dispositivos é o ponto de extremidade único que todos os dispositivos usam para provisionamento automático. A URL é a mesma para todas as instâncias de serviços de provisionamento, para eliminar a necessidade de atualizar novamente os dispositivos com novas informações de conexão nos cenários da cadeia de fornecedores. O escopo da ID garante o isolamento de locatários.

## <a name="linked-iot-hubs"></a>Hubs IoT vinculados

O Serviço de Provisionamento de Dispositivos somente pode provisionar dispositivos para hubs de IoT que tenham sido vinculados a ele. Vincular um Hub IoT a uma instância do serviço de Provisionamento de Dispositivo fornece as permissões de leitura/gravação de serviço ao registro de dispositivo do Hub IoT e, com o vínculo, um serviço de Provisionamento de Dispositivos pode registrar uma ID do dispositivo e definir a configuração inicial no dispositivo gêmeo. Hubs de IoT vinculados podem estar em qualquer região do Azure. Você pode vincular hubs em outras assinaturas ao seu serviço de provisionamento.

## <a name="allocation-policy"></a>Política de alocação

A configuração de nível de serviço determina como o Serviço de Provisionamento de Dispositivos atribui dispositivos a um Hub IoT. Há três políticas de alocação com suporte:

* **Distribuição igualmente ponderada**: Hubs IoT vinculados têm probabilidades iguais de ter dispositivos provisionados a eles. A configuração padrão. Se estiver provisionando dispositivos a apenas um Hub IoT, você poderá manter essa configuração.

* **Menor latência**: dispositivos são provisionados para um Hub IoT com a menor latência para o dispositivo. Se vários Hubs IoT vinculados fornecerem a mesma menor latência, o serviço de provisionamento reciclará dispositivos entre esses hubs

* **Configuração estática por meio da lista de registro**: a especificação do Hub IoT desejado na lista de registro tem prioridade sobre a política de alocação no nível do serviço.

## <a name="enrollment"></a>Registro

Um registro é o registro de dispositivos ou grupos de dispositivos que podem ser registrados por meio de provisionamento automático. O registro contém informações sobre o dispositivo ou grupo de dispositivos, incluindo:
- o [mecanismo de certificação](concepts-security.md#attestation-mechanism) usado pelo dispositivo
- a configuração inicial desejada opcional
- hub IoT desejado
- a ID do dispositivo desejado

Há dois tipos de registros com suporte pelo Serviço de Provisionamento de Dispositivos:

### <a name="enrollment-group"></a>Grupo de registro

Um grupo de registro é um grupo de dispositivos que compartilham um mecanismo de atestado específico. Os grupos de registro dão suporte a X. 509, bem como ao Symmetric. Todos os dispositivos no grupo de registro X. 509 apresentam certificados X. 509 que foram assinados pela mesma raiz ou AC (autoridade de certificação) intermediária. Cada dispositivo no grupo de registro de chave simétrica apresenta tokens SAS derivados da chave simétrica do grupo. O nome do grupo de registro e o nome do certificado devem ter caracteres alfanuméricos, letras minúsculas e podem conter hifens.

> [!TIP]
> É recomendável usar um grupo de registro para um grande número de dispositivos que compartilhem uma configuração inicial desejada ou para dispositivos que vão todos para o mesmo locatário.

### <a name="individual-enrollment"></a>Registro individual

Um registro individual é uma entrada para um único dispositivo que pode registrar. Os registros individuais podem usar certificados folha X.509 ou tokens de SAS (de um TPM físico ou virtual) como mecanismos de certificação. A ID do registro em um registro individual é alfanumérica, com letras minúsculas e pode conter hifens. Registros individuais podem ter a ID de dispositivo de Hub IoT desejada especificada.

> [!TIP]
> É recomendável o uso de registros individuais para dispositivos que exigem configurações iniciais exclusivas ou para dispositivos que somente podem autenticar usando tokens de SAS via atestado de TPM.

## <a name="registration"></a>Registro

Um registro é o registro de um dispositivo com registro/provisionamento com êxito para um Hub IoT por meio do Serviço de Provisionamento de Dispositivos. Registros são criados automaticamente. Eles podem ser excluídos, mas não podem ser atualizados.

## <a name="operations"></a>Operações

As operações são a unidade de cobrança do Serviço de Provisionamento de Dispositivos. Uma operação é a conclusão bem-sucedida de uma instrução para o serviço. As operações incluem registros e repetições de registros de dispositivo, bem como alterações no lado do serviço, como adição e atualização das entradas da lista de registro.
