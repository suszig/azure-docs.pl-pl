---
title: "Wdrażanie modułów do urządzenia brzegowe IoT przy użyciu rozszerzenia IoT Azure CLI 2.0 | Dokumentacja firmy Microsoft"
description: "Wdrażanie modułów na IoT urządzenia przy użyciu rozszerzenia IoT Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 03/02/2018
ms.topic: article
ms.service: iot-edge
ms.custom: mvc
ms.reviewer: kgremban
ms.openlocfilehash: 1986c9881c09ffa480103e009dc42d18aad4e2aa
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Wdrażanie modułów na IoT urządzenia przy użyciu rozszerzenia IoT Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) jest open source cross platform narzędzie wiersza polecenia do zarządzania zasobami platformy Azure, takich jak IoT krawędzi. 2.0 interfejsu wiersza polecenia platformy Azure jest dostępna w systemach Windows, Linux i MacOS.

Azure CLI 2.0 umożliwia zarządzanie zasobami, inicjowania obsługi administracyjnej wystąpień usługi urządzenia i połączone w koncentratory fabrycznej Centrum IoT Azure. Nowe rozszerzenie IoT wzbogaca 2.0 interfejsu wiersza polecenia platformy Azure z funkcjami takimi jak zarządzanie urządzeniami i pełne możliwości IoT krawędzi.

W tym samouczku najpierw wykonaj kroki, aby skonfigurować interfejs wiersza polecenia Azure w wersji 2.0 i rozszerzenia IoT. Następnie zostanie przedstawiony sposób wdrażania modułów do urządzenia IoT przy użyciu dostępnych poleceń interfejsu wiersza polecenia.

## <a name="prerequisites"></a>Wymagania wstępne

* Konto platformy Azure. Jeśli nie masz jeszcze, możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?v=17.39a) dzisiaj. 

* [Python 2.7 x lub Python 3.x](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) w danym środowisku. Co najmniej w wersji 2.0 interfejsu wiersza polecenia platformy Azure musi być 2.0.24 lub nowszej. Użyj `az –-version` do sprawdzania poprawności. Ta wersja obsługuje az rozszerzenia poleceń i wprowadza Knack framework polecenia. Prostym sposobem instalowania w systemie Windows jest pobranie i zainstalowanie [MSI](https://aka.ms/InstallAzureCliWindows).

* [Rozszerzenie IoT Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension):
   1. Uruchom polecenie `az extension add --name azure-cli-iot-ext`. 
   2. Po zakończeniu instalacji, użyj `az extension list` do sprawdzania poprawności obecnie zainstalowanych rozszerzeń lub `az extension show --name azure-cli-iot-ext` aby zobaczyć szczegółowe informacje o rozszerzeniu IoT.
   3. Aby usunąć rozszerzenie, należy użyć `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Tworzenie urządzenia IoT
Ten artykuł zawiera instrukcje dotyczące tworzenia wdrożenia IoT krawędzi. W przykładzie pokazano sposobu Zaloguj się do konta platformy Azure, utworzyć grupy zasobów platformy Azure (kontener, który zawiera powiązane zasoby Azure rozwiązania), tworzenia Centrum IoT, utworzyć trzy tożsamości urządzenia IoT krawędzi, znaczników i wdrożenia krawędzi IoT następnie tworzyć, które Celem tych urządzeń. 

Zaloguj się do konta platformy Azure. Po wprowadzeniu poniższego polecenia logowania zostanie wyświetlony monit o zalogowanie się przy użyciu jednorazowego kodu przy użyciu przeglądarki sieci web: 

   ```cli
   az login
   ```

Utwórz nową grupę zasobów o nazwie **IoTHubCLI** w regionie wschodnie stany USA: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Tworzenie grupy zasobów][2]

Tworzenie Centrum IoT o nazwie **CLIDemoHub** w grupie zasobów nowo utworzone:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Każda subskrypcja jest przydzielony jeden bezpłatne Centrum IoT. Aby utworzyć bezpłatne Centrum przy użyciu polecenia interfejsu wiersza polecenia, zastąp wartość jednostki SKU z `--sku F1`. Jeśli masz już bezpłatne Centrum w ramach subskrypcji, zostanie wyświetlony komunikat o błędzie podczas próby utworzenia drugiego. 

Utwórz urządzenia IoT:

   ```cli
   az iot hub device-identity create --device-id edge001 -hub-name CLIDemoHub --edge-enabled
   ```

   ![Utwórz urządzenie brzegowe IoT][4]

## <a name="configure-the-iot-edge-device"></a>Skonfiguruj urządzenie brzegowe IoT

Tworzenie szablonu JSON wdrożenia i zapisz go w lokalnie jako plik txt. Ścieżka do pliku, należy podczas uruchamiania polecenia Zastosuj konfigurację.

Szablony JSON wdrożenia należy zawsze należy uwzględniać moduły dwóch systemów, edgeAgent i edgeHub. Oprócz tych dwóch ten plik służy do wdrażania dodatkowych modułów na urządzeniu IoT krawędzi. Aby skonfigurować urządzenia IoT z jednego modułu tempSensor, skorzystaj z poniższego przykładu:

   ```json
   {
     "moduleContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": ""
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "microsoft/azureiotedge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-hub:1.0-preview",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {},
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```

Zastosuj konfigurację do urządzenia IoT krawędzi:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

Wyświetl moduły na urządzeniu IoT krawędzi:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Lista modułów][6]

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się, jak [użyć urządzenia IoT jako bramy](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

