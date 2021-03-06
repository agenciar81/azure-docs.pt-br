---
title: Gerenciar recursos no Azure Red Hat OpenShift | Microsoft Docs
description: Gerenciar projetos, modelos, fluxos de imagens em um cluster do Azure Red Hat OpenShift
services: openshift
keywords: os projetos do Red Hat openshift solicitam o autoprovisionador
author: mjudeikis
ms.author: gwallace
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: d4f53238951784a74e6e3fc8a73d1f112ce75608
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139106"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Gerenciar projetos, modelos, fluxos de imagens em um cluster do Azure Red Hat OpenShift 

Em uma plataforma de contêiner OpenShift, os projetos são usados para agrupar e isolar objetos relacionados. Como administrador, você pode dar aos desenvolvedores acesso a projetos específicos, permitir que eles criem seus próprios projetos e conceder a eles direitos administrativos a projetos individuais.

## <a name="self-provisioning-projects"></a>Projetos de autoprovisionamento

Você pode permitir que os desenvolvedores criem seus próprios projetos. Um ponto de extremidade de API é responsável por provisionar um projeto de acordo com um modelo chamado Project-Request. O console Web e o comando `oc new-project` usam esse ponto de extremidade quando um desenvolvedor cria um novo projeto.

Quando uma solicitação de projeto é enviada, a API substitui os seguintes parâmetros no modelo:

| Parâmetro               | DESCRIÇÃO                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | O nome do projeto. Obrigatórios.             |
| PROJECT_DISPLAYNAME     | O nome de exibição do projeto. Pode estar vazio. |
| PROJECT_DESCRIPTION     | A descrição do projeto. Pode estar vazio.  |
| PROJECT_ADMIN_USER      | O nome do usuário administrador.       |
| PROJECT_REQUESTING_USER | O nome do usuário do solicitante.           |

O acesso à API é concedido aos desenvolvedores com a associação de função de cluster de autoprovisionadores. Esse recurso está disponível para todos os desenvolvedores autenticados por padrão.

## <a name="modify-the-template-for-a-new-project"></a>Modificar o modelo para um novo projeto 

1. Faça logon como um usuário com privilégios de `customer-admin`.

2. Edite o modelo de solicitação de projeto padrão.

   ```
   oc edit template project-request -n openshift
   ```

3. Remova o modelo de projeto padrão do processo de atualização do Microsoft Red Hat OpenShift (toa) adicionando a anotação a seguir: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   O modelo de solicitação de projeto não será atualizado pelo processo de atualização da toa. Isso permite que os clientes personalizem o modelo e preserve essas personalizações quando o cluster é atualizado.

## <a name="disable-the-self-provisioning-role"></a>Desabilitar a função de autoprovisionamento

Você pode impedir que um grupo de usuários autenticado provisione novos projetos automaticamente.

1. Faça logon como um usuário com privilégios de `customer-admin`.

2. Edite a associação de função de cluster de autoprovisionadores.

   ```
   oc edit clusterrolebinding.rbac.authorization.k8s.io self-provisioners
   ```

3. Remova a função do processo de atualização da toa adicionando a seguinte anotação: `openshift.io/reconcile-protect: "true"`.

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Altere a associação de função de cluster para impedir que `system:authenticated:oauth` criem projetos:

   ```
   apiVersion: rbac.authorization.k8s.io/v1
   groupNames:
   - osa-customer-admins
   kind: ClusterRoleBinding
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
     labels:
       azure.openshift.io/owned-by-sync-pod: "true"
     name: self-provisioners
   roleRef:
     name: self-provisioner
   subjects:
   - kind: SystemGroup
     name: osa-customer-admins
   ```

## <a name="manage-default-templates-and-imagestreams"></a>Gerenciar modelos padrão e imageStreams

No Azure Red Hat OpenShift, você pode desabilitar atualizações para todos os modelos padrão e fluxos de imagem dentro do namespace `openshift`.
Para desabilitar as atualizações para todos os `Templates` e `ImageStreams` no namespace `openshift`:

1. Faça logon como um usuário com privilégios de `customer-admin`.

2. Editar o namespace `openshift`:

   ```
   oc edit namespace openshift
   ```

3. Remova o namespace `openshift` do processo de atualização da toa adicionando a seguinte anotação: `openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Qualquer objeto individual no namespace `openshift` pode ser removido do processo de atualização adicionando a anotação `openshift.io/reconcile-protect: "true"` a ele.

## <a name="next-steps"></a>Próximas etapas

Experimente o tutorial:
> [!div class="nextstepaction"]
> [Criar um cluster do Red Hat OpenShift no Azure](tutorial-create-cluster.md)
