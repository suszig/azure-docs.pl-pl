---
title: "Utwórz moduł Azure IoT krawędzi za pomocą języka Node.js | Dokumentacja firmy Microsoft"
description: "Jak napisać cz modułu konwertera danych przy użyciu najnowszych pakietów NPM krawędzi IoT Azure i narzędzia Yeoman których są wyświetlane w tym samouczku generatora."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: e23c4aa7bb3eb4fab18d5a13cbad28e07c18d8f2
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Utwórz moduł Azure IoT krawędzi za pomocą języka Node.js

W tym samouczku ilustrację sposobu tworzenia modułu Azure IoT Edge w JS.

W tym samouczku będziemy przeprowadzenie konfiguracji środowiska i jak napisać [cz](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) modułu konwertera danych przy użyciu najnowszych pakietów NPM krawędzi IoT Azure.

## <a name="prerequisites"></a>Wymagania wstępne

W tej sekcji służy do konfigurowania środowiska do tworzenia modułu IoT krawędzi. Dotyczy ona obu *64-bitowym systemie Windows* i *64-bitowego systemu Linux (Ubuntu) 14 +* systemów operacyjnych.

Następujące oprogramowanie jest wymagane:
* [Klient Git](https://git-scm.com/downloads).
* [Węzeł LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architektura

Platforma Azure IoT krawędzi silnie przyjmuje [architektura Neumanna Von](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Co oznacza, że całej architektury krawędzi IoT Azure to system, który przetwarza dane wejściowe i generuje dane wyjściowe; i każdego pojedynczego modułu jest również niewielki rozmiar podsystemu wejścia/wyjścia. W tym samouczku wprowadzeniu dwóch następujących modułów:

1. Moduł, który odbiera symulowanego [cz](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) sygnału i konwertuje ją na sformatowany [JSON](https://en.wikipedia.org/wiki/JSON) wiadomości.
2. Moduł, który wyświetla odebranej [JSON](https://en.wikipedia.org/wiki/JSON) wiadomości.

Poniższa ilustracja przedstawia typowego przepływu pełnego danych dla tego projektu:

![Przepływ danych między modułami trzy](media/iot-hub-iot-edge-create-module/dataflow.png "dane wejściowe: symulowane cz modułu; Procesor: Konwertera modułu; Dane wyjściowe: Moduł drukarki")

## <a name="set-up-the-environment"></a>Konfigurowanie środowiska
Poniżej możemy opisano, jak szybko skonfigurować środowiska, aby zacząć tworzyć pierwszego modułu konwertera cz z JS.

### <a name="create-module-project"></a>Utwórz projekt modułu
1. Otwórz okno wiersza polecenia, uruchom `yo az-iot-gw-module`.
2. Wykonaj kroki opisane na ekranie, aby zakończyć inicjowanie modułu projektu.

### <a name="project-structure"></a>Struktura projektu
Projekt modułu JS składa się z następujących składników:

`modules`-Dostosowane JS modułu plików źródłowych. Zastąp wartość domyślna `sensor.js` i `printer.js` z plikami modułu.

`app.js`Plik wpis, aby uruchomić wystąpienie krawędzi.

`gw.config.json`Plik konfiguracji do dostosowywania modułów, które mają być załadowane krawędzi.

`package.json`— Informacje o metadanych dla modułu projektu.

`README.md`-Podstawowa Dokumentacja modułu projektu.


### <a name="package-file"></a>Plik pakietu

To `package.json` deklaruje wszystkie metadane wymagane przez projekt moduł zawierający nazwę, wersję, zapis, skrypty, środowisko uruchomieniowe i rozwoju zależności.

Poniższy fragment kodu przedstawia sposób konfigurowania dla cz konwertera przykładowy projekt.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Plik wejściowy
`app.js` Definiuje sposób inicjowania wystąpienia krawędzi. W tym miejscu musimy nie wszystkie zmiany.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Interfejs modułu
Moduł Azure IoT krawędzi można traktować jako procesor danych, którego zadaniem jest: wejściowych go przetworzyć i generowania danych wyjściowych.

Dane wejściowe mogą być danych z urządzenia (takie jak czujnik ruchu), wiadomości z innymi modułami lub jakikolwiek inny (na przykład liczba okresowo generowane przez czasomierz).

Wynik jest podobny do danych wejściowych, może ją wywołać zachowanie sprzętu (na przykład migający LED), wiadomości do innych modułów lub jakikolwiek inny (na przykład drukowanie do konsoli).

Moduły komunikują się ze sobą przy użyciu `message` obiektu. **Zawartości** z `message` jest umożliwiającej reprezentujący każdego typu danych, które chcesz tablicy bajtów. **Właściwości** dostępne są również `message` i są po prostu mapowania ciąg ciąg. Można traktować **właściwości** jako nagłówków żądania HTTPS lub pliku metadanych.

Aby opracować moduł Azure IoT krawędzi w JS, trzeba utworzyć nowego obiektu modułu, który implementuje metody wymagane `receive()`. W tym momencie można też do zaimplementowania opcjonalny `create()` lub `start()`, lub `destroy()` również metody. Poniższy fragment kodu przedstawia rusztowania obiektu modułu JS.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Moduł konwertera
| Dane wejściowe                    | Procesor                              | Dane wyjściowe                 | Plik źródłowy            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Komunikat danych temperatury | Analizowanie i utworzyć nowy komunikat JSON | Komunikat JSON struktury | `converter.js` |

Jest to typowe moduł Azure IoT krawędzi. Akceptuje temperatury komunikaty z innymi modułami (sprzętowy moduł, lub w tym przypadku naszej symulowane modułu cz); a następnie normalizowane komunikatu temperatury w strukturze komunikat JSON (w tym dołączanie identyfikator komunikatu ustawienie właściwości czy musimy wyzwolenia alertu temperatury i tak dalej).

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Moduł drukarki
| Dane wejściowe                          | Procesor | Dane wyjściowe                     | Plik źródłowy          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Dowolny komunikat z innymi modułami | Nie dotyczy       | Zaloguj się do konsoli wiadomości | `printer.js` |

Ten moduł jest proste, nie wymaga objaśnień, którego dane wyjściowe odebrane wiadomości (właściwości, zawartość) okno terminalu.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfiguracja
Ostatnim krokiem przed uruchomieniem modułów jest skonfigurowanie krawędzi IoT Azure oraz do zestawiania połączeń między modułami.

Najpierw należy zadeklarować naszych `node` modułu ładującego (od momentu krawędzi IoT Azure obsługuje modułów ładujących różnych języków), które można odwoływać się jego `name` w sekcjach później.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Po zadeklarowaniu możemy naszych ładowarki, potrzebujemy zadeklarować również naszych modułów. Podobnie jak deklarowanie ładowarki, ich mogą się też odwoływać linie ich `name` atrybutu. Przy deklarowaniu modułu, należy określić moduł ładujący on powinien używać (który powinien być zdefiniowanego przed) i punktu wejścia (powinna być nazwa klasy znormalizowane naszych modułu) dla każdego modułu. `simulated_device` Jest natywny moduł, który jest uwzględniona w pakiet Azure IoT krawędzi podstawowy środowiska wykonawczego. Obejmują `args` w formacie JSON plików, nawet jeśli jest ona `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Po zakończeniu konfiguracji nawiązanie połączenia. Każde połączenie jest wyrażona w `source` i `sink`. Powinna zarówno odwołujących się wstępnie zdefiniowanych modułów. Komunikat dane wyjściowe z `source` modułu jest przekazywany w danych wejściowych `sink` modułu.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a>Uruchomione moduły
1. `npm install`
2. `npm start`

Jeśli chcesz zakończyć tę aplikację, naciśnij klawisz `<Enter>` klucza.

> [!IMPORTANT]
> Użyj klawiszy Ctrl + C, aby przerwać aplikacji IoT krawędzi jest niezalecane. Ponieważ w ten sposób może spowodować przerwanie wyjątkowo procesu.
