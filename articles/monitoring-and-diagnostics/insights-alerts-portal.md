---
title: "Utwórz alerty dla usług Azure - Azure portal | Dokumentacja firmy Microsoft"
description: "Wyzwalacz wiadomości e-mail, powiadomienia, Wywołaj adresy URL witryny sieci Web (elementy webhook) lub automatyzacji po spełnieniu warunków, które określisz."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 3e09c145d35665ec1c2467b60f06191ac51a5c16
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>W monitorze Azure tworzyć alerty metryki dla usługi Azure - Azure portal
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [Program PowerShell](insights-alerts-powershell.md)
> * [Interfejs wiersza polecenia](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Omówienie
W tym artykule przedstawiono sposób konfigurowania Azure metryki alertów za pomocą portalu Azure. 

Możesz otrzymywać alertu na podstawie metryki monitorowania lub zdarzenia na usługami Azure.

* **Wartości metryki** — uruchamia alert, gdy wartość określonej metryki przekracza próg przypisać w żadnym kierunku. Oznacza to, że oba wyzwala po spełnieniu warunku zostanie najpierw i następnie później podczas warunku jest już spełniane.    
* **Zdarzenia dziennika aktywności** — alert może wyzwolić na *co* zdarzenia lub tylko wtedy, gdy występuje określone zdarzenia. Dowiedz się więcej o [alertów dotyczących działań w dzienniku](monitoring-activity-log-alerts.md).

Można skonfigurować metryki alert do wyzwala, wykonaj następujące czynności:

* wysyłanie powiadomień e-mail do administratora usługi i współadministratorzy
* Wyślij wiadomość e-mail do dodatkowych wiadomości e-mail przez użytkownika.
* Wywołanie elementu webhook
* Uruchamia wykonywanie elementów runbook platformy Azure (tylko z portalu Azure)

> [!NOTE]
> Azure Monitor obsługuje teraz niemal w czasie rzeczywistym Metryka alertów w publicznej wersji zapoznawczej. Użyj tych grup akcji. Dowiedz się więcej o [niemal w czasie rzeczywistym Metryka alerty](monitoring-near-real-time-metric-alerts.md).
>
>

Można skonfigurować i uzyskać informacje na temat metryki reguły alertów za pomocą

* [Witryna Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Interfejs wiersza polecenia (CLI)](insights-alerts-command-line-interface.md)
* [Interfejs API REST Azure monitora](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Tworzenie reguły alertu na Metryka z portalu Azure
1. W [portal](https://portal.azure.com/)zasobów planuje się monitorowanie Znajdź i zaznacz go.

2. Wybierz **alerty** lub **reguły alertów** w sekcji monitorowanie. Tekst i ikona mogą się nieco różnić dla różnych zasobów.  

    ![Monitorowanie](./media/insights-alerts-portal/AlertRulesButton.png)

3. Wybierz **Dodaj alert** poleceń i wypełnij pola.

    ![Dodaj alert](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nazwa** alertu reguły, a następnie wybierz pozycję **opis**, który pokazuje również w wiadomości e-mail z powiadomieniem.

5. Wybierz **Metryka** chcesz monitorować, a następnie wybierz pozycję **warunku** i **próg** wartość metryki. Wybierz również **okres** czas, przez który metryki reguły muszą zostać spełnione przed wyzwalaczy alertu. Tak na przykład jeśli używasz okresu "w ciągu ostatnich 5 minut" i alertu szuka procesora CPU przekracza 80%, alert jest wyzwalane po procesora CPU było stale powyżej 80% 5 minut. W momencie to pierwszy wyzwalacz, ponownie uruchamia to, gdy Procesora pozostaje poniżej 80% 5 minut. Metryki pomiaru Procesora występuje co minutę.

6. Sprawdź **E-mail właścicieli...**  Jeśli chcesz, aby administratorzy i współadministratorzy w celu przesłania pocztą e-mail po zgłoszeniu alertu.

7. Jeśli chcesz, dodatkowe wiadomości e-mail, aby otrzymać powiadomienie po zgłoszeniu alertu, dodaj je w **email(s) dodatkowe administratora** pola. Wiele wiadomości e-mail należy rozdzielić średnikami -  *email@contoso.com;email2@contoso.com*

8. Umieść w prawidłowym identyfikatorem URI w **Webhook** jeśli ma ona wywoływana po zgłoszeniu alertu.

9. Jeśli używasz usługi Automatyzacja Azure, możesz wybrać element Runbook do uruchomienia po zgłoszeniu alertu.

10. Wybierz **OK** po zakończeniu można utworzyć alertu.   

W ciągu kilku minut alert jest aktywny i wyzwala w sposób opisany wcześniej.

## <a name="managing-your-alerts"></a>Zarządzanie alertami
Po utworzeniu alertu, zostanie ona wybrana oraz:

* Wyświetl wykres przedstawiający próg metryki i rzeczywistymi wartościami z poprzedniego dnia.
* Edytuj lub usuń go.
* **Wyłącz** lub **włączyć** go, jeśli chcesz tymczasowo zatrzymać lub wznowić odbieranie powiadomień dla tego alertu.

## <a name="next-steps"></a>Następne kroki
* [Omówienie monitorowania Azure](monitoring-overview.md) w tym typy informacji, można zbierać i monitorowania.
* Dowiedz się więcej o nowe [pobliżu metryki alertów w czasie rzeczywistym (wersja zapoznawcza)](monitoring-near-real-time-metric-alerts.md)
* Dowiedz się więcej o [konfigurowaniu elementów webhook w alertach](insights-webhooks-alerts.md).
* Dowiedz się więcej o [konfigurowania alertów na zdarzenia dziennika aktywności](monitoring-activity-log-alerts.md).
* Dowiedz się więcej o [elementów Runbook automatyzacji Azure](../automation/automation-starting-a-runbook.md).
* Pobierz [Przegląd dzienników diagnostycznych](monitoring-overview-of-diagnostic-logs.md) i zbieranie szczegółowych metryki wysokiej częstotliwości w usłudze.
* Pobierz [omówienie zbierania metryk](insights-how-to-customize-monitoring.md) się upewnić, że usługa jest dostępna i elastyczny.
