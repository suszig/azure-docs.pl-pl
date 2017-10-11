---
title: "Podłącz urządzenie za pomocą C w systemie Windows | Dokumentacja firmy Microsoft"
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
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania (system Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Tworzenie rozwiązania próbki C w systemie Windows
Poniższe kroki pokazują, jak utworzyć aplikację klienta, który komunikuje się ze zdalnym wstępnie skonfigurowane rozwiązanie monitorowania. Ta aplikacja jest napisany w języku C i wbudowane i uruchamianie w systemie Windows.

Utwórz projekt starter w programie Visual Studio 2015 lub Visual Studio 2017 i dodawanie pakietów NuGet Centrum IoT urządzenia klienta:

1. W programie Visual Studio Utwórz aplikację konsolową C za pomocą programu Visual C++ **aplikacji konsoli Win32** szablonu. Nazwij projekt **RMDevice**.
2. Na **ustawienia aplikacji** strony **Kreator aplikacji Win32**, upewnij się, że **aplikacja Konsolowa** jest zaznaczone, a następnie usuń zaznaczenie pola wyboru **Prekompilowanego nagłówka** i **sprawdza Security Development Lifecycle (SDL)**.
3. W **Eksploratora rozwiązań**, Usuń pliki stdafx.h, targetver.h i stdafx.cpp.
4. W **Eksploratora rozwiązań**, Zmień nazwę pliku RMDevice.cpp na RMDevice.c.
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, a następnie kliknij przycisk **pakiety zarządzania pakietami NuGet**. Kliknij przycisk **Przeglądaj**, następnie wyszukaj i zainstaluj następujące pakiety NuGet:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, a następnie kliknij przycisk **właściwości** można otworzyć projektu **strony właściwości** okno dialogowe. Aby uzyskać więcej informacji, zobacz [Ustawianie właściwości projektu Visual C++][lnk-c-project-properties]. 
7. Kliknij przycisk **konsolidatora** folderu, następnie kliknij przycisk **dane wejściowe** strony właściwości.
8. Dodaj **crypt32.lib** do **dodatkowe zależności** właściwości. Kliknij przycisk **OK** , a następnie **OK** ponownie, aby zapisać wartości właściwości projektu.

Dodaj bibliotekę Parson JSON do **RMDevice** projektu i Dodaj wymagane `#include` instrukcji:

1. W odpowiednich folderu na komputerze klonowanie repozytorium Parson GitHub przy użyciu następującego polecenia:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Skopiuj pliki parson.h i parson.c z lokalną kopię do repozytorium Parson Twojego **RMDevice** folderu projektu.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy **RMDevice** projektu, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **istniejący element**.

1. W **Dodaj istniejący element** okno dialogowe, wybierz opcję parson.h i parson.c plików **RMDevice** folderu projektu. Następnie kliknij przycisk **Dodaj** do dodania tych plików do projektu.

1. W programie Visual Studio Otwórz plik RMDevice.c. Zastąp istniejące `#include` instrukcje następującym kodem:
   
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
    > Teraz możesz sprawdzić, czy projekt ma prawidłowe zależności przez skompilowanie go.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Tworzenie i uruchamianie próbki

Dodaj kod, aby wywołać **zdalnego\_monitorowania\_Uruchom** funkcji, a następnie skompilować i uruchomić aplikację dla urządzeń.

1. Zastąp **głównego** funkcji z następujący kod, aby wywołać **zdalnego\_monitorowania\_Uruchom** funkcji:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kliknij przycisk **kompilacji** , a następnie **Kompiluj rozwiązanie** do skompilowania aplikacji urządzenia.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **RMDevice** projektu, kliknij przycisk **debugowania**, a następnie kliknij przycisk **Start nowe wystąpienie** do uruchomienia przykładu. W konsoli są wyświetlane komunikaty aplikacji wysyła dane telemetryczne próbki do wstępnie skonfigurowane rozwiązanie, otrzymuje wartości żądanej właściwości ustawione na pulpicie nawigacyjnym rozwiązania i odpowiada metody wywoływane z poziomu pulpitu nawigacyjnego rozwiązania.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
