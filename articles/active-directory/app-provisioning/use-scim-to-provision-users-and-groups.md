---
title: Desenvolver um ponto de extremidade SCIM para provisionamento de usuário para aplicativos do Azure AD
description: O sistema para SCIM (gerenciamento de identidade entre domínios) padroniza o provisionamento automático de usuário. Saiba como desenvolver um ponto de extremidade do SCIM, integrar sua API do SCIM com Azure Active Directory e começar a automatizar o provisionamento de usuários e grupos em seus aplicativos de nuvem.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2020
ms.author: mimart
ms.reviewer: arvinh
ms.custom: aaddev;it-pro;seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42fc10c1e7e88e36e4d2174671702e043fb96538
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2020
ms.locfileid: "78926839"
---
# <a name="build-a-scim-endpoint-and-configure-user-provisioning-with-azure-active-directory-azure-ad"></a>Criar um ponto de extremidade SCIM e configurar o provisionamento de usuário com o Azure Active Directory (Azure AD)

Como desenvolvedor de aplicativos, você pode usar o sistema para a API de gerenciamento de usuário do SCIM (gerenciamento de identidade de domínio cruzado) para habilitar o provisionamento automático de usuários e grupos entre seu aplicativo e o Azure AD. Este artigo descreve como criar um ponto de extremidade do SCIM e integrá-lo ao serviço de provisionamento do Azure AD. A especificação SCIM fornece um esquema de usuário comum para provisionamento. Quando usado em conjunto com padrões de Federação como SAML ou OpenID Connect, o SCIM fornece aos administradores uma solução de ponta a ponta baseada em padrões para o gerenciamento de acesso.

SCIM é uma definição padronizada de dois pontos de extremidade: a/Users EndPoint e um ponto de extremidade/groups. Ele usa verbos REST comuns para criar, atualizar e excluir objetos e um esquema predefinido para atributos comuns, como nome do grupo, nome de usuário, nome, sobrenome e email. Os aplicativos que oferecem uma API REST SCIM 2,0 podem reduzir ou eliminar o problema de trabalhar com uma API de gerenciamento de usuário proprietário. Por exemplo, qualquer cliente SCIM compatível sabe como fazer uma POSTAgem HTTP de um objeto JSON para o ponto de extremidade/Users para criar uma nova entrada de usuário. Em vez de precisar de uma API um pouco diferente para as mesmas ações básicas, os aplicativos que estão em conformidade com o padrão SCIM podem aproveitar instantaneamente os clientes, as ferramentas e o código preexistentes. 

![Provisionamento do Azure AD para um aplicativo com SCIM](media/use-scim-to-provision-users-and-groups/scim-provisioning-overview.png)

O esquema de objeto de usuário padrão e as APIs REST para gerenciamento definidos no SCIM 2,0 (RFC [7642](https://tools.ietf.org/html/rfc7642), [7643](https://tools.ietf.org/html/rfc7643), [7644](https://tools.ietf.org/html/rfc7644)) permitem que provedores de identidade e aplicativos sejam integrados com mais facilidade entre si. Os desenvolvedores de aplicativos que criam um ponto de extremidade SCIM podem se integrar a qualquer cliente compatível com SCIM sem precisar fazer trabalho personalizado.

Automatizar o provisionamento para um aplicativo requer a criação e a integração de um ponto de extremidade SCIM com o cliente SCIM do Azure AD. Execute as etapas a seguir para iniciar o provisionamento de usuários e grupos em seu aplicativo. 
    
  * **[Etapa 1: criar seu esquema de usuário e grupo.](#step-1-design-your-user-and-group-schema)** Identifique os objetos e atributos de que seu aplicativo precisa e determine como eles são mapeados para o esquema de usuário e grupo com suporte pela implementação de SCIM do Azure AD.

  * **[Etapa 2: entender a implementação do SCIM do Azure AD.](#step-2-understand-the-azure-ad-scim-implementation)** Entenda como o cliente SCIM do Azure AD é implementado e modele o tratamento e as respostas de solicitação do protocolo SCIM.

  * **[Etapa 3: criar um ponto de extremidade SCIM.](#step-3-build-a-scim-endpoint)** Um ponto de extremidade deve ser compatível com SCIM 2,0 para integração com o serviço de provisionamento do Azure AD. Como opção, você pode usar as bibliotecas do Microsoft Common Language Infrastructure (CLI) e exemplos de código para criar seu ponto de extremidade. Esses exemplos são apenas para referência e teste; é recomendável que você codifique seu aplicativo de produção para assumir uma dependência deles.

  * **[Etapa 4: integrar seu ponto de extremidade do SCIM com o cliente SCIM do Azure AD.](#step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client)** Se sua organização estiver usando um aplicativo de terceiros que implemente o perfil do SCIM 2,0 ao qual o Azure AD dá suporte, você poderá começar a automatizar o provisionamento e o desprovisionamento de usuários e grupos imediatamente.

  * **[Etapa 5: publicar seu aplicativo na Galeria de aplicativos do Azure AD.](#step-5-publish-your-application-to-the-azure-ad-application-gallery)** Facilite para os clientes descobrirem seu aplicativo e configurar facilmente o provisionamento. 

![Etapas para integrar um ponto de extremidade do SCIM com o Azure AD](media/use-scim-to-provision-users-and-groups/process.png)

## <a name="step-1-design-your-user-and-group-schema"></a>Etapa 1: criar seu esquema de usuário e grupo

Cada aplicativo requer atributos diferentes para criar um usuário ou grupo. Inicie sua integração identificando os objetos (usuários, grupos) e atributos (nome, gerente, cargo, etc.) que seu aplicativo requer. O padrão SCIM define um esquema para gerenciar usuários e grupos. O esquema de usuário principal requer apenas três atributos: **ID** (identificador definido do provedor de serviço), **externalId** (identificador definido pelo cliente) e **meta** (metadados somente leitura mantidos pelo provedor de serviços). Todos os outros atributos são opcionais. Além do esquema de usuário principal, o padrão SCIM define uma extensão de usuário empresarial e um modelo para estender o esquema do usuário para atender às necessidades do seu aplicativo. Se, por exemplo, seu aplicativo exigir um gerente de usuário, você poderá usar o esquema de usuário corporativo para coletar o gerente do usuário e o esquema principal para coletar o email do usuário. Para criar seu esquema, siga as etapas abaixo:
  1. Liste os atributos que seu aplicativo requer. Pode ser útil dividir seus requisitos em atributos necessários para autenticação (por exemplo, loginName e email), atributos necessários para gerenciar o ciclo de vida do usuário (por exemplo, status/ativo) e outros atributos necessários para que seu aplicativo específico funcione (por exemplo, Gerenciador, marca).
  2. Verifique se esses atributos já estão definidos no esquema de usuário principal ou esquema de usuário corporativo. Se quaisquer atributos necessários e não forem cobertos nos esquemas principal ou de usuário corporativo, será necessário definir uma extensão para o esquema de usuário que abrange os atributos necessários. No exemplo a seguir, adicionamos uma extensão ao usuário para permitir o provisionamento de uma "marca" em um usuário. É melhor começar apenas com os esquemas de usuário principal e corporativo e expandir para esquemas personalizados adicionais mais tarde.  
  3. Mapeie os atributos SCIM para os atributos de usuário no Azure AD. Se um dos atributos que você definiu em seu ponto de extremidade do SCIM não tiver uma contraparte clara no esquema de usuário do Azure AD, haverá uma boa chance de os dados não serem armazenados no objeto de usuário em todos os locatários. Considere se esse atributo pode ser opcional para a criação de um usuário. Se o atributo for crítico para que seu aplicativo funcione, Oriente o administrador do locatário para estender seu esquema ou use um atributo de extensão, conforme mostrado abaixo, para a propriedade "tags".

### <a name="table-1-outline-the-attributes-that-you-need"></a>Tabela 1: descrever os atributos de que você precisa 
| Etapa 1: determinar os atributos que seu aplicativo requer| Etapa 2: mapear requisitos de aplicativo para o SCIM Standard| Etapa 3: mapear atributos SCIM para os atributos do Azure AD|
|--|--|--|
|loginName|userName|userPrincipalName|
|firstName|name.givenName|givenName|
|lastName|nome. sobrenome|lastName|
|workMail|Emails [tipo EQ "trabalho"]. valor|Email|
|manager|manager|manager|
|marca|urn: IETF: params: SCIM: schemas: Extension: 2.0: CustomExtension: tag|extensionAttribute1|
|status|ativo|isSoftDeleted (valor calculado não armazenado no usuário)|

O esquema definido acima seria representado usando o conteúdo JSON abaixo. Observe que, além dos atributos necessários para o aplicativo, a representação JSON inclui os atributos obrigatórios "ID", "externalId," e "meta".

```json
{
     "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User",
      "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User",
      "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:User"],
     "userName":"bjensen",
     "externalId":"bjensen",
     "name":{
       "familyName":"Jensen",
       "givenName":"Barbara"
     },
     "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
     "Manager": "123456"
   },
     "urn:ietf:params:scim:schemas:extension:CustomExtensionName:2.0:CustomAttribute:User": {
     "tag": "701984",
   },
   "meta": {
     "resourceType": "User",
     "created": "2010-01-23T04:56:22Z",
     "lastModified": "2011-05-13T04:42:34Z",
     "version": "W\/\"3694e05e9dff591\"",
     "location":
 "https://example.com/v2/Users/2819c223-7f76-453a-919d-413861904646"
   }
 ```

### <a name="table-2-default-user-attribute-mapping"></a>Tabela 2: mapeamento de atributo de usuário padrão
Em seguida, você pode usar a tabela abaixo para entender como os atributos que seu aplicativo requer podem ser mapeados para um atributo no Azure AD e a RFC SCIM. Você pode [Personalizar](customize-application-attributes.md) como os atributos são mapeados entre o Azure AD e o ponto de extremidade do SCIM. Observe que você não precisa dar suporte a usuários e grupos ou a todos os atributos mostrados abaixo. Eles são uma referência para como os atributos no Azure AD geralmente são mapeados para propriedades no protocolo SCIM. 

| Usuário do Active Directory do Azure | “urn:ietf:params:scim:schemas:extension:enterprise:2.0:User” |
| --- | --- |
| IsSoftDeleted |ativo |
|department|urn: IETF: params: SCIM: esquemas: extensão: Enterprise: 2.0: User: Department|
| displayName |displayName |
|employeeId|urn: IETF: params: SCIM: schemas: Extension: Enterprise: 2.0: User: employeeNumber|
| Facsimile-TelephoneNumber |phoneNumbers[type eq "fax"].value |
| givenName |name.givenName |
| jobTitle |título |
| email |emails[type eq "work"].value |
| mailNickname |externalId |
| manager |urn: IETF: params: SCIM: schemas: Extension: Enterprise: 2.0: User: Manager |
| Serviço Móvel |phoneNumbers[type eq "mobile"].value |
| postalCode |addresses[type eq "work"].postalCode |
| proxy-Addresses |emails[type eq "other"].Value |
| physical-Delivery-OfficeName |addresses[type eq "other"].Formatted |
| streetAddress |addresses[type eq "work"].streetAddress |
| sobrenome |name.familyName |
| telephone-Number |phoneNumbers[type eq "work"].value |
| user-PrincipalName |userName |


### <a name="table-3-default-group-attribute-mapping"></a>Tabela 3: mapeamento de atributo de grupo padrão

| Grupo do Active Directory do Azure | urn:ietf:params:scim:schemas:core:2.0:Group |
| --- | --- |
| displayName |displayName |
| email |emails[type eq "work"].value |
| mailNickname |displayName |
| membros |membros |
| objectId |externalId |
| proxyAddresses |emails[type eq "other"].Value |

Há vários pontos de extremidade definidos na RFC SCIM. Você pode começar a usar o ponto de extremidade/User e, em seguida, expandir a partir daí. O ponto de extremidade/schemas é útil ao usar atributos personalizados ou se o esquema for alterado com frequência. Ele permite que um cliente recupere o esquema mais atualizado automaticamente. O ponto de extremidade/Bulk é especialmente útil ao dar suporte a grupos. A tabela a seguir descreve os vários pontos de extremidade definidos no padrão SCIM. O ponto de extremidade/schemas é útil ao usar atributos personalizados ou se o esquema for alterado com frequência. Ele permite que um cliente recupere o esquema mais atualizado automaticamente. O ponto de extremidade/Bulk é especialmente útil ao dar suporte a grupos. A tabela a seguir descreve os vários pontos de extremidade definidos no padrão SCIM. 
 
### <a name="table-4-determine-the-endpoints-that-you-would-like-to-develop"></a>Tabela 4: determinar os pontos de extremidade que você deseja desenvolver
|PONTO DE EXTREMIDADE|DESCRIÇÃO|
|--|--|
|/|Executar operações CRUD em um objeto de usuário.|
|/Group|Executar operações CRUD em um objeto de grupo.|
|/ServiceProviderConfig|Fornece detalhes sobre os recursos do padrão SCIM que têm suporte, por exemplo, os recursos com suporte e o método de autenticação.|
|/ResourceTypes|Especifica os metadados sobre cada recurso|
|/Schemas|O conjunto de atributos com suporte de cada cliente e provedor de serviços pode variar. Embora um provedor de serviços possa incluir "nome", "título" e "emails", enquanto outro provedor de serviços usa "nome", "título" e "phoneNumbers". O ponto de extremidade de esquemas permite a descoberta dos atributos com suporte.|
|/Bulk|As operações em massa permitem que você execute operações em uma grande coleção de objetos de recurso em uma única operação (por exemplo, atualizar associações para um grupo grande).|


## <a name="step-2-understand-the-azure-ad-scim-implementation"></a>Etapa 2: entender a implementação de SCIM do Azure AD
> [!IMPORTANT]
> O comportamento da implementação do SCIM do Azure AD foi atualizado pela última vez em 18 de dezembro de 2018. Para obter informações sobre o que mudou, consulte [Conformidade do protocolo SCIM 2.0 do serviço de provisionamento de usuário do Azure AD](application-provisioning-config-problem-scim-compatibility.md).

Se você estiver criando um aplicativo que dá suporte a uma API de gerenciamento de usuário do SCIM 2,0, esta seção descreve detalhadamente como o cliente SCIM do Azure AD é implementado. Ele também mostra como modelar seu tratamento e respostas de solicitação de protocolo SCIM. Depois de implementar o ponto de extremidade do SCIM, você pode testá-lo seguindo o procedimento descrito na seção anterior.

Na [especificação do protocolo SCIM 2,0](http://www.simplecloud.info/#Specification), seu aplicativo deve atender a esses requisitos:

* Dá suporte à criação de usuários e, opcionalmente, também a grupos, de acordo com [a seção 3,3 do protocolo scim](https://tools.ietf.org/html/rfc7644#section-3.3).  
* Dá suporte à modificação de usuários ou grupos com solicitações de PATCH, de acordo com [a seção 3.5.2 do protocolo scim](https://tools.ietf.org/html/rfc7644#section-3.5.2).  
* Dá suporte à recuperação de um recurso conhecido para um usuário ou grupo criado anteriormente, de acordo com [a seção 3.4.1 do protocolo scim](https://tools.ietf.org/html/rfc7644#section-3.4.1).  
* Dá suporte à consulta de usuários ou grupos, de acordo com [a seção 3.4.2 do protocolo scim](https://tools.ietf.org/html/rfc7644#section-3.4.2).  Por padrão, os usuários são recuperados por seus `id` e consultados por seus `username` e `externalid`, e os grupos são consultados pelo `displayName`.  
* Dá suporte à consulta de usuário por ID e pelo gerente, de acordo com a seção 3.4.2 do protocolo SCIM.  
* Dá suporte à consulta de grupos por ID e por membro, de acordo com a seção 3.4.2 do protocolo SCIM.  
* Aceita um único token de portador para autenticação e autorização do Azure AD para seu aplicativo.

Siga estas diretrizes gerais ao implementar um ponto de extremidade SCIM para garantir a compatibilidade com o Azure AD:

* `id` é uma propriedade necessária para todos os recursos. Cada resposta que retorna um recurso deve garantir que cada recurso tenha essa propriedade, exceto para `ListResponse` com zero membros.
* A resposta a uma solicitação de consulta/filtro sempre deve ser uma `ListResponse`.
* Os grupos são opcionais, mas só têm suporte se a implementação do SCIM der suporte a solicitações de PATCH.
* Não é necessário incluir o recurso inteiro na resposta do PATCH.
* Microsoft Azure AD usa apenas os seguintes operadores:  
    - `eq`
    - `and`
* Não exija uma correspondência que diferencia maiúsculas de minúsculas em elementos estruturais em SCIM, em particular `op` valores de operação, conforme definido em https://tools.ietf.org/html/rfc7644#section-3.5.2. O Azure AD emite os valores de "op" como `Add`, `Replace`e `Remove`.
* Microsoft Azure AD faz com que as solicitações busquem um usuário aleatório e um grupo para garantir que o ponto de extremidade e as credenciais sejam válidos. Isso também é feito como parte do fluxo de **conexão de teste** no [portal do Azure](https://portal.azure.com). 
* O atributo no qual os recursos podem ser consultados deve ser definido como um atributo correspondente no aplicativo na [portal do Azure](https://portal.azure.com). Para obter mais informações, consulte [Personalizando mapeamentos de atributo de provisionamento de usuário](customize-application-attributes.md)

### <a name="user-provisioning-and-deprovisioning"></a>Provisionamento e desprovisionamento de usuários

A ilustração a seguir mostra as mensagens que Azure Active Directory envia para um serviço SCIM para gerenciar o ciclo de vida de um usuário no repositório de identidades do aplicativo.  

![mostra a sequência de provisionamento e desprovisionamento de usuário](media/use-scim-to-provision-users-and-groups/scim-figure-4.png)<br/>
*Sequência de provisionamento e desprovisionamento de usuário*

### <a name="group-provisioning-and-deprovisioning"></a>Provisionamento e desprovisionamento de grupo

O provisionamento e o desprovisionamento de grupos são opcionais. Quando implementado e habilitado, a ilustração a seguir mostra as mensagens que o Azure AD envia a um serviço SCIM para gerenciar o ciclo de vida de um grupo no repositório de identidades do aplicativo.  Essas mensagens diferem das mensagens sobre os usuários de duas maneiras:

* Solicitações para recuperar grupos especificam que o atributo Members deve ser excluído de qualquer recurso fornecido em resposta à solicitação.  
* As solicitações para determinar se um atributo de referência tem um determinado valor são sobre o atributo de membros.  

![mostra a sequência de provisionamento e desprovisionamento de grupo](media/use-scim-to-provision-users-and-groups/scim-figure-5.png)<br/>
*Sequência de provisionamento e desprovisionamento de grupo*

### <a name="scim-protocol-requests-and-responses"></a>Solicitações e respostas do protocolo SCIM
Esta seção fornece exemplos de solicitações SCIM emitidas pelo cliente SCIM do Azure AD e exemplos de respostas esperadas. Para obter melhores resultados, você deve codificar seu aplicativo para lidar com essas solicitações nesse formato e emitir as respostas esperadas.

> [!IMPORTANT]
> Para entender como e quando o serviço de provisionamento de usuário do Azure AD emite as operações descritas abaixo, consulte a seção [ciclos de provisionamento: inicial e incremental](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) em [como o provisionamento funciona](how-provisioning-works.md).

[Operações do usuário](#user-operations)
  - [Criar usuário](#create-user) ([solicitar](#request) [resposta](#response) / )
  - [Obter usuário](#get-user) ([solicitação](#request-1) / [resposta](#response-1))
  - [Obter usuário por consulta](#get-user-by-query) ([solicitação](#request-2) / [resposta](#response-2))
  - [Obter resultados do usuário por consulta-zero](#get-user-by-query---zero-results) ([solicitação](#request-3)
/ [resposta](#response-3))
  - [Atualizar usuário [Propriedades com vários valores]](#update-user-multi-valued-properties) ([solicitação](#request-4) /  [resposta](#response-4))
  - [Atualizar usuário [Propriedades de valor único]](#update-user-single-valued-properties) ([solicitação](#request-5)
/ [resposta](#response-5)) 
  - [Desabilitar usuário](#disable-user) ([solicitação](#request-14) / 
[resposta](#response-14))
  - [Excluir usuário](#delete-user) ([solicitação](#request-6) / 
[resposta](#response-6))


[Operações de grupo](#group-operations)
  - [Criar grupo](#create-group) ( [solicitar](#request-7) [resposta](#response-7) / )
  - [Obter grupo](#get-group) ( [solicitar](#request-8) [resposta](#response-8) / )
  - [Obter grupo por DisplayName](#get-group-by-displayname) ([solicitação](#request-9) / [resposta](#response-9))
  - [Grupo de atualização [atributos que não são membros]](#update-group-non-member-attributes) ([solicitação](#request-10) /
 [resposta](#response-10))
  - [Atualizar grupo [adicionar membros]](#update-group-add-members) ( [solicitar](#request-11) /
[resposta](#response-11))
  - [Atualizar grupo [remover membros]](#update-group-remove-members) ( [solicitar](#request-12) /
[resposta](#response-12))
  - [Excluir grupo](#delete-group) ([solicitar](#request-13) [resposta](#response-13) /
)

### <a name="user-operations"></a>Operações do usuário

* Os usuários podem ser consultados por `userName` ou atributos de `email[type eq "work"]`.  

#### <a name="create-user"></a>Criar Usuário

###### <a name="request"></a>Solicitação

*PÓS/Users*
```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"],
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "active": true,
    "emails": [{
        "primary": true,
        "type": "work",
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com"
    }],
    "meta": {
        "resourceType": "User"
    },
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName"
    },
    "roles": []
}
```

##### <a name="response"></a>Resposta

*HTTP/1.1 201 criado*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "48af03ac28ad4fb88478",
    "externalId": "0a21f0f2-8d2a-4f8e-bf98-7363c4aed4ef",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_ab6490ee-1e48-479e-a20b-2d77186b5dd1",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_fd0ea19b-0777-472c-9f96-4f70d2226f2e@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="get-user"></a>Obter usuário

###### <a name="request-1"></a>Quest
*OBTER/Users/5d48a0a8e9f04aa38008* 

###### <a name="response-1"></a>Resposta (usuário encontrado)
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5d48a0a8e9f04aa38008",
    "externalId": "58342554-38d6-4ec8-948c-50044d0a33fd",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_feed3ace-693c-4e5a-82e2-694be1b39934",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_22370c1a-9012-42b2-bf64-86099c2a1c22@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

###### <a name="request"></a>Solicitação
*OBTER/Users/5171a35d82074e068ce2* 

###### <a name="response-user-not-found-note-that-the-detail-is-not-required-only-status"></a>Resposta (usuário não encontrado. Observe que os detalhes não são obrigatórios, apenas status.)

```json
{
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:Error"
    ],
    "status": "404",
    "detail": "Resource 23B51B0E5D7AE9110A49411D@7cca31655d49f3640a494224 not found"
}
```

#### <a name="get-user-by-query"></a>Obter usuário por consulta

##### <a name="request-2"></a>Quest

*GET/useres? Filter = userName EQ "Test_User_dfeef4c5-5681 -4387-b016-bdf221e82081"*

##### <a name="response-2"></a>Responde

*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
        "id": "2441309d85324e7793ae",
        "externalId": "7fce0092-d52e-4f76-b727-3955bd72c939",
        "meta": {
            "resourceType": "User",
            "created": "2018-03-27T19:59:26.000Z",
            "lastModified": "2018-03-27T19:59:26.000Z"
            
        },
        "userName": "Test_User_dfeef4c5-5681-4387-b016-bdf221e82081",
        "name": {
            "familyName": "familyName",
            "givenName": "givenName"
        },
        "active": true,
        "emails": [{
            "value": "Test_User_91b67701-697b-46de-b864-bd0bbe4f99c1@testuser.com",
            "type": "work",
            "primary": true
        }]
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="get-user-by-query---zero-results"></a>Obter o usuário por consulta-zero resultados

##### <a name="request-3"></a>Quest

*GET/useres? Filter = userName EQ "usuário não existente"*

##### <a name="response-3"></a>Responde

*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 0,
    "Resources": [],
    "startIndex": 1,
    "itemsPerPage": 20
}

```

#### <a name="update-user-multi-valued-properties"></a>Atualizar usuário [Propriedades com vários valores]

##### <a name="request-4"></a>Quest

*PATCH/Users/6764549bef60420686bc HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [
            {
            "op": "Replace",
            "path": "emails[type eq \"work\"].value",
            "value": "updatedEmail@microsoft.com"
            },
            {
            "op": "Replace",
            "path": "name.familyName",
            "value": "updatedFamilyName"
            }
    ]
}
```

##### <a name="response-4"></a>Responde

*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "6764549bef60420686bc",
    "externalId": "6c75de36-30fa-4d2d-a196-6bdcdb6b6539",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "userName": "Test_User_fbb9dda4-fcde-4f98-a68b-6c5599e17c27",
    "name": {
        "formatted": "givenName updatedFamilyName",
        "familyName": "updatedFamilyName",
        "givenName": "givenName"
    },
    "active": true,
    "emails": [{
        "value": "updatedEmail@microsoft.com",
        "type": "work",
        "primary": true
    }]
}
```

#### <a name="update-user-single-valued-properties"></a>Atualizar usuário [Propriedades de valor único]

##### <a name="request-5"></a>Quest

*PATCH/Users/5171a35d82074e068ce2 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "userName",
        "value": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com"
    }]
}
```

##### <a name="response-5"></a>Responde

*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id": "5171a35d82074e068ce2",
    "externalId": "aa1eca08-7179-4eeb-a0be-a519f7e5cd1a",
    "meta": {
        "resourceType": "User",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "userName": "5b50642d-79fc-4410-9e90-4c077cdd1a59@testuser.com",
    "name": {
        "formatted": "givenName familyName",
        "familyName": "familyName",
        "givenName": "givenName",
    },
    "active": true,
    "emails": [{
        "value": "Test_User_49dc1090-aada-4657-8434-4995c25a00f7@testuser.com",
        "type": "work",
        "primary": true
    }]
}
```

### <a name="disable-user"></a>Desabilitar usuário

##### <a name="request-14"></a>Quest

*PATCH/Users/5171a35d82074e068ce2 HTTP/1.1*
```json
{
    "Operations": [
        {
            "op": "Replace",
            "path": "active",
            "value": false
        }
    ],
    "schemas": [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"
    ]
}
```

##### <a name="response-14"></a>Responde

```json
{
    "schemas": [
        "urn:ietf:params:scim:schemas:core:2.0:User"
    ],
    "id": "CEC50F275D83C4530A495FCF@834d0e1e5d8235f90a495fda",
    "userName": "deanruiz@testuser.com",
    "name": {
        "familyName": "Harris",
        "givenName": "Larry"
    },
    "active": false,
    "emails": [
        {
            "value": "gloversuzanne@testuser.com",
            "type": "work",
            "primary": true
        }
    ],
    "addresses": [
        {
            "country": "ML",
            "type": "work",
            "primary": true
        }
    ],
    "meta": {
        "resourceType": "Users",
        "location": "/scim/5171a35d82074e068ce2/Users/CEC50F265D83B4530B495FCF@5171a35d82074e068ce2"
    }
}
```
#### <a name="delete-user"></a>Excluir usuário

##### <a name="request-6"></a>Quest

*EXCLUIR/Users/5171a35d82074e068ce2 HTTP/1.1*

##### <a name="response-6"></a>Responde

*HTTP/1.1 204 sem conteúdo*

### <a name="group-operations"></a>Operações de grupo

* Os grupos sempre devem ser criados com uma lista de Membros vazios.
* Os grupos podem ser consultados pelo atributo `displayName`.
* A atualização para a solicitação de PATCH de grupo deve gerar um *HTTP 204 sem conteúdo* na resposta. O retorno de um corpo com uma lista de todos os membros não é aconselhável.
* Não é necessário dar suporte ao retorno de todos os membros do grupo.

#### <a name="create-group"></a>{1&gt;Criar Grupo&lt;1}

##### <a name="request-7"></a>Quest

*POST/groups HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group", "http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/2.0/Group"],
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "displayName": "displayName",
    "meta": {
        "resourceType": "Group"
    }
}
```

##### <a name="response-7"></a>Responde

*HTTP/1.1 201 criado*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "927fa2c08dcb4a7fae9e",
    "externalId": "8aa1a0c0-c4c3-4bc0-b4a5-2ef676900159",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
        
    },
    "displayName": "displayName",
    "members": []
}
```

#### <a name="get-group"></a>Obter grupo

##### <a name="request-8"></a>Quest

*OBTER/groups/40734ae655284ad3abcc? excludeble = Members HTTP/1.1*

##### <a name="response-8"></a>Responde
*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
    "id": "40734ae655284ad3abcc",
    "externalId": "60f1bb27-2e1e-402d-bcc4-ec999564a194",
    "meta": {
        "resourceType": "Group",
        "created": "2018-03-27T19:59:26.000Z",
        "lastModified": "2018-03-27T19:59:26.000Z"
    },
    "displayName": "displayName",
}
```

#### <a name="get-group-by-displayname"></a>Obter grupo por displayName

##### <a name="request-9"></a>Quest
*GET/groups? excludeble = Members & Filter = displayName EQ "displayName" HTTP/1.1*

##### <a name="response-9"></a>Responde

*HTTP/1.1 200 OK*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
    "totalResults": 1,
    "Resources": [{
        "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
        "id": "8c601452cc934a9ebef9",
        "externalId": "0db508eb-91e2-46e4-809c-30dcbda0c685",
        "meta": {
            "resourceType": "Group",
            "created": "2018-03-27T22:02:32.000Z",
            "lastModified": "2018-03-27T22:02:32.000Z",
            
        },
        "displayName": "displayName",
    }],
    "startIndex": 1,
    "itemsPerPage": 20
}
```

#### <a name="update-group-non-member-attributes"></a>Atualizar grupo [atributos de não-membro]

##### <a name="request-10"></a>Quest

*PATCH/groups/fa2ce26709934589afc5 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Replace",
        "path": "displayName",
        "value": "1879db59-3bdf-4490-ad68-ab880a269474updatedDisplayName"
    }]
}
```

##### <a name="response-10"></a>Responde

*HTTP/1.1 204 sem conteúdo*

### <a name="update-group-add-members"></a>Atualizar grupo [adicionar membros]

##### <a name="request-11"></a>Quest

*PATCH/groups/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Add",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response-11"></a>Responde

*HTTP/1.1 204 sem conteúdo*

#### <a name="update-group-remove-members"></a>Atualizar grupo [remover membros]

##### <a name="request-12"></a>Quest

*PATCH/groups/a99962b9f99d4c4fac67 HTTP/1.1*
```json
{
    "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
    "Operations": [{
        "op": "Remove",
        "path": "members",
        "value": [{
            "$ref": null,
            "value": "f648f8d5ea4e4cd38e9c"
        }]
    }]
}
```

##### <a name="response-12"></a>Responde

*HTTP/1.1 204 sem conteúdo*

#### <a name="delete-group"></a>Excluir Grupo

##### <a name="request-13"></a>Quest

*EXCLUIR/groups/cdb1ce18f65944079d37 HTTP/1.1*

##### <a name="response-13"></a>Responde

*HTTP/1.1 204 sem conteúdo*

### <a name="security-requirements"></a>Requisitos de segurança
**Versões do protocolo TLS**

As únicas versões aceitáveis do protocolo TLS são TLS 1,2 e TLS 1,3. Nenhuma outra versão do TLS é permitida. Nenhuma versão do SSL é permitida. 
- As chaves RSA devem ter pelo menos 2.048 bits.
- Chaves ECC devem ter pelo menos 256 bits, geradas usando uma curva elíptica aprovada


**Comprimentos de chave**

Todos os serviços devem usar certificados X. 509 gerados usando chaves de criptografia de comprimento suficiente, o que significa:

**Conjuntos de codificação**

Todos os serviços devem ser configurados para usar os seguintes conjuntos de codificação, na ordem exata especificada abaixo. Observe que, se você tiver apenas um certificado RSA, os conjuntos de codificação ECDSA não terão nenhum efeito. </br>

Barra mínima dos pacotes de criptografia TLS 1,2:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="step-3-build-a-scim-endpoint"></a>Etapa 3: criar um ponto de extremidade SCIM

Agora que você criou seu esquema e entendeu a implementação do Azure AD SCIM, você pode começar a desenvolver seu ponto de extremidade do SCIM. Em vez de começar do zero e criar a implementação completamente por conta própria, você pode contar com várias bibliotecas de SCIM de software livre publicadas pelo commuinty SCIM.  
O [código de referência](https://aka.ms/SCIMReferenceCode) do .NET Core de código-fonte aberto publicado pela equipe de provisionamento do Azure AD é um recurso que pode começar a desenvolver. Depois de criar seu ponto de extremidade SCIM, você desejará testá-lo. Você pode usar a coleção de [testes de postmaster](https://github.com/AzureAD/SCIMReferenceCode/wiki/Test-Your-SCIM-Endpoint) fornecidos como parte do código de referência ou executar as solicitações/respostas de exemplo fornecidas [acima](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#user-operations).  

Observação: o código de referência destina-se a ajudá-lo a começar a criar seu ponto de extremidade SCIM e é fornecido "no estado em que se encontra". As contribuições da Comunidade são boas-vindas para ajudar a criar e manter o código. 

## <a name="step-4-integrate-your-scim-endpoint-with-the-azure-ad-scim-client"></a>Etapa 4: integrar seu ponto de extremidade do SCIM com o cliente SCIM do Azure AD

O Azure AD pode ser configurado para provisionar automaticamente usuários e grupos atribuídos a aplicativos que implementam um perfil específico do [protocolo SCIM 2,0](https://tools.ietf.org/html/rfc7644). As especificidades do perfil são documentadas na [etapa 2: entender a implementação do Azure ad scim](#step-2-understand-the-azure-ad-scim-implementation).

Verifique com o seu provedor de aplicativo ou na documentação do seu provedor de aplicativo as declarações de compatibilidade com esses requisitos.

> [!IMPORTANT]
> A implementação do Azure AD SCIM é criada sobre o serviço de provisionamento de usuários do Azure AD, que é projetado para manter constantemente os usuários sincronizados entre o Azure AD e o aplicativo de destino e implementa um conjunto muito específico de operações padrão. É importante entender esses comportamentos para entender o comportamento do cliente SCIM do Azure AD. Para obter mais informações, consulte a seção [ciclos de provisionamento: inicial e incremental](how-provisioning-works.md#provisioning-cycles-initial-and-incremental) em [como o provisionamento funciona](how-provisioning-works.md).

### <a name="getting-started"></a>Introdução

Os aplicativos que dão suporte ao perfil SCIM descrito neste artigo podem ser conectados ao Azure Active Directory usando o recurso de "aplicativo não galeria" na galeria de aplicativos do Azure AD. Uma vez conectado, o Azure AD executa um processo de sincronização a cada 40 minutos, em que ele consulta o ponto de extremidade SCIM do aplicativo para os usuários e grupos atribuídos e os cria ou modifica de acordo com os detalhes da atribuição.

**Para conectar um aplicativo que dê suporte a SCIM:**

1. Entre no portal de [Azure Active Directory](https://aad.portal.azure.com). Observe que você pode obter acesso a uma avaliação gratuita para Azure Active Directory com licenças P2 inscrevendo-se no [programa de desenvolvedor](https://developer.microsoft.com/office/dev-program)
2. Selecione **aplicativos empresariais** no painel esquerdo. Uma lista de todos os aplicativos configurados é mostrada, incluindo os aplicativos que foram adicionados da galeria.
3. Selecione **+ novo aplicativo** > **todos os** > **aplicativo inexistente na Galeria**.
4. Insira um nome para seu aplicativo e selecione **Adicionar** para criar um objeto de aplicativo. O novo aplicativo é adicionado à lista de aplicativos empresariais e é aberto em sua tela de gerenciamento de aplicativo.

   ![captura de tela mostra a Galeria de aplicativos do Azure AD](media/use-scim-to-provision-users-and-groups/scim-figure-2a.png)<br/>
   *Galeria de aplicativos do Azure AD*

5. Na tela gerenciamento de aplicativos, selecione **provisionamento** no painel esquerdo.
6. No menu **Modo de Provisionamento**, selecione **Automático**.

   ![exemplo: uma página de provisionamento de um aplicativo no portal do Azure](media/use-scim-to-provision-users-and-groups/scim-figure-2b.png)<br/>
   *Configurando o provisionamento no portal do Azure*

7. No campo **URL do locatário** , insira a URL do ponto de extremidade do SCIM do aplicativo. Exemplo: https://api.contoso.com/scim/
8. Se o ponto de extremidade SCIM exigir um token de portador OAuth de um emissor diferente do Azure AD, copie o token de portador OAuth necessário para o campo opcional **Token Secreto**. Se esse campo for deixado em branco, o Azure AD incluirá um token de portador OAuth emitido pelo Azure AD com cada solicitação. Aplicativos que usam o Azure AD como provedor de identidade podem validar esse token emitido pelo Azure AD. 
   > [!NOTE]
   > ***Não*** é recomendável deixar esse campo em branco e contar com um token gerado pelo Azure AD. Essa opção está disponível principalmente para fins de teste.
9. Selecione **testar conexão** para que Azure Active Directory tente se conectar ao ponto de extremidade SCIM. Se a tentativa falhar, as informações de erro serão exibidas.  

    > [!NOTE]
    > **Testar Conexão** consulta o ponto de extremidade SCIM para um usuário que não existe, usando um GUID aleatório como a propriedade correspondente selecionada na configuração do Azure AD. A resposta correta esperada é HTTP 200 OK com uma mensagem de SCIM ListResponse vazia.

10. Se as tentativas de conexão com o aplicativo forem bem-sucedidos, selecione **salvar** para salvar as credenciais de administrador.
11. Na seção **mapeamentos** , há dois conjuntos selecionáveis de [mapeamentos de atributo](customize-application-attributes.md): um para objetos de usuário e outro para objetos de grupo. Selecione cada um para revisar os atributos que são sincronizados do Azure Active Directory para seu aplicativo. Os atributos selecionados como propriedades **Correspondentes** serão usados para fazer a correspondência entre os usuários e os grupos no seu aplicativo para operações de atualização. Para confirmar eventuais alterações, selecione **Salvar**.

    > [!NOTE]
    > Opcionalmente, você pode desabilitar a sincronização dos objetos de grupo desabilitando o mapeamento "grupos".

12. Em **Configurações**, o campo **Escopo** define quais usuários e grupos são sincronizados. Selecione **sincronizar somente usuários e grupos atribuídos** (recomendado) para sincronizar somente usuários e grupos atribuídos na guia **usuários e grupos** .
13. Quando a configuração for concluída, defina o **status de provisionamento** como **ativado**.
14. Selecione **salvar** para iniciar o serviço de provisionamento do Azure AD.
15. Se estiver sincronizando apenas usuários e grupos atribuídos (recomendado), selecione a guia **usuários e grupos** e atribua os usuários ou grupos que você deseja sincronizar.

Depois que o ciclo inicial for iniciado, você poderá selecionar **logs de provisionamento** no painel esquerdo para monitorar o progresso, que mostra todas as ações realizadas pelo serviço de provisionamento em seu aplicativo. Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](check-status-user-account-provisioning.md).

> [!NOTE]
> O ciclo inicial demora mais para ser executado do que as sincronizações posteriores, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço esteja em execução.

## <a name="step-5-publish-your-application-to-the-azure-ad-application-gallery"></a>Etapa 5: publicar seu aplicativo na Galeria de aplicativos do Azure AD

Se você estiver criando um aplicativo que será usado por mais de um locatário, você poderá disponibilizá-lo na Galeria de aplicativos do Azure AD. Isso facilitará para as organizações descobrirem o aplicativo e configurar o provisionamento. Publicar seu aplicativo na galeria do Azure AD e disponibilizar o provisionamento para outras pessoas é fácil. Confira as etapas [aqui](../develop/howto-app-gallery-listing.md). A Microsoft trabalhará com você para integrar seu aplicativo em nossa galeria, testar seu ponto de extremidade e liberar a [documentação](../saas-apps/tutorial-list.md) de integração para os clientes usarem. 

### <a name="gallery-onboarding-checklist"></a>Lista de verificação de integração da Galeria
Siga a lista de verificação abaixo para garantir que seu aplicativo seja integrado de forma rápida e os clientes tenham uma experiência de implantação tranqüila. As informações serão coletadas quando você estiver na integração à galeria. 
> [!div class="checklist"]
> * Suporte a um usuário do [SCIM 2,0](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#step-2-understand-the-azure-ad-scim-implementation) e ponto de extremidade do grupo (apenas um é necessário, mas ambos são recomendados)
> * Suporte a pelo menos 25 solicitações por segundo por locatário (obrigatório)
> * Estabelecer contatos de engenharia e suporte para orientar os clientes na integração da galeria (obrigatório)
> * 3 credenciais de teste sem expiração para seu aplicativo (obrigatório)
> * Suporte à concessão de código de autorização OAuth ou a um token de vida útil longa, conforme descrito abaixo (obrigatório)
> * Estabelecer uma engenharia e um ponto de suporte de contato para dar suporte aos clientes postando a galeria (obrigatório)
> * Suporte à atualização de várias associações de grupo com um único PATCH (recomendado) 
> * Documentar seu ponto de extremidade SCIM publicamente (recomendado) 
> * [Descoberta de esquema de suporte](https://tools.ietf.org/html/rfc7643#section-6) (recomendado)


### <a name="authorization-for-provisioning-connectors-in-the-application-gallery"></a>Autorização para o provisionamento de conectores na Galeria de aplicativos
A especificação SCIM não define um esquema específico de SCIM para autenticação e autorização. Ele se baseia no uso de padrões do setor existentes. O cliente de provisionamento do Azure AD dá suporte a dois métodos de autorização para aplicativos na galeria. 

|Método de autorização|Vantagens|Desvantagens|Suporte|
|--|--|--|--|
|Nome de usuário e senha (não recomendado ou com suporte do Azure AD)|Fácil de implementar|Inseguro – [seu PA $ $Word não importa](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984)|Com suporte caso a caso para aplicativos da galeria. Sem suporte para aplicativos que não são da galeria.|
|Token de portador de vida longa|Tokens de vida longa não exigem que um usuário esteja presente. Eles são fáceis para os administradores usarem ao configurar o provisionamento.|Tokens de vida útil longa podem ser difíceis de compartilhar com um administrador sem usar métodos inseguros, como email. |Com suporte para aplicativos de galeria e não galeria. |
|Concessão de código de autorização OAuth|Os tokens de acesso são muito mais curtos do que as senhas e têm um mecanismo de atualização automatizado que os tokens de portador de vida longa não têm.  Um usuário real deve estar presente durante a autorização inicial, adicionando um nível de responsabilidade. |Exige que um usuário esteja presente. Se o usuário sair da organização, o token será inválido e a autorização precisará ser concluída novamente.|Com suporte para aplicativos da galeria. O suporte para aplicativos que não são da galeria está em andamento.|
|Concessão de credenciais de cliente OAuth|Os tokens de acesso são muito mais curtos do que as senhas e têm um mecanismo de atualização automatizado que os tokens de portador de vida longa não têm. A concessão de código de autorização e a concessão de credenciais de cliente criam o mesmo tipo de token de acesso; portanto, a movimentação entre esses métodos é transparente para a API.  O provisionamento pode ser completamente automatizado e novos tokens podem ser silenciosamente solicitados sem interação do usuário. ||Sem suporte para aplicativos de galeria e não galeria. O suporte está em nossa pendência.|

[!NOTE] Não é recomendável deixar o campo de token em branco na interface do usuário do aplicativo personalizado de configuração de provisionamento do Azure AD. O token gerado está disponível principalmente para fins de teste.

**Fluxo de concessão de código de autorização OAuth:** O serviço de provisionamento dá suporte à [concessão de código de autorização](https://tools.ietf.org/html/rfc6749#page-24). Depois de enviar sua solicitação para publicar seu aplicativo na Galeria, nossa equipe trabalhará com você para coletar as seguintes informações:
*  URL de autorização: uma URL pelo cliente para obter autorização do proprietário do recurso por meio do redirecionamento de agente do usuário. O usuário é redirecionado para essa URL para autorizar o acesso. 
*  URL de troca de token: uma URL pelo cliente para trocar uma concessão de autorização para um token de acesso, normalmente com autenticação de cliente.
*  ID do cliente: o servidor de autorização emite o cliente registrado um identificador de cliente, que é uma cadeia de caracteres exclusiva que representa as informações de registro fornecidas pelo cliente.  O identificador do cliente não é um segredo; Ele é exposto ao proprietário do recurso e **não deve** ser usado sozinho para autenticação de cliente.  
*  Segredo do cliente: o segredo do cliente é um segredo gerado pelo servidor de autorização. Deve ser um valor exclusivo conhecido apenas para o servidor de autorização. 

Observe que o OAuth v1 não tem suporte devido à exposição do segredo do cliente. Há suporte para OAuth v2.  

Práticas recomendadas (recomendado, mas não obrigatório):
* Suporte a várias URLs de redirecionamento. Os administradores podem configurar o provisionamento de "portal.azure.com" e "aad.portal.azure.com". O suporte a várias URLs de redirecionamento garantirá que os usuários possam autorizar o acesso do Portal.
* Dê suporte a vários segredos para garantir uma renovação de segredo suave, sem tempo de inatividade. 

**Tokens de portador OAuth de vida útil longa:** Se o seu aplicativo não oferecer suporte ao fluxo de concessão de código de autorização OAuth, você também poderá gerar um token de portador OAuth de vida útil longa do que um administrador pode usar para configurar a integração de provisionamento. O token deve ser permanente ou, caso contrário, o trabalho de provisionamento será [colocado em quarentena](application-provisioning-quarantine-status.md) quando o token expirar. Esse token deve estar abaixo de 1 KB de tamanho.  

Para métodos de autenticação e autorização adicionais, informe-nos no [UserVoice](https://aka.ms/appprovisioningfeaturerequest).

### <a name="gallery-go-to-market-launch-check-list"></a>Lista de verificação de início do lançamento no mercado da Galeria
Para ajudar a impulsionar o reconhecimento e a demanda de nossa integração conjunta, recomendamos que você atualize sua documentação existente e aumente a integração em seus canais de marketing.  A seguir está um conjunto de atividades de lista de verificação que recomendamos que você conclua para dar suporte ao lançamento

* **Preparação para vendas e atendimento ao cliente.** Verifique se suas equipes de vendas e de suporte estão cientes e podem falar com os recursos de integração. Resuma sua equipe de vendas e suporte, forneça perguntas frequentes e inclua a integração em seus materiais de vendas. 
* **Postagem no blog e/ou pressione a versão.** Crie uma postagem no blog ou pressione a versão que descreve a integração conjunta, os benefícios e como começar. [Exemplo: Imprivata e Azure Active Directory pressione Release](https://www.imprivata.com/company/press/imprivata-introduces-iam-cloud-platform-healthcare-supported-microsoft) 
* **Mídia social.** Aproveite sua mídia social, como o Twitter, o Facebook ou o LinkedIn, para promover a integração aos seus clientes. Certifique-se de incluir @AzureAD de forma que possamos retweetr sua postagem. [Exemplo: postagem do Twitter Imprivata](https://twitter.com/azuread/status/1123964502909779968)
* **Site de marketing.** Crie ou atualize suas páginas de marketing (por exemplo, página de integração, página de parceiro, página de preços, etc.) para incluir a disponibilidade da integração conjunta. [Exemplo: página de integração do Pingboard](https://pingboard.com/org-chart-for), [página de integração do SmartSheet](https://www.smartsheet.com/marketplace/apps/microsoft-azure-ad), página de preços do [Monday.com](https://monday.com/pricing/) 
* **Documentação técnica.** Crie um artigo do centro de ajuda ou documentação técnica sobre como os clientes podem começar. [Exemplo: Envoy + integração de Microsoft Azure Active Directory.](https://envoy.help/en/articles/3453335-microsoft-azure-active-directory-integration/
) 
* **Comunicação do cliente.** Alerte os clientes da nova integração por meio da comunicação do cliente (boletins informativos mensais, campanhas de email, notas de versão do produto). 

### <a name="allow-ip-addresses-used-by-the-azure-ad-provisioning-service-to-make-scim-requests"></a>Permitir que os endereços IP usados pelo serviço de provisionamento do Azure AD façam solicitações SCIM

Determinados aplicativos permitem o tráfego de entrada para seu aplicativo. Para que o serviço de provisionamento do Azure AD funcione conforme o esperado, os endereços IP usados devem ser permitidos. Para obter uma lista de endereços IP para cada tag de serviço/região, confira o arquivo JSON – [Intervalos de IP do Azure e marcas de serviço – nuvem pública](https://www.microsoft.com/download/details.aspx?id=56519). Você pode baixar e programar esses IPs em seu firewall, conforme necessário. Os intervalos de IP reservados para o provisionamento do Azure AD podem ser encontrados em "AzureActiveDirectoryDomainServices".

## <a name="related-articles"></a>{1&gt;{2&gt;Artigos relacionados&lt;2}&lt;1}

* [Automatizar o provisionamento e o desprovisionamento de usuários para aplicativos SaaS](user-provisioning.md)
* [Personalizar mapeamentos de atributos para provisionamento do usuário](customize-application-attributes.md)
* [Gravando expressões para mapeamentos de atributo](functions-for-customizing-application-data.md)
* [Filtros de escopo para provisionamento de usuário](define-conditional-rules-for-provisioning-user-accounts.md)
* [Notificações de provisionamento de conta](user-provisioning.md)
* [Lista de tutoriais sobre como integrar aplicativos SaaS](../saas-apps/tutorial-list.md)
