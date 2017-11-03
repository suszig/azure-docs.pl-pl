---
title: "Użyj właściwości dwie urządzenia Azure IoT Hub (.NET/węzła) | Dokumentacja firmy Microsoft"
description: "Jak używać do konfigurowania urządzeń twins urządzenia Azure IoT Hub. Przy użyciu urządzenia Azure IoT SDK dla środowiska Node.js aplikacji symulowane urządzenie i usługa Azure IoT SDK dla platformy .NET do implementacji usługi aplikacji, które modyfikuje konfigurację urządzenia przy użyciu podwójnego urządzenia."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 8f9626fc47e7d32cb104b960bea9b7de9efa6f3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices"></a>Konfigurowanie urządzeń za pomocą żądanej właściwości
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na końcu tego samouczka masz dwie aplikacje konsoli:

* **SimulateDeviceConfiguration.js**, aplikację symulowane urządzenie, która oczekuje na aktualizację wymaganą konfiguracją i raportowanie stanu procesu aktualizacji konfiguracji symulowane.
* **SetDesiredConfigurationAndQuery**, aplikacji zaplecza .NET, który konfiguruje wymaganą konfiguracją na urządzeniu i zapytanie proces aktualizacji konfiguracji.

> [!NOTE]
> Artykuł [Azure IoT SDK] [ lnk-hub-sdks] informacje na temat zestawów SDK IoT Azure można tworzyć aplikacje zarówno urządzenia, jak i zaplecza.
> 
> 

Do ukończenia tego samouczka należy spełnić następujące warunki:

* Program Visual Studio 2015 lub Visual Studio 2017.
* Środowisko Node.js w wersji 4.0.x lub nowszej.
* Aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut.

Po wykonaniu [Rozpoczynanie pracy z urządzenia twins] [ lnk-twin-tutorial] samouczek, masz już Centrum IoT i tożsamość urządzenia o nazwie **myDeviceId**. W takim przypadku możesz przejść do [tworzenie aplikacji symulowane urządzenie] [ lnk-how-to-configure-createapp] sekcji.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a>Tworzenie aplikacji symulowane urządzenie
W tej sekcji zostanie utworzona aplikacja konsoli Node.js łączący się do Centrum jako **myDeviceId**, czeka na aktualizację wymaganą konfiguracją, a następnie przedstawia aktualizacje na proces aktualizacji konfiguracji symulowane.

1. Utwórz nowy, pusty folder o nazwie **simulatedeviceconfiguration**. W **simulatedeviceconfiguration** folderu, Utwórz nowy plik package.json za pomocą następującego polecenia z wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne.
   
    ```
    npm init
    ```
1. Z wiersza polecenia w **simulatedeviceconfiguration** folderu, uruchom następujące polecenie, aby zainstalować **azure iot urządzenia** i **azure-iot urządzenie mqtt** pakiety:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Za pomocą edytora tekstu, Utwórz nową **SimulateDeviceConfiguration.js** w pliku **simulatedeviceconfiguration** folderu.
1. Dodaj następujący kod do **SimulateDeviceConfiguration.js** plików i Zastąp **{ciąg połączenia urządzenia}** symbol zastępczy parametrów połączenia urządzenia został skopiowany podczas tworzenia **myDeviceId** tożsamości urządzenia:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    **Klienta** obiekt udostępnia wszystkie metody, które są wymagane do interakcji z twins urządzenia z urządzenia. Ten kod inicjuje **klienta** obiektów, pobiera dwie urządzenia dla **myDeviceId**i następnie dołącza program obsługi aktualizacji na *żądanego właściwości*. Program obsługi sprawdza, który jest żądanie zmiany konfiguracji rzeczywiste porównując configIds, a następnie wywołuje metodę, która rozpoczyna się zmian w konfiguracji.
   
    Należy pamiętać, że dla uproszczenia, ten kod używa domyślnego ustalony ukończenie początkowej konfiguracji. Rzeczywiste aplikacji będzie prawdopodobnie załadować tej konfiguracji z magazynu lokalnego.
   
   > [!IMPORTANT]
   > Zdarzenia zmiany żądanej właściwości zawsze są emitowane raz na połączenie z urządzeniem. Upewnij się sprawdzić, czy jest rzeczywistą zmianę w odpowiednich właściwościach przed wykonaniem jakiegokolwiek działania.
   > 
   > 
1. Dodaj następujące metody przed `client.open()` wywołania:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    **InitConfigChange** metody aktualizuje zgłoszone właściwości w obiekcie dwie urządzenia lokalnego żądanie aktualizacji konfiguracji i ustawia stan **oczekujące**, następnie aktualizuje na dwie urządzenia Usługa. Po pomyślnym zaktualizowaniu dwie urządzenia, jego symuluje długotrwała proces, który kończy się podczas wykonywania **completeConfigChange**. Ta metoda aktualizacji zgłoszone właściwości lokalne ustawienie stanu **Powodzenie** i usuwanie **pendingConfig** obiektu. Aktualizuje dwie urządzenia w usłudze.
   
    Pamiętaj, że, aby oszczędzić przepustowość, zgłaszany właściwości są aktualizowane, określając właściwości tylko do zmodyfikowania (o nazwie **poprawki** w powyższym kodzie), a nie zastępuje całego dokumentu.
   
   > [!NOTE]
   > W tym samouczku nie zasymulować wszystkie zachowania aktualizacji konfiguracji współbieżnych. Niektóre procesy aktualizacji konfiguracji może mieć możliwość uwzględnienia zmian konfiguracji docelowej aktualizacji jest uruchomiony, gdy niektóre może okazać się je z kolejki i niektóre można odrzucić warunek błędu. Upewnij się, że należy wziąć pod uwagę zachowanie procesu określonej konfiguracji, a następnie dodaj odpowiednie logikę przed zainicjowaniem zmian w konfiguracji.
   > 
   > 
1. Uruchamianie aplikacji urządzenia:
   
        node SimulateDeviceConfiguration.js
   
    Powinien zostać wyświetlony komunikat `retrieved device twin`. Nie zatrzymuj aplikacji.

## <a name="create-the-service-app"></a>Tworzenie aplikacji usługi
W tej sekcji utworzysz aplikacji konsoli .NET, która aktualizuje *żądanego właściwości* na dwie urządzeń skojarzonych z **myDeviceId** z obiektem konfiguracji telemetrii. Następnie odpytuje twins urządzenia przechowywane w Centrum IoT i pokazano różnicę między konfiguracji żądanego i zgłoszonego urządzenia.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania przy użyciu szablonu projektu **Aplikacja konsolowa**. Nazwij projekt **SetDesiredConfigurationAndQuery**.
   
    ![Nowy projekt Visual C# Windows Classic Desktop][img-createapp]
1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **SetDesiredConfigurationAndQuery** projektu, a następnie kliknij przycisk **Zarządzaj pakietami NuGet...** .
1. W oknie **Menedżer pakietów NuGet** wybierz opcję **Przeglądaj**, wyszukaj pozycję **microsoft.azure.devices**, wybierz opcję **Zainstaluj**, aby zainstalować pakiet **Microsoft.Azure.Devices**, i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odwołania do pakietu NuGet [zestawu SDK usługi Azure IoT][lnk-nuget-service-sdk] oraz jego zależności.
   
    ![Okno Menedżera pakietów NuGet][img-servicenuget]
1. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Dodaj następujące pola do klasy **Program**: Zastąp wartość symbolu zastępczego parametrami połączenia usługi IoT Hub dla centrum IoT utworzonego w poprzedniej sekcji.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Dodaj następującą metodę do klasy **Program**:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    **Rejestru** obiekt udostępnia wszystkie metody, które są wymagane do interakcji z twins urządzenia z usługi. Ten kod inicjuje **rejestru** obiektów, pobiera dwie urządzenia dla **myDeviceId**, a następnie aktualizuje jego żądanej właściwości z obiektem konfiguracji telemetrii.
    Po wykonaniu tej go odpytuje twins urządzenia przechowywane w Centrum IoT co 10 sekund i wyświetla konfiguracje żądaną i zaraportowanych danych telemetrycznych. Zapoznaj się [język zapytań Centrum IoT] [ lnk-query] informacje na temat generowania raportów zaawansowanych na Twoich urządzeniach.
   
   > [!IMPORTANT]
   > Ta aplikacja kwerendę Centrum IoT co 10 sekund w celach ilustracyjnych. Użyj zapytań do generowania raportów dla użytkownika na wielu urządzeniach, a nie wykrywa zmian. Jeśli rozwiązanie wymaga powiadomień w czasie rzeczywistym zdarzeń urządzenia, należy użyć [dwie powiadomienia][lnk-twin-notifications].
   > 
   > 
1. Na koniec dodaj następujące wiersze do metody **Główne**:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. W Eksploratorze rozwiązań Otwórz **projektów startowych ustawić...**  i upewnij się, że **akcji** dla **SetDesiredConfigurationAndQuery** projekt jest **Start**. Skompiluj rozwiązanie.
1. Z **SimulateDeviceConfiguration.js** uruchomiona, uruchom aplikację .NET z programu Visual Studio za pomocą **F5** i powinna zostać wyświetlona zgłoszone zmiany w konfiguracji **Powodzenie**do **oczekujące** do **Powodzenie** ponownie za pomocą nowego aktywny wysyłać częstotliwość pięć minut zamiast 24 godziny.

 ![Urządzenie zostało pomyślnie skonfigurowane][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Istnieje opóźnienie maksymalnie minuty między operacja raportu urządzenia i wyniku zapytania. To jest umożliwienie infrastruktury zapytania do pracy w bardzo dużej skali. Można pobrać spójne widoków użytkowania dwie jednego urządzenia **getDeviceTwin** metody w **rejestru** klasy.
   > 
   > 

## <a name="next-steps"></a>Następne kroki
W tym samouczku, ustaw żądaną konfiguracją jako *żądanego właściwości* z rozwiązania zaplecza i zapisano aplikacji przez urządzenia do wykrywania tej zmiany i symulowanie procesu aktualizacji wieloetapowych raportowania stanu przez wartość właściwości.

Użyj następujących zasobów, aby dowiedzieć się, jak:

* wysyłanie danych telemetrycznych z urządzenia z [Rozpoczynanie pracy z Centrum IoT] [ lnk-iothub-getstarted] samouczka
* Zaplanuj lub wykonywania operacji na dużych zestawów urządzeń, zobacz [emisji zadania i harmonogramu] [ lnk-schedule-jobs] samouczka.
* urządzenia interakcyjne (takich jak włączanie wentylator z aplikacji kontrolowane przez użytkownika) i sterować za pomocą [metody bezpośredniego] [ lnk-methods-tutorial] samouczka.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app
