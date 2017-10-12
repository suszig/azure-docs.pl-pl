---
title: "Przewodnik po rozwiązaniu konserwacji predykcyjnej | Microsoft Docs"
description: "Przewodnik po wstępnie skonfigurowanym rozwiązaniu Azure IoT do konserwacji predykcyjnej."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: a68a8fdc3976ade0d1036d5ed58c8b2eb6d32a5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Przewodnik po wstępnie skonfigurowanym rozwiązaniu konserwacji predykcyjnej

Wstępnie skonfigurowane, kompleksowe rozwiązanie konserwacji predykcyjnej, dotyczy scenariusza biznesowego, w którym przewidywany jest moment prawdopodobnego wystąpienia awarii. To wstępnie skonfigurowane rozwiązanie można aktywnie wykorzystać w celu zoptymalizowania konserwacji. Rozwiązanie łączy kluczowe usługi Pakietu IoT Azure, takie jak usługa IoT Hub, analiza strumienia oraz obszar roboczy usługi [Azure Machine Learning][lnk-machine-learning]. Ten obszar roboczy zawiera model oparty na publicznym zestawie przykładowych danych, które umożliwiają prognozowanie pozostałego czasu eksploatacji (RUL, Remaining Useful Life) silnika samolotu. Ponadto w rozwiązaniu w pełni zaimplementowano scenariusz biznesowy IoT jako punkt wyjściowy planowania i wdrażania rozwiązania zgodnego z potrzebami firmy.

## <a name="logical-architecture"></a>Architektura logiczna

Poniższy diagram przedstawia składniki logiczne wstępnie skonfigurowanego rozwiązania:

![][img-architecture]

Niebieskie elementy oznaczają usługi platformy Azure zaprowizowane w regionie, w którym wdrożono wstępnie skonfigurowane rozwiązanie. Lista regionów, w których można wdrożyć wstępnie skonfigurowane rozwiązanie, znajduje się na [stronie aprowizacji][lnk-azureiotsuite].

Zielony element oznacza symulowane urządzenie, które odpowiada silnikowi samolotu. Więcej informacji na temat tych symulowanych urządzeń można znaleźć w poniższej sekcji.

Szare elementy oznaczają składniki z zaimplementowanymi funkcjami *zarządzania urządzeniami*. Bieżąca wersja wstępnie skonfigurowanego rozwiązania konserwacji predykcyjnej nie umożliwia aprowizowania tych zasobów. Aby dowiedzieć się więcej o zarządzaniu urządzeniami, zapoznaj się z [wstępnie skonfigurowanym rozwiązaniem monitorowania zdalnego][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Symulowane urządzenia

W tym wstępnie skonfigurowanym rozwiązaniu symulowane urządzenie odpowiada silnikowi samolotu. W ramach rozwiązania są aprowizowane dwa silniki (dla jednego samolotu). Każdy silnik emituje cztery rodzaje danych telemetrycznych: z czujnika 9, czujnika 11, czujnika 14 i czujnika 15. Czujniki dostarczają dane wymagane przez model usługi Machine Learning do obliczenia pozostałego czasu eksploatacji silnika. Poszczególne symulowane urządzenia wysyłają następujące komunikaty telemetryczne do usługi IoT Hub:

*Liczba cykli*. Cykl reprezentuje ukończony lot o długości od 2 do 10 godzin. Podczas lotu dane telemetryczne są przechwytywane co pół godziny.

*Dane telemetryczne*. Są cztery czujniki, które reprezentują parametry silnika. Czujniki te mają ogólne etykiety: czujnik 9, czujnik 11, czujnik 14 i czujnik 15. Te 4 czujniki reprezentują dane telemetryczne, które są wystarczające do uzyskania przydatnych wyników z modelu pozostałego czasu eksploatacji silnika. Model wykorzystywany we wstępnie skonfigurowanym zadaniu został utworzony na podstawie publicznego zestawu danych zawierającego dane z czujników w rzeczywistych silnikach. Aby uzyskać więcej informacji dotyczących tworzenia modelu na podstawie oryginalnego zestawu danych, zobacz [szablon konserwacji predykcyjnej w witrynie Cortana Intelligence Gallery][lnk-cortana-analytics].

Symulowane urządzenia obsługują w rozwiązaniu następujące polecenia wysyłane z centrum IoT:

| Polecenie | Opis |
| --- | --- |
| StartTelemetry |Umożliwia sterowanie stanem symulacji.<br/>Włącza przesyłanie danych telemetrycznych przez urządzenie. |
| StopTelemetry |Umożliwia sterowanie stanem symulacji.<br/>Umożliwia zatrzymanie przesyłania danych telemetrycznych przez urządzenie. |

Usługa IoT Hub udostępnia potwierdzenia poleceń wysyłanych do urządzeń.

## <a name="azure-stream-analytics-job"></a>Zadanie usługi Azure Stream Analytics

**Zadanie Telemetria** przetwarza strumień danych telemetrycznych przychodzących z urządzeń przy użyciu dwóch instrukcji:

* Pierwsza z nich pobiera wszystkie dane telemetryczne z urządzeń i wysyła je do magazynu obiektów blob. Stamtąd trafiają one do aplikacji internetowej w celu wizualizacji.
* Druga instrukcja oblicza średnie wartości z czujników w ramach przesuwającego się okna czasowego trwającego dwie minuty i wysyła te wartości do **procesora zdarzeń** za pośrednictwem centrum zdarzeń.

## <a name="event-processor"></a>Procesor zdarzeń
**Host procesora zdarzeń** jest uruchamiany w zadaniu sieci Web Azure. **Procesor zdarzeń** przyjmuje średnie wartości z czujników dla ukończonego cyklu. Następnie przekazuje te wartości do interfejsu API, który dostarcza je nauczonemu modelowi w celu obliczenia pozostałego czasu eksploatacji silnika. Interfejs API jest udostępniany przez obszar roboczy usługi Machine Learning, który jest aprowizowany w ramach rozwiązania.

## <a name="machine-learning"></a>Usługa Machine Learning
Składnik Machine Learning wykorzystuje model opracowany na podstawie danych zebranych z rzeczywistych silników samolotów. Do obszaru roboczego usługi Machine Learning możesz przejść z poziomu kafelka na stronie [azureiotsuite.com][lnk-azureiotsuite] zaprowizowanego rozwiązania. Kafelek jest dostępny, gdy rozwiązanie jest w stanie **Gotowe**.


## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się ze składnikami wstępnie skonfigurowanego rozwiązania do konserwacji zapobiegawczej możesz je dostosować. Zobacz [Przewodnik dostosowywania wstępnie skonfigurowanych rozwiązań][lnk-customize].

Możesz także wypróbować niektóre inne funkcje i możliwości wstępnie skonfigurowanych rozwiązań Pakietu IoT:

* [Często zadawane pytania dotyczące Pakietu IoT][lnk-faq]
* [Zabezpieczenia IoT od podstaw][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/