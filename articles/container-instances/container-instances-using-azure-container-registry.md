---
title: "Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure"
description: "Wdrażanie wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4205b47dc67920021812c1e573a98de64ad198ec
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure

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

2. Wybierz **repozytoria**, a następnie wybierz pozycję repozytorium, który chcesz wdrożyć, kliknij prawym przyciskiem myszy tag obrazu kontenera chcesz wdrożyć, a następnie wybierz **uruchomione wystąpienie**.

    !["Uruchomione wystąpienie" w rejestrze kontenera platformy Azure w portalu Azure][acr-runinstance-contextmenu]

3. Wprowadź nazwę kontenera i nazwy grupy zasobów. Jeśli chcesz, możesz również zmienić wartości domyślne.

    ![Tworzenie menu dla wystąpień kontenera platformy Azure][acr-create-deeplink]

4. Po zakończeniu wdrożenia, można przejść do kontenera grupy z okienka powiadomienia, aby znaleźć adres IP i inne właściwości.

    ![Widok szczegółów grupy kontener wystąpień kontenera platformy Azure][aci-detailsview]

## <a name="next-steps"></a>Kolejne kroki

Informacje o sposobie tworzenia kontenery, wypychanie ich rejestru Kontener prywatny i wdrożyć je do wystąpień kontenera Azure przez [wykonywania kroków samouczka](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create