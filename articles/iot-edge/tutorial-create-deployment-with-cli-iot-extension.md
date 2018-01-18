---
title: "Wdrażanie modułów do urządzenia brzegowe IoT przy użyciu rozszerzenia IoT Azure CLI 2.0 | Dokumentacja firmy Microsoft"
description: "Wdrażanie modułów na IoT urządzenia przy użyciu rozszerzenia IoT Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Wdrażanie modułów na IoT urządzenia przy użyciu rozszerzenia IoT Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) jest typu open source cross platform narzędzia wiersza polecenia do zarządzania zasobami platformy Azure, takich jak IoT krawędzi. 2.0 interfejsu wiersza polecenia platformy Azure jest dostępna w systemach Windows, Linux i MacOS.

Azure CLI 2.0 umożliwia zarządzanie zasobami, inicjowania obsługi administracyjnej wystąpień usługi urządzenia i połączone w koncentratory fabrycznej Centrum IoT Azure. Nowe rozszerzenie IoT wzbogaca 2.0 interfejsu wiersza polecenia platformy Azure z funkcjami takimi jak zarządzanie urządzeniami i pełne możliwości IoT krawędzi.

W tym samouczku najpierw wykonaj kroki, aby skonfigurować interfejs wiersza polecenia Azure w wersji 2.0 i rozszerzenia IoT. Następnie zostanie przedstawiony sposób wdrażania modułów do urządzenia IoT przy użyciu dostępnych poleceń interfejsu wiersza polecenia.

## <a name="installation"></a>Instalacja 

### <a name="step-1---install-python"></a>Krok 1 — instalacja języka Python

[Python 2.7 x lub Python 3.x](https://www.python.org/downloads/) jest wymagana.

### <a name="step-2---install-azure-cli-20"></a>Krok 2 - zainstaluj interfejs wiersza polecenia platformy Azure 2.0

Postępuj zgodnie z [instrukcje dotyczące instalacji](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) można skonfigurować 2.0 interfejsu wiersza polecenia platformy Azure w danym środowisku. Co najmniej w wersji 2.0 interfejsu wiersza polecenia platformy Azure musi być 2.0.24 lub nowszej. Użyj `az –version` do sprawdzania poprawności. Ta wersja obsługuje az rozszerzenia poleceń i wprowadza Knack framework polecenia. Prostym sposobem instalowania w systemie Windows jest pobranie i zainstalowanie [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Krok 3 — IoT instalacji rozszerzenia

[Plik readme rozszerzenia IoT](https://github.com/Azure/azure-iot-cli-extension) opisano kilka sposobów, aby zainstalować to rozszerzenie. Najprostszym sposobem jest uruchomienie `az extension add --name azure-cli-iot-ext`. Po zakończeniu instalacji, można użyć `az extension list` do sprawdzania poprawności obecnie zainstalowanych rozszerzeń lub `az extension show --name azure-cli-iot-ext` aby zobaczyć szczegółowe informacje o rozszerzeniu IoT. Aby usunąć rozszerzenia, można użyć `az extension remove --name azure-cli-iot-ext`.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Wdrażanie modułów do urządzenia IoT
Z tego samouczka dowiesz się, sposób tworzenia wdrożenia IoT krawędzi. W przykładzie przedstawiono możesz zalogować się do konta platformy Azure, utworzyć grupy zasobów platformy Azure (kontener, który zawiera powiązane zasoby dla rozwiązania Azure), tworzenia Centrum IoT, Utwórz trzy tożsamości urządzenia IoT krawędzi, ustawienie tagów i wdrożenia krawędzi IoT następnie tworzyć, które Celem tych urządzeń. Wykonaj kroki instalacji opisanych powyżej, aby rozpocząć. Jeśli nie masz konta platformy Azure, ale możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/free/?v=17.39a) dzisiaj. 


### <a name="1-login-to-the-azure-account"></a>1. Zaloguj się do konta platformy Azure
  
    az login

![logowanie][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Utwórz grupę zasobów IoTHubBlogDemo w eastus

    az group create -l eastus -n IoTHubBlogDemo

![Tworzenie grupy zasobów][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Tworzenie Centrum IoT blogDemoHub w ramach grupy nowo utworzonego zasobu

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Create IoT Hub][3]


### <a name="4-create-an-iot-edge-device"></a>4. Tworzenie urządzenia IoT

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![Utwórz urządzenie brzegowe IoT][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. Zastosuj konfigurację do urządzenia IoT krawędzi

Zapisz szablon JSON wdrożenia lokalnie jako plik txt. Ścieżka do pliku, należy podczas uruchamiania polecenia Zastosuj konfigurację.

Oto przykładowy szablon JSON wdrożenia, który zawiera jeden moduł tempSensor:

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
              "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
              "createOptions": "{}"
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
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
              "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
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

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Zastosuj konfigurację][5]

### <a name="6-list-modules"></a>6. Lista modułów
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Lista modułów][6]

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku został utworzony funkcji platformy Azure, który zawiera kod, aby filtrować nieprzetworzone dane generowane przez urządzenia IoT krawędzi. Aby zachować Eksplorowanie usługi Azure IoT krawędzi, Dowiedz się, jak używać urządzenia IoT jako brama. 

> [!div class="nextstepaction"]
> [Tworzenie urządzenia bramy IoT](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG

