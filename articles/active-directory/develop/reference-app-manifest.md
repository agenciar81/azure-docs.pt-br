---
title: Entendendo o manifesto do aplicativo do Azure Active Directory | Microsoft Docs
description: A cobertura detalhada do manifesto do aplicativo do Azure Active Directory, que representa a configuração de identidade de um aplicativo em um locatário do Azure AD, e é usado para facilitar a autorização do OAuth, a experiência de consentimento e muito mais.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/03/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: a12715ba9aac77461d4968bd9b8f3de30af243c4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79262744"
---
# <a name="azure-active-directory-app-manifest"></a>Manifesto do aplicativo do Azure Active Directory

O manifesto do aplicativo contém uma definição de todos os atributos de um objeto de aplicativo na plataforma de identidade da Microsoft. Ele também serve como um mecanismo para atualizar o objeto do aplicativo. Para obter mais informações sobre a entidade de aplicativo e seu esquema, consulte a [documentação da entidade de aplicativo API do Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity).

Você pode configurar os atributos de um aplicativo por meio da portal do Azure ou programaticamente usando a [API REST](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) ou o [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications). No entanto, existem alguns cenários em que você precisará editar o manifesto do aplicativo para configurar o atributo de um aplicativo. Esses cenários incluem:

* Se você registrou o aplicativo como Azure AD multilocatário e contas pessoais da Microsoft, não será possível alterar as contas da Microsoft com suporte na interface do usuário. Em vez disso, você deve usar o editor de manifesto do aplicativo para alterar o tipo de conta suportado.
* Se você precisa definir permissões e funções que seu aplicativo suporta, você deve modificar o manifesto do aplicativo.

## <a name="configure-the-app-manifest"></a>Configurar o manifesto do aplicativo

Para configurar o manifesto do aplicativo:

1. Vá para o [Portal do Azure](https://portal.azure.com). Procure e selecione o serviço de **Azure Active Directory** .
1. Selecione **Registros do Aplicativo**.
1. Selecione o aplicativo que você deseja configurar.
1. Na página **Visão Geral** do aplicativo, selecione a seção **Manifesto**. Um editor de manifesto baseado na Web é aberto, permitindo que você edite o manifesto no portal. Opcionalmente, você pode selecionar **Download** para editar o manifesto localmente e usar **Upload** para reaplicá-lo ao seu aplicativo.

## <a name="manifest-reference"></a>Referência do manifesto


### <a name="key-value-type-accesstokenacceptedversion-nullable-int32"></a>Chave, tipo de valor: `accessTokenAcceptedVersion`, Int32 anulável 
Especifica a versão do token de acesso esperada pelo recurso. Esse parâmetro altera a versão e o formato do JWT produzido independentemente do ponto de extremidade ou cliente usado para solicitar o token de acesso.

O ponto de extremidade usado, v1.0 ou v2.0, é escolhido pelo cliente e só afeta a versão do id_tokens. Os recursos precisam configurar explicitamente `accesstokenAcceptedVersion` para indicar o formato do token de acesso com suporte.

Os valores possíveis para `accesstokenAcceptedVersion` são 1, 2 ou nulo. Se o valor for NULL, esse parâmetro usa como padrão 1, que corresponde ao ponto de extremidade v 1.0. 

Se `signInAudience` for `AzureADandPersonalMicrosoftAccount`, o valor deverá ser `2`  

Valor de exemplo: `2` 

### <a name="key-value-type-addins-collection"></a>Chave, tipo de valor: `addIns`, coleção 
Define o comportamento personalizado que um serviço de consumo pode usar para chamar um aplicativo em contextos específicos. Por exemplo, os aplicativos que podem renderizar fluxos de arquivos podem definir a propriedade addIns para sua funcionalidade "FileHandler". Esse parâmetro permitirá que serviços como o Office 365 chamem o aplicativo no contexto de um documento no qual o usuário está trabalhando. 

Valor de exemplo: 
<code>{<br>&nbsp;&nbsp;&nbsp;"id":"968A844F-7A47-430C-9163-07AE7C31D407"<br>&nbsp;&nbsp;&nbsp;"type": "FileHandler",<br>&nbsp;&nbsp;&nbsp;"properties": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{"key": "version", "value": "2" }<br>&nbsp;&nbsp;&nbsp;]<br>}</code>

### <a name="key-value-type-allowpublicclient-boolean"></a>Chave, tipo de valor: `allowPublicClient`, booliano 
Especifica o tipo de aplicativo de fallback. O Azure AD infere o tipo de aplicativo do replyUrlsWithType por padrão. Há determinados cenários em que o Azure AD não pode determinar o tipo de aplicativo cliente. Por exemplo, um cenário desse tipo é o fluxo [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) em que a solicitação HTTP ocorre sem um redirecionamento de URL. Nesses casos, o AD do Azure irá interpretar o tipo de aplicativo com base no valor dessa propriedade. Se esse valor for definido como true, o tipo de aplicativo de fallback será definido como cliente público, como um aplicativo instalado em execução em um dispositivo móvel. O valor padrão é false, que significa que o tipo de aplicativo de fallback é cliente confidencial, como o aplicativo Web. 

Valor de exemplo: `false` 

### <a name="key-value-type-availabletoothertenants-boolean"></a>Chave, tipo de valor: `availableToOtherTenants`, booliano 
true se o aplicativo for compartilhado com outros locatários; caso contrário, false. <br><br> Observação: isso está disponível apenas na experiência de **registros de aplicativo (herdada)** . Substituído por `signInAudience` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

### <a name="key-value-type-appid-string"></a>Chave, tipo de valor: `appId`, Cadeia de caracteres 
Especifica o identificador exclusivo do aplicativo que é atribuído a um aplicativo pelo Azure AD. 

Valor de exemplo: `"601790de-b632-4f57-9523-ee7cb6ceba95"` 

### <a name="key-value-type-approles-collection"></a>Chave, tipo de valor: `appRoles`, coleção 
Especifica a coleção de funções que um aplicativo pode declarar. Essas funções podem ser atribuídas a usuários, grupos ou entidades de serviço. Para ver exemplos e mais informações, consulte: [Adicionar funções de aplicativo ao seu aplicativo e recebê-las no token](howto-add-app-roles-in-azure-ad-apps.md) 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  

### <a name="key-value-type-displayname-string"></a>Chave, tipo de valor: `displayName`, Cadeia de caracteres 
O nome de exibição do aplicativo. <br><br> Observação: isso está disponível apenas na experiência de **registros de aplicativo (herdada)** . Substituído por `name` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

Valor de exemplo: `"MyRegisteredApp"` 

### <a name="key-value-type-errorurl-string"></a>Chave, tipo de valor: `errorUrl`, Cadeia de caracteres 
Sem suporte. 

### <a name="key-value-type-groupmembershipclaims-string"></a>Chave, tipo de valor: `groupMembershipClaims`, Cadeia de caracteres 
Configura a declaração `groups` emitida em um usuário ou o token de acesso OAuth 2.0 que o aplicativo espera. Para definir esse atributo, use um dos seguintes valores válidos da cadeia de caracteres:
- `"None"`
- `"SecurityGroup"` (para grupos de segurança e funções do Azure AD)
- `"All"` (isso obterá todos os grupos de segurança, grupos de distribuição e funções de diretório do Azure AD dos quais o usuário conectado é membro. 

Valor de exemplo: `"SecurityGroup"` 

### <a name="key-value-type-homepage-string"></a>Chave, tipo de valor: `homepage`, Cadeia de caracteres 
A URL para a home page do aplicativo. <br><br> Observação: isso está disponível apenas na experiência de **registros de aplicativo (herdada)** . Substituído por `signInUrl` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

Valor de exemplo: `"https://MyRegisteredApp"` 

### <a name="key-value-type-objectid-string"></a>Chave, tipo de valor: `objectId`, Cadeia de caracteres 
O identificador exclusivo do aplicativo no diretório. <br><br> Isso está disponível apenas na experiência de **registros de aplicativo (herdada)** . Substituído por `id` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

Valor de exemplo: `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` 

### <a name="key-value-type-optionalclaims-string"></a>Chave, tipo de valor: `optionalClaims`, Cadeia de caracteres 
As declarações opcionais retornadas no token pelo serviço de token de segurança para este aplicativo específico.<br>No momento, os aplicativos que oferecem suporte a contas pessoais e ao Azure AD (registrados por meio do portal de registro de aplicativos) não podem usar declarações opcionais. No entanto, os aplicativos registrados apenas para o Azure AD usando o ponto de extremidade v2.0 podem obter as declarações opcionais solicitadas no manifesto. Para obter mais informações, confira [declarações opcionais](active-directory-optional-claims.md). 

Valor de exemplo:  
`null` 

### <a name="key-value-type-id-string"></a>Chave, tipo de valor: `id`, Cadeia de caracteres 
O identificador exclusivo do aplicativo no diretório. Essa ID não é o identificador usado para identificar o aplicativo em qualquer transação de protocolo. Ele é usado para referenciar o objeto em consultas de diretório. 

Valor de exemplo: `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` 

### <a name="key-value-type-identifieruris-string-array"></a>Chave, tipo de valor: `identifierUris`, matriz de cadeia de caracteres 
Os URIs definidos pelo usuário que identificam exclusivamente um aplicativo Web em seu locatário do Azure AD ou em um domínio personalizado verificado, quando o aplicativo é multilocatário. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> 

### <a name="key-value-type-informationalurls-string"></a>Chave, tipo de valor: `informationalUrls`, Cadeia de caracteres 
Especifica os links para os termos de serviço e a política de privacidade do aplicativo. Os termos de serviço e a declaração de privacidade são revelados aos usuários por meio da experiência de consentimento do usuário. Para obter mais informações, confira [Como adicionar termos de serviço e política de privacidade para aplicativos do Azure AD registrados](howto-add-terms-of-service-privacy-statement.md). 

Valor de exemplo: 
<code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> 

### <a name="key-value-type-keycredentials-collection"></a>Chave, tipo de valor: `keyCredentials`, coleção 
Contém referências a credenciais, segredos compartilhados com base em cadeia de caracteres e certificados X.509 atribuídos ao aplicativo. ). 

Valor de exemplo: 
<code>[<br>&nbsp;{<br>&nbsp;&These credentials are used when requesting access tokens (when the app is acting as a client rather that as resourcenbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-knownclientapplications-string-array"></a>Chave, tipo de valor: `knownClientApplications`, matriz de cadeia de caracteres 
Usado para agrupamento de consentimento no caso de uma solução que contenha duas partes: um aplicativo cliente e um aplicativo de API Web personalizado. Se você inserir a appID do aplicativo cliente nesse valor, o usuário precisará consentir somente uma vez no aplicativo cliente. O AD do Azure saberá que o consentimento para o cliente significa implicitamente consentir a API da Web. Ele provisionará automaticamente as entidades de serviço para o cliente e a API da Web ao mesmo tempo. O cliente e o aplicativo de API Web precisam ser registrados no mesmo locatário. 

Valor de exemplo: `["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"]` 

### <a name="key-value-type-logourl-string"></a>Chave, tipo de valor: `logoUrl`, Cadeia de caracteres 
Leia apenas o valor que aponta para a URL da CDN para o logotipo que foi carregado no portal. 

Valor de exemplo: `"https://MyRegisteredAppLogo"` 

### <a name="key-value-type-logouturl-string"></a>Chave, tipo de valor: `logoutUrl`, Cadeia de caracteres 
A URL para fazer logoff do aplicativo. 

Valor de exemplo:  
`"https://MyRegisteredAppLogout"` 

### <a name="key-value-type-name-string"></a>Chave, tipo de valor: `name`, Cadeia de caracteres 
O nome de exibição do aplicativo. 

Valor de exemplo: `"MyRegisteredApp"` 

### <a name="key-value-type-oauth2allowimplicitflow-boolean"></a>Chave, tipo de valor: `oauth2AllowImplicitFlow`, booliano 
Especifica se este aplicativo Web pode solicitar tokens de acesso de fluxo implícitos OAuth 2.0. O padrão é false. Esse sinalizador é usado para aplicativos baseados em navegador, como aplicativos de página única do JavaScript. Para saber mais, digite `OAuth 2.0 implicit grant flow` no sumário e consulte os tópicos sobre fluxo implícito. 

Valor de exemplo: `false` 

### <a name="key-value-type-oauth2allowidtokenimplicitflow-boolean"></a>Chave, tipo de valor: `oauth2AllowIdTokenImplicitFlow`, booliano 
Especifica se este aplicativo Web pode solicitar os tokens de ID de fluxo implícitos OAuth 2.0. O padrão é false. Esse sinalizador é usado para aplicativos baseados em navegador, como aplicativos de página única do JavaScript. 

Valor de exemplo: `false` 

### <a name="key-value-type-oauth2permissions-collection"></a>Chave, tipo de valor: `oauth2Permissions`, coleção 
Especifica a coleção de escopos de permissão do OAuth 2.0 que o aplicativo de API Web (recurso) expõe aos aplicativos clientes. Esses escopos de permissões podem ser concedidos a aplicativos clientes durante o consentimento. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>

### <a name="key-value-type-oauth2requiredpostresponse-boolean"></a>Chave, tipo de valor: `oauth2RequiredPostResponse`, booliano 
Especifica se, como parte das solicitações de token OAuth 2.0, o Azure AD permitirá solicitações POST, em vez de solicitações GET. O padrão é false, que especifica que somente as solicitações GET serão permitidas. 

Valor de exemplo: `false` 

### <a name="key-value-type-parentalcontrolsettings-string"></a>Chave, tipo de valor: `parentalControlSettings`, Cadeia de caracteres 

`countriesBlockedForMinors` especifica os países em que o aplicativo está bloqueado para menores.<br>`legalAgeGroupRule` especifica a regra de grupo de faixa etária que se aplica a usuários do aplicativo. Pode ser definido como `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` ou `BlockMinors`.  

Valor de exemplo: 
<code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> 

### <a name="key-value-type-passwordcredentials-collection"></a>Chave, tipo de valor: `passwordCredentials`, coleção 
Confira a descrição da propriedade `keyCredentials`. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> 

### <a name="key-value-type-preauthorizedapplications-collection"></a>Chave, tipo de valor: `preAuthorizedApplications`, coleção 
Lista os aplicativos e as permissões solicitadas para consentimento implícito. Exige que um administrador tenha fornecido o consentimento para o aplicativo. preAuthorizedApplications não exigem que o usuário consinta com as permissões solicitadas. As permissões listadas em preAuthorizedApplications não exigem o consentimento do usuário. No entanto, quaisquer permissões solicitadas adicionais não listadas no preAuthorizedApplications exigem o consentimento do usuário. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-publicclient-boolean"></a>Chave, tipo de valor: `publicClient`, booliano 
Especifica se este aplicativo é um cliente público (como um aplicativo instalado em execução em um dispositivo móvel). 

Essa propriedade está disponível apenas na experiência **registros de aplicativo (herdada)** . Substituído por `allowPublicClient` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

### <a name="key-value-type-publisherdomain-string"></a>Chave, tipo de valor: `publisherDomain`, Cadeia de caracteres 
O domínio do Publicador verificado para o aplicativo. Somente leitura. 

Valor de exemplo: `https://www.contoso.com`

### <a name="key-value-type-replyurls-string-array"></a>Chave, tipo de valor: `replyUrls`, matriz de cadeia de caracteres 
Essa propriedade de vários valores contém a lista de valores redirect_uri registrados que o Azure AD aceitará como destinos quando retornar tokens. <br><br> Essa propriedade está disponível apenas na experiência **registros de aplicativo (herdada)** . Substituído por `replyUrlsWithType` na experiência de [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) . 

### <a name="key-value-type-replyurlswithtype-collection"></a>Chave, tipo de valor: `replyUrlsWithType`, coleção 
Essa propriedade de vários valores contém a lista de valores redirect_uri registrados que o Azure AD aceitará como destinos quando retornar tokens. Cada valor de URI deve conter um valor de tipo de aplicativo associado. Os valores de tipo com suporte são: <ul><li>`Web`</li><li>`InstalledClient`</li></ul><br> Saiba mais sobre [restrições e limitações do replyUrl](https://docs.microsoft.com/azure/active-directory/develop/reply-url). 

Valor de exemplo: 
<code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> 

### <a name="key-value-type-requiredresourceaccess-collection"></a>Chave, tipo de valor: `requiredResourceAccess`, coleção 
Com o consentimento dinâmico, o `requiredResourceAccess` gera a experiência de consentimento do administrador e a experiência de consentimento do usuário para usuários que estão usando o consentimento estático. No entanto, esse parâmetro não orienta a experiência de consentimento do usuário para o caso geral.<br>`resourceAppId` é o identificador exclusivo do recurso ao qual o aplicativo requer acesso. Esse valor deve ser igual à appId declarada no aplicativo do recurso de destino.<br>`resourceAccess` é uma matriz que lista os escopos de permissão do OAuth2.0 e as funções de aplicativo que o aplicativo exige do recurso especificado. Contém os valores `id` e `type` dos recursos especificados. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> 

### <a name="key-value-type-samlmetadataurl-string"></a>Chave, tipo de valor: `samlMetadataUrl`, Cadeia de caracteres 
A URL dos metadados SAML do aplicativo. 

Valor de exemplo: `https://MyRegisteredAppSAMLMetadata` 

### <a name="key-value-type-signinurl-string"></a>Chave, tipo de valor: `signInUrl`, Cadeia de caracteres 

Especifica a URL da home page do aplicativo. 

Valor de exemplo: `https://MyRegisteredApp` 

### <a name="key-value-type-signinaudience-string"></a>Chave, tipo de valor: `signInAudience`, Cadeia de caracteres 
Especifica quais contas da Microsoft são suportadas para o aplicativo atual. Os valores com suporte são:
- **AzureADMyOrg** -usuários com uma conta corporativa ou de estudante da Microsoft no locatário do Azure ad da minha organização (por exemplo, um único locatário)
- **AzureADMultipleOrgs** -usuários com uma conta corporativa ou de estudante da Microsoft no locatário do Azure AD de qualquer organização (por exemplo, multilocatário)
- **AzureADandPersonalMicrosoftAccount** -usuários com um conta Microsoft pessoal ou uma conta corporativa ou de estudante no locatário do Azure AD de qualquer organização 

Valor de exemplo:  
`AzureADandPersonalMicrosoftAccount` 

### <a name="key-value-type-tags-string-array"></a>Chave, tipo de valor: `tags`, matriz de cadeia de caracteres 
Cadeias de caracteres personalizadas que podem ser usadas para categorizar e identificar o aplicativo. 

Valor de exemplo: 
<code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code>

## <a name="common-issues"></a>Problemas comuns

### <a name="manifest-limits"></a>Limites de manifesto

Um manifesto de aplicativo tem vários atributos que são chamados de coleções; por exemplo, approles, keycredentials, knownClientApplications, identifierUris, rediretUris, requiredResourceAccess e oauth2Permissions. No manifesto completo do aplicativo para qualquer aplicativo, o número total de entradas em todas as coleções combinadas tem sido limitado em 1200. Se você especificar anteriormente os URIs de redirecionamento 100 no manifesto do aplicativo, você estará apenas com 1100 entradas restantes para usar em todas as outras coleções combinadas que compõem o manifesto.

> [!NOTE]
> Caso você tente adicionar mais de 1200 entradas no manifesto do aplicativo, você poderá ver um erro **"falha ao atualizar o aplicativo xxxxxx. Detalhes do erro: o tamanho do manifesto excedeu seu limite. Reduza o número de valores e repita a solicitação. "**

### <a name="unsupported-attributes"></a>Atributos sem suporte

O manifesto do aplicativo representa o esquema do modelo de aplicativo subjacente no Azure AD. Conforme o esquema subjacente evolui, o editor de manifesto será atualizado para refletir o novo esquema de tempos em tempos. Como resultado, você pode notar novos atributos aparecendo no manifesto do aplicativo. Em raras ocasiões, você pode notar uma alteração sintática ou semântica nos atributos existentes ou pode encontrar um atributo que existia anteriormente não é mais suportado. Por exemplo, você verá novos atributos na [registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908), que são conhecidos com um nome diferente na experiência de registros de aplicativo (herdada).

| Registros de aplicativo (Herdado)| Registros de aplicativo           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Para obter descrições para esses atributos, consulte a seção de [referência do manifesto](#manifest-reference) .

Ao tentar carregar um manifesto baixado anteriormente, você poderá ver um dos erros a seguir. Esse erro é provável porque o editor de manifesto agora dá suporte a uma versão mais recente do esquema, que não corresponde ao que você está tentando carregar.

* "Falha ao atualizar o aplicativo xxxxxx. Detalhe do erro: identificador de objeto ' indefinido ' inválido. []."
* "Falha ao atualizar o aplicativo xxxxxx. Detalhe do erro: um ou mais valores de propriedade especificados são inválidos. []."
* "Falha ao atualizar o aplicativo xxxxxx. Detalhe do erro: não é permitido definir availableToOtherTenants nesta versão da API para atualização. []."
* "Falha ao atualizar o aplicativo xxxxxx. Detalhe do erro: as atualizações para a propriedade ' replyUrls ' não são permitidas para este aplicativo. Use a propriedade ' replyUrlsWithType ' em seu lugar. []."
* "Falha ao atualizar o aplicativo xxxxxx. Detalhe do erro: um valor sem um nome de tipo foi encontrado e nenhum tipo esperado está disponível. Quando o modelo é especificado, cada valor na carga deve ter um tipo que pode ser especificado na carga, explicitamente pelo chamador ou inferido implicitamente do valor pai. []"

Quando você vir um desses erros, recomendamos as seguintes ações:

1. Edite os atributos individualmente no editor de manifesto em vez de carregar um manifesto baixado anteriormente. Use a tabela de [referência de manifesto](#manifest-reference) para entender a sintaxe e a semântica dos atributos novos e antigos para que você possa editar com êxito os atributos nos quais está interessado. 
1. Se o fluxo de trabalho exigir que você salve os manifestos no repositório de origem para uso posterior, sugerimos a REBASE dos manifestos salvos em seu repositório com aquele que você vê na experiência de **registros de aplicativo** .

## <a name="next-steps"></a>Próximas etapas

* Para obter mais informações sobre a relação entre o aplicativo de um aplicativo e objetos de entidade de serviço, consulte [objetos de aplicativo e entidade de serviço no Azure ad](app-objects-and-service-principals.md).
* Consulte o [Glossário de desenvolvedor da plataforma de identidade da Microsoft](developer-glossary.md) para obter definições de alguns conceitos principais de desenvolvedor da plataforma de identidade da Microsoft.

Use a seção de comentários a seguir para dar sua opinião e nos ajudar a aprimorar e adaptar nosso conteúdo.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
