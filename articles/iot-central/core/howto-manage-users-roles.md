---
title: Gerenciar usuários e funções no aplicativo IoT Central do Azure | Microsoft Docs
description: Como administrador, como gerenciar usuários e funções no aplicativo IoT Central do Azure
author: lmasieri
ms.author: lmasieri
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 8826ec5b8876a3f9e5b613641cc0d759545f04c4
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77018941"
---
# <a name="manage-users-and-roles-in-your-iot-central-application"></a>Gerenciar usuários e funções em seu aplicativo IoT Central



Este artigo descreve como, como administrador, você pode adicionar, editar e excluir usuários em seu aplicativo de IoT Central do Azure. O artigo também descreve como gerenciar funções no aplicativo IoT Central do Azure.

Para acessar e usar a seção **Administração**, você deve estar na função **Administrador** no aplicativo Azure IoT Central. Se você criar um aplicativo de IoT Central do Azure, você será adicionado automaticamente à função de **administrador** para esse aplicativo.

## <a name="add-users"></a>Adicionar usuários

Cada usuário deve ter uma conta de usuário antes de poder entrar e acessar um aplicativo Azure IoT Central. As MSAs (Contas Microsoft) e contas do Azure AD (Azure Active Directory) têm suporte no Azure IoT Central. Atualmente, os grupos do Azure Active Directory não têm suporte no Azure IoT Central.

Para saber mais, confira a [Ajuda da Conta Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) e o [Início Rápido: Adicionar novos usuários ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para adicionar um usuário a um aplicativo IoT Central, acesse a página **Usuários** na seção **Administração**.
    
    > [!div class="mx-imgBorder"]
    >![Gerenciar usuários](media/howto-manage-users-roles/manage-users-pnp.png)

1. Na página **Usuários**, escolha **+ Adicionar usuário** para adicionar um usuário.

1. Escolha uma função para o usuário na lista suspensa **Função**. Saiba mais sobre as funções na seção [Gerenciar funções](#manage-roles) deste artigo.

    > [!div class="mx-imgBorder"]
    >![Adicionar usuário e selecionar uma função](media/howto-manage-users-roles/add-user-pnp.png)

    > [!NOTE]
    > Um usuário que está em uma função personalizada que concede a eles a permissão para adicionar outros usuários só pode adicionar usuários a uma função com as mesmas ou menos permissões do que sua própria função.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Editar as funções atribuídas aos usuários

As funções não podem ser alteradas depois que são atribuídas. Para alterar a função atribuída a um usuário, exclua o usuário e adicione-o novamente com uma função diferente.

> [!NOTE]
> As funções atribuídas são específicas para IoT Central aplicativo e não podem ser gerenciadas no portal do Azure.

## <a name="delete-users"></a>Excluir usuários

Para excluir usuários, marque uma ou mais caixas de seleção na página **Usuários**. Em seguida, selecione **Excluir**.

## <a name="manage-roles"></a>Gerenciar funções

As funções permitem que você controle quem dentro de sua organização tem permissão para realizar várias tarefas no IoT Central. Há três funções internas que você pode atribuir a usuários do seu aplicativo. Você também pode [criar funções personalizadas](#create-a-custom-role) se precisar de um controle mais refinado.

> [!div class="mx-imgBorder"]
> ![a seleção gerenciar funções](media/howto-manage-users-roles/manage-roles-pnp.png)

### <a name="administrator"></a>Administrador

Os usuários na função **administrador** podem gerenciar e controlar todas as partes do aplicativo, incluindo a cobrança.

O usuário que cria um aplicativo é automaticamente designado à função **Administrador**. Sempre deverá haver pelo menos um usuário na função **Administrador**.

### <a name="builder"></a>Construtor

Os usuários na função de **Construtor** podem gerenciar todas as partes do aplicativo, mas não podem fazer alterações nas guias administração ou exportação de dados contínuas.

### <a name="operator"></a>Operador

Os usuários na função **operador** podem monitorar a integridade e o status do dispositivo. Eles não podem fazer alterações nos modelos de dispositivo ou administrar o aplicativo. Os operadores podem adicionar e excluir dispositivos, gerenciar conjuntos de dispositivos e executar análises e trabalhos. 

## <a name="create-a-custom-role"></a>Criar uma função personalizada

Se sua solução exigir controles de acesso mais refinados, você poderá criar funções personalizadas com conjuntos personalizados de permissões. Para criar uma função personalizada, navegue até a página **funções** na seção **Administração** do seu aplicativo. Em seguida, selecione **+ nova função**e adicione um nome e uma descrição para sua função. Selecione as permissões que sua função requer e, em seguida, selecione **salvar**.

Você pode adicionar usuários à sua função personalizada da mesma maneira que adiciona usuários a uma função interna.

> [!div class="mx-imgBorder"]
> ![criar uma função personalizada](media/howto-manage-users-roles/create-custom-role-pnp.png)

### <a name="custom-role-options"></a>Opções de função personalizadas

Ao definir uma função personalizada, você escolhe o conjunto de permissões que um usuário recebe se ele for um membro da função. Algumas permissões dependem de outras. Por exemplo, se você adicionar a permissão **Atualizar painéis do aplicativo** a uma função, a permissão **Exibir painéis do aplicativo** será adicionada automaticamente. As tabelas a seguir resumem as permissões disponíveis e suas dependências, que você pode usar ao criar funções personalizadas.

#### <a name="managing-devices"></a>Gerenciando dispositivos

**Permissões de modelo de dispositivo**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Gerenciamento | Exibir <br/> Outras dependências: exibir instâncias de dispositivo  |
| Controle Total | Exibir, gerenciar <br/> Outras dependências: exibir instâncias de dispositivo |

**Permissões de instância de dispositivo**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos |
| Atualizar | Exibir <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos  |
| Create | Exibir <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos  |
| Excluir | Exibir <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos  |
| Executar comandos | Atualizar, exibir <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos  |
| Controle Total | Exibir, atualizar, criar, excluir, executar comandos <br/> Outras dependências: exibir modelos de dispositivo e grupos de dispositivos  |

**Permissões de grupos de dispositivos**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum <br/> Outras dependências: exibir modelos de dispositivo e instâncias de dispositivo |
| Atualizar | Exibir <br/> Outras dependências: exibir modelos de dispositivo e instâncias de dispositivo   |
| Create | Exibir, atualizar <br/> Outras dependências: exibir modelos de dispositivo e instâncias de dispositivo   |
| Excluir | Exibir <br/> Outras dependências: exibir modelos de dispositivo e instâncias de dispositivo   |
| Controle Total | Exibir, atualizar, criar, excluir <br/> Outras dependências: exibir modelos de dispositivo e instâncias de dispositivo |

**Permissões de gerenciamento de conectividade do dispositivo**

| Nome | Dependências |
| ---- | -------- |
| Ler instância | Nenhum <br/> Outras dependências: exibir modelos de dispositivo, grupos de dispositivos, instâncias de dispositivo |
| Gerenciar instância | Nenhum |
| Ler global | Nenhum   |
| Gerenciar global | Ler global |
| Controle Total | Ler instância, gerenciar instância, ler global, gerenciar global. <br/> Outras dependências: exibir modelos de dispositivo, grupos de dispositivos, instâncias de dispositivo |

**Permissões de trabalhos**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos |
| Atualizar | Exibir <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos |
| Create | Exibir, atualizar <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos |
| Excluir | Exibir <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos |
| Execute (executar) | Exibir <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos; Atualizar instâncias de dispositivo; Executar comandos em instâncias de dispositivo |
| Controle Total | Exibir, atualizar, criar, excluir, executar <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo e grupos de dispositivos; Atualizar instâncias de dispositivo; Executar comandos em instâncias de dispositivo |

**Permissões de regras**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum <br/> Outras dependências: exibir modelos de dispositivo |
| Atualizar | Exibir <br/> Outras dependências: exibir modelos de dispositivo |
| Create | Exibir, atualizar <br/> Outras dependências: exibir modelos de dispositivo |
| Excluir | Exibir <br/> Outras dependências: exibir modelos de dispositivo |
| Controle Total | Exibir, atualizar, criar, excluir <br/> Outras dependências: exibir modelos de dispositivo |

#### <a name="managing-the-app"></a>Gerenciando o aplicativo

**Permissões de configurações do aplicativo**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Copiar | Exibir <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo, grupos de dispositivos, painéis, exportação de dados, identidade visual, links de ajuda, funções personalizadas, regras |
| Excluir | Exibir   |
| Controle Total | Exibir, atualizar, copiar, excluir <br/> Outras dependências: exibir modelos de dispositivo, grupos de dispositivos, painéis de aplicativos, exportação de dados, identidade visual, links de ajuda, funções personalizadas, regras |

**Permissões de exportação de modelo de aplicativo**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Exportar | Exibir <br/> Outras dependências: exibir modelos de dispositivo, instâncias de dispositivo, grupos de dispositivos, painéis, exportação de dados, identidade visual, links de ajuda, funções personalizadas, regras |
| Controle Total | Exibir, exportar <br/> Outras dependências: exibir modelos de dispositivo, grupos de dispositivos, painéis de aplicativos, exportação de dados, identidade visual, links de ajuda, funções personalizadas, regras |

**Permissões de cobrança**

| Nome | Dependências |
| ---- | -------- |
| Gerenciamento | Nenhum     |
| Controle Total | Gerenciamento |

#### <a name="managing-users-and-roles"></a>Gerenciando usuários e funções

**Permissões de funções personalizadas**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum |
| Atualizar | Exibir |
| Create | Exibir, atualizar |
| Excluir | Exibir |
| Controle Total | Exibir, atualizar, criar, excluir |

**Permissões de gerenciamento de usuário**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum <br/> Outras dependências: exibir funções personalizadas |
| Adicionar | Exibir <br/> Outras dependências: exibir funções personalizadas |
| Excluir | Exibir <br/> Outras dependências: exibir funções personalizadas |
| Controle Total | Exibir, adicionar, excluir <br/> Outras dependências: exibir funções personalizadas |

> [!NOTE]
> Um usuário que está em uma função personalizada que concede a eles a permissão para adicionar outros usuários só pode adicionar usuários a uma função com as mesmas ou menos permissões do que sua própria função.

#### <a name="customizing-the-app"></a>Personalizando o aplicativo

**Permissões do painel de aplicativo**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Create | Exibir, atualizar |
| Excluir | Exibir   |
| Controle Total | Exibir, atualizar, criar, excluir |

**Permissões de painéis pessoais**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Create | Exibir, atualizar   |
| Excluir | Exibir   |
| Controle Total | Exibir, atualizar, criar, excluir |

**Permissões de identidade visual, favicon e cores**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Controle Total | Exibir, atualizar |

**Permissões de links de ajuda**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Controle Total | Exibir, atualizar |

#### <a name="extending-the-app"></a>Estendendo o aplicativo

**Permissões de exportação de dados**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Atualizar | Exibir   |
| Create | Exibir, atualizar  |
| Excluir | Exibir   |
| Controle Total | Exibir, atualizar, criar, excluir |

**Permissões de token de API**

| Nome | Dependências |
| ---- | -------- |
| Exibir | Nenhum     |
| Create | Exibir   |
| Excluir | Exibir   |
| Controle Total | Exibir, criar, excluir |

## <a name="next-steps"></a>Próximos passos

Agora que você aprendeu sobre como gerenciar usuários e funções em seu aplicativo IoT Central do Azure, a próxima etapa sugerida é aprender a [gerenciar sua fatura](howto-view-bill.md).
