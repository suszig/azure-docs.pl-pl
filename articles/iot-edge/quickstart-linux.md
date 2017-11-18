---
title: "Szybki Start Azure IoT krawędzi + systemu Linux | Dokumentacja firmy Microsoft"
description: "Wypróbować usługę Azure IoT krawędzi, uruchamiając analytics na urządzeniu symulowane krawędzi"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/16/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: bfa6652eac34f88baf09f55353cf58227a20e4cf
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-linux-device---preview"></a>Szybki Start: Wdrażanie modułu pierwszy krawędzi IoT z portalu Azure na urządzeniu z systemem Linux — w wersji preview

Usługa Azure IoT krawędzi przenosi mocy chmury urządzeń Internetu rzeczy. W tym temacie Dowiedz się, jak użyć interfejsu chmury do zdalnego wdrożenia wbudowane kodu do urządzenia IoT.

Jeśli nie masz aktywnych subskrypcji platformy Azure, Utwórz [bezpłatne konto] [ lnk-account] przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania tego zadania, należy użyć komputer lub maszynę wirtualną do symulacji urządzenia Internetu rzeczy. Następujące usługi są wymagane do pomyślnego wdrożenia urządzenia IoT:

- [Zainstaluj Docker w systemie Linux] [ lnk-docker-ubuntu] i upewnij się, że jest uruchomiona. 
- Większość dystrybucje systemu Linux, łącznie z Ubuntu, już Python 2.7 zainstalowane. Użyj następującego polecenia, aby upewnić się, że zainstalowano narzędzia pip: `sudo apt-get install python-pip`.

## <a name="create-an-iot-hub-with-azure-cli"></a>Tworzenie Centrum IoT z wiersza polecenia platformy Azure

Utwórz Centrum IoT w Twojej subskrypcji platformy Azure. Poziom wolnego Centrum IoT działa w przypadku tego przewodnika Szybki Start. Jeśli używano Centrum IoT w przeszłości i już koncentratora wolnego utworzony, można pominąć tę sekcję i przejdź do [zarejestrować urządzenia IoT][anchor-register]. Każda subskrypcja może mieć tylko jeden bezpłatne Centrum IoT. 

1. Zaloguj się w witrynie [Azure Portal][lnk-portal]. 
1. Wybierz **powłoki chmury** przycisku. 

   ![Przycisk powłoki w chmurze][1]

1. Utwórz grupę zasobów. Poniższy kod tworzy grupę zasobów o nazwie **IoTEdge** w **zachodnie stany USA** regionu:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Utwórz Centrum IoT w nowej grupy zasobów. Poniższy kod tworzy bezpłatny **F1** koncentratora o nazwie **MyIotHub** w grupie zasobów **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Zarejestruj urządzenia IoT

Tworzenie tożsamości urządzenia symulowane urządzenie, dzięki czemu może komunikować się z Centrum IoT. Ponieważ urządzenia IoT brzegowe zachowują się i mogą być zarządzane inaczej niż typowe urządzenia IoT, zadeklarować to być urządzenia IoT od początku. 

1. W portalu Azure przejdź do Centrum IoT.
1. Wybierz **IoT krawędzi (wersja zapoznawcza)**.
1. Wybierz **dodać IoT urządzenia**.
1. Nadaj symulowane urządzenie identyfikator unikatowy urządzenia.
1. Wybierz **zapisać** Aby dodać własne urządzenie.
1. Wybierz nowe urządzenie z listy urządzeń. 
1. Skopiuj wartość **parametry połączenia — klucz podstawowy** i zapisz go. Ta wartość będzie używane do konfigurowania środowiska uruchomieniowego IoT krawędzi w następnej sekcji. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Zainstaluj i uruchom środowisko uruchomieniowe krawędzi IoT

Środowisko uruchomieniowe krawędzi IoT jest wdrażana na wszystkich urządzeniach IoT krawędzi. Zawiera dwa moduły. Najpierw agenta krawędzi IoT ułatwia wdrażanie i monitorowanie modułów na urządzeniu IoT krawędzi. Po drugie Centrum IoT krawędzi zarządza komunikacji między modułami na urządzeniu IoT krawędzi i między urządzeniami a Centrum IoT. 

Na komputerze, na którym będzie uruchamiane urządzenie brzegowe IoT Pobierz skrypt kontroli krawędzi IoT:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Skonfiguruj środowisko uruchomieniowe za pomocą parametrów połączenia urządzenia IoT krawędzi z poprzedniej sekcji:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Uruchom środowisko uruchomieniowe:
```cmd
sudo iotedgectl start
```

Docker wyboru, aby zobaczyć, czy agent krawędzi IoT jest uruchomiony jako moduł:
```cmd
sudo docker ps
```

![Zobacz edgeAgent w Docker](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Wdrażanie modułu

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Widok wygenerowany danych

W tego przewodnika Szybki Start utworzyć nowe urządzenie brzegowe IoT, a na nim zainstalowany środowiska uruchomieniowego IoT krawędzi. Następnie użyto portalu Azure do umieszczenia krawędzi IoT modułu do uruchomienia na urządzeniu bez konieczności zmiany na urządzeniu. W takim przypadku moduł, który zostanie przypisany tworzy dane środowiska, używanego programu samouczków. 

Otwórz wiersz polecenia na komputerze z uruchomionym symulowane urządzenie ponownie. Upewnij się, że moduł wdrożonych w chmurze jest uruchomiona na urządzeniu IoT krawędzi:

```cmd
sudo docker ps
```

![Wyświetl trzech modułów na urządzeniu](./media/tutorial-simulate-device-linux/docker-ps2.png)

Wyświetl komunikaty wysyłane z modułu tempSensor do chmury:

```cmd
sudo docker logs -f tempSensor
```

![Wyświetlanie danych z modułu](./media/tutorial-simulate-device-linux/docker-logs.png)

Można również wyświetlić dane telemetryczne, wysyła urządzenia przy użyciu [narzędzia explorer Centrum IoT][lnk-iothub-explorer]. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Podczas tworzenia Centrum IoT nie są już potrzebne, można użyć [usuwania Centrum iot az] [ lnk-delete] polecenie, aby usunąć zasób i wszystkie skojarzone z nią urządzenia:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Następne kroki

Wiesz, jak wdrożyć moduł krawędzi IoT urządzenia IoT. Teraz spróbuj wdrażanie różnych typów usług Azure modułów, dzięki czemu można analizować dane na krawędzi. 

* [Wdrożyć własny kod jako moduł](tutorial-csharp-module.md)
* [Wdrażanie funkcji platformy Azure jako moduł](tutorial-deploy-function.md)
* [Wdrażanie usługi Azure Stream Analytics jako modułu](tutorial-deploy-stream-analytics.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
