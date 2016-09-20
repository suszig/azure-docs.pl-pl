<properties
 pageTitle="Przewodnik po wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego | Microsoft Azure"
 description="Opis wstępnie skonfigurowanego rozwiązania Azure IoT do monitorowania zdalnego wraz z informacjami dotyczącymi architektury rozwiązania."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# Przewodnik po wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego

## Wprowadzenie

[Wstępnie skonfigurowane rozwiązanie][lnk-preconfigured-solutions] do zdalnego monitorowania pakietu IoT to implementacja end-to-end rozwiązania do monitorowania wielu maszyn działających w zdalnych lokalizacjach. Rozwiązanie łączy kluczowe usługi platformy Azure w celu zapewnienia ogólnej implementacji scenariusza biznesowego. Możesz go użyć jako punktu wyjścia dla własnej implementacji. Rozwiązanie możesz [dostosować][lnk-customize], aby spełniało określone wymagania Twojej firmy.

Ten artykuł przeprowadzi Cię przez niektóre kluczowe elementy rozwiązania do monitorowania zdalnego, aby umożliwić Ci zrozumienie jego sposobu działania. Ta wiedza ułatwi Ci:

- Rozwiązywanie problemów w rozwiązaniu.
- Planowanie sposobu dostosowywania rozwiązania, aby spełniało Twoje wymagania. 
- Projektowanie własnego rozwiązania IoT korzystającego z usług Azure.

## Architektura logiczna

Poniższy diagram przedstawia składniki logiczne wstępnie skonfigurowanego rozwiązania:

![Architektura logiczna](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## Symulowane urządzenia

W tym wstępnie skonfigurowanym rozwiązaniu symulowane urządzenie odpowiada urządzeniu chłodzącemu (np. klimatyzatorowi lub centrali wentylacyjnej w budynku). Podczas wdrażania wstępnie skonfigurowanego rozwiązania automatycznie aprowizujesz też cztery symulowane urządzenia działające w zadaniu [WebJob Azure][lnk-webjobs]. Symulowane urządzenia ułatwiają badanie zachowania rozwiązania bez konieczności wdrażania jakichkolwiek urządzeń fizycznych. Aby wdrożyć prawdziwe urządzenie fizyczne, zobacz samouczek [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem do monitorowania zdalnego][lnk-connect-rm].

Każde symulowane urządzenie może wysyłać do usługi IoT Hub następujące typy wiadomości:

| Komunikat  | Opis |
|----------|-------------|
| Uruchamianie  | Po uruchomieniu urządzenie wysyła do zaplecza komunikat **device-info** zawierający informacje o tym urządzeniu. Te dane obejmują identyfikator urządzenia, metadane urządzenia, listę poleceń obsługiwanych przez urządzenie oraz bieżącą konfigurację urządzenia. |
| Obecność | Urządzenie okresowo wysyła komunikat **presence** w celu zgłoszenia, czy urządzenie może wykrywać obecność czujnika. |
| Telemetria | Urządzenie okresowo wysyła komunikat **telemetry**, który zgłasza symulowane wartości temperatury i wilgotności zebrane z symulowanych czujników urządzenia. |


Symulowane urządzenia wysyłają następujące właściwości w komunikacie **device-info**:

| Właściwość               |  Przeznaczenie |
|------------------------|--------- |
| Identyfikator urządzenia              | Ten identyfikator jest udostępniany lub przypisywany podczas tworzenia urządzenia w rozwiązaniu. |
| Producent           | Producent urządzenia |
| Numer modelu           | Numer modelu urządzenia |
| Numer seryjny          | Numer seryjny urządzenia |
| Oprogramowanie układowe               | Bieżąca wersja oprogramowania układowego na urządzeniu |
| Platforma               | Architektura platformy urządzenia |
| Procesor              | Procesor działający w urządzeniu |
| Zainstalowana pamięć RAM          | Ilość pamięci RAM zainstalowanej w urządzeniu |
| Stan Centrum      | Właściwość stanu usługi IoT Hub dla urządzenia |
| Czas utworzenia           | Czas utworzenia urządzenia w rozwiązaniu |
| Czas aktualizacji           | Czas ostatniej aktualizacji właściwości urządzenia |
| Szerokość geograficzna               | Współrzędna szerokości geograficznej lokalizacji urządzenia |
| Długość geograficzna              | Współrzędna długości geograficznej lokalizacji urządzenia |

W symulowanych urządzeniach te właściwości są wypełniane przykładowymi wartościami.  Za każdym razem, gdy symulator inicjuje symulowane urządzenie, wysyła ono wstępnie zdefiniowane metadane do usługi IoT Hub. Zwróć uwagę, jak zastępuje to wszystkie aktualizacje metadanych w portalu urządzenia.


Symulowane urządzenia obsługują następujące polecenia wysyłane z pulpitu nawigacyjnego rozwiązania za pośrednictwem usługi IoT Hub:

| Polecenie                | Opis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Sprawdza, czy urządzenie jest aktywne, za pomocą polecenia _ping_.   |
| StartTelemetry         | Włącza przesyłanie danych telemetrycznych przez urządzenie.                 |
| StopTelemetry          | Wyłącza przesyłanie danych telemetrycznych przez urządzenie.             |
| ChangeSetPointTemp     | Zmienia ustaloną wartość, na podstawie której są generowane dane losowe. |
| DiagnosticTelemetry    | Włącza wysyłanie dodatkowej wartości telemetrycznej (externalTemp — temperatura zewnętrzna) przez symulator urządzenia. |
| ChangeDeviceState      | Zmienia właściwość rozszerzonego stanu urządzenia i wysyła komunikat z informacjami o urządzeniu. |

Usługa IoT Hub udostępnia potwierdzenia poleceń urządzenia wysyłanych do zaplecza rozwiązania.

## Usługa IoT Hub

Usługa [IoT Hub][lnk-iothub] pozyskuje dane wysyłane z urządzeń do chmury i udostępnia je zadaniom usługi Azure Stream Analytics (ASA). Usługa IoT Hub wysyła również polecenia do urządzeń w imieniu portalu urządzeń. Każde zadanie strumieniowe ASA odczytuje strumień komunikatów z urządzeń przy użyciu osobnej grupy konsumentów usługi IoT Hub.

## Azure Stream Analytics

W rozwiązaniu monitorowania zdalnego usługa [Azure Stream Analytics][lnk-asa] (ASA) wysyła komunikaty urządzeń odebrane przez usługę IoT Hub do innych składników zaplecza w celu ich przetwarzania lub magazynowania. Różne zadania ASA pełnią określone funkcje w zależności od zawartości komunikatów.

**Zadanie 1: Informacje o urządzeniu** odfiltrowuje komunikaty z informacjami o urządzeniu ze strumienia komunikatów przychodzących i wysyła je do punktu końcowego Centrum zdarzeń. Komunikat z informacjami o urządzeniu jest wysyłany podczas uruchamiania urządzenia oraz w odpowiedzi na polecenie **SendDeviceInfo**. To zadanie identyfikuje komunikaty **device-info** przy użyciu następującej definicji zapytania:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

To zadanie wysyła dane wyjściowe do centrum zdarzeń w celu dalszego przetwarzania.

**Zadanie 2: Reguły** porównuje przychodzące dane telemetryczne dotyczące temperatury i wilgotności z wartościami progowymi określonymi dla urządzenia. Wartości progowe są ustawiane za pomocą edytora reguł dostępnego na pulpicie nawigacyjnym rozwiązania. Poszczególne pary urządzenie/wartość są uporządkowane według sygnatur czasowych i przechowywane w obiekcie blob, który jest odczytywany przez usługę Stream Analytics jako **dane referencyjne**. W ramach zadania wszystkie niepuste wartości są porównywane względem progu ustalonego dla urządzenia. W przypadku przekroczenia warunku „>” zadanie generuje zdarzenie **alarmu** dotyczące przekroczenia wartości progowej, które udostępnia dane o urządzeniu, wartości i sygnaturze czasowej. To zadanie identyfikuje komunikaty telemetryczne, które powinny wyzwolić alarm, przy użyciu następującej definicji zapytania:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Zadanie wysyła dane wyjściowe do centrum zdarzeń w celu dalszego przetwarzania i zapisuje szczegóły każdego alertu w magazynie obiektów blob, z którego pulpit nawigacyjny rozwiązania może odczytać informacje o alertach.

**Zadanie 3: Telemetria** przetwarza strumień przychodzących z urządzeń danych telemetrycznych przy użyciu dwóch procedur. Pierwsza z nich przesyła wszystkie komunikaty telemetryczne z urządzeń do trwałego magazynu obiektów blob w celu ich długotrwałego przechowywania. Druga procedura wyznacza średnią, minimalną i maksymalną wartość wilgotności w ramach przesuwającego się okna czasowego o wielkości pięciu minut i wysyła te dane do magazynu obiektów blob. Pulpit nawigacyjny rozwiązania odczytuje dane telemetryczne z magazynu obiektów blob w celu wypełnienia wykresów. W tym zadaniu jest używana następująca definicja zapytania:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## Usługa Event Hubs

Zadania ASA **informacje o urządzeniu** i **reguły** przesyłają dane wyjściowe do centrum zdarzeń w celu ich niezawodnego przekazania do **procesora zdarzeń** uruchomionego w zadaniu WebJob.

## Azure Storage

Rozwiązanie korzysta z magazynu obiektów blob platformy Azure, aby utrwalić wszystkie nieprzetworzone i podsumowane dane telemetryczne z urządzeń w rozwiązaniu. Pulpit nawigacyjny odczytuje dane telemetryczne z magazynu obiektów blob w celu wypełnienia wykresów. Aby wyświetlić alerty, pulpit nawigacyjny odczytuje dane z magazynu obiektów blob, który rejestruje, kiedy wartości telemetryczne przekraczają skonfigurowane wartości progowe. Rozwiązanie używa również magazynu obiektów blob do rejestrowania wartości progowych określonych na pulpicie nawigacyjnym.

## Zadania WebJob

Oprócz hostowania symulatorów urządzenia, zadania WebJob w rozwiązaniu hostują także **procesor zdarzeń** uruchomiony w zadaniu WebJob Azure, które obsługuje komunikaty z informacjami o urządzeniu i odpowiedzi na polecenia. Procesor zdarzeń korzysta z następujących elementów:

- Komunikaty z informacjami o urządzeniach umożliwiające aktualizację rejestru urządzeń (przechowywanego w bazie danych DocumentDB) przy użyciu bieżących informacji.
- Komunikaty z odpowiedziami na polecenia umożliwiające aktualizację historii poleceń urządzenia (przechowywanej w bazie danych DocumentDB).

## DocumentDB

Rozwiązanie przechowuje informacje o urządzeniach połączonych z rozwiązaniem, używając bazy danych DocumentDB. Informacje te obejmują metadane urządzeń oraz historię poleceń wysłanych do urządzeń z pulpitu nawigacyjnego.

## Aplikacje sieci Web

### Pulpit nawigacyjny monitorowania zdalnego
Na tej stronie aplikacji sieci Web są używane kontrolki JavaScript usługi Power BI (zobacz [repozytorium PowerBI-visuals](https://www.github.com/Microsoft/PowerBI-visuals)), które umożliwiają wizualizację danych telemetrycznych z urządzeń. Rozwiązanie zapisuje dane telemetryczne w magazynie obiektów blob, używając zadań telemetrii usługi ASA.


### Portal administrowania urządzeniami

Ta aplikacja sieci Web umożliwia wykonywanie następujących czynności:

- Aprowizacja nowego urządzenia. Ta czynność obejmuje określenie unikatowego identyfikatora urządzenia i wygenerowanie klucza uwierzytelniania. Zapisuje ona informacje o urządzeniu w rejestrze tożsamości usługi IoT Hub oraz w bazie danych DocumentDB określonego rozwiązania.
- Zarządzanie właściwościami urządzenia. Ta czynność obejmuje wyświetlanie istniejących właściwości i aktualizowanie ich.
- Wysyłanie poleceń do urządzenia.
- Wyświetlanie historii poleceń dla urządzenia.
- Włączanie i wyłączanie urządzeń.

## Następne kroki

Następujące wpisy na blogu w witrynie TechNet zawierają więcej informacji dotyczących wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego:

- [IoT Suite - Under The Hood - Remote Monitoring (Za kulisami pakietu IoT — monitorowanie zdalne)](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (Pakiet IoT — monitorowanie zdalne — dodawanie rzeczywistych i symulowanych urządzeń)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Możesz kontynuować poznawanie Pakietu IoT, czytając następujące artykuły:

- [Łączenie urządzenia ze wstępnie skonfigurowanym rozwiązaniem do monitorowania zdalnego][lnk-connect-rm]
- [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md


<!--HONumber=sep16_HO1-->


