---
title: Funções e requisitos do Azure Data Share
description: Saiba mais sobre as permissões necessárias para compartilhar e receber dados usando o compartilhamento de dados do Azure.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 36a492f6a3e86cfb2fc9505550cc2d9f4746e070
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79265500"
---
# <a name="roles-and-requirements-for-azure-data-share"></a>Funções e requisitos do Azure Data Share 

Este artigo descreve as funções e as permissões necessárias para compartilhar e receber dados usando o serviço de compartilhamento de dados do Azure. 

## <a name="roles-and-requirements"></a>Requisitos e funções

Com o serviço de compartilhamento de dados do Azure, você pode compartilhar dados sem trocar credenciais entre o provedor de dados e o consumidor. O serviço de compartilhamento de dados do Azure usa identidades gerenciadas (anteriormente conhecidas como MSIs) para autenticar no repositório de dados do Azure. 

A identidade gerenciada do recurso de compartilhamento de dados do Azure precisa receber acesso ao armazenamento de dados do Azure. O serviço de compartilhamento de dados do Azure usa essa identidade gerenciada para ler e gravar dados para compartilhamento baseado em instantâneo e para estabelecer um link simbólico para o compartilhamento in-loco. 

Para compartilhar ou receber dados de um armazenamento de dados do Azure, o usuário precisa de pelo menos as permissões a seguir. Permissões adicionais são necessárias para o compartilhamento baseado em SQL.
* Permissão para gravar no armazenamento de dados do Azure. Normalmente, essa permissão existe na função **colaborador** .
* Permissão para criar a atribuição de função no armazenamento de dados do Azure. Normalmente, a permissão para criar atribuições de função existe na função **proprietário** , na função Administrador de acesso do usuário ou em uma função personalizada com a permissão Microsoft. Authorization/role assignments/Write atribuída. Essa permissão não será necessária se a identidade gerenciada do recurso de compartilhamento de dados já tiver acesso concedido ao armazenamento de dados do Azure. Consulte a tabela abaixo para obter a função necessária.

Veja abaixo um resumo das funções atribuídas à identidade gerenciada do recurso de compartilhamento de dados:

| |  |  |
|---|---|---|
|**Tipo de repositório de dados**|**Armazenamento de dados de origem Provedor de Dados**|**Armazenamento de dados de destino de consumidor de dados**|
|Armazenamento do Blobs do Azure| Leitor de dados de blob de armazenamento | Colaborador de dados do blob de armazenamento
|Azure Data Lake Gen1 | Proprietário | Sem suporte
|Azure Data Lake Gen2 | Leitor de dados de blob de armazenamento | Colaborador de dados do blob de armazenamento
|Azure SQL Server | Colaborador do banco de dados SQL | Colaborador do banco de dados SQL
|Cluster de Data Explorer do Azure | Colaborador | Colaborador
|

Para o compartilhamento baseado em SQL, um usuário do SQL precisa ser criado a partir de um provedor externo no banco de dados SQL com o mesmo nome que o recurso de compartilhamento do Azure data share. Veja abaixo um resumo da permissão exigida pelo usuário do SQL.

| |  |  |
|---|---|---|
|**Tipo de banco de dados SQL**|**Permissão de usuário do Provedor de Dados SQL**|**Permissão de usuário SQL de consumidor de dados**|
|Banco de Dados SQL do Azure | db_datareader | db_datareader, db_datawriter, db_ddladmin
|Azure Synapse Analytics (antigo SQL DW) | db_datareader | db_datareader, db_datawriter, db_ddladmin
|


### <a name="data-provider"></a>Provedor de dados 
Para adicionar um conjunto de dados no Azure data share, a identidade gerenciada do recurso de compartilhamento de dados do provedor precisa receber acesso ao armazenamento de dados de origem do Azure. Por exemplo, no caso da conta de armazenamento, a identidade gerenciada do recurso de compartilhamento de dados recebe a função de leitor de dados de blob de armazenamento. 

Isso é feito automaticamente pelo serviço de compartilhamento de dados do Azure quando o usuário está adicionando um conjunto por portal do Azure e o usuário tem a permissão apropriada. Por exemplo, o usuário é um proprietário do armazenamento de dados do Azure ou é um membro de uma função personalizada que tem a permissão Microsoft. Authorization/role/Write atribuída. 

Como alternativa, o usuário pode ter o proprietário do armazenamento de dados do Azure adicionar a identidade gerenciada do recurso de compartilhamento de dados ao armazenamento de dados do Azure manualmente. Essa ação só precisa ser executada uma vez por recurso de compartilhamento de dados.

Para criar uma atribuição de função para a identidade gerenciada do recurso de compartilhamento de dados, siga as etapas abaixo:

1. Navegue até o armazenamento de dados do Azure.
1. Selecione **Controle de Acesso (IAM)** .
1. Selecione **Adicionar uma atribuição de função**.
1. Em *função*, selecione a função na tabela atribuição de função acima (por exemplo, para conta de armazenamento, selecione *leitor de dados de blob de armazenamento*).
1. Em *selecionar*, digite o nome do recurso de compartilhamento de dados do Azure.
1. Clique em *Save* (Salvar).

Para fontes baseadas em SQL, além das etapas acima, um usuário do SQL precisa ser criado a partir de um provedor externo no banco de dados SQL com o mesmo nome que o recurso de compartilhamento do Azure data share. Esse usuário precisa receber a permissão *db_datareader* . Um script de exemplo junto com outros pré-requisitos para o compartilhamento baseado em SQL pode ser encontrado no tutorial [compartilhar seus dados](share-your-data.md) . 

### <a name="data-consumer"></a>Consumidor de dados
Para receber dados, a identidade gerenciada do recurso de compartilhamento de dados do consumidor precisa receber acesso ao armazenamento de dados do Azure de destino. Por exemplo, no caso da conta de armazenamento, a identidade gerenciada do recurso de compartilhamento de dados recebe a função de colaborador de dados de blob de armazenamento. 

Isso é feito automaticamente pelo serviço de compartilhamento de dados do Azure se o usuário especificar um armazenamento de dados de destino via portal do Azure e o usuário tiver a permissão adequada. Por exemplo, o usuário é um proprietário do armazenamento de dados do Azure ou é um membro de uma função personalizada que tem a permissão Microsoft. Authorization/role/Write atribuída. 

Como alternativa, o usuário pode ter o proprietário do armazenamento de dados do Azure adicionar a identidade gerenciada do recurso de compartilhamento de dados ao armazenamento de dados do Azure manualmente. Essa ação só precisa ser executada uma vez por recurso de compartilhamento de dados.

Para criar uma atribuição de função para a identidade gerenciada do recurso de compartilhamento de dados manualmente, siga as etapas abaixo:

1. Navegue até o armazenamento de dados do Azure.
1. Selecione **Controle de Acesso (IAM)** .
1. Selecione **Adicionar uma atribuição de função**.
1. Em *função*, selecione a função na tabela atribuição de função acima (por exemplo, para conta de armazenamento, selecione *leitor de dados de blob de armazenamento*).
1. Em *selecionar*, digite o nome do recurso de compartilhamento de dados do Azure.
1. Clique em *Save* (Salvar).

Para destino baseado em SQL, além das etapas acima, um usuário do SQL precisa ser criado a partir de um provedor externo no banco de dados SQL com o mesmo nome que o recurso do Azure data share. Esse usuário precisa receber a permissão *db_datareader, db_datawriter db_ddladmin* . Um script de exemplo junto com outros pré-requisitos para o compartilhamento baseado em SQL pode ser encontrado no tutorial [aceitar e receber dados](subscribe-to-data-share.md) . 

Se você estiver compartilhando dados usando APIs REST, precisará criar essas atribuições de função manualmente. 

Para saber mais sobre como adicionar uma atribuição de função, consulte [esta documentação,](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment). 

## <a name="resource-provider-registration"></a>Registro do provedor de recursos 

Para exibir o convite de compartilhamento de dados do Azure pela primeira vez em seu locatário do Azure, talvez seja necessário registrar manualmente o provedor de recursos Microsoft. DataShare em sua assinatura do Azure. Siga estas etapas para registrar o provedor de recursos Microsoft. DataShare em sua assinatura do Azure. Você precisa de acesso de *colaborador* à assinatura do Azure para registrar o provedor de recursos.

1. No portal do Azure, navegue até **assinaturas**.
1. Selecione a assinatura que você está usando para o compartilhamento de dados do Azure.
1. Clique em **provedores de recursos**.
1. Procure Microsoft. DataShare.
1. Clique em **Registrar**.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as funções no Azure – [Entender as definições de função](../role-based-access-control/role-definitions.md)

