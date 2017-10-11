---
title: "Tworzenie Centrum IoT przy użyciu wiersza polecenia platformy Azure (az.py) | Dokumentacja firmy Microsoft"
description: "Jak utworzyć Centrum Azure IoT przy użyciu interfejsu wiersza polecenia Azure i platform w 2.0 (az.py)."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 161089159999a4a63a39b059e69a08b7a9297445
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>Tworzenie Centrum IoT przy użyciu 2.0 interfejsu wiersza polecenia platformy Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Wprowadzenie

Azure CLI 2.0 (az.py) umożliwia tworzenie i zarządzanie nimi centra Azure IoT programowo. W tym artykule przedstawiono sposób umożliwiają utworzenie Centrum IoT Azure CLI 2.0 (az.py).

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) — interfejsu wiersza polecenia dla modeli wdrażania zarządzania classic i zasobów.
* Azure CLI 2.0 (az.py) - generacji interfejsu wiersza polecenia do zarządzania model wdrażania zasobów zgodnie z opisem w tym artykule.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut.
* [Azure CLI 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Zaloguj się i ustawić konta platformy Azure

Zaloguj się do konta platformy Azure i wyboru subskrypcji.

1. W wierszu polecenia Uruchom [polecenia logowania][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Postępuj zgodnie z instrukcjami w celu uwierzytelnienia przy użyciu kodu i zaloguj się do konta platformy Azure za pośrednictwem przeglądarki sieci web.

2. Jeśli masz wiele subskrypcji Azure, logowanie do platformy Azure przydziela dostęp do wszystkich kont platformy Azure skojarzone z poświadczeniami użytkownika. Należy użyć następującego [polecenia do listy kont Azure] [ lnk-az-account-command] dostępne do użycia:
    
    ```azurecli
    az account list 
    ```

    Użyj następującego polecenia, aby wybrać subskrypcję, która ma być używany do uruchamiania poleceń, aby utworzyć Centrum IoT. Przy użyciu subskrypcji nazwa lub identyfikator z danych wyjściowych poprzednie polecenie:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Tworzenie Centrum IoT

Użyj wiersza polecenia platformy Azure, Utwórz grupę zasobów, a następnie dodaj Centrum IoT.

1. Podczas tworzenia Centrum IoT należy utworzyć ją w grupie zasobów. Użyj istniejącej grupy zasobów, albo uruchom następujące polecenie [polecenie, aby utworzyć grupę zasobów][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Poprzedni przykład tworzy grupy zasobów w lokalizacji zachodnie stany USA. Można wyświetlić listę dostępnych lokalizacji, uruchamiając polecenie `az account list-locations -o table`.
    >
    >

2. Uruchom następujące polecenie [polecenie, aby utworzyć Centrum IoT] [ lnk-az-iot-command] w grupie zasobów, przy użyciu globalnie unikatowej nazwy Centrum IoT:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> Poprzednie polecenie powoduje utworzenie Centrum IoT w S1, dla której są rozliczane warstwy cenowej. Aby uzyskać więcej informacji, zobacz [cennik Centrum IoT Azure][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Usuń Centrum IoT

Korzystając z wiersza polecenia platformy Azure do [usunąć pojedynczego zasobu][lnk-az-resource-command], na przykład Centrum IoT lub Usuń grupę zasobów i wszystkie jego zasoby, w tym wszystkie centra IoT.

Aby usunąć Centrum IoT, uruchom następujące polecenie:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Aby usunąć grupę zasobów i wszystkie jego zasoby, uruchom następujące polecenie:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o tworzeniu aplikacji Centrum IoT, zobacz następujące artykuły:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przy użyciu portalu Azure do zarządzania Centrum IoT][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
