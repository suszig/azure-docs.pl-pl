---
title: "Udostępnianie urządzeń z systemem Windows do monitorowania zdalnego w języku C - Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób podłącz urządzenie do wstępnie pakiet IoT Azure zdalnego monitorowania rozwiązania przy użyciu Aplikacja napisana w języku C uruchomiony w systemie Windows."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: ba213ff9e5f2f1cb71d1ae6bbe38d4989f882255
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania (system Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Ten samouczek pokazuje, jak nawiązać zdalnego wstępnie skonfigurowane rozwiązanie monitorowania urządzenia fizycznego.

## <a name="create-a-c-client-solution-on-windows"></a>Tworzenie rozwiązania klienta C w systemie Windows

Podobnie jak w przypadku najbardziej osadzonych aplikacji na urządzeniach ograniczone, kodu klienta dla aplikacji urządzenia są zapisywane w C. W tym samouczku tworzenia aplikacji na maszynie z systemem Windows.

### <a name="create-the-starter-project"></a>Utwórz projekt początkowy

Utwórz projekt starter w Visual Studio 2017 i dodawanie pakietów NuGet Centrum IoT urządzenia klienta:

1. W programie Visual Studio Utwórz aplikację konsolową C za pomocą programu Visual C++ **aplikacji konsoli systemu Windows** szablonu. Nazwij projekt **RMDevice**.

    ![Utwórz aplikację Konsolową programu Visual C++ systemu Windows](media/iot-suite-connecting-devices/visualstudio01.png)

1. W **Eksploratora rozwiązań**, Usuń pliki `stdafx.h`, `targetver.h`, i `stdafx.cpp`.

1. W **Eksploratora rozwiązań**, Zmień nazwę pliku `RMDevice.cpp` do `RMDevice.c`.

    ![Plik RMDevice.c zmieniona przedstawiający Eksploratora rozwiązań](media/iot-suite-connecting-devices/visualstudio02.png)

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, a następnie kliknij przycisk **pakiety zarządzania pakietami NuGet**. Wybierz **Przeglądaj**, następnie wyszukaj i zainstaluj następujące pakiety NuGet:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![Menedżer pakietów NuGet zawiera zainstalowane pakiety Microsoft.Azure.IoTHub](media/iot-suite-connecting-devices/visualstudio03.png)

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, a następnie wybierz pozycję **właściwości** można otworzyć projektu **strony właściwości** okno dialogowe. Aby uzyskać więcej informacji, zobacz [Ustawianie właściwości projektu Visual C++](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Wybierz **C/C++** folder, a następnie wybierz **prekompilowanych nagłówków** strony właściwości.

1. Ustaw **Prekompilowanego nagłówka** do **prekompilowane nagłówki nie są używane**. Następnie wybierz pozycję **Zastosuj**.

    ![Właściwości projektu Pokaż projekt nie używa prekompilowanych nagłówków](media/iot-suite-connecting-devices/visualstudio04.png)

1. Wybierz **konsolidatora** folder, a następnie wybierz **dane wejściowe** strony właściwości.

1. Dodaj `crypt32.lib` do **dodatkowe zależności** właściwości. Aby zapisać wartości właściwości projektu, wybierz **OK** , a następnie **OK** ponownie.

    ![Właściwości projektu Pokaż tym crypt32.lib konsolidatora](media/iot-suite-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Dodaj bibliotekę Parson JSON

Dodaj bibliotekę Parson JSON do **RMDevice** projektu i Dodaj wymagane `#include` instrukcji:

1. W odpowiednich folderu na komputerze klonowanie repozytorium Parson GitHub przy użyciu następującego polecenia:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopiuj `parson.h` i `parson.c` plików z kopii lokalnej repozytorium Parson do Twojej **RMDevice** folderu projektu.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy **RMDevice** projektu, wybierz **Dodaj**, a następnie wybierz pozycję **istniejący element**.

1. W **Dodaj istniejący element** okno dialogowe, wybierz opcję `parson.h` i `parson.c` plików **RMDevice** folderu projektu. Aby dodać tych plików do projektu, wybierz **Dodaj**.

    ![Pliki parson.h i parson.c pokazuje Eksploratora rozwiązań](media/iot-suite-connecting-devices/visualstudio06.png)

1. W programie Visual Studio Otwórz `RMDevice.c` pliku. Zastąp istniejące `#include` instrukcje następującym kodem:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Teraz możesz sprawdzić, czy projekt ma prawidłowe zależności przez budowania rozwiązania.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Tworzenie i uruchamianie próbki

Dodaj kod, aby wywołać **zdalnego\_monitorowania\_Uruchom** działanie, a następnie skompilować i uruchomić aplikację dla urządzeń:

1. Do wywołania **zdalnego\_monitorowania\_Uruchom** działać, Zastąp **głównego** funkcji z następującym kodem:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Wybierz **kompilacji** , a następnie **Kompiluj rozwiązanie** do skompilowania aplikacji urządzenia. Ignoruj ostrzeżenia dotyczące **gmtime —** funkcji.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, wybierz **debugowania**, a następnie wybierz pozycję **Start nowe wystąpienie** do uruchomienia przykładu . W konsoli są wyświetlane komunikaty w postaci:

    * Aplikacja wysyła przykładowe dane telemetryczne do wstępnie skonfigurowane rozwiązanie.
    * Odbiera wartości żądanej właściwości ustawione na pulpicie nawigacyjnym rozwiązania.
    * Odnosi się do metody wywoływane z poziomu pulpitu nawigacyjnego rozwiązania.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
