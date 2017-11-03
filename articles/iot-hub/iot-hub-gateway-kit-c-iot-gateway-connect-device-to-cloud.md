---
title: "Użyj bramy IoT połączenia z urządzeniem z Centrum IoT Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać Intel NUC jako brama IoT łączenia Sensor tag analizy czasowej oraz wysyłania danych czujnika do Centrum IoT Azure w chmurze."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Brama iot Podłącz urządzenie do chmury"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a>Użyj bramy IoT nawiązać rzeczy cloud - Sensor tag do Centrum IoT Azure

> [!NOTE]
> Przed rozpoczęciem tego samouczka, upewnij się, przeprowadzisz [Konfigurowanie NUC Intel jako brama IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). W [Konfigurowanie NUC Intel jako brama IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), skonfiguruj urządzenie Intel NUC jako brama IoT.

## <a name="what-you-will-learn"></a>Co dowiesz się

Sposób łączenia Texas Instruments Sensor tag (CC2650STK) z Centrum IoT Azure za pomocą bramy IoT. Brama IoT wysyła temperatury i wilgotności dane zbierane z Sensor tag do Centrum IoT Azure.

## <a name="what-you-will-do"></a>Będzie wykonywać

- Tworzenie Centrum IoT.
- Zarejestruj urządzenie w Centrum IoT dla Sensor tag.
- Włącz połączenie między bramą IoT i Sensor tag.
- Uruchom przykładową aplikację cz wysyłać dane Sensor tag do Centrum IoT.

## <a name="what-you-need"></a>Co jest potrzebne

- Samouczek [Konfigurowanie NUC Intel jako brama IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) zakończone, w której skonfigurowaniu NUC Intel jako brama IoT.
- * Aktywna subskrypcja platformy Azure. Jeśli nie masz konta platformy Azure, [utworzyć bezpłatne konto próbne Azure](https://azure.microsoft.com/free/) za kilka minut.
- Klient SSH, który jest uruchamiany na komputerze hosta. PuTTY jest zalecane w systemie Windows. Linux i macOS już dostarczane za pomocą klienta SSH.
- Adres IP i nazwy użytkownika i hasło, aby uzyskać dostęp do bramy z klienta SSH.
- Połączenie internetowe.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Tutaj możesz zarejestrować to nowe urządzenie dla użytkownika Sensor tag

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a>Włącz połączenie między bramą IoT i Sensor tag

W tej sekcji należy wykonać następujące zadania:

- Pobierz adres MAC Sensor tag dla połączenia Bluetooth.
- Inicjowanie połączenia Bluetooth z bramy IoT do Sensor tag.

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a>Pobierz adres MAC Sensor tag połączenia Bluetooth

1. Na komputerze hosta z klientem SSH i połączyć się z bramą IoT.
1. Odblokuj Bluetooth, uruchamiając następujące polecenie:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Uruchom usługę Bluetooth na bramie IoT i wprowadź powłoki Bluetooth, aby konfigurować funkcję Bluetooth, uruchamiając następujące polecenia:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Włącz kontrolera Bluetooth, uruchamiając następujące polecenie w powłoce Bluetooth:

   ```bash
   power on
   ```

   ![Włącz kontrolera Bluetooth na bramie IoT z bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Uruchom skanowanie w poszukiwaniu pobliskimi urządzeniami Bluetooth, uruchamiając następujące polecenie:

   ```bash
   scan on
   ```

   ![Skanowanie w pobliżu urządzenia Bluetooth z bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Naciśnij przycisk parowania na Sensor tag. Zielony PRZEPROWADZONY na błysków Sensor tag.
1. W powłoce Bluetooth powinien pojawić się Sensor tag został znaleziony. Zanotuj adres MAC Sensor tag. W tym przykładzie jest to adres MAC Sensor tag `24:71:89:C0:7F:82`.
1. Wyłącz skanowanie za pomocą następującego polecenia:

   ```bash
   scan off
   ```

   ![Zatrzymuje skanowanie w pobliżu urządzenia Bluetooth z bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a>Inicjowanie połączenia Bluetooth z bramy IoT do Sensor tag

1. Podłącz do Sensor tag, uruchamiając następujące polecenie:

   ```bash
   connect <MAC address>
   ```

   ![Nawiązać Sensor tag z bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Odłącz od Sensor tag, a Wyjdź z powłoki Bluetooth, uruchamiając następujące polecenia:

   ```bash
   disconnect
   exit
   ```

   ![Odłącz od Sensor tag z bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Pomyślnie włączono połączenie między Sensor tag, a brama IoT.

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a>Uruchom przykładową aplikację cz wysyłać dane Sensor tag do Centrum IoT

Przykładowa aplikacja energii małej Bluetooth (cz) są udostępniane przez Azure IoT krawędzi. Przykładowa aplikacja zbiera dane z połączenia cz i wysyłać dane do Centrum IoT. Aby uruchomić przykładową aplikację, musisz:

1. Konfigurowanie przykładowej aplikacji.
1. Uruchom przykładową aplikację w bramie IoT.

### <a name="configure-the-sample-application"></a>Konfigurowanie przykładowej aplikacji

1. Przejdź do folderu przykładowej aplikacji, uruchamiając następujące polecenie:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Otwórz plik konfiguracyjny, uruchamiając następujące polecenie:

   ```bash
   vi ble_gateway.json
   ```

1. Wypełnij następujące wartości w pliku konfiguracji:

   **IoTHubName**: Nazwa centrum IoT.

   **IoTHubSuffix**: Pobierz IoTHubSuffix z klucza podstawowego ciąg połączenia urządzenia zanotowaną w dół. Upewnij się, że uzyskać klucz podstawowy ciąg połączenia urządzenia nie klucz podstawowy parametry połączenia Centrum IoT. Klucz podstawowy urządzenia ciągu połączenia jest w formacie `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Transport**: wartość domyślna to `amqp`. Ta wartość przedstawia protokołu podczas transpotation. Może to być `http`, `amqp`, lub `mqtt`.

   **macAddress**: adres MAC Sensor tag, zanotowaną w dół.

   **deviceID**: identyfikator urządzenia, który został utworzony w Centrum IoT.

   **deviceKey**: klucz podstawowy ciąg połączenia urządzenia.

   ![Zakończenie pliku konfiguracji cz przykładowej aplikacji](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Naciśnij klawisz `ESC` i typ `:wq` można zapisać pliku.

### <a name="run-the-sample-application"></a>Uruchom przykładową aplikację

1. Upewnij się, że Sensor tag jest włączona.
1. Uruchom następujące polecenie:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Następne kroki

[Użyj bramy IoT dla transformacji danych czujnika z krawędzią IoT Azure](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
