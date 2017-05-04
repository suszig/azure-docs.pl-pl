---
title: "Omówienie usługi Azure Time Series Insights | Microsoft Docs"
description: "Wprowadzenie do usługi Azure Time Series Insights — nowej usługi do analizy danych w postaci szeregów czasowych i obsługi rozwiązań IoT"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/20/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: cc35c00bec18ee1d9ed52b29b68ddacc1136dc70
ms.lasthandoff: 04/25/2017

---

# <a name="overview-of-azure-time-series-insights"></a>Omówienie usługi Azure Time Series Insights

Usługa Azure Time Series Insights jest w pełni zarządzaną usługą w chmurze zawierającą składniki do obsługi magazynu, analiz i wizualizacji, które pozwalają niezwykle łatwo pozyskiwać, przechowywać, eksplorować i analizować miliardy zdarzeń jednocześnie. Usługa ta zapewnia globalny wgląd w dane i umożliwia szybkie weryfikowanie rozwiązań IoT. Ponadto zapobiega kosztownym przestojom w pracy urządzeń o krytycznym znaczeniu, pomagając w wykrywaniu ukrytych trendów i anomalii oraz przeprowadzaniu analiz głównych przyczyn niemal w czasie rzeczywistym. Usługa Time Series Insights pozwala pozyskiwać dane w postaci szeregów czasowych z brokerów zdarzeń (np. centrów IoT Hub lub centrów zdarzeń), indeksować te dane i wycofywać je na podstawie konfigurowalnych zasad przechowywania. Użytkownicy korzystają z danych w ramach scenariusza biznesowego za pośrednictwem intuicyjnego środowiska użytkownika lub interfejsów API REST zapytań.

![Omówienie usługi Time Series Insights](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Podstawowe scenariusze

* Walidacja i monitorowanie rozwiązań IoT w ciągu kilku minut
* Intuicyjna wizualizacja i analiza danych IoT na dużą skalę
* Usprawnianie analiz głównych przyczyn problemów i wykrywanie anomalii
* Tworzenie globalnego widoku obejmującego wiele urządzeń, zakładów i danych

## <a name="key-capabilities-and-benefits"></a>Najważniejsze funkcje i korzyści

* **Łatwe rozpoczęcie pracy**: usługa Azure Time Series Insights nie wymaga wcześniejszego przygotowania danych, a ponadto jest bardzo szybka — w ciągu kilku minut można połączyć się z miliardami zdarzeń w usłudze Azure IoT Hub lub centrum zdarzeń. Po nawiązaniu połączenia możesz w ciągu kilku sekund wizualizować dane z czujników i wchodzić z nimi w interakcję, aby szybko przeprowadzić walidację swojego rozwiązania IoT. Usługa Time Series Insights jest bardzo intuicyjna i łatwa w użyciu, a interakcja z danymi jest możliwa bez pisania własnego kodu.  Nie trzeba uczyć się nowego języka — usługa Time Series Insights udostępnia szczegółową warstwę zapytań obejmujących dowolny tekst przeznaczoną dla zaawansowanych użytkowników. Początkujące osoby mogą korzystać z funkcji wskazywania i kliknięcia.

* **Uzyskiwanie wglądu w kilka sekund**: lepsze wykorzystanie danych w postaci szeregów czasowych dzięki składnikom do obsługi magazynu, analiz i wizualizacji, dostępnym w jednym miejscu. Usługa Time Series Insights może pozyskiwać setki milionów zdarzeń z czujników dziennie z jednominutowym opóźnieniem, co pozwala szybko reagować na zmiany. Możliwość szybkiego wykrywania trendów i anomalii zapewnia lepszy wgląd w dane z czujników, co ułatwia przeprowadzanie złożonych analiz głównych przyczyn problemów i pozwala uniknąć kosztownych przestojów. Po włączeniu korelacji między informacjami uzyskiwanymi w czasie rzeczywistym i danymi historycznymi można uwidocznić ukryte trendy w danych.

* **Tworzenie niestandardowych rozwiązań**: dane usługi Azure Time Series Insights można osadzić w istniejących aplikacjach. Przy użyciu interfejsów API REST usługi Time Series Insights można też tworzyć nowe, niestandardowe rozwiązania.  Ponadto można w prosty sposób tworzyć i udostępniać spersonalizowane widoki, co ułatwia innym osobom zapoznawanie się z odkrytymi nowościami.

* **Skalowalność**: usługa Time Series Insights obsługuje urządzenia IoT na dużą skalę. Wersja zapoznawcza umożliwia pozyskiwanie od 1 miliona do 100 milionów zdarzeń dziennie, które domyślnie są przechowywane przez 31 dni. Usługa Time Series Insights pozwala użytkownikom niemal w czasie rzeczywistym wizualizować i analizować strumienie bieżących danych na tle dużych ilości danych historycznych. Skala przyszłych możliwości w zakresie pozyskiwania i przechowywania danych jeszcze się zwiększy w celu dostosowanie ich do stale rosnących potrzeb przedsiębiorstw.

## <a name="taxonomy-of-time-series-insights"></a>Taksonomia usługi Time Series Insights

* **Środowisko**: środowisko jest zasobem platformy Azure, który umożliwia pozyskiwanie i magazynowanie danych.  Klienci mogą aprowizować środowiska z uwzględnieniem wymaganej wydajności za pośrednictwem witryny Azure Portal.
* **Źródło zdarzeń**: źródło zdarzeń pochodzi od brokera zdarzeń, takiego jak usługa Azure Event Hubs.  Usługa Time Series Insights łączy się bezpośrednio ze źródłami zdarzeń, umożliwiając pozyskiwanie strumienia danych bez konieczności pisania kodu przez użytkowników. Obecnie usługa Time Series Insights obsługuje usługi Azure Event Hubs i Azure IoT Hubs. Dodatkowe źródła zdarzeń zostaną dodane w przyszłości.
* **Dane referencyjne**: usługa Time Series Insights umożliwia łączenie danych w postaci szeregów czasowych z danymi referencyjnymi.  Dane referencyjne mogą obejmować metadane dotyczące urządzeń lub inne statyczne dane, które zmieniają się stosunkowo rzadko. Dane referencyjne są łączone ze strumieniami danych, a użytkownicy mogą wizualizować i analizować informacje niemal w czasie rzeczywistym.

