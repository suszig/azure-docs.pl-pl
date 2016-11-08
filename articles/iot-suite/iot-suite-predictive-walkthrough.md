---
title: Przewodnik po rozwiązaniu konserwacji predykcyjnej | Microsoft Docs
description: Przewodnik po wstępnie skonfigurowanym rozwiązaniu Azure IoT do konserwacji predykcyjnej.
services: ''
suite: iot-suite
documentationcenter: ''
author: aguilaaj
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: araguila

---
# Przewodnik po wstępnie skonfigurowanym rozwiązaniu konserwacji predykcyjnej
## Wprowadzenie
Wstępnie skonfigurowane, kompleksowe rozwiązanie konserwacji predykcyjnej, dostępne w Pakiecie IoT, dotyczy scenariusza biznesowego, w którym przewidywany jest moment prawdopodobnego wystąpienia awarii. To wstępnie skonfigurowane rozwiązanie można aktywnie wykorzystać w celu zoptymalizowania konserwacji. Rozwiązanie łączy kluczowe usługi Pakietu IoT Azure, w tym obszar roboczy [Azure Machine Learning][lnk_machine_learning]. Ten obszar roboczy zawiera eksperymenty oparte na publicznym zestawie danych przykładowych, które umożliwiają prognozowanie pozostałego czasu eksploatacji (RUL, Remaining Useful Life) silnika samolotu. Ponadto w rozwiązaniu w pełni zaimplementowano scenariusz biznesowy IoT jako punkt wyjściowy planowania i wdrażania rozwiązania zgodnego z potrzebami firmy.

## Architektura logiczna
Poniższy diagram przedstawia składniki logiczne wstępnie skonfigurowanego rozwiązania:

![][img-architecture]

Niebieskie elementy oznaczają usługi platformy Azure aprowizowane w lokalizacji wybranej podczas przeprowadzania aprowizacji wstępnie skonfigurowanego rozwiązania. Rozwiązanie to można aprowizować w regionach Wschodnie stany USA, Europa Północna lub Azja Wschodnia.

Niektóre zasoby nie są dostępne w regionach, w których jest aprowizowane to rozwiązanie. Pomarańczowe elementy diagramu oznaczają usługi platformy Azure aprowizowane w najbliższym dostępnym regionie (Południowo-środkowe stany USA, Europa Zachodnia lub Azja Południowo-Wschodnia).

Zielony element oznacza symulowane urządzenie, które odpowiada silnikowi samolotu. Więcej informacji na temat tych symulowanych urządzeń można znaleźć w poniższej sekcji.

Szare elementy oznaczają składniki z zaimplementowanymi funkcjami *administrowania urządzeniami*. Bieżąca wersja wstępnie skonfigurowanego rozwiązania konserwacji predykcyjnej nie umożliwia aprowizowania tych zasobów. Aby dowiedzieć się więcej o administrowaniu urządzeniami, zapoznaj się z [wstępnie skonfigurowanym rozwiązaniem monitorowania zdalnego][lnk-remote-monitoring].

## Symulowane urządzenia
W tym wstępnie skonfigurowanym rozwiązaniu symulowane urządzenie odpowiada silnikowi samolotu. W ramach rozwiązania są aprowizowane dwa silniki (dla jednego samolotu). Każdy silnik emituje cztery rodzaje danych telemetrycznych: z czujnika 9, czujnika 11, czujnika 14 i czujnika 15. Czujniki dostarczają dane wymagane przez model usługi Machine Learning do obliczenia pozostałego czasu eksploatacji silnika. Poszczególne symulowane urządzenia wysyłają następujące komunikaty telemetryczne do usługi IoT Hub:

*Liczba cykli*. Cykl oznacza ukończony lot o zmiennym czasie trwania (od 2 do 10 godzin), podczas którego co pół godziny są przechwytywane dane telemetryczne.

*Dane telemetryczne*. Są cztery czujniki, które reprezentują parametry silnika. Czujniki te mają ogólne etykiety: czujnik 9, czujnik 11, czujnik 14 i czujnik 15. Czujniki reprezentują dane telemetryczne, które są wystarczające do uzyskania przydatnych wyników dotyczących pozostałego czasu eksploatacji silnika za pośrednictwem modelu usługi Machine Learning. Model ten został utworzony na podstawie publicznego zestawu danych zawierającego dane z czujników w rzeczywistych silnikach. Aby uzyskać więcej informacji dotyczących tworzenia modelu na podstawie oryginalnego zestawu danych, zobacz artykuł [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Szablon konserwacji predykcyjnej w witrynie Cortana Intelligence Gallery).

Symulowane urządzenia obsługują następujące polecenia wysyłane z usługi IoT Hub:

| Polecenie | Opis |
| --- | --- |
| StartTelemetry |Umożliwia sterowanie stanem symulacji.<br/>Włącza przesyłanie danych telemetrycznych przez urządzenie. |
| StopTelemetry |Umożliwia sterowanie stanem symulacji.<br/>Umożliwia zatrzymanie przesyłania danych telemetrycznych przez urządzenie. |

Usługa IoT Hub udostępnia potwierdzenia poleceń wysyłanych do urządzeń.

## Zadanie usługi Azure Stream Analytics
**Zadanie Telemetria** przetwarza strumień danych telemetrycznych przychodzących z urządzeń przy użyciu dwóch instrukcji. Pierwsza z nich pobiera wszystkie dane telemetryczne z urządzeń i wysyła je do magazynu obiektów blob. Stamtąd trafiają one do aplikacji sieci Web w celu wizualizacji. Druga instrukcja oblicza średnie wartości z czujników w ramach przesuwającego się okna czasowego o wielkości dwóch minut i wysyła te wartości do **procesora zdarzeń** za pośrednictwem centrum zdarzeń.

## Procesor zdarzeń
**Procesor zdarzeń** przyjmuje średnie wartości z czujników dla ukończonego cyklu. Przekazuje te wartości do interfejsu API, który dostarcza je uczonemu modelowi usługi Machine Learning w celu obliczenia pozostałego czasu eksploatacji silnika.

## Azure Machine Learning
Aby uzyskać więcej informacji dotyczących tworzenia modelu na podstawie oryginalnego zestawu danych, zobacz artykuł [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Szablon konserwacji predykcyjnej w witrynie Cortana Intelligence Gallery).

## Praktyczny przewodnik
W tej sekcji przedstawiono składniki rozwiązania i opisano jego zamierzone zastosowanie oraz podano przykłady użycia.

### Pulpit nawigacyjny konserwacji predykcyjnej
Na tej stronie aplikacji sieci Web są używane kontrolki JavaScript usługi Power BI (zobacz [repozytorium PowerBI-visuals][lnk-powerbi]), które umożliwiają wizualizację następujących elementów:

* Dane wyjściowe zadań usługi Stream Analytics przechowywane w magazynie obiektów blob.
* Liczba cykli i pozostały czas eksploatacji silnika.

### Monitorowanie działania rozwiązania w chmurze
W portalu Azure przejdź do grupy zasobów z nazwą wybranego rozwiązania, aby wyświetlić aprowizowane zasoby.

![][img-resource-group]

Po przeprowadzeniu aprowizacji wstępnie skonfigurowanego rozwiązania otrzymasz wiadomość e-mail z linkiem do obszaru roboczego usługi Machine Learning. Do obszaru roboczego Machine Learning możesz także przejść ze strony [azureiotsuite.com][lnk-azureiotsuite] swojego aprowizowanego rozwiązania, gdy jego stan zmieni się na **Gotowe**.

![][img-machine-learning]

W portalu rozwiązania możesz zobaczyć, że do aprowizacji przykładu użyto czterech symulowanych urządzeń (dwa samoloty z dwoma silnikami każdy) i czterech czujników na silnik. Gdy po raz pierwszy przejdziesz do portalu rozwiązania, zobaczysz, że symulacja jest zatrzymana.

![][img-simulation-stopped]

Aby rozpocząć symulację, kliknij przycisk **Rozpocznij symulację**. Na pulpicie nawigacyjnym zostaną wyświetlone: liczba cykli, historia danych z czujników i pozostały czas eksploatacji wraz z historią.

![][img-simulation-running]

Jeśli wartość pozostałego czasu eksploatacji jest mniejsza niż 160 (arbitralna wartość progowa dla celów demonstracyjnych), w portalu rozwiązania zostanie wyświetlony symbol ostrzeżenia obok pozostałego czasu eksploatacji, a silnik samolotu zostanie wyróżniony żółtym kolorem. Zwróć uwagę na to, jak pozostały czas eksploatacji ma ogólną tendencję zniżkową ze skokami w górę i w dół. Takie zachowanie wynika z dokładności modelu i różnych czasów trwania cykli.

![][img-simulation-warning]

Pełna symulacja, obejmująca 148 cykli, trwa około 35 minut. Pozostały czas eksploatacji po raz pierwszy osiąga wartość progową równą 160 po upływie około 5 minut, a oba silniki osiągają wartości progowe po upływie około 8 minut.

W trakcie symulacji obejmującej 148 cykli jest przetwarzany pełny zestaw danych, co prowadzi do określenia końcowej liczby cykli i wartości pozostałego czasu eksploatacji.

Symulację można zatrzymać w dowolnym momencie, ale kliknięcie przycisku **Rozpocznij symulację** powoduje ponowne uruchomienie symulacji od początku zestawu danych.

## Następne kroki
Po uruchomieniu wstępnie skonfigurowanego rozwiązania konserwacji predykcyjnej można je zmodyfikować. Informacje na ten temat znajdziesz w artykule [Guidance on customizing preconfigured solutions][lnk-customize] (Wskazówki dotyczące dostosowywania wstępnie skonfigurowanych rozwiązań).

Wpis na blogu [IoT Suite - Under The Hood - Predictive Maintenance](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) (Za kulisami pakietu IoT — konserwacja predykcyjna) w witrynie TechNet zawiera więcej szczegółów dotyczących wstępnie skonfigurowanego rozwiązania konserwacji predykcyjnej.

Możesz także wypróbować niektóre inne funkcje i możliwości wstępnie skonfigurowanych rozwiązań Pakietu IoT:

* [Często zadawane pytania dotyczące Pakietu IoT][lnk-faq]
* [Zabezpieczenia IoT od podstaw][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Sep16_HO3-->


