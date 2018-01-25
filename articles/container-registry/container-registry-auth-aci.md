---
title: "Uwierzytelniania za pomocą rejestru kontenera platformy Azure z wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak zapewnić dostęp do obrazów w rejestrze Kontener prywatny z wystąpień kontenera platformy Azure przy użyciu nazwy głównej usługi Azure Active Directory."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 00d9632a5d0c42eceee1b412f8963bbadbea651f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Uwierzytelniania za pomocą rejestru kontenera platformy Azure z wystąpień kontenera platformy Azure

Nazwy głównej usługi Azure Active Directory (Azure AD) można użyć w celu zapewnienia dostępu do sieci prywatnej kontenera rejestrów w rejestrze kontenera Azure.

W tym artykule dowiesz się utworzyć i skonfigurować nazwę główną usługi Azure AD z *ściągania* uprawnienia do rejestru. Następnie należy uruchomić kontener w Azure kontener wystąpień (ACI) ściągający jego obrazu z rejestru prywatne, do uwierzytelniania przy użyciu nazwy głównej usługi.

## <a name="when-to-use-a-service-principal"></a>Kiedy należy używać nazwy głównej usługi

Należy użyć nazwy głównej usługi uwierzytelniania z ACI w **bezobsługowe scenariusze**, takich jak aplikacje lub usługi, które tworzenia wystąpień kontenera w trybie nienadzorowanym automatycznych lub w inny sposób.

Na przykład, jeśli masz zautomatyzowanego skryptu, co noc, co działającą i tworzy [wystąpienia opartego na zadaniach kontenera](../container-instances/container-instances-restart-policy.md) przetwarzania niektórych danych, można użyć nazwy głównej usługi tylko ściąganych (czytnik) uprawnienia do uwierzytelniania w rejestrze. Można obrócić poświadczenia nazwy głównej usługi lub odwołać dostęp do niej całkowicie bez wpływu na inne usługi i aplikacje.

Nazwy główne usług powinna być również używane podczas rejestru [administrator](container-registry-authentication.md#admin-account) jest wyłączona.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Uwierzytelnianie przy użyciu nazwy głównej usługi

Aby uruchomić kontener wystąpień kontenera platformy Azure przy użyciu nazwy głównej usługi, należy określić jego identyfikator `--registry-username`, wraz z hasłem dla `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="next-steps"></a>Kolejne kroki

Poniższe artykuły zawierają dodatkowe szczegółowe informacje na temat pracy z nazw głównych usług i ACR:

* [Azure rejestru kontenera uwierzytelniania za pomocą nazwy główne usług](container-registry-auth-service-principal.md)
* [Uwierzytelniania za pomocą rejestru kontenera platformy Azure z usługą kontenera Azure (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->
