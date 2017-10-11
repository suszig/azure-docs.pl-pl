---
title: "Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure | Dokumentacja platformy Azure"
description: "Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: aa1c4ea379c10dff246e2f924a345f9fa444aa64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Wdrażanie do wystąpień kontenera platformy Azure z rejestru kontenera platformy Azure

Rejestr kontenera Azure jest rejestru Azure, prywatnego obrazów kontenera Docker. W tym artykule opisano sposób wdrażania obrazów kontenera przechowywane w rejestrze kontenera Azure do wystąpień kontenera platformy Azure.

## <a name="using-the-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure

Interfejsu wiersza polecenia Azure zawiera polecenia do tworzenia i zarządzania kontenerami w wystąpień kontenera platformy Azure. Jeśli określisz prywatnej obrazu w `create` polecenia, można również określić hasło rejestru obrazu, które są wymagane do uwierzytelniania za pomocą rejestru kontenera.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

`create` Polecenia obsługuje również określenie `registry-login-server` i `registry-username`. Jednak serwery logowania rejestru kontenera platformy Azure są zawsze *registryname*. azurecr.io i domyślna nazwa użytkownika jest *registryname*, więc te wartości są wywnioskować na podstawie nazwy obrazu, jeśli nie udostępniony.

## <a name="using-an-azure-resource-manager-template"></a>Przy użyciu szablonu usługi Azure Resource Manager

Można określić właściwości rejestru kontenera platformy Azure w szablonie usługi Azure Resource Manager przez dołączenie `imageRegistryCredentials` właściwości w definicji kontenera grupy:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Aby uniknąć przechowywania hasła rejestru kontenera bezpośrednio w szablonie, firma Microsoft zaleca, zapisz go jako klucza tajnego w [usługi Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) i odwołaj się za pomocą szablonu [natywna integracja pomiędzy usługą platformy Azure Resource Manager i magazynu kluczy](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-the-azure-portal"></a>Korzystanie z witryny Azure Portal

Jeśli obsługa obrazów kontenera w rejestrze kontenera platformy Azure, można łatwo utworzyć kontener w wystąpień kontenera Azure za pomocą portalu Azure.

1. W portalu Azure przejdź do kontenera rejestru.

2. Wybierz repozytoriów.

    ![Menu rejestru kontenera platformy Azure w portalu Azure][acr-menu]

3. Wybierz przewidzianą do wdrożenia z repozytorium.

4. Kliknij prawym przyciskiem myszy tag obrazu kontenera, który chcesz wdrożyć.

    ![Menu kontekstowe dla uruchamiania kontener z wystąpień kontenera platformy Azure][acr-runinstance-contextmenu]

5. Wprowadź nazwę kontenera i nazwy grupy zasobów. Jeśli chcesz, możesz również zmienić wartości domyślne.

    ![Tworzenie menu dla wystąpień kontenera platformy Azure][acr-create-deeplink]

6. Po zakończeniu wdrożenia, można przejść do kontenera grupy z okienka powiadomienia, aby znaleźć adres IP i inne właściwości.

    ![Widok szczegółów grupy kontener wystąpień kontenera platformy Azure][aci-detailsview]

## <a name="next-steps"></a>Następne kroki

Informacje o sposobie tworzenia kontenery, wypychanie ich rejestru Kontener prywatny i wdrożyć je do wystąpień kontenera Azure przez [wykonywania kroków samouczka](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
