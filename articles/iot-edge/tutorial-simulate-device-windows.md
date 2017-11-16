---
title: Symulowanie Azure IoT Edge w systemie Windows | Dokumentacja firmy Microsoft
description: "Instalowanie środowiska uruchomieniowego Azure IoT Edge na symulowane urządzenie w systemie Windows i wdrażanie pierwszego modułu"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 08c501b9132bb21f47f099725d1fad5556befb4c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Wdrażanie usługi Azure IoT krawędzi na symulowane urządzenie w systemie Windows — podglądu

Usługa Azure IoT krawędzi przenosi mocy chmury urządzeń Internetu rzeczy (IoT). Ten samouczek przeprowadzi Cię przez proces tworzenia symulowane urządzenie brzegowe IoT, które generuje dane czujników. Omawiane kwestie:

![Architektura samouczka][2]

Symulowane urządzenie, które możesz utworzyć w tym samouczku jest monitorem na turbiny knie generujący temperatury, wilgotności i wykorzystania danych. Interesują Cię te dane, ponieważ Twoje turbin są wykonywane na różne poziomy wydajności w zależności od pogodą. Innych samouczków usługi Azure IoT krawędzi zależą od pracy, w tym miejscu poprzez wdrożenie modułów, które analiz biznesowych. 

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Windows do symulacji urządzenia Internetu rzeczy. 

>[!TIP]
>Jeśli korzystasz z systemu Windows na maszynie wirtualnej, należy włączyć [zagnieżdżonych wirtualizacji] [ lnk-nested] i przydziel co najmniej 2 GB pamięci RAM. 

1. Upewnij się, że używasz wersji systemu Windows:
   * Windows 10 
   * Windows Server
2. Zainstaluj [Docker dla systemu Windows] [ lnk-docker] i upewnij się, że jest uruchomiona.
3. Zainstaluj [Python 2.7 w systemie Windows] [ lnk-python] i upewnij się, możesz użyć polecenia pip.
4. Uruchom następujące polecenie, aby pobrać skryptu kontroli IoT krawędzi.

   ```
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Kontenery systemu Windows lub Linux kontenerów, można uruchomić Azure IoT krawędzi. Aby użyć kontenery systemu Windows, należy uruchomić:
>    * Windows 10 twórców spadku zaktualizować, lub
>    * Windows Server (kompilacja 16299) 1709, lub
>    * Windows IoT Core (16299 kompilacji) na urządzenia z systemem x64
>
> Dla systemu Windows IoT Core, postępuj zgodnie z instrukcjami [zainstaluj środowisko uruchomieniowe IoT Edge w systemie Windows IoT Core][lnk-install-iotcore]. W przeciwnym razie po prostu [skonfigurować Docker na używanie kontenerów Windows][lnk-docker-containers]i opcjonalnie zweryfikować Twoje wymagania wstępne przy użyciu następującego polecenia programu powershell:
>    ```
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT

Uruchom samouczek tworzenia Centrum IoT.
![Tworzenie Centrum IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Zarejestruj urządzenia IoT

Zarejestruj urządzenia IoT z nowo utworzonego Centrum IoT.
![Zarejestruj urządzenie][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Konfigurowanie środowiska uruchomieniowego krawędzi IoT

Zainstaluj i uruchom środowiska uruchomieniowego Azure IoT Edge na urządzeniu. 
![Zarejestruj urządzenie][5]

Środowisko uruchomieniowe krawędzi IoT jest wdrażana na wszystkich urządzeniach IoT krawędzi. Zawiera dwa moduły. Najpierw agenta krawędzi IoT ułatwia wdrażanie i monitorowanie modułów na urządzeniu IoT krawędzi. Po drugie Centrum IoT krawędzi zarządza komunikacji między modułami na urządzeniu IoT krawędzi i między urządzeniami a Centrum IoT. 


Aby zainstalować i uruchomić środowiska uruchomieniowego krawędzi IoT, wykonaj następujące kroki:

1. Konfigurowanie środowiska uruchomieniowego z parametrów połączenia urządzenia IoT krawędzi z poprzedniej sekcji.

   ```
   iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Uruchom środowisko uruchomieniowe.

   ```
   iotedgectl start
   ```

1. Sprawdź Docker, aby zobaczyć, czy agent krawędzi IoT jest uruchomiony jako moduł.

   ```
   docker ps
   ```

## <a name="deploy-a-module"></a>Wdrażanie modułu

Zarządzanie urządzeniem krawędzi IoT Azure w chmurze, aby wdrożyć moduł, który będzie wysyłać dane telemetryczne z Centrum IoT.
![Zarejestruj urządzenie][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Widok wygenerowany danych

W tego przewodnika Szybki Start utworzyć nowe urządzenie brzegowe IoT, a na nim zainstalowany środowiska uruchomieniowego IoT krawędzi. Następnie użyto portalu Azure do umieszczenia krawędzi IoT modułu do uruchomienia na urządzeniu bez konieczności zmiany na urządzeniu. W takim przypadku moduł, który zostanie przypisany tworzy dane środowiska, używanego programu samouczków. 

Wyświetl komunikaty wysyłane z modułu tempSensor:

```cmd/sh
sudo docker logs -f tempSensor
```

Można również wyświetlić dane telemetryczne, wysyła urządzenia przy użyciu [narzędzia explorer Centrum IoT][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku został utworzony nowe urządzenie brzegowe IoT i interfejs chmury Azure IoT krawędzi można wdrożyć kod na urządzeniu. Pojawiły się symulowane urządzenie generowania nieprzetworzone dane o jego środowisku. 

W tym samouczku jest wstępnym dla wszystkich innych samouczków IoT krawędzi. Można kontynuować do wszystkich innych samouczków, aby dowiedzieć się, jak Azure IoT krawędzi można przekształcić danych biznesowych na krawędzi.

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie kodu C# jako moduł](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md