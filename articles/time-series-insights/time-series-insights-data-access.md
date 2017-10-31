---
title: "Zasady dostępu do danych w usłudze Azure Time Series Insights | Microsoft Docs"
description: "W tym samouczku nauczysz się zarządzać zasadami dostępu do danych w usłudze Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: 6a0f04d79ac5487a347e28445c1a6677d5b8b16a
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Przyznawanie dostępu do danych w środowisku usługi Time Series Insights przy użyciu witryny Azure Portal

Środowiska usługi Time Series Insights udostępniają dwa niezależne rodzaje zasad dostępu:

* Zasady dostępu do zarządzania
* Zasady dostępu do danych

Oba rodzaje zasad umożliwiają przyznawanie podmiotom usługi Azure Active Directory (użytkownikom i aplikacjom) różnych uprawnień w określonym środowisku. Podmioty (użytkownicy i aplikacje) muszą należeć do aktywnego katalogu (lub „dzierżawy platformy Azure”) skojarzonego z subskrypcją zawierającą środowisko.

Zasady dostępu do zarządzania pozwalają przyznawać uprawnienia związane z konfiguracją środowiska, takie jak
*   tworzenie i usuwanie środowiska, źródła zdarzeń, zestawy danych referencyjnych i
*   zarządzanie zasadami dostępu do danych.

Zasady dostępu do danych umożliwiają przyznawanie uprawnień do wysyłania zapytań dotyczących danych, manipulowania danymi referencyjnymi w środowisku oraz udostępniania zapisanych zapytań i perspektyw skojarzonych ze środowiskiem.

Dwa rodzaje zasad pozwalają wyraźnie oddzielić dostęp do zarządzania środowiskiem od dostępu do danych znajdujących się w tym środowisku. Można na przykład tak skonfigurować środowisko, aby jego właściciel/twórca nie miał dostępu do danych. Ponadto użytkownicy i usługi z uprawnieniami do odczytu danych w środowisku mogą nie mieć dostępu do konfiguracji tego środowiska.

## <a name="grant-data-access"></a>Przyznawanie dostępu do danych
Poniższe kroki przedstawiają procedurę przyznawania dostępu do danych dla nazwy głównej użytkownika:

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2.  W oknie wyszukiwania wpisz „Time Series”.
3.  Kliknij opcję środowiska usługi Time Series.
4.  Wybierz środowisko usługi Time Series Insights z listy.

  ![Zarządzanie źródłem usługi Time Series Insights — środowisko](media/data-access/getstarted-grant-data-access1.png)

4.  Wybierz opcję „Zasady dostępu do danych”, a następnie kliknij przycisk „Dodaj”.

  ![Zarządzanie źródłem usługi Time Series Insights — dodawanie](media/data-access/getstarted-grant-data-access2.png)

5.  Kliknij przycisk „Wybierz użytkownika”.
6.  Wyszukaj użytkownika według adresu e-mail i wybierz go.
7.  Kliknij pozycję „Wybierz” w bloku „Wybierz użytkownika”.

  ![Zarządzanie źródłem usługi Time Series Insights — wybieranie użytkownika](media/data-access/getstarted-grant-data-access3.png)

8.  Kliknij pozycję „Wybierz rolę”.
9.  Zaznacz opcję „Współautor”, jeśli chcesz zezwolić użytkownikowi na zmianę danych referencyjnych oraz udostępnianie zapisanych zapytań i perspektyw innym użytkownikom środowiska. W przeciwnym razie zaznacz opcję „Czytelnik”, aby zezwolić użytkownikowi na wysyłanie zapytań dotyczących danych i zapisywanie osobistych zapytań (nie udostępnianych) w środowisku.
10. Kliknij przycisk „OK” w bloku „Wybierz rolę”.

  ![Zarządzanie źródłem usługi Time Series Insights — wybieranie roli](media/data-access/getstarted-grant-data-access4.png)

11. Kliknij przycisk „OK” w bloku „Wybierz rolę użytkownika”.
12. Powinien zostać wyświetlony następujący ekran:

  ![Zarządzanie źródłem usługi Time Series Insights — wyniki](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Następne kroki

* [Tworzenie źródła zdarzeń](time-series-insights-add-event-source.md)
* [Wysyłanie zdarzeń](time-series-insights-send-events.md) do źródła zdarzeń
* Wyświetlanie środowiska w [Portalu usługi Time Series Insights](https://insights.timeseries.azure.com)
