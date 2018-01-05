---
title: "Budowy modułu w usłudze Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co jest umieszczane w modułach Azure IoT krawędzi i jak można ponownie"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f3bc2f14b182e502c651ff44ef49b88cd34e1f50
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Skonfigurowane, zrozumieć, jak moduły krawędzi IoT mogą być używane i ponownie - preview

Azure IoT krawędzi umożliwia utworzenie wielu modułów krawędzi IoT przed ich wdrożeniem na urządzeniach krawędzi IoT. W tym artykule opisano najważniejsze pojęcia wokół redagowania wiele modułów krawędzi IoT przed przystąpieniem do wdrożenia. 

## <a name="the-deployment-manifest"></a>Manifest rozmieszczenia
*Manifest rozmieszczenia* jest dokumentem JSON, który opisuje:

* Które moduły krawędzi IoT ma zostać wdrożony, wraz z ich tworzenie i opcje zarządzania;
* Konfiguracja koncentratora krawędzi, opisujące, jak komunikaty powinien przepływ między modułami i między modułami i Centrum IoT;
* Opcjonalnie wartości, które można ustawić we właściwościach żądaną twins modułu do konfigurowania aplikacji pojedynczego modułu.

W samouczkach Azure IoT krawędzi tworzenia manifest rozmieszczenia za pośrednictwem kreatora w portalu Azure IoT krawędzi. Można także zastosować dla manifest wdrażania, programowo przy użyciu przerwę lub zestawu SDK usług Centrum IoT. Zapoznaj się [wdrażanie i monitorowanie] [ lnk-deploy] uzyskać więcej informacji o wdrożeniach IoT krawędzi.

Na wysokim poziomie manifest wdrażania konfiguruje odpowiednie właściwości modułów krawędzi IoT wdrożone na urządzeniu IoT krawędzi. Zawsze występują dwa z tych modułów: agent krawędzi i koncentrator krawędzi.

Manifest tej struktury są następujące:

    {
        "moduleContent": {
            "$edgeAgent": {
                "properties.desired": {
                    // desired properties of the Edge agent
                }
            },
            "$edgeHub": {
                "properties.desired": {
                    // desired properties of the Edge hub
                }
            },
            "{module1}": {  // optional
                "properties.desired": {
                    // desired properties of module with id {module1}
                }
            },
            "{module2}": {  // optional
                ...
            },
            ...
        }
    }

Przykład manifest rozmieszczenia jest zgłaszany na końcu tej sekcji.

> [!IMPORTANT]
> Wszystkie urządzenia IoT krawędzi, należy skonfigurować z manifestu wdrożenia. Nowo zainstalowany moduł wykonawczy krawędzi IoT raporty do czasu skonfigurowania z prawidłowym manifestem kod błędu. 

### <a name="specify-the-modules"></a>Określ modułów
Żądane właściwości dwie modułu agenta krawędzi zawierają: żądany modułów, opcje konfiguracji i zarządzania, oraz parametry konfiguracji agenta krawędzi.

Na wysokim poziomie ta sekcja manifestu zawiera odwołania do modułu obrazów i opcje zarządzania dla modułów środowiska uruchomieniowego IoT krawędzi (krawędź agent i koncentrator krawędzi) i moduły określone przez użytkownika.

Zapoznaj się [żądanego właściwości agenta krawędzi] [ lnk-edgeagent-desired] szczegółowy opis tej sekcji manifestu.

> [!NOTE]
> Manifest wdrażania, zawierający tylko krawędzi IoT środowiska uruchomieniowego (agenta i koncentrator) jest nieprawidłowy.


### <a name="specify-the-routes"></a>Podaj trasy
Koncentrator krawędzi zapewnia sposób deklaratywnego tras wiadomości między modułami i między modułami i Centrum IoT.

Trasy ma następującą składnię:

        FROM <source>
        [WHERE <condition>]
        INTO <sink>

*Źródła* może być dowolny z następujących czynności:

| Element źródłowy | Opis |
| ------ | ----------- |
| `/*` | Wszystkie komunikaty urządzenia do chmury z każdego urządzenia lub modułu |
| `/messages/*` | Dowolny komunikat urządzenia do chmury wysyłanych przez urządzenia, czy moduł za pomocą niektórych lub nie danych wyjściowych |
| `/messages/modules/*` | Dowolny komunikat urządzenia do chmury wysyłane przez moduł za pomocą niektórych lub nie danych wyjściowych |
| `/messages/modules/{moduleId}/*` | Dowolny komunikat urządzenia do chmury wysyłane przez {moduleId} z żadnych danych wyjściowych |
| `/messages/modules/{moduleId}/outputs/*` | Dowolny komunikat urządzenia do chmury wysyłane przez {moduleId}, przy czym niektóre dane wyjściowe |
| `/messages/modules/{moduleId}/outputs/{output}` | Dowolny komunikat urządzenia do chmury wysyłany przy użyciu {moduleId} {wyjściowych} |

Warunek może być warunki obsługiwane przez [język zapytań Centrum IoT] [ lnk-iothub-query] dla reguły routingu Centrum IoT.

Obiekt sink może być jedną z następujących czynności:

| Ujście | Opis |
| ---- | ----------- |
| `$upstream` | Wysyła komunikat do Centrum IoT |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Wyślij wiadomość do wprowadzania `{input}` modułu`{moduleId}` |

Należy pamiętać, że Centrum krawędzi zapewnia na najmniej jednokrotne gwarancje, co oznacza, że komunikaty będą przechowywane lokalnie w przypadku trasy nie może dostarczyć wiadomości do jego odbioru, np. Centrum krawędzi nie może połączyć się Centrum IoT lub moduł docelowy nie jest połączona.

Koncentrator krawędzi przechowuje komunikaty do przez czas określony w `storeAndForwardConfiguration.timeToLiveSecs` właściwości Centrum krawędzi żądanego właściwości.

### <a name="specifying-the-desired-properties-of-the-module-twin"></a>Określanie odpowiednie właściwości dwie modułu

Manifest wdrażania można określić odpowiednie właściwości dwie modułu każdego użytkownika modułów określonych w sekcji agent krawędzi.

Jeśli odpowiednie właściwości są określone w manifeście rozmieszczenia, zastępują wszystkie odpowiednie właściwości aktualnie w dwie modułu.

Jeśli nie określisz dwie modułu odpowiednie właściwości w manifeście rozmieszczenia, Centrum IoT nie zmodyfikuje dwie modułu w jakikolwiek sposób i będzie można programowo Ustaw odpowiednie właściwości.

### <a name="deployment-manifest-example"></a>Przykład manifestu wdrożenia

Ten przykład dokumentu JSON manifestu wdrożenia.

    {
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                "schemaVersion": "1.0",
                "runtime": {
                    "type": "docker",
                    "settings": {
                        "minDockerVersion": "v1.25",
                        "loggingOptions": ""
                    }
                },
                "systemModules": {
                    "edgeAgent": {
                        "type": "docker",
                        "settings": {
                        "image": "microsoft/azureiotedge-agent:1.0-preview",
                        "createOptions": ""
                        }
                    },
                    "edgeHub": {
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "microsoft/azureiotedge-hub:1.0-preview",
                        "createOptions": ""
                        }
                    }
                },
                "modules": {
                    "tempSensor": {
                        "version": "1.0",
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                        "createOptions": "{}"
                        }
                    },
                    "filtermodule": {
                        "version": "1.0",
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "myacr.azurecr.io/filtermodule:latest",
                        "createOptions": "{}"
                        }
                    }
                }
            }
        },
        "$edgeHub": {
            "properties.desired": {
                "schemaVersion": "1.0",
                "routes": {
                    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
                    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
                },
                "storeAndForwardConfiguration": {
                    "timeToLiveSecs": 10
                }
            }
        }
    }
    }

## <a name="reference-edge-agent-module-twin"></a>Odwołanie: Dwie modułu agenta krawędzi

Dwie modułu dla agenta krawędzi jest nazywany `$edgeAgent` i koordynuje komunikacji między agentem krawędzi uruchomionej na urządzeniu i Centrum IoT.
Żądane właściwości są ustawiane podczas stosowania manifest wdrażania na urządzeniach określonych jako część wdrożenia jednym urządzeniu lub na dużą skalę. Zobacz [wdrażania i monitorowania] [ lnk-deploy] Aby uzyskać więcej informacji na temat wdrażania modułów na urządzenia brzegowe IoT.

### <a name="edge-agent-twin-desired-properties"></a>Krawędź agent potrzebne dwie właściwości

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| Atrybut schemaVersion | Musi być "1.0" | Yes |
| Runtime.Type | Musi być "docker" | Yes |
| runtime.settings.minDockerVersion | Wartość minimalna wersja platformy Docker wymaganych przez ten manifest wdrażania | Yes |
| runtime.settings.loggingOptions | Stringified JSON, zawierające opcje rejestrowania dla kontenera agenta krawędzi. [Opcje rejestrowania platformy docker][lnk-docker-logging-options] | Nie |
| systemModules.edgeAgent.type | Musi być "docker" | Yes |
| systemModules.edgeAgent.settings.image | Identyfikator URI agenta krawędzi obrazu. Krawędź agent nie jest obecnie mogli automatycznie zaktualizowana. | Yes |
| systemModules.edgeAgent.settings.createOptions | Stringified JSON, zawierające opcje tworzenia kontenera agenta krawędzi. [Opcje tworzenia docker][lnk-docker-create-options] | Nie |
| systemModules.edgeAgent.configuration.id | Identyfikator wdrożenia, które wdrożyć ten moduł. | Zostało to określone przez Centrum IoT, po zastosowaniu tego manifestu przy użyciu wdrożenia. Nie jest częścią manifest wdrażania. |
| systemModules.edgeHub.type | Musi być "docker" | Yes |
| systemModules.edgeHub.status | Musi być "uruchomiona" | Yes |
| systemModules.edgeHub.restartPolicy | Musi być "zawsze" | Yes |
| systemModules.edgeHub.settings.image | Identyfikator URI obrazu Centrum krawędzi. | Yes |
| systemModules.edgeHub.settings.createOptions | Stringified JSON, zawierające opcje tworzenia kontenera Centrum krawędzi. [Opcje tworzenia docker][lnk-docker-create-options] | Nie |
| systemModules.edgeHub.configuration.id | Identyfikator wdrożenia, które wdrożyć ten moduł. | Zostało to określone przez Centrum IoT, po zastosowaniu tego manifestu przy użyciu wdrożenia. Nie jest częścią manifest wdrażania. |
| moduły. {moduleId} .version | Zdefiniowane przez użytkownika ciąg reprezentujący wersję tego modułu. | Yes |
| moduły. .type {moduleId} | Musi być "docker" | Yes |
| moduły. .restartPolicy {moduleId} | {"nigdy" \| "-nie powiodło się" \| "na — zła" \| "zawsze"} | Yes |
| moduły. .settings.image {moduleId} | Identyfikator URI do obrazu modułu. | Yes |
| moduły. .settings.createOptions {moduleId} | Stringified JSON, zawierające opcje tworzenia kontenera modułu. [Opcje tworzenia docker][lnk-docker-create-options] | Nie |
| moduły. .configuration.id {moduleId} | Identyfikator wdrożenia, które wdrożyć ten moduł. | Zostało to określone przez Centrum IoT, po zastosowaniu tego manifestu przy użyciu wdrożenia. Nie jest częścią manifest wdrażania. |

### <a name="edge-agent-twin-reported-properties"></a>Krawędź agent dwie zgłoszone właściwości

Agent krawędzi zgłosił, że właściwości obejmują trzy główne informacje:

1. Stan stosowania widoczne ostatnie odpowiednie właściwości;
2. Stan modułów aktualnie uruchomionej na urządzeniu, zgłoszonej przez agenta krawędzi; i
3. Kopia odpowiednie właściwości, które są aktualnie uruchomione na urządzeniu.

Ten ostatni element informacji jest przydatne w przypadku najnowszych odpowiednie właściwości nie są stosowane pomyślnie w czasie wykonywania, a urządzenie jest nadal uruchomiona poprzedniej manifest wdrażania.

> [!NOTE]
> Zgłoszony właściwości agenta krawędzi są przydatne, jak mogą być przeszukiwane przy [język zapytań Centrum IoT] [ lnk-iothub-query] do badania stanu wdrożeń na dużą skalę. Zapoznaj się [wdrożeń] [ lnk-deploy] Aby uzyskać więcej informacji na temat korzystania z tej funkcji.

Poniższa tabela nie zawiera informacje, które jest kopiowana żądanej właściwości.

| Właściwość | Opis |
| -------- | ----------- |
| lastDesiredVersion | Ta int odwołuje się do ostatniej wersji odpowiednie właściwości, które są przetwarzane przez agenta krawędzi. |
| lastDesiredStatus.code | To jest kod stanu odwołujących się do ostatniego odpowiednie właściwości zostały odebrane przez agenta krawędzi. Dozwolone wartości: `200` sukces, `400` nieprawidłową konfigurację `412` wersji nieprawidłowy schemat `417` odpowiednie właściwości są puste, `500` nie powiodło się |
| lastDesiredStatus.description | Opis stanu |
| DeviceHealth | `healthy`Jeśli stan środowiska uruchomieniowego dla wszystkich modułów `running` lub `stopped`, `unhealthy` inaczej |
| configurationHealth. .health {deploymentId} | `healthy`Jeśli stan środowiska uruchomieniowego dla wszystkich modułów ustawione przez wdrożenie {deploymentId} `running` lub `stopped`, `unhealthy` inaczej |
| runtime.platform.OS | Raportowanie systemu operacyjnego działającej na urządzeniu |
| Runtime.platform.Architecture | Raportowanie architektury procesora na urządzeniu |
| systemModules.edgeAgent.runtimeStatus | Zgłaszany stan agenta krawędzi: {"uruchomiona" \| "nieprawidłowy"} |
| systemModules.edgeAgent.statusDescription | Opis tekstowy zgłaszany stan agenta krawędzi. |
| systemModules.edgeHub.runtimeStatus | Bieżący stan koncentratora krawędzi: {"uruchomiona" \| 'stopped' \| "nie powiodło się" \| "wycofywania" \| "nieprawidłowy"} |
| systemModules.edgeHub.statusDescription | Opis tekstowy przy bieżącym stanie krawędzi koncentratora, jeśli złej kondycji. |
| systemModules.edgeHub.exitCode | Jeśli został zakończony, kod zakończenia zgłoszone przez kontener Centrum krawędzi |
| systemModules.edgeHub.startTimeUtc | Godzina ostatniego uruchomienia Centrum krawędzi |
| systemModules.edgeHub.lastExitTimeUtc | Czas, po Centrum krawędź ostatniego zakończony |
| systemModules.edgeHub.lastRestartTimeUtc | Czas, gdy Centrum krawędź ostatniego ponownego uruchomienia |
| systemModules.edgeHub.restartCount | Ile razy ten moduł został ponownie uruchomiony jako część zasada ponownego uruchamiania. |
| moduły. .runtimeStatus {moduleId} | Bieżący stan modułu: {"uruchomiona" \| 'stopped' \| "nie powiodło się" \| "wycofywania" \| "nieprawidłowy"} |
| moduły. .statusDescription {moduleId} | Opis tekstowy przy bieżącym stanie modułu, jeśli jest zła. |
| moduły. .exitCode {moduleId} | Jeśli został zakończony, kod zakończenia zgłaszane przez moduł kontenera |
| moduły. .startTimeUtc {moduleId} | Godzina ostatniego uruchomienia modułu |
| moduły. .lastExitTimeUtc {moduleId} | Czas, gdy moduł ostatnio zakończony |
| moduły. .lastRestartTimeUtc {moduleId} | Czas, gdy moduł ostatniego ponownego uruchomienia |
| moduły. .restartCount {moduleId} | Ile razy ten moduł został ponownie uruchomiony jako część zasada ponownego uruchamiania. |

## <a name="reference-edge-hub-module-twin"></a>Odwołanie: Dwie modułu Centrum krawędzi

Dwie modułu dla koncentratora krawędzi jest nazywany `$edgeHub` i koordynuje komunikacji między centrum krawędzi uruchomionej na urządzeniu i Centrum IoT.
Żądane właściwości są ustawiane podczas stosowania manifest wdrażania na urządzeniach określonych jako część wdrożenia jednym urządzeniu lub na dużą skalę. Zobacz [wdrożeń] [ lnk-deploy] Aby uzyskać więcej informacji na temat wdrażania modułów na urządzenia brzegowe IoT.

### <a name="edge-hub-twin-desired-properties"></a>Krawędź Centrum potrzebne dwie właściwości

| Właściwość | Opis | Wymagane w manifeście rozmieszczenia |
| -------- | ----------- | -------- |
| Atrybut schemaVersion | Musi być "1.0" | Yes |
| trasy. {routeName} | Ciąg reprezentujący trasę koncentratora krawędzi. | `routes` Element może być istnieje, ale jest pusty. |
| storeAndForwardConfiguration.timeToLiveSecs | Czas w sekundach, które przechowuje krawędzi Centrum wiadomości w przypadku odłączonych routingu punktów końcowych, np. odłączony od centrum IoT lub lokalnego modułu | Yes |

### <a name="edge-hub-twin-reported-properties"></a>Dwie Centrum krawędzi zgłosił właściwości

| Właściwość | Opis |
| -------- | ----------- |
| lastDesiredVersion | Ta int odwołuje się do ostatniej wersji odpowiednie właściwości, które są przetwarzane przez Centrum krawędzi. |
| lastDesiredStatus.code | To jest kod stanu odwołujących się do ostatniego odpowiednie właściwości zostały odebrane przez Centrum krawędzi. Dozwolone wartości: `200` sukces, `400` nieprawidłową konfigurację `500` nie powiodło się |
| lastDesiredStatus.description | Opis stanu |
| Klienci. .status {tożsamość urządzenia lub moduł} | Stan połączenia to urządzenie lub modułu. Możliwe wartości {"połączone" \| "odłączony"}. Tylko moduł tożsamości mogą być w stanie rozłączenia. Podrzędne urządzeń nawiązujących połączenie z koncentratorem krawędzi są wyświetlane tylko w przypadku połączenia. |
| Klienci. .lastConnectTime {tożsamość urządzenia lub moduł} | Czas ostatniego urządzenia lub moduł połączone |
| Klienci. .lastDisconnectTime {tożsamość urządzenia lub moduł} | Czas ostatniego urządzenia lub moduł odłączona |

## <a name="next-steps"></a>Kolejne kroki

Teraz, gdy wiesz, jak są używane moduły krawędzi IoT, [zrozumieć wymagania i narzędzi do tworzenia modułów krawędzi IoT][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-edgeagent-desired]: #edge-agent-twin-desired-properties
[lnk-edgeagent-reported]: #edge-agent-twin-reported-properties
[lnk=edgehub-desired]: #edge-hub-twin-desired-properties
[lnk-edgehub-reported]: #edge-hub-twin-reported-properties
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
