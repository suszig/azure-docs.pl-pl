---
title: "Konwersja danych w bramie IoT Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Użyj bramy IoT próbę przekonwertowania formatu danych czujników za pomocą dostosowanego modułu z Azure IoT krawędzi."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Konwersja danych bramy iot, przekształcania danych bramy iot"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Użyj bramy IoT dla transformacji danych czujnika z krawędzią IoT Azure

> [!NOTE]
> Przed rozpoczęciem tego samouczka, upewnij się, że wnioski następujące zakończyła się w sekwencji:
> * [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) (Konfigurowanie urządzenia Intel NUC jako bramy IoT)
> * [Użyj bramy IoT nawiązać rzeczy cloud - Sensor tag do Centrum IoT Azure](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Jeden bramy Iot służy do przetwarzania zebranych danych przed wysłaniem do chmury. Usługa Azure IoT krawędzi wprowadza modułów, które można tworzyć i zebranych w celu utworzenia przepływu pracy przetwarzania danych. Moduł odbiera komunikat, wykonuje akcję na nim i przenieść ją na innych modułów do przetworzenia.

## <a name="what-you-learn"></a>Omawiane zagadnienia

Jak utworzyć moduł do konwersji wiadomości z Sensor tag do innego formatu.

## <a name="what-you-do"></a>Co zrobić

* Utwórz moduł można skonwertować odebranej wiadomości do formatu JSON.
* Kompiluj moduł.
* Dodanie modułu do aplikacji przykładowej cz od Azure IoT krawędzi.
* Uruchom przykładową aplikację.

## <a name="what-you-need"></a>Co jest potrzebne

* Następujące samouczki zostało ukończone w sekwencji:
  * [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) (Konfigurowanie urządzenia Intel NUC jako bramy IoT)
  * [Użyj bramy IoT nawiązać rzeczy cloud - Sensor tag do Centrum IoT Azure](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Klient SSH, który jest uruchamiany na komputerze hosta. PuTTY jest zalecane w systemie Windows. Linux i macOS już dostarczane za pomocą klienta SSH.
* Adres IP i nazwy użytkownika i hasło, aby uzyskać dostęp do bramy z klienta SSH.
* Połączenie internetowe.

## <a name="create-a-module"></a>Tworzenie modułu

1. Na komputerze hosta z klientem SSH i połączyć się z bramą IoT.
1. Klonowanie pliki źródłowe modułu konwersji z serwisu GitHub do katalogu macierzystego bramy IoT, uruchamiając następujące polecenia:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Jest to moduł macierzysty krawędzi Azure napisany w języku programowania C. Moduł konwertuje formatu odebranej wiadomości do następującego:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>Kompiluj moduł

Aby skompilować modułu, uruchom następujące polecenia:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

Możesz uzyskać `libmy_module.so` pliku po zakończeniu kompilacji. Zanotuj ścieżkę bezwzględną tego pliku.

## <a name="add-the-module-to-the-ble-sample-application"></a>Dodanie modułu do cz przykładowej aplikacji

1. Przejdź do folderu przykładów, uruchamiając następujące polecenie:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Otwórz plik konfiguracyjny, uruchamiając następujące polecenie:

   ```bash
   vi ble_gateway.json
   ```

1. Dodaj moduł wstawiając następujący kod do `modules` sekcji.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Zastąp `[Your libmy_module.so path]` w kodzie z ścieżka bezwzględna libmy_module.so "pliku.
1. Zastąp kod w `links` sekcji z elementem następujące:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Naciśnij klawisz `ESC`, a następnie wpisz `:wq` można zapisać pliku.

## <a name="run-the-sample-application"></a>Uruchom przykładową aplikację

1. Włącz Sensor tag.
1. Ustaw zmienną środowiskową SSL_CERT_FILE, uruchamiając następujące polecenie:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Uruchom przykładową aplikację z dodanym module, uruchamiając następujące polecenie:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Następne kroki

Został pomyślnie używać bramy IoT można przekonwertować komunikatu z Sensor tag do formatu JSON.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
