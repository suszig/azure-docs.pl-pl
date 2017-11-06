---
title: "Urządzenie fizyczne za pomocą usługi Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Jak korzystać z urządzenia Texas Instruments w Sensor tag do wysyłania danych do Centrum IoT przez bramę krawędzi IoT uruchamianie na urządzeniu malina Pi 3. Brama jest utworzony przy użyciu usługi Azure IoT krawędzi."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: andbuc
ms.openlocfilehash: b24828ee1a09ba8e5f657954e11936f124270173
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="forward-device-to-cloud-messages-to-iot-hub-using-azure-iot-edge-on-a-raspberry-pi"></a>Przekazywania komunikatów urządzenia do chmury przy użyciu usługi Azure IoT Edge na Pi malina Centrum IoT

Ten przewodnik [próbki niski energii Bluetooth] [ lnk-ble-samplecode] przedstawiono sposób użycia [Azure IoT krawędzi] [ lnk-sdk] do:

* Przesyła dane telemetryczne urządzenia do chmury Centrum IoT z urządzenia fizycznego.
* Trasy poleceń z Centrum IoT urządzenia fizycznego.

Przewodnik składa się z następujących elementów:

* **Architektura**: ważnych architektury informacji dotyczących przykładowych niski energii Bluetooth.
* **Kompilowanie i uruchamianie**: czynności wymagane do skompilowania i uruchomienia przykładu.

## <a name="architecture"></a>Architektura

Przewodniku zademonstrowano, jak skompilować i uruchomić brama brzegowa IoT na malina Pi 3 z systemem Raspbian Linux. Brama jest utworzony przy użyciu IoT krawędzi. W przykładzie użyto urządzenia Texas Instruments Sensor tag Bluetooth małej energii (cz) do zbierania danych temperatury.

Po uruchomieniu brama brzegowa IoT go:

* Nawiązuje połączenie z urządzeniem Sensor tag przy użyciu protokołu energii małej Bluetooth (cz).
* Nawiązuje połączenie z Centrum IoT przy użyciu protokołu HTTPS.
* Przekazuje dane telemetryczne z urządzeń Sensor tag do Centrum IoT.
* Polecenia tras z Centrum IoT na urządzeniu Sensor tag.

Brama zawiera następujące moduły krawędzi IoT:

* A *modułu cz* który interfejsy z urządzeniem cz otrzymywanie temperatury danych z urządzenia i wysyłać polecenia do urządzenia.
* A *cz chmury do urządzenia moduł* konwertujący JSON wiadomości wysyłane z Centrum IoT w instrukcji cz *modułu cz*.
* A *modułu rejestratora* który rejestruje komunikaty bramy do lokalnego pliku.
* *Moduł mapowania tożsamości* który tłumaczy adresy MAC cz urządzenia i tożsamości urządzenia Azure IoT Hub.
* *Modułu Centrum IoT* który przekazuje dane telemetryczne w Centrum IoT i odbiera polecenia urządzenia z Centrum IoT.
* A *cz drukarki moduł* który interpretuje dane telemetryczne z urządzenia cz i drukowanie sformatowanych danych do konsoli umożliwiające rozwiązywanie problemów i debugowania.

### <a name="how-data-flows-through-the-gateway"></a>Jak dane przepływają przez bramę

Na poniższym diagramie bloku przedstawiono potoku przepływu danych przekazywania telemetrii:

![Potok bramy przekazywania telemetrii](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

Dostępne są następujące kroki, które przyjmuje elementu telemetrii podróży z urządzenia cz z Centrum IoT:

1. Urządzenie cz generuje próbki temperatury i wysyła je przez sieć Bluetooth modułu cz w bramie.
1. Moduł cz odbiera próbki i publikuje ją do brokera oraz adres MAC urządzenia.
1. Moduł mapowania tożsamości przejmuje ten komunikat i używa wewnętrznej tabeli do tłumaczenia adres MAC urządzenia na tożsamości urządzenia IoT Hub. Centrum IoT urządzenia tożsamości składa się z Identyfikatorem urządzenia i klucz urządzenia.
1. Moduł mapowania tożsamości publikuje nowy komunikat, który zawiera następujące informacje:
   - Temperatury przykładowe dane
   - Adres MAC urządzenia
   - Identyfikator urządzenia
   - Klucz urządzenia  
1. Moduł Centrum IoT to nowego komunikatu (generowane przez moduł mapowania tożsamości) i publikuje go do Centrum IoT.
1. Moduł rejestratora rejestruje wszystkie komunikaty z brokera w lokalnym pliku.

Na poniższym diagramie bloku przedstawiono potoku przepływu danych polecenia urządzenia:

![Urządzenia polecenie bramy potoku](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. Moduł Centrum IoT okresowo sonduje Centrum IoT nowe wiadomości polecenia.
1. Moduł Centrum IoT odebrania nowego komunikatu polecenia publikuje ją do brokera.
1. Moduł mapowania tożsamości przejmuje komunikat polecenia i używa wewnętrznej tabeli można przetłumaczyć Identyfikatora urządzenia Centrum IoT na adres MAC urządzenia. Go następnie publikuje nowy komunikat zawierający adres MAC urządzenia docelowego w planie właściwości komunikatu.
1. Moduł cz chmury do urządzenia przejmuje ten komunikat i przekształca ją w odpowiednie instrukcje cz modułu cz. Go następnie publikuje nowy komunikat.
1. Moduł cz przejmuje ten komunikat i wykonuje instrukcję We/Wy przy komunikacji z urządzeniem cz.
1. Moduł rejestratora rejestruje wszystkie komunikaty z brokera pliku na dysku.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania kroków tego samouczka jest potrzebna aktywna subskrypcja platformy Azure.

> [!NOTE]
> Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk-free-trial].

Należy klient SSH na komputerze pulpitu umożliwia zdalny dostęp do wiersza polecenia na malina Pi.

- System Windows nie zawiera klienta SSH. Firma Microsoft zaleca używanie [PuTTY](http://www.putty.org/).
- Większość dystrybucje systemu Linux i Mac OS obejmują narzędzia wiersza polecenia SSH. Aby uzyskać więcej informacji, zobacz [SSH za pomocą systemu Linux lub Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Przygotowanie sprzętu

Ten samouczek zakłada się, czy używasz [Texas Instruments w Sensor tag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) urządzenie podłączone do uruchamiania Raspbian malina Pi 3.

### <a name="install-raspbian"></a>Zainstaluj Raspbian

Do zainstalowania na urządzeniu malina Pi 3 Raspbian, można użyć jednej z następujących opcji.

* Aby zainstalować najnowszą wersję Raspbian, użyj [NOOBS] [ lnk-noobs] graficznego interfejsu użytkownika.
* Ręcznie [Pobierz][lnk-raspbian] i zapisywać najnowsze obrazu systemu operacyjnego Raspbian na karcie SD.

### <a name="sign-in-and-access-the-terminal"></a>Zaloguj się i uzyskać dostęp terminala

Dostępne są dwie opcje środowisku końcowych dla sieci Pi malina dostępu do:

* Jeśli masz klawiatury i monitora podłączone do sieci Pi malina umożliwia Raspbian graficzny interfejs użytkownika dostępu okno terminala.

* Dostęp do wiersza polecenia na Twoje Pi malina przy użyciu protokołu SSH z komputera stacjonarnego.

#### <a name="use-a-terminal-window-in-the-gui"></a>Okno terminalu w graficznym interfejsie użytkownika

Nazwa użytkownika są domyślne poświadczenia dla Raspbian **pi** i hasło **malina**. Na pasku zadań w interfejsie GUI, uruchom **Terminal** narzędzie przy użyciu ikony, która wygląda jak monitora.

#### <a name="sign-in-with-ssh"></a>Zaloguj się przy użyciu protokołu SSH

Umożliwia SSH dla wiersza polecenia dostępu użytkownika Pi malina. Artykuł [SSH (Secure Shell)] [ lnk-pi-ssh] w tym artykule opisano sposób konfigurowania SSH na Twoje Pi malina oraz sposób nawiązywania połączenia z [Windows] [ lnk-ssh-windows] lub [ System operacyjny Linux & Mac][lnk-ssh-linux].

Zaloguj się przy użyciu nazwy użytkownika **pi** i hasło **malina**.

### <a name="install-bluez-537"></a>Zainstaluj BlueZ 5.37

Moduły cz skontaktować sprzętu Bluetooth za pośrednictwem BlueZ stosu. Wymagana wersja 5.37 BlueZ dla modułów, aby działać poprawnie. Tych instrukcji upewnij się, że jest zainstalowana prawidłowa wersja BlueZ.

1. Zatrzymaj bieżącego demona bluetooth:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Zainstaluj zależności BlueZ:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf libtool glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Pobierz kod źródłowy BlueZ z bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Rozpakuj kodu źródłowego:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Zmień katalogi na nowo utworzony folder:

    ```sh
    cd bluez-5.37
    ```

1. Skonfiguruj kodu BlueZ ma zostać utworzony:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Tworzenie BlueZ:

    ```sh
    make
    ```

1. Zainstaluj BlueZ po zakończeniu kompilowania:

    ```sh
    sudo make install
    ```

1. Zmienianie konfiguracji usługi systemd Bluetooth, aby wskazywał Nowy demon bluetooth w pliku `/lib/systemd/system/bluetooth.service`. Zastąp linię "ExecStart" następujący tekst:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a>Włączyć łączność z urządzeń Sensor tag z urządzenia malina Pi 3

Przed uruchomieniem próbki, należy sprawdzić, czy Twoje malina Pi 3 można połączyć się z urządzeniem Sensor tag.

1. Upewnij się, `rfkill` zainstalowano narzędzie:

    ```sh
    sudo apt-get install rfkill
    ```

1. Odblokowany bluetooth na 3 Pi malina i sprawdź, czy numer wersji jest **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. Aby wprowadzić powłoki interakcyjne bluetooth, uruchom usługę bluetooth i wykonać **bluetoothctl** polecenia:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Wprowadź polecenie **włączania zasilania** do zasilanie kontrolera bluetooth. Polecenie zwraca dane wyjściowe podobne do poniższego przykładu:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. W powłoce interakcyjne bluetooth, wprowadź polecenie **na skanowania** skanowania pod kątem urządzenia bluetooth. Polecenie zwraca dane wyjściowe podobne do poniższego przykładu:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Urządzeń Sensor tag stał się wykrywalny, naciskając przycisk małe (zielony LED powinien flash). Pi 3 malina powinien odnajdywanie urządzeń Sensor tag:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    W tym przykładzie widać, że adres MAC urządzenia Sensor tag jest **A0:E6:F8:B5:F6:00**.

1. Wyłącz skanowanie wprowadzając **skanowania poza** polecenia:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Łączenie się z urządzeniem Sensor tag przy użyciu adresu MAC, wprowadzając **połączyć \<adres MAC\>**. Następujące przykładowe dane wyjściowe jest skracana dla jasności:

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Możesz wyświetlić listę właściwości GATT urządzenia ponownie, używając **listę atrybutów** polecenia.

1. Teraz możesz odłączyć z urządzeniami przy użyciu **rozłączyć** polecenia, a następnie Zamknij z powłoki bluetooth przy użyciu **Zamknij** polecenia:

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Teraz możesz uruchomić przykładowy krawędzi IoT cz na Twoje malina Pi 3.

## <a name="run-the-iot-edge-ble-sample"></a>Uruchamianie przykładowych cz krawędzi IoT

Aby uruchomić przykładowe cz krawędzi IoT, należy wykonać trzy zadania:

* Skonfiguruj dwa przykładowe urządzenia w Centrum IoT.
* Tworzenie IoT Edge na urządzeniu malina Pi 3.
* Konfigurowania i uruchamiania przykładowych cz na urządzeniu malina Pi 3.

W momencie pisania krawędzi IoT obsługuje modułów cz tylko w bramy z systemem Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Skonfiguruj dwa przykładowe urządzenia w Centrum IoT

* [Tworzenie Centrum IoT] [ lnk-create-hub] w Twojej subskrypcji platformy Azure, potrzebna jest nazwa Centrum w tym przewodniku. Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut.
* Dodaj urządzenia o nazwie **SensorTag_01** do Centrum IoT i zanotować jego klucza identyfikator i urządzenia. Można użyć [explorer urządzenie lub Centrum iothub explorer] [ lnk-explorer-tools] narzędzia dodać to urządzenie w Centrum IoT utworzony w poprzednim kroku, a następnie pobrać jego klucza. To urządzenie należy mapować urządzeń Sensor tag podczas konfigurowania bramy.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Tworzenie Azure IoT krawędzi w Twojej malinowe Pi 3

Zainstaluj zależności Edge IoT Azure:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev libtool
```

Klonowanie krawędzi IoT i wszystkie jego modułów podrzędnych do katalogu macierzystego, należy użyć następujących poleceń:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Jeśli masz pełną kopię repozytorium krawędzi IoT na Twoje malina Pi 3 można tworzyć za pomocą następującego polecenia z folderu, który zawiera zestaw SDK:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a>Konfigurowanie i uruchamianie przykładowych cz na Twoje malina Pi 3

Bootstrap i uruchomić próbki, należy skonfigurować każdy moduł IoT krawędzi, który uczestniczy w bramie. Tej konfiguracji znajduje się w pliku JSON, a następnie należy skonfigurować pięć uczestniczących modułach IoT krawędzi. Brak przykładowy plik JSON w repozytorium o nazwie **bramy\_sample.json** czy służy jako punkt początkowy do tworzenia pliku konfiguracji. Ten plik jest **przykłady/ble_gateway/src** folderu w lokalnej kopii repozytorium IoT krawędzi.

W poniższych sekcjach opisano sposób edytowania tego pliku konfiguracji dla przykładu cz. Przyjęto założenie, że repozytorium krawędzi IoT jest **/home/pi/iot-edge /** folderu na Twoje malina Pi 3. Jeśli repozytorium jest w innym miejscu, należy odpowiednio zmień ścieżki.

#### <a name="logger-configuration"></a>Konfiguracja rejestratora

Zakładając, że repozytorium bramy znajduje się w **/home/pi/iot-edge /** folderu, skonfigurować moduł rejestratora w następujący sposób:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>Konfiguracja modułu cz

Przykładowa konfiguracja urządzenia cz zakłada urządzenia Texas Instruments w Sensor tag. Standardowe urządzenie cz może działać jako GATT peryferyjnych powinny działać, ale może być konieczne zaktualizowanie identyfikatorów charakterystyczny GATT i danych. Dodaj adres MAC urządzenia Sensor tag:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Jeśli nie używasz urządzeń Sensor tag, zapoznaj się z dokumentacją urządzenia cz określić, czy konieczne jest aktualizacja cecha GATT identyfikatorów i wartości danych.

#### <a name="iot-hub-module"></a>Moduł Centrum IoT

Dodaj nazwę Centrum IoT. Wartość sufiksu jest zwykle **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Konfiguracja modułu mapowania tożsamości

Dodaj adres MAC urządzenia Sensor tag identyfikator urządzenia i klucz **SensorTag_01** urządzenie dodane do Centrum IoT:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Konfiguracja modułu cz drukarki

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>Konfiguracja modułu BLEC2D

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Konfiguracja routingu

Następująca konfiguracja zapewnia następujące routingu między modułami krawędzi IoT:

* **Rejestratora** modułu odbiera i rejestruje wszystkie komunikaty.
* **Sensor tag** modułu wysyła komunikaty do obu **mapowania** i **drukarki cz** modułów.
* **Mapowania** moduł powoduje wysyłanie komunikatów do **Centrum IoTHub** moduł ma zostać wysłany w do Centrum IoT.
* **Centrum IoTHub** moduł powoduje wysyłanie komunikatów z powrotem do **mapowania** modułu.
* **Mapowania** moduł powoduje wysyłanie komunikatów do **BLEC2D** modułu.
* **BLEC2D** moduł powoduje wysyłanie komunikatów z powrotem do **Sensor Tag** modułu.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

Aby uruchomić przykład, Przekaż ścieżka do pliku konfiguracji JSON jako parametr **cz\_bramy** binarnego. Polecenie zakłada używasz **gateway_sample.json** pliku konfiguracji. Wykonaj to polecenie z **krawędzi iot** folderu na Pi malina:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Konieczne może być naciśnięcie przycisku małych na urządzeniu Sensor tag, aby był wykrywalny przed uruchomieniem próbki.

Po uruchomieniu próbki, można użyć [explorer urządzenia](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) lub [explorer Centrum iothub](https://github.com/Azure/iothub-explorer) narzędzia do monitorowania wiadomości brama brzegowa IoT przekazuje z urządzeń Sensor tag. Na przykład za pomocą Eksploratora Centrum iothub można monitorować wiadomości urządzenia do chmury przy użyciu następującego polecenia:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Wysyłanie komunikatów z chmury do urządzeń

Moduł cz obsługuje również wysyłanie poleceń z Centrum IoT na urządzeniu. Można użyć [explorer urządzenia](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) lub [explorer Centrum iothub](https://github.com/Azure/iothub-explorer) narzędzia do wysyłania wiadomości JSON, które moduł bramy cz przekazuje do urządzenia cz.

Jeśli korzystasz z urządzenia Texas Instruments w Sensor tag, można włączyć red LED, zielony LED lub Brzęczyk przez wysyłanie poleceń z Centrum IoT. Przed wysłaniem poleceń z Centrum IoT należy najpierw wysłać następujące dwa komunikaty JSON w kolejności. Następnie możesz wysłać do dowolnego z poleceń, aby włączyć świateł lub Brzęczyk.

1. Resetuj wszystkie LED i brzęczyka (wyłączone):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. Skonfiguruj we/wy jako "remote":

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Teraz możesz wysłać dowolne z poniższych poleceń, aby włączyć świateł lub Brzęczyk na urządzeniu Sensor tag:

* Włącz LED czerwony:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Włącz LED zielony:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Włącz brzęczyka:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz uzyskać większą wiedzę krawędzi IoT i wypróbować niektóre przykłady kodu, odwiedź następujące samouczki deweloperów i zasoby:

* [Krawędź IoT Azure][lnk-sdk]

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
