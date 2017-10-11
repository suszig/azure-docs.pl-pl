---
title: "Użyj dynamicznych telemetrii | Dokumentacja firmy Microsoft"
description: "Postępuj zgodnie z tym samouczkiem, aby dowiedzieć się, jak dynamiczna telemetrii za pomocą zdalnego wstępnie skonfigurowane rozwiązanie monitorujące, pakiet IoT Azure."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 0114f27f9b8ae76e1170d04ddf66e2c4bf20686a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dynamiczne telemetrii za pomocą zdalnego wstępnie skonfigurowane rozwiązanie monitorowania

Dynamiczne telemetrii umożliwia wizualizować wszystkie dane telemetryczne wysyłane do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania. Symulowanego urządzenia, które wdrożyć przy użyciu wstępnie skonfigurowane rozwiązanie wysłać telemetrii temperatury i wilgotności, które można zwizualizować na pulpicie nawigacyjnym. Dostosowywanie istniejących urządzeń symulowane, Utwórz nowe symulowanego urządzenia, czy nawiązać urządzeń fizycznych wstępnie skonfigurowane rozwiązanie możesz wysłać innych wartości telemetrii, takie jak zewnętrzny temperatury obr. / min i prędkość wiatru. Następnie można zwizualizować to dodatkowe dane telemetryczne na pulpicie nawigacyjnym.

W tym samouczku korzysta z prostego symulowane urządzenie Node.js, który można łatwo zmodyfikować eksperymentować dynamiczne telemetrii.

Do ukończenia tego samouczka będą potrzebne:

* Aktywna subskrypcja platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].
* [Node.js] [ lnk-node] wersji 0.12.x lub nowszej.

Można wykonać w tym samouczku we wszystkich systemach operacyjnych, takich jak Windows lub Linux, w którym można zainstalować środowiska Node.js.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Dodaj typ telemetrii

Następnym krokiem jest zastąpienie telemetrii generowany przez urządzenie symulowane Node.js nowy zestaw wartości:

1. Zatrzymaj Node.js symulowane urządzenie, wpisując **klawisze Ctrl + C** w wierszu polecenia lub powłoki.
2. W pliku remote_monitoring.js można zobaczyć wartości danych podstawowych istniejących temperatury, wilgotności i telemetrii temperatury zewnętrznych. Dodaj wartość danych podstawowych **obr. / min** w następujący sposób:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js symulowane urządzenie używa **generateRandomIncrement** funkcji w pliku remote_monitoring.js, aby dodać przyrostu losowych wartości danych podstawowych. Ustaw losowy **obr. / min** wartości przez dodanie wiersza kodu po randomizations istniejących w następujący sposób:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Dodaj nową wartość obr. / min do ładunek JSON, które urządzenie wysyła do Centrum IoT:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Uruchom Node.js symulowane urządzenie za pomocą następującego polecenia:

    `node remote_monitoring.js`

6. Zwróć nowy typ telemetrii obr. / min wyświetlane na wykresie na pulpicie nawigacyjnym:

![Dodaj do pulpitu nawigacyjnego obr. / min][image3]

> [!NOTE]
> Należy wyłączyć, a następnie włączyć w urządzeniu Node.js na **urządzeń** strony na pulpicie nawigacyjnym, aby zobaczyć zmianę natychmiast.

## <a name="customize-the-dashboard-display"></a>Dostosowywanie wyświetlania pulpitu nawigacyjnego

**Informacje o urządzeniu** komunikatu może zawierać metadane dotyczące telemetrii urządzenia można wysyłać do Centrum IoT. Te metadane można określić typy telemetrii wysyłanych przez urządzenia. Modyfikowanie **deviceMetaData** wartości w pliku remote_monitoring.js, aby uwzględnić **Telemetrii** definicji następujących **polecenia** definicji. Poniższy kod fragment kodu przedstawia **polecenia** definicji (należy pamiętać o dodaniu `,` po **polecenia** definicji):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> Rozwiązanie monitorowania zdalnego używa bez uwzględniania wielkości liter dopasowanie do porównania definicji metadanych z danymi w strumieniu danych telemetrycznych.


Dodawanie **Telemetrii** definicji, jak pokazano w poprzednim fragment kodu nie zmienia zachowanie pulpitu nawigacyjnego. Jednak metadane mogą również obejmować **DisplayName** atrybutu, aby dostosować wyświetlanie na pulpicie nawigacyjnym. Aktualizacja **Telemetrii** definicji metadanych, jak pokazano w poniższy fragment kodu:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Poniższy zrzut ekranu pokazuje, jak ta zmiana modyfikuje legendy wykresu w pulpicie nawigacyjnym:

![Dostosowywanie legendy wykresu][image4]

> [!NOTE]
> Należy wyłączyć, a następnie włączyć w urządzeniu Node.js na **urządzeń** strony na pulpicie nawigacyjnym, aby zobaczyć zmianę natychmiast.

## <a name="filter-the-telemetry-types"></a>Filtruj typy telemetrii

Domyślnie wykresu w pulpicie nawigacyjnym przedstawia co serii danych w strumieniu danych telemetrycznych. Można użyć **informacje o urządzeniu** metadanych do Pomija wyświetlanie typów określonych danych telemetrycznych na wykresie. 

Aby wykres Pokaż tylko dane telemetryczne temperatury i wilgotności, Pomiń **ExternalTemperature** z **informacje o urządzeniu** **Telemetrii** metadanych w następujący sposób:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Na zewnątrz temperatury** nie jest już wyświetlany na wykresie:

![Filtrowanie danych telemetrycznych na pulpicie nawigacyjnym][image5]

Ta zmiana wpływa tylko na wykres. **ExternalTemperature** wartości danych nadal są przechowywane i dostępne do jakiegokolwiek przetwarzania wewnętrznej bazy danych.

> [!NOTE]
> Należy wyłączyć, a następnie włączyć w urządzeniu Node.js na **urządzeń** strony na pulpicie nawigacyjnym, aby zobaczyć zmianę natychmiast.

## <a name="handle-errors"></a>Obsługa błędów

Dla strumienia danych do wyświetlenia na wykresie jego **typu** w **informacje o urządzeniu** metadanych musi odpowiadać typowi danych telemetrycznych wartości. Na przykład, jeśli metadane Określa, że **typu** wilgotność dane są **int** i **podwójne** znajduje się w strumieniu danych telemetrycznych telemetrii wilgotności nie są wyświetlane, a następnie na wykresie. Jednak **wilgotności** wartości nadal są przechowywane i dostępne do przetwarzania dowolnego zaplecza.

## <a name="next-steps"></a>Następne kroki

Skoro już znasz, jak używać dynamicznej telemetrii, możesz dowiedzieć się więcej o jak wstępnie skonfigurowanego rozwiązania używają informacji o urządzeniu: [urządzenia informacji metadanych do monitorowania zdalnego wstępnie skonfigurowane rozwiązanie] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
