---
title: Wstępnie skonfigurowane rozwiązania Azure IoT | Microsoft Docs
description: Opis wstępnie skonfigurowanych rozwiązań Azure IoT, w tym informacje dotyczące ich architektury oraz linki prowadzące do dodatkowych zasobów.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2016
ms.author: dobett

---
# Co to są wstępnie skonfigurowane rozwiązania Pakietu IoT Azure?
Wstępnie skonfigurowane rozwiązania Pakietu IoT Azure to implementacje typowych wzorców rozwiązań IoT, które można wdrożyć na platformie Azure przy użyciu posiadanej subskrypcji. Wstępnie skonfigurowanych rozwiązań można używać w następujący sposób:

* Jako punktu wyjściowego dla własnych rozwiązań IoT.
* Jako platformy umożliwiającej zapoznanie się z typowymi wzorcami projektowania i tworzenia rozwiązań IoT.

Każde wstępnie skonfigurowane rozwiązanie stanowi kompletną implementację, która korzysta z symulowanych urządzeń do generowania danych telemetrycznych.

Oprócz wdrażania i uruchamiania rozwiązań na platformie Azure można pobrać pełny kod źródłowy, a następnie dostosować i rozbudować dane rozwiązanie pod kątem własnych wymagań dotyczących urządzeń IoT.

> [!NOTE]
> Aby wdrożyć wstępnie skonfigurowane rozwiązanie, odwiedź witrynę [Pakietu IoT Azure firmy Microsoft][lnk-azureiotsuite]. Więcej informacji dotyczących wdrażania i uruchamiania przykładowego rozwiązania można znaleźć w artykule [Wprowadzenie do wstępnie skonfigurowanych rozwiązań IoT][lnk-getstarted-preconfigured].
> 
> 

W poniższej tabeli przedstawiono odwzorowanie rozwiązań na określone funkcje IoT:

| Rozwiązanie | Wprowadzanie danych | Tożsamość urządzenia | Sterowanie i kontrola | Reguły i akcje | Analiza predykcyjna |
| --- | --- | --- | --- | --- | --- |
| [Monitorowanie zdalne][lnk-getstarted-preconfigured] |Tak |Tak |Tak |Tak |- |
| [Konserwacja predykcyjna][lnk-predictive-maintenance] |Tak |Tak |Tak |Tak |Tak |

* *Wprowadzanie danych*: transfer danych do chmury na dużą skalę.
* *Tożsamość urządzenia*: zarządzanie unikatowymi tożsamościami wszystkich połączonych urządzeń.
* *Sterowanie i kontrola*: wysyłanie komunikatów z chmury do urządzenia zawierających polecenia wykonania określonego działania.
* *Reguły i akcje*: przy użyciu reguł zaplecze rozwiązania wykonuje działania na podstawie określonych danych przesyłanych z urządzenia do chmury.
* *Analiza predykcyjna*: zaplecze rozwiązania analizuje dane przesyłane z urządzenia do chmury i przewiduje działania do wykonania. Na przykład analiza danych telemetrycznych silnika samolotu umożliwia określenie konieczności przeprowadzenia konserwacji silnika.

## Omówienie wstępnie skonfigurowanego rozwiązania monitorowania zdalnego
W tym artykule omówiono wstępnie skonfigurowane rozwiązanie monitorowania zdalnego, ponieważ zawiera ono wiele typowych elementów projektu, które są używane w innych rozwiązaniach.

Na poniższym diagramie przedstawiono najważniejsze elementy rozwiązania monitorowania zdalnego. Następujące sekcje zawierają więcej informacji na temat tych elementów.

![Architektura wstępnie skonfigurowanego rozwiązania monitorowania zdalnego][img-remote-monitoring-arch]

## Urządzenia
Po wdrożeniu wstępnie skonfigurowanego rozwiązania monitorowania zdalnego następuje wstępna aprowizacja czterech symulowanych urządzeń w ramach rozwiązania, które pozoruje pracę urządzenia chłodzącego. Symulowane urządzenia mają wbudowany model generowania wartości temperatury i wilgotności, który emituje dane telemetryczne. Symulowane urządzenia umożliwiają zilustrowanie całościowego przepływu danych przez rozwiązanie i udostępniają przydatne źródło danych telemetrycznych oraz stanowią obiekty docelowe poleceń wysyłanych przez deweloperów zaplecza, którzy korzystają z rozwiązania jako punktu wyjściowego dla niestandardowych implementacji.

Gdy dane urządzenie pierwszy raz łączy się z usługą IoT Hub we wstępnie skonfigurowanym rozwiązaniu monitorowania zdalnego, do usługi IoT Hub jest wysyłany komunikat z informacjami o urządzeniu, który zawiera listę poleceń obsługiwanych przez to urządzenie. W przypadku wstępnie skonfigurowanego rozwiązania monitorowania zdalnego dostępne są następujące polecenia: 

* *Ping Device*: urządzenie potwierdza odebranie tego polecenia. Jest to przydatne w przypadku sprawdzania, czy urządzenie jest nadal aktywne i czy działa nasłuchiwanie.
* *Start Telemetry*: nakazuje urządzeniu rozpoczęcie wysyłania danych telemetrycznych.
* *Stop Telemetry*: nakazuje urządzeniu zaprzestanie wysyłania danych telemetrycznych.
* *Change Set Point Temperature*: umożliwia sterowanie symulowanymi wartościami telemetrycznymi temperatury wysyłanymi przez urządzenie. Jest to przydatne w przypadku testowania logiki wewnętrznej.
* *Diagnostic Telemetry*: umożliwia określenie, czy informacje o temperaturze zewnętrznej mają być wysyłane przez urządzenie w postaci danych telemetrycznych.
* *Change Device State*: ustawia właściwość metadanych stanu urządzenia zgłaszanego przez urządzenie. Jest to przydatne w przypadku testowania logiki wewnętrznej.

Do rozwiązania można dodać kolejne symulowane urządzenia, które emitują te same dane telemetryczne i obsługują te same polecenia. 

## Usługa IoT Hub
W tym wstępnie skonfigurowanym rozwiązaniu wystąpienie usługi IoT Hub odpowiada *bramie chmury* w typowej [architekturze rozwiązania IoT][lnk-what-is-azure-iot].

Usługa IoT Hub odbiera dane telemetryczne z urządzeń w jednym punkcie końcowym. Udostępnia również punkty końcowe umożliwiające poszczególnym urządzeniom pobranie wysyłanych do nich poleceń.

Usługa IoT Hub udostępnia odebrane dane telemetryczne za pośrednictwem punktu końcowego odczytu danych telemetrycznych po stronie usługi.

## Usługa Azure Stream Analytics
Filtrowanie strumienia danych telemetrycznych pochodzących z urządzeń we wstępnie skonfigurowanym rozwiązaniu odbywa się za pomocą trzech zadań usługi [Azure Stream Analytics][lnk-asa] (ASA).

* *Zadanie dotyczące informacji o urządzeniach* — wysyła dane do centrum zdarzeń, które przekazuje specyficzne komunikaty dotyczące rejestracji urządzeń, wysyłane przy pierwszym połączeniu urządzenia lub w odpowiedzi na polecenie **Change device state**, do rejestru urządzeń rozwiązania (bazy danych DocumentDB). 
* *Zadanie dotyczące telemetrii* — wysyła wszystkie nieprzetworzone dane telemetryczne do magazynu obiektów blob Azure w celu przechowania i oblicza zagregowane wartości danych telemetrycznych wyświetlane na pulpicie nawigacyjnym rozwiązania.
* *Zadanie dotyczące reguł* — filtruje strumień danych telemetrycznych w poszukiwaniu danych przekraczających wartości progowe reguł i przesyła dane do centrum zdarzeń. W przypadku wykrycia przekroczenia zdarzenie jest wyświetlane jako nowy wiersz w tabeli historii alarmów w widoku pulpitu nawigacyjnego portalu rozwiązania i jest wywoływana akcja określona w ustawieniach widoków reguł i akcji w portalu rozwiązania.

W tym wstępnie skonfigurowanym rozwiązaniu zadania usługi ASA stanowią część **zaplecza rozwiązania IoT** w typowej [architekturze rozwiązania IoT][lnk-what-is-azure-iot].

## Procesor zdarzeń
W tym wstępnie skonfigurowanym rozwiązaniu procesor zdarzeń stanowi część **zaplecza rozwiązania IoT** w typowej [architekturze rozwiązania IoT][lnk-what-is-azure-iot].

Zadania usługi ASA dotyczące **reguł** i **informacji o urządzeniach** wysyłają dane wyjściowe do centrów zdarzeń, z których dane są przekazywane do innych usług zaplecza. Do odczytu komunikatów z centrów zdarzeń jest używane wystąpienie klasy [EventProcessorHost][lnk-event-processor] uruchomione w zadaniu [WebJob][lnk-web-job]. Klasa **EventProcessorHost** aktualizuje dane urządzeń w bazie danych DocumentDB za pomocą danych zadania **dotyczącego informacji o urządzeniach** oraz wywołuje aplikację logiki i aktualizuje alerty wyświetlane w portalu rozwiązania przy użyciu danych zadania **dotyczącego reguł**.

## Rejestr tożsamości urządzeń i baza danych DocumentDB
Każde wystąpienie usługi IoT Hub zawiera [rejestr tożsamości urządzeń][lnk-identity-registry], który przechowuje klucze urządzeń. Usługa IoT Hub używa tych informacji do uwierzytelniania urządzeń — dane urządzenie musi być zarejestrowane i mieć prawidłowy klucz, zanim będzie mogło połączyć się z centrum.

W tym rozwiązaniu są przechowywane dodatkowe informacje o urządzeniach, takie jak stan, obsługiwane polecenia i inne metadane. Dane dotyczące urządzeń w ramach konkretnego rozwiązania są przechowywane w bazie danych DocumentDB, skąd są pobierane w celu ich wyświetlania i edytowania w portalu rozwiązania.

Informacje przechowywane w rejestrze tożsamości urządzeń muszą być zsynchronizowane z zawartością bazy danych DocumentDB. Klasa **EventProcessorHost** zarządza synchronizacją przy użyciu danych z zadania analizy strumienia dotyczącego **informacji o urządzeniach**.

## Portal rozwiązania
![Pulpit nawigacyjny rozwiązania][img-dashboard]

Portal rozwiązania to oparty na sieci Web interfejs użytkownika wdrożony w chmurze w ramach wstępnie skonfigurowanego rozwiązania. Umożliwia on wykonywanie następujących czynności:

* Wyświetlanie historii danych telemetrycznych i alarmów na pulpicie nawigacyjnym.
* Aprowizacja nowych urządzeń.
* Monitorowanie urządzeń i zarządzanie nimi.
* Wysyłanie poleceń do określonych urządzeń.
* Zarządzanie regułami i akcjami.

W tym wstępnie skonfigurowanym rozwiązaniu portal rozwiązania stanowi część **zaplecza rozwiązania IoT** oraz **przetwarzania i łączności biznesowej** w typowej [architekturze rozwiązania IoT][lnk-what-is-azure-iot].

## Następne kroki
Aby uzyskać więcej informacji na temat architektury rozwiązań IoT, zobacz dokument [Microsoft Azure IoT services: Reference Architecture][lnk-refarch] (Usługi Microsoft Azure IoT: architektura referencyjna).

Teraz, kiedy już wiesz, czym jest wstępnie skonfigurowane rozwiązanie, możesz rozpocząć wdrażanie wstępnie skonfigurowanego rozwiązania *monitorowania zdalnego* : [Wprowadzenie do wstępnie skonfigurowanych rozwiązań][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md


<!--HONumber=Oct16_HO1-->


