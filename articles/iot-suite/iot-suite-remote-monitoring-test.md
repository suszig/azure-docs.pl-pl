---
title: "Symulacji urządzenia w rozwiązaniu monitorowania zdalnego - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób symulator urządzeń za pomocą zdalnego wstępnie skonfigurowane rozwiązanie monitorowania."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 426b7ff6114fd0b79a6af71a78705f11b80862bf
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="test-your-solution-with-simulated-devices"></a>Testowanie rozwiązania z symulowanego urządzenia

Ten samouczek przedstawia sposób użycia dostosować mikrousługi symulator urządzeń ze zdalnego wstępnie skonfigurowane rozwiązanie monitorowania. Aby pokazać możliwości symulator urządzeń dwa scenariusze w tym samouczku jest używany w aplikacji Contoso IoT.

W pierwszego scenariusza firma Contoso chce przetestować nowe urządzenie żarówka inteligentne. Aby wykonać testów, należy utworzyć nowe symulowane urządzenie o następującej charakterystyce:

*Właściwości*

| Nazwa                     | Wartości                      |
| ------------------------ | --------------------------- |
| Kolor                    | Białe, czerwony, niebieski            |
| Jasność               | 0 do 100                    |
| Szacowany pozostały okres | Odliczania w dół od 10 000 godzin |

*Telemetrii*

W poniższej tabeli przedstawiono dane ligthbulb raportów w chmurze jako strumień danych:

| Nazwa   | Wartości      |
| ------ | ----------- |
| Stan | "na" "wyłączone" |

*Metody*

W poniższej tabeli przedstawiono akcje, które obsługuje nowe urządzenie:

| Nazwa        |
| ----------- |
| Przełącz   |
| Wyłącz  |

*Stan początkowy*

W poniższej tabeli przedstawiono początkowy stan urządzenia:

| Nazwa                     | Wartości |
| ------------------------ | -------|
| Kolor początkowy            | Biały  |
| Jasność początkowej       | 75     |
| Początkowa pozostały okres   | 10 000 |
| Stan początkowy telemetrii | "on"   |

W drugi scenariusz, należy dodać nowy typ danych telemetrycznych do firmy Contoso jego istniejącą **Chłodnica** urządzenia.

W tym samouczku przedstawiono sposób symulator urządzeń za pomocą zdalnego wstępnie skonfigurowane rozwiązanie monitorowania:

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

>[!div class="checklist"]
> * Tworzenie nowego typu urządzenia
> * Symulowanie zachowanie urządzeń niestandardowych
> * Dodawanie nowego typu urządzenia do pulpitu nawigacyjnego
> * Wysyłać dane telemetryczne niestandardowych z istniejącym typem urządzenia

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy wdrożone wystąpienie zdalnego rozwiązanie monitorowania w ramach subskrypcji platformy Azure.

Jeśli jeszcze tego nie wdrożono rozwiązanie monitorowania zdalnego jeszcze, należy wykonać [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md) samouczka.

<!-- Dominic please this use as your reference https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models -->

## <a name="the-device-simulation-service"></a>Usługa symulacji urządzenia

Usługa symulacji urządzeń w rozwiązaniu wstępnie skonfigurowane umożliwia zmianę typy wbudowane symulowane urządzenie i Utwórz nowe typy symulowane urządzenie. Typy urządzeń niestandardowych służy do testowania zachowanie zdalnego rozwiązanie monitorowania przed nawiązaniem połączenia urządzenia fizycznego do rozwiązania.

## <a name="create-a-simulated-device-type"></a>Tworzenie typu symulowane urządzenie

Najprostszym sposobem tworzenia nowego typu urządzenia w symulacji mikrousługi jest na skopiowaniu i zmodyfikowaniu istniejącego typu. Poniższe kroki pokazują sposób kopiowania wbudowane **Chłodnica** urządzenia, aby utworzyć nową **żarówka** urządzenia:

1. Użyj następującego polecenia, można sklonować **symulacji urządzenia** repozytorium GitHub na komputerze lokalnym:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Dla każdego typu urządzenia jest plikiem JSON modelu i skojarzone skrypty w `Services/data/devicemodels` folderu. Kopiuj **Chłodnica** pliki, aby utworzyć **żarówka** plików, jak pokazano w poniższej tabeli:

    | Element źródłowy                      | Element docelowy                   |
    | --------------------------- | ----------------------------- |
    | Chłodnica 01.json             | żarówka 01.json             |
    | skrypty/Chłodnica-01-state.js | skrypty/żarówka-01-state.js |
    | skrypty/ponowne uruchomienie komputera method.js    | skrypty/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Zdefiniowanie nowego typu urządzenia

`lightbulb-01.json` Plik definiuje właściwości typu, takich jak dane telemetryczne generuje i obsługuje metod. Następujące kroki aktualizacji `lightbulb-01.json` pliku, aby określić **żarówka** urządzenia:

1. W `lightbulb-01.json` pliku, zaktualizuj metadane urządzenia, jak pokazano w poniższy fragment kodu:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. W `lightbulb-01.json` plików, należy zaktualizować definicję symulacji pokazane na następujący fragment kodu:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. W `lightbulb-01.json` plików, aktualizowanie właściwości typu urządzenia, jak pokazano w poniższy fragment kodu:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. W `lightbulb-01.json` pliku, aktualizację definicji telemetrii typu urządzenia, jak pokazano w poniższy fragment kodu:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "status": "text"
          }
        }
      }
    ],
    ```

1. W `lightbulb-01.json` pliku, zaktualizować metody typu urządzenia, jak pokazano w poniższy fragment kodu:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Zapisz `lightbulb-01.json` pliku.

### <a name="simulate-custom-device-behavior"></a>Symulowanie zachowanie urządzeń niestandardowych

`scripts/lightbulb-01-state.js` Plik definiuje zachowanie symulacji **żarówka** typu. Następujące kroki aktualizacji `scripts/lightbulb-01-state.js` pliku, aby określić zachowanie **żarówka** urządzenia:

1. Edytowanie definicji stanu w `scripts/lightbulb-01-state.js` plików, jak pokazano w poniższy fragment kodu:

    ```js
    // Default state
    var state = {
      online: true,
      status: "on"
    };
    ```

1. Zastąp **różnią się** funkcji z następującymi **Przerzuć** funkcji:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Edytuj **głównego** funkcji, aby zaimplementować zachowanie, jak pokazano w poniższy fragment kodu:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Zapisz `scripts/lightbulb-01-state.js` pliku.

`scripts/SwitchOn-method.js` Pliku implementuje **przełącznika na** metody w **żarówka** urządzenia. Następujące kroki aktualizacji `scripts/SwitchOn-method.js` pliku:

1. Edytowanie definicji stanu w `scripts/SwitchOn-method.js` plików, jak pokazano w poniższy fragment kodu:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Aby włączyć żarówka, należy edytować **głównego** działają w następujący sposób:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Zapisz `scripts/SwitchOn-method.js` pliku.

1. Utwórz kopię `scripts/SwitchOn-method.js` plik o nazwie `scripts/SwitchOff-method.js`.

1. Aby wyłączyć żarówka, należy edytować **głównego** działać w `scripts/SwitchOff-method.js` plików w następujący sposób:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Zapisz `scripts/SwitchOff-method.js` pliku.

### <a name="test-the-lightbulb-device-type"></a>Typ urządzenia żarówka testu

Aby przetestować **żarówka** typu urządzenia, należy najpierw przetestować danego typu urządzenia działa zgodnie z oczekiwaniami, uruchamiając lokalną kopię **symulacji urządzenia** usługi. Po przetestowane i debugować lokalnie nowego typu urządzenia należy odbudować kontenera i wdrożenie **symulacji urządzenia** usługi na platformie Azure.

Do testowania i debugowania zmiany lokalnie, zobacz [omówienie symulacji urządzenia](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md).

Konfigurowanie projektu, aby skopiować nowe **żarówka** plików urządzeń do katalogu wyjściowego.

Aby przetestować nowe urządzenie w rozwiązaniu wdrożone, zobacz jedną z opcji:

* [Wdrażanie kontenerów z konta niestandardowego Centrum docker](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Aktualizacja kontenera wdrożone za pośrednictwem ręcznego kopiowania](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Na **urządzeń** strony, można udostępnić wystąpienia nowego typu:

![Umożliwia wyświetlenie listy dostępnych symulacje](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Można wyświetlić dane telemetryczne z symulowane urządzenie:

![Dane telemetryczne wyświetleń żarówka](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Możesz wywołać **SwitchOn** i **SwitchOff** metod na urządzeniu:

![Wywołanie metody żarówka](media/iot-suite-remote-monitoring-test/devicesmethods.png)

Aby utworzyć obraz Docker z nowym typem urządzenia do wdrożenia na platformie Azure, zobacz [tworzenia dostosowanego obrazu Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="add-a-new-telemetry-type"></a>Dodaj nowy typ telemetrii

Ta sekcja opisuje sposób modyfikowania istniejącego typu symulowane urządzenie na potrzeby obsługi nowego typu danych telemetrycznych.

### <a name="locate-the-chiller-device-type-files"></a>Zlokalizuj pliki Chłodnica typ urządzenia

Następujących krokach przedstawiono sposób wyszukiwania plików, które definiują wbudowane **Chłodnica** urządzenia:

1. Jeśli nie zostało to jeszcze zrobione, użyj następującego polecenia, można sklonować **symulacji urządzenia** repozytorium GitHub na komputerze lokalnym:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Dla każdego typu urządzenia jest plikiem JSON modelu i skojarzone skrypty w `Services/data/devicemodels` folderu. Plików, które definiują symulowane **Chłodnica** są typu urządzenia:
    * `Services/data/devicemodels/chiller-01.json`
    * `Services/data/devicemodels/scripts/chiller-01-state.js`

### <a name="specify-the-new-telemetry-type"></a>Określ nowy typ telemetrii

Następujących krokach przedstawiono sposób dodawania nowego **temperatury wewnętrznej** typ **Chłodnica** typu urządzenia:

1. Otwórz plik `chiller-01.json`.

1. Aktualizacja **SchemaVersion** wartości w następujący sposób:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. W **stan początkowy** Dodaj definicje Oto dwa:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. W **Telemetrii** tablicy, dodaj następującą definicję:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Zapisz `chiller-01.json` pliku.

1. Otwórz plik `scripts/chiller-01-state.js`.

1. Dodaj następujące pola do **stanu** zmienną:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Dodaj następujący wiersz do **głównego** funkcji:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Zapisz `scripts/chiller-01-state.js` pliku.

### <a name="test-the-chiller-device-type"></a>Typ urządzenia Chłodnica testu

Aby przetestować zaktualizowane **Chłodnica** typu urządzenia, należy najpierw przetestować danego typu urządzenia działa zgodnie z oczekiwaniami, uruchamiając lokalną kopię **symulacji urządzenia** usługi. Po przetestowane i debugować danego typu urządzenia zaktualizowane lokalnie, należy odbudować kontenera i wdrożenie **symulacji urządzenia** usługi na platformie Azure.

Po uruchomieniu **symulacji urządzenia** usługi lokalnie, wysyła dane telemetryczne do zdalnego rozwiązanie monitorowania. Na **urządzeń** strony, można udostępnić wystąpienia typu zaktualizowane.

Do testowania i debugowania zmiany lokalnie, zobacz [uruchomiona usługa z programem Visual Studio](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#running-the-service-with-visual-studio) lub [skompilować i uruchomić z wiersza polecenia](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#build-and-run-from-the-command-line).

Aby przetestować nowe urządzenie w rozwiązaniu wdrożone, zobacz jedną z opcji:

* [Wdrażanie kontenerów z konta niestandardowego Centrum docker](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Aktualizacja kontenera wdrożone za pośrednictwem ręcznego kopiowania](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Na **urządzeń** strony, można udostępnić wystąpienia typu zaktualizowane:

![Dodaj Chłodnica zaktualizowane](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Możesz wyświetlić nowy **temperatury wewnętrznej** dane telemetryczne z symulowane urządzenie.

Aby utworzyć obraz Docker z nowym typem urządzenia do wdrożenia na platformie Azure, zobacz [tworzenia dostosowanego obrazu Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono należy jak do:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Tworzenie nowego typu urządzenia
> * Symulowanie zachowanie urządzeń niestandardowych
> * Dodawanie nowego typu urządzenia do pulpitu nawigacyjnego
> * Wysyłać dane telemetryczne niestandardowych z istniejącym typem urządzenia

Teraz wiesz jak używać usługi symulacji urządzenia, sugerowane następnym krokiem jest Dowiedz się, jak [fizyczne urządzenie podłączone do zdalnego rozwiązanie monitorowania](iot-suite-connecting-devices-node.md).

Aby uzyskać więcej informacji o deweloperów o zdalnego rozwiązanie monitorowania zobacz:

* [Przewodnik informacyjny dla deweloperów](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Przewodnik po rozwiązywaniu problemów dla deweloperów](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->