---
title: "Metryki alertów w czasie rzeczywistym w Monitorze systemu Azure w pobliżu | Dokumentacja firmy Microsoft"
description: "Niemal w czasie rzeczywistym Metryka alerty pozwalają monitorować nawet 1 min metryki zasobów platformy Azure."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: aeeb6c2fb87e6c19991ef243ee7230f4e8f4e251
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza)
Azure Monitor obsługuje obecnie nowy typ metryki alertów o nazwie niemal w czasie rzeczywistym Metryka alerty (wersja zapoznawcza). Ta funkcja jest obecnie w wersji zapoznawczej.
Te alerty różnią się od regularne alerty metryki na kilka sposobów

- **Ulepszone opóźnienia** -niemal w czasie rzeczywistym Metryka alertów można monitorować zmiany w wartości metryki tak szybko, jak 1 min.
- **Większa kontrola nad metryki warunki** -niemal w czasie rzeczywistym Metryka alerty Zezwalaj użytkownikom na definiowanie bardziej rozbudowane reguły alertów. Alerty są teraz obsługiwane monitorowanie maksymalnej, minimalnej, średni i całkowitej wartości metryki.
- **Połączone monitorowanie wielu metryki** -niemal w czasie rzeczywistym Metryka alertów można monitorować metryki wielu (obecnie dwa) przy użyciu jednej reguły. Alert pobiera uruchomiony, jeśli obie metryki naruszyć ich odpowiednich progów w określonym przedziale czasu.
- **System powiadomień moduły** — niemal w czasie rzeczywistym alerty metryki użycia [grupy akcji](monitoring-action-groups.md). Ta funkcja umożliwia użytkownikom tworzenie akcji w sposób moduły i użyj ich ponownie dla wielu reguł alertów.

> [!NOTE]
> Niemal w czasie rzeczywistym alerty metryki funkcja jest obecnie w wersji zapoznawczej. Funkcje i środowisko użytkownika może ulec zmianie.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Jakie zasoby można tworzyć niemal w czasie rzeczywistym alerty metryki dla?
Pełną listę typów zasobów, które są obsługiwane przez niemal w czasie rzeczywistym alerty metryki:

* Microsoft.ApiManagement/service
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Sql/servers/elasticpools
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.Timeseriesinsights
* Microsoft.CognitiveServices/accounts


## <a name="create-a-near-real-time-metric-alert"></a>Utwórz Alert Near metryki w czasie rzeczywistym
Obecnie niemal w czasie rzeczywistym Metryka alertów można tworzyć tylko za pośrednictwem portalu Azure. Obsługa konfigurowania niemal metryki alertów w czasie rzeczywistym za pomocą programu PowerShell, interfejsu wiersza polecenia (CLI) i interfejsu API REST Monitor Azure będzie dostępna wkrótce.

1. W [portal](https://portal.azure.com/)zasobów planuje się monitorowanie Znajdź i zaznacz go. Ten zasób powinien być jednego z typów zasobów wymienionych w [poprzedniej sekcji](#what-resources-can-i-create-near-real-time-metric-alerts-for). Możesz również wykonać takie same dla wszystkich typów zasobów obsługiwanych centralnie z monitora > alerty.

2. Wybierz **alerty** lub **reguły alertów** w sekcji monitorowanie. Tekst i ikona mogą się nieco różnić dla różnych zasobów.
   ![Monitorowanie](./media/insights-alerts-portal/AlertRulesButton.png)

3. Kliknij przycisk **Dodaj alert metryki czasu rzeczywistego (wersja zapoznawcza) o zbliżającym się** polecenia. Jeśli polecenie jest niedostępny, upewnij się, że wybrano zasobu w filtrze.

    ![Dodaj obok przycisku alertu metryk w czasie rzeczywistym](./media/monitoring-near-real-time-metric-alerts/AddNRTAlertButton.png)

4. **Nazwa** alertu reguły, a następnie wybierz pozycję **opis**, który pokazuje również w wiadomości e-mail z powiadomieniem.
5. Wybierz **Metryka** chcesz monitorować, a następnie wybierz pozycję **warunku**, **Agregacja czasu**, i **próg** wartość metryki. Opcjonalnie wybierz inny **Metryka** chcesz monitorować, a następnie wybierz pozycję **warunku**, **Agregacja czasu**, i **próg** wartość druga metryka. 

    ![Dodaj niemal w czasie rzeczywistym metryki Alert1](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert1.png) ![dodać niemal Alert2 metryk w czasie rzeczywistym](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert2.png)
6. Wybierz **okres** czas, przez który metryki reguły muszą zostać spełnione przed wyzwalaczy alertu. Tak na przykład jeśli używasz okresu "w ciągu ostatnich 5 minut" i Twój wygląd alertów dla Procesora powyżej 80% (i NetworkIn ponad 500 MB), alert jest wyzwalane po procesora CPU było stale powyżej 80% 5 minut. W momencie to pierwszy wyzwalacz, ponownie uruchamia to, gdy Procesora pozostaje poniżej 80% 5 minut. Alert jest obliczane zgodnie z **częstotliwość oceny**


6. Wybierz odpowiednie **ważność** z listy rozwijanej.

7. Określ, czy użyć nowego lub istniejącego **grupy akcji**.

8. Jeśli wybierzesz opcję utworzenia **nowy** nadać grupie akcji, nazwy i krótka nazwa grupy akcji, określ akcje (programu SMS, wiadomości E-mail, Webhook) i wypełnij odpowiednie szczegóły.


8. Wybierz **OK** po zakończeniu można utworzyć alertu.   

W ciągu kilku minut alert jest aktywny i wyzwala w sposób opisany wcześniej.

## <a name="managing-near-real-time-metric-alerts"></a>Zarządzanie niemal w czasie rzeczywistym metryki alertów
Po utworzeniu alertu, zostanie ona wybrana oraz:

* Wyświetl wykres przedstawiający próg metryki i rzeczywistymi wartościami z poprzedniego dnia.
* Edytuj lub usuń go.
* **Wyłącz** lub **włączyć** go, jeśli chcesz tymczasowo zatrzymać lub wznowić odbieranie powiadomień dla tego alertu.



