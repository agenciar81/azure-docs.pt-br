---
title: Listar atribuições de função usando o RBAC e CLI do Azure do Azure
description: Saiba como determinar quais recursos os usuários, grupos, entidades de serviço ou identidades gerenciadas têm acesso ao uso do RBAC (controle de acesso baseado em função) do Azure e CLI do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b02ec00544ef11ca1048fd6d3bd9bdf3fccd8c8c
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471407"
---
# <a name="list-role-assignments-using-azure-rbac-and-azure-cli"></a>Listar atribuições de função usando o RBAC e CLI do Azure do Azure

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] este artigo descreve como listar atribuições de função usando CLI do Azure.

> [!NOTE]
> Se sua organização tiver funções de gerenciamento terceirizadas para um provedor de serviços que usa o [Gerenciamento de recursos delegado do Azure](../lighthouse/concepts/azure-delegated-resource-management.md), as atribuições de função autorizadas por esse provedor de serviços não serão mostradas aqui.

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

- [Bash em Azure cloud Shell](/azure/cloud-shell/overview) ou [CLI do Azure](/cli/azure)

## <a name="list-role-assignments-for-a-user"></a>Listar as atribuições de função de um usuário

Para listar as atribuições de função para um usuário específico, use a [lista de atribuições de função az](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Por padrão, somente as atribuições de função para a assinatura atual serão exibidas. Para exibir as atribuições de função para a assinatura atual e abaixo, adicione o parâmetro `--all`. Para exibir as atribuições de função herdadas, adicione o parâmetro `--include-inherited`.

O exemplo a seguir lista as atribuições de função atribuídas diretamente ao usuário do *patlong\@contoso.com* :

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

## <a name="list-role-assignments-for-a-resource-group"></a>Listar as atribuições para um grupo de recursos

Para listar as atribuições de função que existem em um escopo de grupo de recursos, use a [lista de atribuição de função AZ](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

O exemplo a seguir lista as atribuições de função para o grupo de recursos *Pharma-Sales* :

```azurecli
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="list-role-assignments-for-a-subscription"></a>Listar atribuições de função de um usuário

Para listar todas as atribuições de função em um escopo de assinatura, use a [lista de atribuição de função AZ](/cli/azure/role/assignment#az-role-assignment-list). Para obter a ID da assinatura, você pode encontrá-la na folha **assinaturas** no portal do Azure ou pode usar a lista de [contas AZ](/cli/azure/account#az-account-list).

```azurecli
az role assignment list --subscription <subscription_name_or_id>
```

```Example
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-management-group"></a>Listar as atribuições de função para um grupo de gerenciamento

Para listar todas as atribuições de função em um escopo do grupo de gerenciamento, use a [lista de atribuição de função AZ](/cli/azure/role/assignment#az-role-assignment-list). Para obter a ID do grupo de gerenciamento, você pode encontrá-la na folha **grupos de gerenciamento** no portal do Azure ou pode usar [AZ Account Management-grupo List](/cli/azure/account/management-group#az-account-management-group-list).

```azurecli
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

```Example
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Listar atribuições de função para uma identidade gerenciada

1. Obtenha a ID de objeto da identidade gerenciada atribuída pelo sistema ou pelo usuário. 

    Para obter a ID de objeto de uma identidade gerenciada atribuída pelo usuário, você pode usar [AZ ad SP List](/cli/azure/ad/sp#az-ad-sp-list) ou [AZ Identity List](/cli/azure/identity#az-identity-list).

    ```azurecli
    az ad sp list --display-name "<name>" --query [].objectId --output tsv
    ```

    Para obter a ID de objeto de uma identidade gerenciada atribuída pelo sistema, você pode usar [AZ ad SP List](/cli/azure/ad/sp#az-ad-sp-list).

    ```azurecli
    az ad sp list --display-name "<vmname>" --query [].objectId --output tsv
    ```

1. Para listar as atribuições de função, use a [lista de atribuição de função AZ](/cli/azure/role/assignment#az-role-assignment-list).

    Por padrão, somente as atribuições de função para a assinatura atual serão exibidas. Para exibir as atribuições de função para a assinatura atual e abaixo, adicione o parâmetro `--all`. Para exibir as atribuições de função herdadas, adicione o parâmetro `--include-inherited`.

    ```azurecli
    az role assignment list --assignee <objectid>
    ```

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Adicionar ou remover atribuições de função usando o Azure RBAC e CLI do Azure](role-assignments-cli.md)
