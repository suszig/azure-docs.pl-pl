---
title: "Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure"
description: "Dowiedz się, jak wdrażanie kontenerów w wystąpień kontenera platformy Azure przy użyciu obrazów kontenera w rejestrze kontenera platformy Azure."
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/24/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c69b95f66bf2eaf4975961da5b25f5ac6172798c
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure

Rejestr kontenera Azure jest rejestru Azure, prywatnego obrazów kontenera Docker. W tym artykule opisano sposób wdrażania obrazów kontenera przechowywane w rejestrze kontenera Azure do wystąpień kontenera platformy Azure.

## <a name="deploy-with-azure-cli"></a>Wdrażanie przy użyciu interfejsu wiersza polecenia platformy Azure

Interfejsu wiersza polecenia Azure zawiera polecenia do tworzenia i zarządzania kontenerami w wystąpień kontenera platformy Azure. Jeśli określisz prywatnej obrazu w [utworzyć kontener az] [ az-container-create] polecenia, można również określić hasło rejestru obrazu, które są wymagane do uwierzytelniania za pomocą rejestru kontenera.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

[Utworzyć kontener az] [ az-container-create] polecenia obsługuje również określenie `--registry-login-server` i `--registry-username`. Jednak serwery logowania rejestru kontenera platformy Azure są zawsze *registryname*. azurecr.io i domyślna nazwa użytkownika jest *registryname*, więc te wartości są wywnioskować na podstawie nazwy obrazu, jeśli nie udostępniony.

## <a name="deploy-with-azure-resource-manager-template"></a>Rozmieszczanie za pomocą szablonu usługi Azure Resource Manager

Można określić właściwości rejestru kontenera platformy Azure w szablonie usługi Azure Resource Manager przez dołączenie `imageRegistryCredentials` właściwości w definicji kontenera grupy:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Aby uniknąć przechowywania hasła rejestru kontenera bezpośrednio w szablonie, firma Microsoft zaleca, zapisz go jako klucza tajnego w [usługi Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) i odwołaj się za pomocą szablonu [natywna integracja pomiędzy usługą platformy Azure Resource Manager i magazynu kluczy](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Rozmieszczanie za pomocą portalu Azure

Jeśli obsługa obrazów kontenera w rejestrze kontenera platformy Azure, można łatwo utworzyć kontener w wystąpień kontenera Azure za pomocą portalu Azure.

1. W portalu Azure przejdź do kontenera rejestru.

1. Wybierz **repozytoria**, a następnie wybierz pozycję repozytorium, który chcesz wdrożyć, kliknij prawym przyciskiem myszy tag obrazu kontenera chcesz wdrożyć, a następnie wybierz **uruchomione wystąpienie**.

    !["Uruchomione wystąpienie" w rejestrze kontenera platformy Azure w portalu Azure][acr-runinstance-contextmenu]

1. Wprowadź nazwę kontenera i nazwy grupy zasobów. Jeśli chcesz, możesz również zmienić wartości domyślne.

    ![Tworzenie menu dla wystąpień kontenera platformy Azure][acr-create-deeplink]

1. Po zakończeniu wdrożenia, można przejść do kontenera grupy z okienka powiadomienia, aby znaleźć adres IP i inne właściwości.

    ![Widok szczegółów grupy kontener wystąpień kontenera platformy Azure][aci-detailsview]

## <a name="service-principal-authentication"></a>Uwierzytelnianie jednostki usługi

Jeśli dla użytkownika administracyjnego rejestru kontenera platformy Azure jest wyłączone, możesz użyć usługi Azure Active Directory [nazwy głównej usługi](../container-registry/container-registry-auth-service-principal.md) do uwierzytelniania w rejestrze, podczas tworzenia wystąpienia obiektu kontenera. Do uwierzytelniania przy użyciu nazwy głównej usługi jest również zalecane w scenariuszach bezobsługowe, takich jak skryptu lub aplikacji do tworzenia wystąpień kontenera w trybie nienadzorowanym.

Aby uzyskać więcej informacji, zobacz [Uwierzytelnij rejestrem kontenera platformy Azure z wystąpień kontenera Azure](../container-registry/container-registry-auth-aci.md).

## <a name="next-steps"></a>Kolejne kroki

Informacje o sposobie tworzenia kontenery, wypychanie ich rejestru Kontener prywatny i wdrożyć je do wystąpień kontenera Azure przez [wykonywania kroków samouczka](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create