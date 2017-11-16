---
title: "Wdrażanie usługi Azure Machine Learning model na urządzenia Azure IoT | Dokumentacja firmy Microsoft"
description: "W tym dokumencie opisano, jak modeli uczenia maszynowego Azure mogą być wdrażane urządzenia Azure IoT krawędzi."
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: garyericson, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/10/2017
ms.openlocfilehash: 924766aee7486f0cf5006dd89d5d77b83753833b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Wdrażanie i usługi Azure Machine Learning model do urządzenia IoT Azure

Wszystkie modele uczenia maszynowego Azure konteneryzowanych jako usługi sieci web opartych na Docker można również uruchomić na urządzenia Azure IoT krawędzi. Dodatkowe skrypty oraz instrukcje można znaleźć w [Toolkit AI Azure IoT Edge](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Operacjonalizuj modelu
Operacjonalizować model zgodnie z instrukcjami w [wdrażanie usługi sieci Web zarządzania modelu Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy) do utworzenia obrazu Docker z modelu.

## <a name="deploy-to-azure-iot-edge"></a>Wdrażanie na krawędzi IoT Azure
Azure IoT krawędzi przenosi analizy chmury i niestandardowe reguły biznesowe na urządzeniach. Wszystkie modele uczenia maszynowego można uruchamiać na urządzenia brzegowe IoT. Dokumentacja urządzenia IoT i utworzyć wdrożenie, znajduje się w temacie [aka.ms/azure-iot — krawędzi doc](https://aka.ms/azure-iot-edge-doc).

Poniżej przedstawiono dodatkowe rzeczy do uwzględnienia.

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Dodawanie poświadczeń rejestru do środowiska wykonawczego Edge na urządzeniu krawędzi
Na komputerze, na którym uruchomiane krawędzi IoT należy dodać poświadczenia rejestru, aby środowiska uruchomieniowego mogą mieć dostęp do ściągnięcia kontenera.

W systemie Windows uruchom następujące polecenie:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Dla systemu Linux uruchom następujące polecenie:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>Znajdź lokalizację obrazu kontenera uczenia maszynowego
Należy lokalizacji obrazu kontenera uczenia maszynowego. Aby znaleźć lokalizację kontenera obrazu:

1. Zaloguj się do [portalu Azure](http://portal.azure.com/).
2. W **rejestru kontenera Azure**, wybierz rejestru chcesz sprawdzić.
3. W rejestrze, kliknij przycisk **repozytoria** umożliwia wyświetlenie listy wszystkich repozytoria i obrazów.













