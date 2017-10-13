---
title: "Omówienie usługi Azure Time Series Insights | Microsoft Docs"
description: "Wprowadzenie do usługi Azure Time Series Insights — nowej usługi do analizy danych w postaci szeregów czasowych i obsługi rozwiązań IoT"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 1814459e47280af62450a4093140ab6ab9b765fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-time-series-insights"></a>Co to jest usługa Azure Time Series Insights?

Usługa Azure Time Series Insights jest zarządzaną usługą w chmurze zawierającą składniki do obsługi magazynu, analiz i wizualizacji, które pozwalają łatwo pozyskiwać, przechowywać, eksplorować i analizować miliardy zdarzeń jednocześnie. Usługa Time Series Insights zapewnia globalny wgląd w dane i umożliwia szybkie weryfikowanie rozwiązań IoT. Ponadto zapobiega kosztownym przestojom pracy urządzeń, pomagając w wykrywaniu ukrytych trendów i anomalii oraz przeprowadzaniu analiz głównych przyczyn problemów niemal w czasie rzeczywistym. Usługa Time Series Insights pozwala pozyskiwać dane w postaci szeregów czasowych z brokerów zdarzeń (np. centrów IoT Hub lub centrów zdarzeń), indeksować te dane i wycofywać je na podstawie konfigurowalnych zasad przechowywania. Użytkownicy korzystają z danych za pośrednictwem intuicyjnego środowiska użytkownika lub interfejsów API REST zapytań.

![Omówienie usługi Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Podstawowe scenariusze

* Monitorowanie i weryfikowanie rozwiązań IoT w ciągu kilku minut.
* Wizualizowanie i analizowanie danych IoT na dużą skalę.
* Usprawnianie analiz głównych przyczyn problemów i wykrywanie anomalii.
* Tworzenie globalnego widoku obejmującego wiele urządzeń, zakładów i danych.

## <a name="capabilities-and-benefits"></a>Funkcje i korzyści

* **Łatwe rozpoczynanie pracy**: usługa Azure Time Series Insights nie wymaga wcześniejszego przygotowania danych, a ponadto jest bardzo szybka. W ciągu kilku minut możliwe jest połączenie się z miliardami zdarzeń w usłudze Azure IoT Hub lub centrum zdarzeń. Po połączeniu możesz wizualizować i wchodzić w interakcję z danymi czujników w ciągu kilku sekund, aby zweryfikować swoje rozwiązania IoT. Usługa Time Series Insights jest łatwa w użyciu, a interakcja z danymi jest możliwa bez potrzeby napisania jednego wiersza kodu.  Nie trzeba uczyć się nowego języka — usługa Time Series Insights udostępnia szczegółową warstwę zapytań obejmujących dowolny tekst przeznaczoną dla zaawansowanych użytkowników. Dla wszystkich użytkowników dostępna jest również funkcja wskazywania i kliknięcia.

* **Uzyskiwanie wglądu w kilka sekund**: usługa Time Series Insights może pozyskiwać setki milionów zdarzeń z czujników dziennie z jednominutowym opóźnieniem, co pozwala szybko reagować na zmiany. Usługa Time Series Insights umożliwia uzyskiwanie szczegółowych informacji dotyczących danych z czujników dzięki ułatwieniu szybkiego wykrywania trendów i anomalii, przeprowadzaniu złożonych analiz głównych przyczyn problemów i unikaniu kosztownych przestojów. Po włączeniu korelacji między informacjami uzyskiwanymi w czasie rzeczywistym i danymi historycznymi usługa Time Series Insights pomaga uwidocznić ukryte trendy w danych.

* **Tworzenie niestandardowych rozwiązań**: dane usługi Azure Time Series Insights można osadzić w istniejących aplikacjach. Przy użyciu interfejsów API REST usługi Time Series Insights można też tworzyć nowe, niestandardowe rozwiązania. Możliwe jest również tworzenie i udostępnianie spersonalizowanych widoków, które ułatwiają innym użytkownikom zapoznanie się z odkrytymi przez Ciebie informacjami.

* **Skalowalność**: usługa Time Series Insights obsługuje urządzenia IoT na dużą skalę. Wersja zapoznawcza umożliwia pozyskiwanie od 1 miliona do 100 milionów zdarzeń dziennie, które domyślnie są przechowywane przez 31 dni. Możliwe jest wizualizowanie i analizowanie strumieni bieżących danych niemal w czasie rzeczywistym na tle dużych ilości danych historycznych. Skala przyszłych możliwości w zakresie pozyskiwania i przechowywania danych jeszcze się zwiększy w celu dostosowanie ich do stale rosnących potrzeb przedsiębiorstw.

## <a name="time-series-insights-glossary"></a>Słownik dotyczący usługi Time Series Insights

* **Środowisko**: środowisko jest zasobem platformy Azure, który umożliwia pozyskiwanie i magazynowanie danych.  Klienci mogą aprowizować środowiska z uwzględnieniem wymaganej wydajności za pośrednictwem witryny Azure Portal.
* **Źródło zdarzeń**: źródło zdarzeń pochodzi od brokera zdarzeń, takiego jak usługa Azure Event Hubs.  Usługa Time Series Insights łączy się bezpośrednio ze źródłami zdarzeń, umożliwiając pozyskiwanie strumienia danych bez konieczności pisania kodu. Obecnie usługa Time Series Insights obsługuje centra zdarzeń Azure Event Hub i Azure IoT Hub.
* **Dane referencyjne**: usługa Time Series Insights umożliwia łączenie danych w postaci szeregów czasowych z danymi referencyjnymi.  Dane referencyjne mogą obejmować metadane dotyczące urządzeń lub inne statyczne dane, które zmieniają się stosunkowo rzadko. Dane referencyjne są łączone ze strumieniami danych, a użytkownicy mogą wizualizować i analizować informacje niemal w czasie rzeczywistym.
