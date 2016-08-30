<properties
 pageTitle="Przewodnik po wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego | Microsoft Azure"
 description="Opis wstępnie skonfigurowanego rozwiązania Azure IoT do monitorowania zdalnego wraz z informacjami dotyczącymi architektury rozwiązania."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="03/02/2016"
 ms.author="stevehob"/>

# Przewodnik po wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego

## Wprowadzenie

Wstępnie skonfigurowane, proste, kompleksowe rozwiązanie monitorowania zdalnego, dostępne w Pakiecie IoT, dotyczy scenariusza biznesowego, który obejmuje szereg urządzeń działających w lokalizacjach zdalnych. Rozwiązanie to obejmuje najważniejsze usługi Pakietu IoT Azure i udostępnia ogólną implementację scenariusza biznesowego, a także stanowi punkt wyjściowy dla klientów, którzy planują wdrożenie tego typu rozwiązań IoT zgodnie z potrzebami firmy.

## Architektura logiczna

Poniższy diagram przedstawia składniki logiczne wstępnie skonfigurowanego rozwiązania:

![](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


### Symulowane urządzenia

W tym wstępnie skonfigurowanym rozwiązaniu symulowane urządzenie odpowiada urządzeniu chłodzącemu (np. klimatyzatorowi lub centrali wentylacyjnej w budynku). Poszczególne symulowane urządzenia wysyłają następujące komunikaty telemetryczne do usługi IoT Hub:


| Komunikat  | Opis |
|----------|-------------|
| Uruchamianie  | Gdy urządzenie zostanie uruchomione, wysyła komunikat z **informacjami o urządzeniu** zawierający identyfikator, metadane, listę obsługiwanych poleceń i bieżącą konfigurację. |


Symulowane urządzenia wysyłają następujące właściwości w postaci metadanych:

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

W symulowanych urządzeniach te właściwości są wypełniane przykładowymi wartościami.  Za każdym razem, gdy symulator inicjuje symulowane urządzenie, wysyła ono wstępnie zdefiniowane metadane do usługi IoT Hub. Warto pamiętać, że powoduje to zastąpienie wszystkich aktualizacji metadanych w portalu urządzenia.


Symulowane urządzenia obsługują następujące polecenia wysyłane z usługi IoT Hub:

| Polecenie                | Opis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Sprawdza, czy urządzenie jest aktywne, za pomocą polecenia _ping_.   |
| StartTelemetry         | Włącza przesyłanie danych telemetrycznych przez urządzenie.                 |
| StopTelemetry          | Wyłącza przesyłanie danych telemetrycznych przez urządzenie.             |
| ChangeSetPointTemp     | Zmienia ustaloną wartość, na podstawie której są generowane dane losowe. |
| DiagnosticTelemetry    | Włącza wysyłanie dodatkowej wartości telemetrycznej (externalTemp — temperatura zewnętrzna) przez symulator urządzenia. |
| ChangeDeviceState      | Zmienia właściwość rozszerzonego stanu urządzenia i wysyła komunikat z informacjami o urządzeniu. |


Usługa IoT Hub udostępnia potwierdzenia poleceń wysyłanych do urządzeń.


### Zadania usługi Azure Stream Analytics


**Zadanie 1: Informacje o urządzeniu** odfiltrowuje komunikaty z informacjami o urządzeniu ze strumienia komunikatów przychodzących i wysyła je do punktu końcowego Centrum zdarzeń. Komunikat z informacjami jest wysyłany podczas uruchomienia urządzenia oraz w odpowiedzi na polecenie **SendDeviceInfo**. W tym zadaniu jest używana następująca definicja zapytania:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

**Zadanie 2: Reguły** porównuje przychodzące dane telemetryczne dotyczące temperatury i wilgotności z wartościami progowymi określonymi dla urządzenia. Wartości progowe są ustawiane za pomocą edytora reguł dostępnego w rozwiązaniu. Poszczególne pary urządzenie/wartość są uporządkowane według sygnatur czasowych i przechowywane w obiekcie blob, który jest odczytywany przez usługę Stream Analytics jako **dane referencyjne** W ramach zadania wszystkie niepuste wartości są porównywane względem progu ustalonego dla urządzenia. W przypadku przekroczenia warunku „>” zadanie generuje zdarzenie **alarmu** dotyczące przekroczenia wartości progowej, które udostępnia dane o urządzeniu, wartości i sygnaturze czasowej. W tym zadaniu jest używana następująca definicja zapytania:

```
WITH AlarmsData AS 
(
SELECT
     Stream.DeviceID,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.DeviceID,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
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

**Zadanie 3: Telemetria** przetwarza strumień przychodzących z urządzeń danych telemetrycznych przy użyciu dwóch procedur. Pierwsza z nich przesyła wszystkie komunikaty telemetryczne z urządzeń do trwałego magazynu obiektów blob. Druga procedura wyznacza średnią, minimalną i maksymalną wartość wilgotności w ramach przesuwającego się okna czasowego o wielkości pięciu minut. Te dane również są przesyłane do magazynu obiektów blob. W tym zadaniu jest używana następująca definicja zapytania:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM 
      [IoTHubStream] 
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    *
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    DeviceId,
    AVG (Humidity) AS [AverageHumidity], 
    MIN(Humidity) AS [MinimumHumidity], 
    MAX(Humidity) AS [MaxHumidity], 
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM
    [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    DeviceId, 
    SlidingWindow (mi, 5)
```

### Procesor zdarzeń

**Procesor zdarzeń** obsługuje odpowiedzi na polecenia i komunikaty z informacjami o urządzeniach. Procesor zdarzeń korzysta z następujących elementów:

- Komunikaty z informacjami o urządzeniach umożliwiające aktualizację rejestru urządzeń (przechowywanego w bazie danych DocumentDB) przy użyciu bieżących informacji.
- Komunikaty z odpowiedziami na polecenia umożliwiające aktualizację historii poleceń urządzenia (przechowywanej w bazie danych DocumentDB).

## Praktyczny przewodnik

W tej sekcji przedstawiono składniki rozwiązania i opisano jego zamierzone zastosowanie oraz podano przykłady użycia.

### Pulpit nawigacyjny monitorowania zdalnego
Na tej stronie aplikacji sieci Web są używane kontrolki JavaScript usługi Power BI (zobacz [repozytorium PowerBI-visuals](https://www.github.com/Microsoft/PowerBI-visuals)), które umożliwiają wizualizację danych wyjściowych zadań usługi Stream Analytics przechowywanych w magazynie obiektów blob.


### Portal administrowania urządzeniami

Ta aplikacja sieci Web umożliwia wykonywanie następujących czynności:

- Aprowizacja nowego urządzenia, co obejmuje określenie unikatowego identyfikatora urządzenia i wygenerowanie klucza uwierzytelniania.
- Zarządzanie właściwościami urządzenia, w tym wyświetlanie istniejących właściwości i aktualizowanie ich.
- Wysyłanie poleceń do urządzenia.
- Wyświetlanie historii poleceń dla urządzenia.

### Monitorowanie działania rozwiązania w chmurze
Aby wyświetlić aprowizowane zasoby, w witrynie [Azure Portal](https://portal.azure.com) przejdź do grupy zasobów z nazwą wybranego rozwiązania.

![](media/iot-suite-remote-monitoring-sample-walkthrough/azureportal_01.png)

Przy pierwszym uruchomieniu przykładowego rozwiązania dostępne są cztery wstępnie skonfigurowane, symulowane urządzenia:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_01.png)

Za pomocą portalu administrowania urządzeniami można dodać nowe symulowane urządzenie:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_02.png)

Początkowy stan nowego urządzenia w portalu ma wartość **Oczekiwanie**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_03.png)

Gdy wdrażanie symulowanego urządzenia w aplikacji zakończy się, stan urządzenia wyświetlany w portalu administrowania urządzeniami zmieni się na **Uruchomione**, jak pokazano na poniższym zrzucie ekranu. Zadanie usługi Stream Analytics dotyczące **informacji o urządzeniach** przesyła informacje o stanie urządzenia z urządzenia do portalu administrowania urządzeniami.

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_04.png)

Za pomocą portalu rozwiązania można wysyłać do urządzeń polecenia, takie jak **ChangeSetPointTemp**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_05.png)

Gdy urządzenie zgłosi wykonanie polecenia, stan jest ustawiany na **Powodzenie**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_06.png)

Portal rozwiązania pozwala wyszukiwać urządzenia o konkretnych cechach, takich jak numer modelu:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_07.png)

Można wyłączyć urządzenie, a następnie je usunąć:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_08.png)


## Następne kroki

Następujące wpisy na blogu w witrynie TechNet zawierają dodatkowe informacje dotyczące wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego:

- [IoT Suite - Under The Hood - Remote Monitoring (Za kulisami pakietu IoT — monitorowanie zdalne)](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (Pakiet IoT — monitorowanie zdalne — dodawanie rzeczywistych i symulowanych urządzeń)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)


<!--HONumber=jun16_HO2-->


