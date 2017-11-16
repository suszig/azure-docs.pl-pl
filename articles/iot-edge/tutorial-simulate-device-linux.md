---
title: Symulowanie Azure IoT Edge w systemie Linux | Dokumentacja firmy Microsoft
description: "Instalowanie środowiska uruchomieniowego Azure IoT Edge na symulowane urządzenie w systemie Linux i wdrażanie pierwszego modułu"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>Wdrażanie usługi Azure IoT krawędzi na symulowane urządzenie w systemie Linux — podglądu

Krawędź IoT Azure umożliwia przeprowadzanie analizy i przetwarzania danych na urządzeniach, zamiast do dystrybuowania wszystkich danych w chmurze. Samouczki krawędzi IoT przedstawiają sposób wdrażania różnego rodzaju moduły skompilowane z usług Azure lub kodu niestandardowego, ale najpierw należy urządzenia do testowania. 

Ten samouczek przedstawia tworzenie symulowane urządzenie brzegowe IoT, następnie wdrażania moduł, który generuje dane czujników. Omawiane kwestie:

![Architektura samouczka][2]

Symulowane urządzenie, które możesz utworzyć w tym samouczku jest monitor, który generuje temperatury, wilgotności i wykorzystania danych. Innych samouczków usługi Azure IoT krawędzi zależą od pracy, w tym miejscu poprzez wdrożenie modułów, które analiz biznesowych. 

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Linux do symulacji urządzenia Internetu rzeczy. Następujące usługi są wymagane do pomyślnego wdrożenia urządzenia IoT:

- [Zainstaluj dla systemu Linux Docker] [ lnk-docker-ubuntu] i upewnij się, że jest uruchomiona. 
- Większość dystrybucje systemu Linux, łącznie z Ubuntu, już Python 2.7 zainstalowane. Użyj następującego polecenia, aby upewnić się, że zainstalowano narzędzia pip: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT

Uruchom samouczek tworzenia Centrum IoT.
![Tworzenie Centrum IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Zarejestruj urządzenia IoT

Zarejestruj urządzenia IoT z nowo utworzonego Centrum IoT.
![Zarejestruj urządzenie][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Zainstaluj i uruchom środowisko uruchomieniowe krawędzi IoT

Zainstaluj i uruchom środowiska uruchomieniowego Azure IoT Edge na urządzeniu. 
![Zarejestruj urządzenie][5]

Środowisko uruchomieniowe krawędzi IoT jest wdrażana na wszystkich urządzeniach IoT krawędzi. Zawiera dwa moduły. Najpierw agenta krawędzi IoT ułatwia wdrażanie i monitorowanie modułów na urządzeniu IoT krawędzi. Po drugie Centrum IoT krawędzi zarządza komunikacji między modułami na urządzeniu IoT krawędzi i między urządzeniami a Centrum IoT. 

Aby zainstalować i uruchomić środowiska uruchomieniowego krawędzi IoT, wykonaj następujące kroki:

1. Na komputerze, na którym będzie uruchamiane urządzenie brzegowe IoT Pobierz skrypt kontroli IoT krawędzi.

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. Konfigurowanie środowiska uruchomieniowego z parametrów połączenia urządzenia IoT krawędzi z poprzedniej sekcji.

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Uruchom środowisko uruchomieniowe.

   ```
   sudo iotedgectl start
   ```

1. Sprawdź Docker, aby zobaczyć, czy agent krawędzi IoT jest uruchomiony jako moduł.

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>Wdrażanie modułu

Zarządzanie urządzeniem krawędzi IoT Azure w chmurze, aby wdrożyć moduł, który będzie wysyłać dane telemetryczne z Centrum IoT.
![Zarejestruj urządzenie][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Widok wygenerowany danych

W tego przewodnika Szybki Start utworzyć nowe urządzenie brzegowe IoT, a na nim zainstalowany środowiska uruchomieniowego IoT krawędzi. Następnie użyto portalu Azure do umieszczenia krawędzi IoT modułu do uruchomienia na urządzeniu bez konieczności zmiany na urządzeniu. W takim przypadku moduł, który zostanie przypisany tworzy dane środowiska, używanego programu samouczków. 

Wyświetl komunikaty wysyłane z modułu tempSensor:

```cmd/sh
docker logs -f tempSensor
```

Można również wyświetlić dane telemetryczne, wysyła urządzenia przy użyciu [narzędzia explorer Centrum IoT][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku został utworzony nowe urządzenie brzegowe IoT i interfejs chmury Azure IoT krawędzi można wdrożyć kod na urządzeniu. Pojawiły się symulowane urządzenie generowania nieprzetworzone dane o jego środowisku. 

W tym samouczku jest wstępnym dla wszystkich innych samouczków IoT krawędzi. Można kontynuować do wszystkich innych samouczków, aby dowiedzieć się, jak Azure IoT krawędzi można przekształcić danych biznesowych na krawędzi.

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie kodu C# jako moduł](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
