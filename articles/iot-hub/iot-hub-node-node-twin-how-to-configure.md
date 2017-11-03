---
title: "Użyj właściwości dwie urządzenia Azure IoT Hub (węzeł) | Dokumentacja firmy Microsoft"
description: "Jak używać do konfigurowania urządzeń twins urządzenia Azure IoT Hub. Przy użyciu zestawów SDK IoT Azure dla środowiska Node.js aplikacji symulowane urządzenie i aplikacji usługi, który modyfikuje konfigurację urządzenia przy użyciu podwójnego urządzenia."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: elioda
ms.openlocfilehash: 6ff6f1c331d5a77e7ac0a47af6806f5d90fb0fdc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a>Żądane właściwości umożliwiają konfigurowanie urządzeń (węzeł)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Na końcu tego samouczka masz dwie aplikacje konsoli Node.js:

* **SimulateDeviceConfiguration.js**, aplikację symulowane urządzenie, która oczekuje na aktualizację wymaganą konfiguracją i raportowanie stanu procesu aktualizacji konfiguracji symulowane.
* **SetDesiredConfigurationAndQuery.js**, aplikacji zaplecza Node.js, który konfiguruje wymaganą konfiguracją na urządzeniu i zapytanie proces aktualizacji konfiguracji.

> [!NOTE]
> Artykuł [Azure IoT SDK] [ lnk-hub-sdks] informacje na temat zestawów SDK IoT Azure można tworzyć aplikacje zarówno urządzenia, jak i zaplecza.
> 
> 

Do ukończenia tego samouczka należy spełnić następujące warunki:

* Środowisko Node.js w wersji 4.0.x lub nowszej.
* Aktywne konto platformy Azure. (Jeśli go nie masz, możesz utworzyć [bezpłatne konto próbne][lnk-free-trial] w zaledwie kilka minut).

Po wykonaniu [Rozpoczynanie pracy z urządzenia twins] [ lnk-twin-tutorial] samouczek, masz już Centrum IoT i tożsamość urządzenia o nazwie **myDeviceId**; i można przejść do [tworzenie aplikacji symulowane urządzenie] [ lnk-how-to-configure-createapp] sekcji.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>Tworzenie aplikacji symulowane urządzenie
W tej sekcji zostanie utworzona aplikacja konsoli Node.js łączący się do Centrum jako **myDeviceId**, czeka na aktualizację wymaganą konfiguracją, a następnie przedstawia aktualizacje na proces aktualizacji konfiguracji symulowane.

1. Utwórz nowy, pusty folder o nazwie **simulatedeviceconfiguration**. W **simulatedeviceconfiguration** folderu, Utwórz nowy plik package.json za pomocą następującego polecenia z wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z wiersza polecenia w **simulatedeviceconfiguration** folderu, uruchom następujące polecenie, aby zainstalować **azure iot urządzenia**, i **azure-iot urządzenie mqtt** pakietu:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Za pomocą edytora tekstu, Utwórz nową **SimulateDeviceConfiguration.js** w pliku **simulatedeviceconfiguration** folderu.
4. Dodaj następujący kod do **SimulateDeviceConfiguration.js** plików i Zastąp **{ciąg połączenia urządzenia}** symbol zastępczy parametrów połączenia urządzenia został skopiowany podczas tworzenia **myDeviceId** tożsamości urządzenia:
   
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
   
    **Klienta** obiekt udostępnia wszystkie metody, które są wymagane do interakcji z twins urządzenia z urządzenia. Poprzedni kod po jego inicjuje **klienta** obiektów, pobiera dwie urządzenia dla **myDeviceId**i dołącza program obsługi aktualizacji na odpowiednich właściwościach. Program obsługi sprawdza, który jest żądanie zmiany konfiguracji rzeczywiste porównując configIds, a następnie wywołuje metodę, która rozpoczyna się zmian w konfiguracji.
   
    Należy pamiętać, że dla uproszczenia, poprzedni kod używa domyślnego ustalony ukończenie początkowej konfiguracji. Rzeczywiste aplikacji będzie prawdopodobnie załadować tej konfiguracji z magazynu lokalnego.
   
   > [!IMPORTANT]
   > Zdarzenia zmiany żądanej właściwości zawsze są emitowane raz na połączenie z urządzeniem, upewnij się sprawdzić, czy jest rzeczywistą zmianę w odpowiednich właściwościach przed wykonaniem jakiegokolwiek działania.
   > 
   > 
5. Dodaj następujące metody przed `client.open()` wywołania:
   
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
   
    **InitConfigChange** metody aktualizuje zgłoszone właściwości obiektu dwie urządzenia lokalnego żądanie aktualizacji konfiguracji i ustawia stan **oczekujące**, następnie aktualizuje dwie urządzenia w usłudze. Po pomyślnym zaktualizowaniu dwie urządzenia, jego symuluje długotrwała proces, który kończy się podczas wykonywania **completeConfigChange**. Ta metoda aktualizuje właściwości zgłoszone dwie urządzenia lokalnego ustawienie stanu **Powodzenie** i usuwanie **pendingConfig** obiektu. Aktualizuje dwie urządzenia w usłudze.
   
    Pamiętaj, że, aby oszczędzić przepustowość, zgłaszany właściwości są aktualizowane, określając właściwości tylko do zmodyfikowania (o nazwie **poprawki** w powyższym kodzie), a nie zastępuje całego dokumentu.
   
   > [!NOTE]
   > W tym samouczku nie zasymulować wszystkie zachowania aktualizacji konfiguracji współbieżnych. Niektóre procesy aktualizacji konfiguracji może mieć możliwość uwzględnienia zmian konfiguracji docelowej aktualizacji jest uruchomiony, gdy inne osoby mogły je z kolejki i innych użytkowników można odrzucić warunek błędu. Upewnij się, że należy wziąć pod uwagę zachowanie procesu określonej konfiguracji, a następnie dodaj odpowiednie logikę przed zainicjowaniem zmian w konfiguracji.
   > 
   > 
6. Uruchamianie aplikacji urządzenia:
   
        node SimulateDeviceConfiguration.js
   
    Powinien zostać wyświetlony komunikat `retrieved device twin`. Nie zatrzymuj aplikacji.

## <a name="create-the-service-app"></a>Tworzenie aplikacji usługi
W tej sekcji utworzysz aplikację konsoli Node.js, która aktualizuje *żądanego właściwości* na dwie urządzeń skojarzonych z **myDeviceId** z obiektem konfiguracji telemetrii. Następnie odpytuje twins urządzenia przechowywane w Centrum IoT i pokazano różnicę między konfiguracji żądanego i zgłoszonego urządzenia.

1. Utwórz nowy, pusty folder o nazwie **setdesiredandqueryapp**. W **setdesiredandqueryapp** folderu, Utwórz nowy plik package.json za pomocą następującego polecenia z wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z wiersza polecenia w **setdesiredandqueryapp** folderu, uruchom następujące polecenie, aby zainstalować **Centrum iothub azure** pakietu:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Za pomocą edytora tekstu, Utwórz nową **SetDesiredAndQuery.js** w pliku **setdesiredandqueryapp** folderu.
4. Dodaj następujący kod do **SetDesiredAndQuery.js** plików i Zastąp **{parametry połączenia Centrum iot}** symbol zastępczy parametrów połączenia Centrum IoT skopiowane podczas tworzenia Centrum:
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    **Rejestru** obiekt udostępnia wszystkie metody, które są wymagane do interakcji z twins urządzenia z usługi. Poprzedni kod po jego inicjuje **rejestru** obiektów, pobiera dwie urządzenia dla **myDeviceId**i aktualizuje jego właściwości żądany nowy obiekt konfiguracji telemetrii. Po wykonaniu tej wywołuje **queryTwins** funkcji zdarzeń 10 sekund.

    > [!IMPORTANT]
    > Ta aplikacja kwerendę Centrum IoT co 10 sekund w celach ilustracyjnych. Użyj zapytań do generowania raportów dla użytkownika na wielu urządzeniach, a nie wykrywa zmian. Jeśli rozwiązanie wymaga powiadomień w czasie rzeczywistym zdarzeń urządzenia, należy użyć [dwie powiadomienia][lnk-twin-notifications].
    > 
    >.

1. Dodaj następujący kod uprawnienia przed `registry.getDeviceTwin()` wywołania do zaimplementowania **queryTwins** funkcji:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    Poprzednich zapytań kod twins urządzenia przechowywane w Centrum IoT i wyświetla konfiguracje żądaną i zaraportowanych danych telemetrycznych. Zapoznaj się [język zapytań Centrum IoT] [ lnk-query] informacje na temat generowania raportów zaawansowanych na Twoich urządzeniach.
2. Z **SimulateDeviceConfiguration.js** uruchomiona, uruchom aplikację klawiszem:
   
        node SetDesiredAndQuery.js 5m
   
    Powinny pojawić się zgłoszone zmiany w konfiguracji **Powodzenie** do **oczekujące** do **Powodzenie** ponownie za pomocą nowego aktywny wysyłać częstotliwość pięć minut zamiast 24 godziny.
   
   > [!IMPORTANT]
   > Istnieje opóźnienie maksymalnie minuty między operacja raportu urządzenia i wyniku zapytania. To jest umożliwienie infrastruktury zapytania do pracy w bardzo dużej skali. Można pobrać spójne widoków użytkowania dwie jednego urządzenia **getDeviceTwin** metody w **rejestru** klasy.
   > 
   > 

## <a name="next-steps"></a>Następne kroki
W tym samouczku, ustaw żądaną konfiguracją jako *żądanego właściwości* z poziomu aplikacji zaplecza i zapisano aplikacji symulowane urządzenie, aby wykryć zmiana i symulowanie procesu aktualizacji wieloetapowych raportowania stanu jako *zgłosił właściwości* na dwie urządzenia.

Użyj następujących zasobów, aby dowiedzieć się, jak:

* wysyłanie danych telemetrycznych z urządzenia z [Rozpoczynanie pracy z Centrum IoT] [ lnk-iothub-getstarted] samouczka
* Zaplanuj lub wykonywania operacji na dużych zestawów urządzeń, zobacz [emisji zadania i harmonogramu] [ lnk-schedule-jobs] samouczka.
* urządzenia interakcyjne (takich jak włączanie wentylator z aplikacji kontrolowane przez użytkownika) i sterować za pomocą [metody bezpośredniego] [ lnk-methods-tutorial] samouczka.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

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
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
