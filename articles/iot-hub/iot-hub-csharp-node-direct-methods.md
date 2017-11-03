---
title: "Centrum IoT Azure bezpośredniego metody (.NET/węzła) | Dokumentacja firmy Microsoft"
description: "Jak używać bezpośrednich metod Centrum IoT Azure. Przy użyciu urządzenia Azure IoT SDK dla środowiska Node.js aplikacji symulowane urządzenie, która zawiera metoda bezpośrednia i usługę Azure IoT SDK dla platformy .NET zaimplementować aplikację usługi, która wywołuje metodę bezpośredniego."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2017
ms.author: nberdy
ms.openlocfilehash: 76f1d32b4afeacae1488b4cf28be6c8cf7f4ea37
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-netnode"></a>Użyj metody bezpośredniego (.NET/węzeł)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

W tym samouczku zamierzamy opracowanie .NET i Node.js aplikację konsoli:

* **CallMethodOnDevice.sln**, aplikacji zaplecza .NET, która wywołuje metodę w aplikacji symulowane urządzenie i wyświetla odpowiedzi.
* **SimulatedDevice.js**, aplikacji Node.js, która symuluje nawiązywania połączenia z Centrum IoT z tożsamości urządzenia utworzony wcześniej urządzenia i odpowiada metodzie wywołanej przez chmurę.

> [!NOTE]
> Artykuł [Azure IoT SDKs][lnk-hub-sdks] (Zestawy SDK usług Azure IoT) zawiera informacje dotyczące zestawów SDK usług Azure IoT, przy użyciu których można tworzyć aplikacje zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.
> 
> 

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Program Visual Studio 2015 lub Visual Studio 2017.
* Środowisko Node.js w wersji 4.0.x lub nowszej.
* Aktywne konto platformy Azure. (Jeśli go nie masz, możesz utworzyć [bezpłatne konto próbne][lnk-free-trial] w zaledwie kilka minut).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulowanego urządzenia
W tej sekcji służy do tworzenia aplikacji konsoli Node.js, która odpowiada do metody wywoływane przez rozwiązania zakończenia.

1. Utwórz nowy pusty folder o nazwie **simulateddevice**. W folderze **simulateddevice** utwórz plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z wiersza polecenia w **simulateddevice** folderu, uruchom następujące polecenie, aby zainstalować **azure iot urządzenia** i **azure-iot urządzenie mqtt** pakiety:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Za pomocą edytora tekstu, Utwórz plik w **simulateddevice** folder i nadaj mu nazwę **SimulatedDevice.js**.
4. Dodaj następujące instrukcje `require` na początku pliku **SimulatedDevice.js**:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Dodaj **connectionString** zmiennej i użyj go, aby utworzyć **DeviceClient** wystąpienia. Zastąp **{ciąg połączenia urządzenia}** z połączenie z urządzeniem ciągu zostanie wygenerowany w *tworzenie tożsamości urządzenia* sekcji:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Dodaj następującą funkcję do zaimplementowania metoda bezpośrednia na urządzeniu:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Otwórz połączenie z Centrum IoT i zainicjować odbiornika metody:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Zapisz i zamknij plik **SimulatedDevice.js**.

> [!NOTE]
> Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym, należy zaimplementować zasady ponawiania (np. Ponów próbę połączenia), zgodnie z sugestią podaną w artykuł w witrynie MSDN [obsługi błędów przejściowych][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Wywołanie metody bezpośrednio na urządzeniu
W tej sekcji służy do tworzenia aplikacji konsoli .NET, która wywołuje metodę w aplikacji symulowane urządzenie, a następnie wyświetla odpowiedzi.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania przy użyciu szablonu projektu **Aplikacja konsolowa**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **CallMethodOnDevice**.
   
    ![Nowy projekt Visual C# Windows Classic Desktop][10]
2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **CallMethodOnDevice** projektu, a następnie kliknij przycisk **Zarządzaj pakietami NuGet...** .
3. W oknie **Menedżer pakietów NuGet** wybierz opcję **Przeglądaj**, wyszukaj pozycję **microsoft.azure.devices**, wybierz opcję **Zainstaluj**, aby zainstalować pakiet **Microsoft.Azure.Devices**, i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odwołania do pakietu NuGet [zestawu SDK usługi Azure IoT][lnk-nuget-service-sdk] oraz jego zależności.
   
    ![Okno Menedżera pakietów NuGet][11]

4. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Dodaj następujące pola do klasy **Program**: Zastąp wartość symbolu zastępczego parametrami połączenia usługi IoT Hub dla centrum IoT utworzonego w poprzedniej sekcji.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Dodaj następującą metodę do klasy **Program**:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Ta metoda wywołuje metodę bezpośredniego o nazwie `writeLine` na `myDeviceId` urządzenia. Następnie zapisuje odpowiedź udostępnianym przez urządzenie na konsoli. Należy pamiętać, jak można określić wartość limitu czasu na odpowiedź urządzenia.
7. Na koniec dodaj następujące wiersze do metody **Główne**:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a>Uruchamianie aplikacji
Teraz można uruchomić aplikacje.

1. W Eksploratorze rozwiązań programu Visual Studio kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Ustaw projekty startowe...** . Wybierz **jednego projektu startowego**, a następnie wybierz **CallMethodOnDevice** projektu w menu rozwijanym.

2. W wierszu polecenia **simulateddevice** folderu, uruchom następujące polecenie, aby rozpocząć nasłuchiwania dla wywołań metod z Centrum IoT:
   
    ```
    node SimulatedDevice.js
    ```
   Poczekaj, aż symulowane urządzenie otworzyć:![][7]
3. Teraz, gdy urządzenie jest podłączone i Oczekiwanie na wywołania metody uruchamiania programu .NET **CallMethodOnDevice** aplikacji można wywołać metody w aplikacji symulowane urządzenie. Powinna zostać wyświetlona odpowiedź urządzenia zapisywane w konsoli.
   
    ![][8]
4. Wydrukowanie tej wiadomości urządzenia reaguje do metody:
   
    ![][9]

## <a name="next-steps"></a>Następne kroki
W tym samouczku opisano konfigurowanie nowego centrum IoT Hub w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tej tożsamości urządzenia są używane do włączenia aplikacji symulowane urządzenie zareagować na metody wywoływane przez chmurę. Utworzono aplikację, która wywołuje metody na urządzeniu i wyświetla odpowiedzi z urządzenia. 

Aby kontynuować wprowadzenie do usługi IoT Hub i zapoznać się z innymi scenariuszami IoT, zobacz:

* [Rozpoczynanie pracy z Centrum IoT]
* [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Aby dowiedzieć się, jak rozszerzyć IoT, Twoje rozwiązanie i harmonogram metoda wywołuje na wielu urządzeniach, zobacz [emisji zadania i harmonogramu] [ lnk-tutorial-jobs] samouczka.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Rozpoczynanie pracy z Centrum IoT]: iot-hub-node-node-getstarted.md
