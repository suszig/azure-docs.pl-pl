---
title: "Monitorowanie urządzenia Surface Hub z Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Rozwiązanie Surface Hub umożliwia śledzenie kondycji z urządzeń Surface Hub i zrozumieć, jak są one używane."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6ecd0d09589fec85c1633f528afc1165c346b7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Monitorowanie urządzeń Surface Hub z analizy dzienników śledzenia ich kondycję

![Surface Hub symbol](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

W tym artykule opisano, jak używasz rozwiązania Surface Hub w analizy dzienników do monitorowania urządzeń Microsoft Surface Hub z Microsoft Operations Management Suite (OMS). Analiza dzienników pomaga śledzić kondycji z urządzeń Surface Hub dobrze zrozumieć, jak są one używane.

Każdy Surface Hub został zainstalowany program Microsoft Monitoring Agent. Jego przez agenta, czy użytkownik może wysyłać dane z Twojego Surface Hub OMS. Pliki dziennika są odczytywane z urządzeń Surface Hub i są następnie są wysyłane z usługą OMS. Zagadnienia, takie jak serwery w trybie offline, Kalendarz nie synchronizuje, lub jeśli nie może zalogować się do konta urządzeń Skype są przedstawione w OMS na pulpicie nawigacyjnym Surface Hub. Przy użyciu danych na pulpicie nawigacyjnym, można zidentyfikować urządzenia nie są uruchomione lub są inne problemy i potencjalnie zastosować poprawki dotyczące wykrytych problemów.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązania
Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania. Aby zarządzać z urządzeń Surface Hub z Microsoft Operations Management Suite (OMS), będą potrzebne następujące czynności:

* Prawidłowych subskrypcji umożliwiających [OMS](http://www.microsoft.com/oms).
* [Subskrypcji OMS](https://azure.microsoft.com/pricing/details/log-analytics/) poziomu, która będzie obsługiwać liczbę urządzeń, które chcesz monitorować. Cennik pakietu OMS zmienia się w zależności od liczby urządzeń zarejestrowanych i ilość danych go procesów. Należy to uwzględnić podczas planowania wdrożenia Surface Hub.

Następnie zostanie dodać subskrypcję pakietu OMS do istniejącej subskrypcji Microsoft Azure lub Utwórz nowy obszar roboczy bezpośrednio za pomocą portalu OMS. Szczegółowe instrukcje do przy użyciu jednej z metod jest w [wprowadzenie do analizy dzienników](log-analytics-get-started.md). Po skonfigurowaniu subskrypcji OMS istnieją dwa sposoby zarejestrować swoje urządzenia Surface Hub:

* Automatycznie za pomocą usługi Intune
* Ręcznie za pomocą **ustawienia** urządzenia Surface Hub.

## <a name="set-up-monitoring"></a>Konfigurowanie monitorowania
Można monitorować kondycję i działanie Centrum powierzchni przy użyciu analizy dzienników w OMS. Surface Hub w OMS można zarejestrować za pomocą usługi Intune lub lokalnie przy użyciu **ustawienia** na Surface Hub.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Urządzenia Surface Hub nawiązać połączenia z usługą OMS za pomocą usługi Intune
Wymagany identyfikator i klucz obszaru roboczego dla obszaru roboczego OMS, która ma zarządzać z urządzeń Surface Hub. Możesz pobrać z portalu OMS.

Usługa Intune jest produktem firmy Microsoft, która umożliwia centralne zarządzanie OMS ustawienia konfiguracji, które są stosowane do co najmniej jednego z urządzeń. Wykonaj następujące kroki, aby skonfigurować urządzenia za pomocą usługi Intune:

1. Zaloguj się do usługi Intune.
2. Przejdź do **ustawienia** > **połączone źródła**.
3. Utwórz lub Edytuj zasady na podstawie szablonu Surface Hub.
4. Przejdź do sekcji OMS (Azure Operational Insights), zasad, a następnie dodaj *identyfikator obszaru roboczego* i *klucz obszaru roboczego* z zasadami.
5. Zapisać zasady.
6. Skojarzyć zasady z odpowiedniej grupy urządzeń.

   ![Zasady usługi Intune](./media/log-analytics-surface-hubs/intune.png)

Następnie usługa Intune przeprowadza synchronizację ustawień OMS z urządzeniami w grupie docelowej zarejestrowanie ich w obszarze roboczym pakietu OMS.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Nawiązać urządzeń Surface Hub OMS przy użyciu ustawień aplikacji
Wymagany identyfikator i klucz obszaru roboczego dla obszaru roboczego OMS, która ma zarządzać z urządzeń Surface Hub. Możesz pobrać z portalu OMS.

Jeśli nie używasz usługi Intune do zarządzania używanym środowiskiem, można zarejestrować ręcznie za pomocą urządzenia **ustawienia** na każdym Surface Hub:

1. Z Centrum powierzchni, otwórz **ustawienia**.
2. Wprowadź poświadczenia administratora urządzenia, po wyświetleniu monitu.
3. Kliknij przycisk **to urządzenie**i w obszarze **monitorowanie**, kliknij przycisk **Konfigurowanie ustawień OMS**.
4. Wybierz **Włącz monitorowanie**.
5. W oknie dialogowym Ustawienia OMS wpisz **identyfikator obszaru roboczego** i wpisz **klucz obszaru roboczego**.  
   ![Ustawienia](./media/log-analytics-surface-hubs/settings.png)
6. Kliknij przycisk **OK** w celu ukończenia konfiguracji.

Pojawia się potwierdzenie informujący o tym, czy konfiguracja OMS została pomyślnie zastosowana do urządzenia. Jeśli tak jest, pojawi się komunikat z informacją, że agent pomyślnie nawiązano połączenie z usługą OMS. Urządzenie następnie rozpoczyna wysyłanie danych do OMS, gdzie można przeglądać i działa na nim.

## <a name="monitor-surface-hubs"></a>Monitor urządzenia Surface Hub.
Monitorowanie sieci urządzeń Surface Hub przy użyciu pakietu OMS jest znacznie takich jak monitorowanie innych zarejestrowanych urządzeń.

1. Zaloguj się do portalu OMS.
2. Przejdź do pulpitu nawigacyjnego pakietu rozwiązania Surface Hub.
3. Kondycja urządzenia jest wyświetlana.

   ![Surface Hub pulpitu nawigacyjnego](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Można utworzyć [alerty](log-analytics-alerts.md) oparte na istniejących lub niestandardowych dziennik wyszukiwania. Przy użyciu dane OMS zbiera dane z Twojego urządzeń Surface Hub, możesz wyszukać problemy i alert na warunki, które definiują urządzeń.

## <a name="next-steps"></a>Następne kroki
* Użyj [Zaloguj wyszukiwania analizy dzienników](log-analytics-log-searches.md) Aby wyświetlić szczegółowe dane Surface Hub.
* Utwórz [alerty](log-analytics-alerts.md) do powiadamiania, gdy występują problemy z Twojej urządzeń Surface Hub.
