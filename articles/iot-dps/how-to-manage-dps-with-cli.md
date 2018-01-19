---
title: "Jak zarządzać inicjowania obsługi usługi urządzeń za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 i rozszerzenia IoT | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać urządzeniami świadczenia usług za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 i rozszerzenia IoT"
services: iot-dps
keywords: 
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 674245f1e284e7308474fed0f6c53b350ec1c819
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Jak zarządzać inicjowania obsługi usługi urządzeń za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 i rozszerzenia IoT

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) jest open source cross platform narzędzie wiersza polecenia do zarządzania zasobami platformy Azure, takich jak IoT krawędzi. 2.0 interfejsu wiersza polecenia platformy Azure jest dostępna w systemach Windows, Linux i MacOS. Azure CLI 2.0 umożliwia zarządzanie zasobami, inicjowania obsługi administracyjnej wystąpień usługi urządzenia i połączone w koncentratory fabrycznej Centrum IoT Azure.

Rozszerzenie IoT wzbogaca 2.0 interfejsu wiersza polecenia platformy Azure z funkcjami takimi jak zarządzanie urządzeniami i pełne możliwości IoT krawędzi.

W tym samouczku najpierw wykonaj kroki, aby skonfigurować interfejs wiersza polecenia Azure w wersji 2.0 i rozszerzenia IoT. Następnie zostanie przedstawiony sposób uruchamiania poleceń interfejsu wiersza polecenia do wykonania operacji usługi inicjowania obsługi administracyjnej podstawowe urządzeń. 

## <a name="installation"></a>Instalacja 

### <a name="step-1---install-python"></a>Krok 1 — instalacja języka Python

[Python 2.7 x lub Python 3.x](https://www.python.org/downloads/) jest wymagana.

### <a name="step-2---install-azure-cli-20"></a>Krok 2 - zainstaluj interfejs wiersza polecenia platformy Azure 2.0

Postępuj zgodnie z [instrukcje dotyczące instalacji](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) można skonfigurować 2.0 interfejsu wiersza polecenia platformy Azure w danym środowisku. Co najmniej w wersji 2.0 interfejsu wiersza polecenia platformy Azure musi być 2.0.24 lub nowszej. Użyj `az –version` do sprawdzania poprawności. Ta wersja obsługuje az rozszerzenia poleceń i wprowadza Knack framework polecenia. Prostym sposobem instalowania w systemie Windows jest pobranie i zainstalowanie [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Krok 3 — IoT instalacji rozszerzenia

[Plik readme rozszerzenia IoT](https://github.com/Azure/azure-iot-cli-extension) opisano kilka sposobów, aby zainstalować to rozszerzenie. Najprostszym sposobem jest uruchomienie `az extension add --name azure-cli-iot-ext`. Po zakończeniu instalacji, można użyć `az extension list` do sprawdzania poprawności obecnie zainstalowanych rozszerzeń lub `az extension show --name azure-cli-iot-ext` aby zobaczyć szczegółowe informacje o rozszerzeniu IoT. Aby usunąć rozszerzenia, można użyć `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Urządzenie podstawowe inicjowania obsługi operacji usługi
W przykładzie pokazano jak zalogować się do konta platformy Azure, utworzyć grupy zasobów platformy Azure (kontener, który zawiera powiązane zasoby Azure rozwiązania), tworzenia Centrum IoT, utworzyć inicjowania obsługi administracyjnej służbę na urządzenie, listy istniejące urządzenie inicjowania obsługi administracyjnej usług i Tworzenie połączonego Centrum IoT z poleceń interfejsu wiersza polecenia. 

Wykonaj kroki instalacji opisanych powyżej, aby rozpocząć. Jeśli nie masz konta platformy Azure, ale możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?v=17.39a) dzisiaj. 


### <a name="1-log-in-to-the-azure-account"></a>1. Zaloguj się do konta platformy Azure
  
    az login

![logowanie][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Utwórz grupę zasobów IoTHubBlogDemo w eastus

    az group create -l eastus -n IoTHubBlogDemo

![Tworzenie grupy zasobów][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Utwórz dwa urządzenia świadczenia usług.

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Tworzenie punktu dystrybucji][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Wyświetl listę istniejących urządzeń, inicjowania obsługi administracyjnej usług w tej grupie zasobów

    az iot dps list --resource-group IoTHubBlogDemo

![Lista punktu dystrybucji][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Tworzenie Centrum IoT blogDemoHub w ramach grupy nowo utworzonego zasobu

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Create IoT Hub][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Link jedną istniejącą Centrum IoT na urządzeniu świadczenie usługi

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Link Hub][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Kolejne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Zarejestrowanie urządzenia
> * Start urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

Przejście do dalej samouczkiem, aby dowiedzieć się, jak udostępnić wieloma urządzeniami przez koncentratory równoważeniem obciążenia. 

> [!div class="nextstepaction"]
> [Zainicjować obsługę administracyjną urządzeń między centra IoT równoważeniem obciążenia](./tutorial-provision-multiple-hubs.md)
