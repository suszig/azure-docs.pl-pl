---
title: "Wstępnie skonfigurowane rozwiązanie konserwacji predykcyjnej | Microsoft Docs"
description: "Opis wstępnie skonfigurowanego rozwiązania Azure IoT do konserwacji predykcyjnej."
services: 
suite: iot-suite
documentationcenter: 
author: stevehob
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2016
ms.author: araguila
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2d44af03b8e16a2bd936fc805ed4f0c4e6c5fbfc


---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Omówienie wstępnie skonfigurowanego rozwiązania konserwacji predykcyjnej
Rozwiązanie *konserwacji predykcyjnej* jest jednym z [wstępnie skonfigurowanych rozwiązań][lnk_preconfigured_solutions] udostępnianych w ramach [Pakietu IoT Azure firmy Microsoft][lnk_iot_suite]. Rozwiązanie to obejmuje zbieranie danych telemetrycznych z urządzeń w czasie rzeczywistym i model predykcyjny utworzony za pomocą usługi [Azure Machine Learning][lnk_machine_learning].

Pakiet IoT Azure pozwala przedsiębiorstwom szybko i łatwo łączyć się z zasobami i monitorować je oraz analizować dane w czasie rzeczywistym. Dane te są przetwarzane przez wstępnie skonfigurowane rozwiązanie konserwacji predykcyjnej, które przy użyciu rozbudowanych pulpitów nawigacyjnych i zaawansowanych wizualizacji dostarcza nowe informacje analityczne. Pozwalają one firmom zwiększyć wydajność i wygenerować dodatkowe źródła przychodów.

## <a name="the-scenario"></a>Scenariusz
Fabrikam to regionalny przewoźnik lotniczy, ukierunkowany na zapewnienie doskonałej obsługi klientów przy zachowaniu konkurencyjnych cen. Jedną z przyczyn powodujących opóźnienia lotów są kwestie związane z obsługą techniczną samolotów. Dotyczy to w szczególności konserwacji silników. Za wszelką cenę należy zapobiegać awariom silników podczas lotu, dlatego Fabrikam przeprowadza regularne przeglądy sprzętu zgodnie z zaplanowanym programem konserwacji. Jednak występują różnice dotyczące stopnia zużycia silników samolotów. Zdarzają się przypadki wykonania prac konserwacyjnych, które nie były konieczne. Co więcej, pojawiają się problemy, które mogą prowadzić do uziemienia danego samolotu, dopóki nie zostanie przeprowadzona konserwacja. Powoduje to kosztowne opóźnienia, zwłaszcza jeśli samolot znajduje się w lokalizacji, w której nie są dostępne części zamienne lub odpowiednio wykwalifikowany personel.

Silniki samolotów linii Fabrikam są wyposażone w czujniki, które monitorują stan silnika podczas lotu. Do gromadzenia danych zebranych z czujników podczas lotu jest używany Pakiet IoT Azure. Zbierane przez całe lata dane dotyczące pracy i awarii silników umożliwiły inżynierom danych w firmie Fabrikam opracowanie modelu przewidywania pozostałego czasu eksploatacji silnika samolotu. Udało się określić zależność między danymi pochodzącymi z czterech czujników w silniku a zużyciem silnika, które może prowadzić do wystąpienia awarii. Linie Fabrikam wciąż regularnie przeprowadzają przeglądy w celu zapewnienia bezpieczeństwa, ale dysponują również modelami, które umożliwiają obliczenie pozostałego czasu eksploatacji poszczególnych silników po każdym locie przy użyciu danych telemetrycznych zebranych podczas lotu. Pozwala to przewidywać przyszłe awarie i odpowiednio wcześniej zaplanować prace konserwacyjne i naprawcze.

> [!NOTE]
> W modelu rozwiązania wykorzystano dane dotyczące rzeczywistego zużycia silników.
> 
> 

Dzięki możliwości przewidywania terminu wymaganej obsługi technicznej firma Fabrikam może zoptymalizować swoje operacje, aby obniżyć koszty. Współpraca koordynatorów ds. konserwacji i personelu odpowiedzialnego za rozkład lotów przekłada się na opracowanie harmonogramu, który umożliwia przeprowadzanie prac technicznych podczas zaplanowanego postoju samolotu w danej lokalizacji. Czas serwisowania samolotu nie powoduje występowania zakłóceń w rozkładzie. Pracę personelu technicznego można odpowiednio zaplanować, co pozwala na efektywne serwisowanie bez przestojów. Menedżerowie odpowiedzialni za magazyny podzespołów mają dostęp do planów konserwacji, dzięki czemu mogą zoptymalizować zapasy części zamiennych i proces ich zamawiania. Wszystko to umożliwia firmie Fabrikam zminimalizowanie czasu obsługi naziemnej samolotów i zmniejszenie kosztów operacyjnych przy jednoczesnym zapewnieniu bezpieczeństwa pasażerów i załóg.

Aby dowiedzieć się, jakie funkcje dostępne w [Pakiecie IoT Azure][lnk_iot_suite] umożliwiają klientom wykorzystanie potencjalnych możliwości konserwacji predykcyjnej, zapoznaj się z [grafiką informacyjną][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Jak zbudowane jest rozwiązanie konserwacji predykcyjnej
Rozwiązanie korzysta z istniejącego modelu usługi Azure Machine Learning dostępnego w postaci szablonu, który pozwala na demonstrację działania funkcji przy użyciu danych telemetrycznych pochodzących z urządzeń i zebranych za pomocą usług Pakietu IoT. Firma Microsoft opracowała [model regresji][lnk_regression_model] silnika samolotu i opublikowała dane<sup>\[1\]</sup>, kompletny szablon oraz szczegółowe wskazówki dotyczące używania tego modelu.

Wstępnie skonfigurowane rozwiązanie Azure IoT do konserwacji predykcyjnej używa modelu regresji utworzonego na podstawie tego szablonu. Rozwiązanie jest wdrożone w ramach subskrypcji platformy Azure i dostępne za pośrednictwem automatycznie generowanego interfejsu API. Rozwiązanie to zawiera podzbiór danych testowych odpowiadających 4 ze 100 silników oraz strumieniom danych z 4 z 21 czujników, co pozwala uzyskać dokładny wynik za pomocą uczonego modelu.

*\[1\] A. Saxena and K. Goebel (2008). „Turbofan Engine Degradation Simulation Data Set” (Zestaw danych dotyczących symulacji degradacji silnika turbowentylatorowego), repozytorium danych prognostycznych NASA w Ames (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), ośrodek badawczy NASA w Ames, Moffett Field, Kalifornia*

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o obsłudze scenariuszy konserwacji predykcyjnej w Pakiecie IoT Azure, zapoznaj się z dokumentem[Capture value from the Internet of Things][lnk_capture_value] (Korzyści z Internetu rzeczy).

Zapoznaj się z [przewodnikiem][lnk-predictive-walkthrough] po wstępnie skonfigurowanym rozwiązaniu konserwacji predykcyjnej.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Możesz także wypróbować niektóre inne funkcje i możliwości wstępnie skonfigurowanych rozwiązań Pakietu IoT:

* [Często zadawane pytania dotyczące Pakietu IoT][lnk-faq]
* [Zabezpieczenia IoT od podstaw][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Nov16_HO2-->


