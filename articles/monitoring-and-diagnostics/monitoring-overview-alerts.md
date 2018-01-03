---
title: "Przegląd alertów w Monitorze systemu Azure i usługi Microsoft Azure | Dokumentacja firmy Microsoft"
description: "Alerty umożliwiają monitorowanie metryki zasobów platformy Azure, zdarzeń i dzienniki i otrzymać powiadomienie, gdy spełniony jest warunek, który określisz."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1f0182f27cfb8441a09abd2031b365a4ab4315a
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="what-are-alerts-in-microsoft-azure"></a>Co to są alerty na platformie Microsoft Azure?
W tym artykule opisano różne źródła alertów na platformie Microsoft Azure, jakie są cele dla alertów, ich zalety oraz sposobu wprowadzenie do korzystania z nich. Ten w szczególności dotyczy Azure Monitor, ale zawiera łącza do innych usług z alertami, a także. Alerty oferują metodą monitorowania na platformie Azure, która pozwala na konfigurowanie warunków nad danymi i stają się powiadomienie, gdy warunki pasują do monitorowania najnowszych danych.


## <a name="taxonomy-of-azure-alerts"></a>Taksonomii Azure alertów
Azure używa następujących elementów do opisywania alertów i ich funkcje:
* **Alert** -definicję kryteria (jeden lub więcej reguł lub warunków), która zostanie aktywowany, gdy spełnione.
* **Aktywne** — stan, gdy są spełnione kryteria zdefiniowane przez alert.
* **Rozwiązane** — stan, gdy kryteria zdefiniowane przez alert nie jest już spełniany po wcześniej spełnione.
* **Powiadomienie** — akcję wykonywaną na podstawie wylogowuje alert aktywację.
* **Akcja** -wywołań wysyłane do odbiorcy powiadomienia (na przykład wysyłanie wiadomości e-mail adres lub publikowanie do adresu URL elementu webhook). Powiadomienia zwykle można wyzwolić wiele akcji.

    > [!NOTE]
    > W ramach rozwoju alerty na platformie Azure nowe środowisko unified jest dostępna w wersji zapoznawczej. Nowe środowisko alerty (wersja zapoznawcza) używa innego taksonomii. Dowiedz się więcej o [alertów (wersja zapoznawcza)](monitoring-overview-unified-alerts.md). 
    >

## <a name="alerts-in-different-azure-services"></a>Alerty w różnych usług platformy Azure
Alerty są dostępne przez kilka Azure monitorowanie usług. Aby uzyskać informacje o tym, jak i kiedy należy używać tych usług [znajduje się w artykule](./monitoring-overview.md). W tym miejscu jest podział typów alertów dostępna na Azure:


| Usługa | Typ alertu | Obsługiwane usługi | Opis |
|---|---|---|---|
| Azure Monitor | [Metryki alertów](./insights-alerts-portal.md) | [Obsługiwane metryki z monitora Azure](./monitoring-supported-metrics.md) | Otrzymasz powiadomienie, gdy wszystkie metryki platformy poziomu spełnia określony warunek (na przykład procesora CPU Wynoszące % na maszynie Wirtualnej jest większa niż 90 ostatnich 5 minut). |
|Azure Monitor | [Niemal w czasie rzeczywistym alerty metryki (wersja zapoznawcza)](./monitoring-near-real-time-metric-alerts.md)| [Obsługiwane zasobów Azure monitora](./monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for) | Otrzymasz powiadomienie szybciej niż metryki alerty, gdy co najmniej jedną metrykę platformy poziomu spełnia określone warunki (na przykład jest większa niż 90 procent użycia procesora CPU na maszynie Wirtualnej i sieci w jest większa niż 500 MB przez ostatnich 5 minut). |
| Azure Monitor | [Alerty dziennika aktywności](./monitoring-activity-log-alerts.md) | Wszystkie typy zasobów dostępnych w usłudze Azure Resource Manager | Otrzymasz powiadomienie, gdy dowolne nowe zdarzenie w [dziennika aktywności platformy Azure](./monitoring-overview-activity-logs.md) spełnia określone warunki (na przykład podczas operacji "Usuwanie maszyny Wirtualnej" występuje w myProductionResourceGroup lub po wyświetleniu nowego zdarzenia kondycji usługi z "Active" jako stan). |
| Application Insights | [Metryki alertów](../application-insights/app-insights-alerts.md) | Dowolnej aplikacji narzędzia do wysyłania danych do usługi Application Insights | Otrzymasz powiadomienie, gdy wszystkie metryki poziomu aplikacji spełnia określony warunek (na przykład czas odpowiedzi serwera jest większa niż 2 sekundy). |
| Application Insights | [Alerty testu sieci Web](../application-insights/app-insights-monitor-web-app-availability.md) | Wszystkie witryny sieci Web narzędzia do wysyłania danych do usługi Application Insights | Otrzymasz powiadomienie, gdy dostępność lub czasu odpowiedzi witryny sieci Web jest poniżej oczekiwań. |
| Log Analytics | [Alerty analizy dzienników](../log-analytics/log-analytics-alerts.md) | Każda usługa skonfigurowany do wysyłania danych do analizy dzienników | Otrzymasz powiadomienie, gdy analiza dzienników zapytania wyszukiwania danych metryki i/lub zdarzeń spełnia określone kryteria. |

## <a name="alerts-on-azure-monitor-data"></a>Alerty dotyczące danych monitora Azure
Istnieją trzy typy alertów znajdujące się na nich danych, dostępnej w sklepie Azure Monitor — alerty metryki, obok metryki alertów w czasie rzeczywistym (wersja zapoznawcza) i dziennika aktywności alerty.

* **Alerty metryki** — ten alert jest wyzwalane po wartości progowej, którą należy przypisać przecina wartość określonej metryki. Ten alert generuje powiadomienie, gdy alert jest "aktywny" (po przekroczeniu progu i spełnieniu warunku alertu), a także gdy "Problemu" (po przekroczeniu progu ponownie i nie jest spełniony warunek). Rosnącym listę dostępne metryki obsługiwane przez Azure monitor, zobacz [listy metryki obsługiwane na monitorze Azure](monitoring-supported-metrics.md).
* **Metryki alertów w czasie rzeczywistym (wersja zapoznawcza) w pobliżu** — te alerty są podobne do metryki alertów, ale różnią się na kilka sposobów. Po pierwsze zgodnie z sugestią, nazwa te alerty można wywołać w niemal w czasie rzeczywistym (tak szybko, jak 1 min). One również obsługiwać monitorowanie wielu metryki (obecnie dwa).  Ten alert generuje powiadomienie, gdy alert jest "aktywny" (po przekroczeniu progów dla każdego metryki w tym samym czasie i spełnieniu warunku alertu), a także gdy "Problemu" (gdy co najmniej jedna Metryka przekracza wartość progową ponownie i warunku nie już spełniany).

    > [!NOTE]
    > Niemal w czasie rzeczywistym Metryka alerty są obecnie w wersji zapoznawczej. Funkcje i środowisko użytkownika może ulec zmianie.
    >
    >

* **Alerty dziennika aktywności** -przesyłania strumieniowego alertu dziennika, który uaktywnia jest generowane zdarzenie dziennika aktywności, że dopasowań filtrować kryteria, które zostały przypisane. Te alerty mają tylko jeden stan "Aktywować", ponieważ aparat alertów po prostu dotyczy kryteria filtrowania wszelkie nowe zdarzenie. Te alerty umożliwiają stają się powiadomienie po wystąpieniu nowego zdarzenia kondycji usługi lub gdy użytkownik lub aplikacja wykonuje operację w ramach subskrypcji, na przykład "Usuń maszynę wirtualną".

W przypadku dzienników diagnostycznych danych dostępne za pośrednictwem Monitora Azure Sugerujemy routingu danych do analizy dziennika i przy użyciu alertu analizy dzienników. Poniższy diagram przedstawia źródeł danych w Azure monitora i, koncepcyjnym, jak może generować alerty znajdujące się na nich danych.

![Opis alertów](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>Sposób otrzymywania powiadomień na alert monitora Azure?
W przeszłości Azure alertów z różnych usług używać metod wbudowanych powiadomień. Idąc dalej, Azure Monitor oferuje grupowanie wielokrotnego użytku powiadomienia o nazwie grupy akcji. Grupy akcji określ zestaw odbiorcy powiadomienia — dowolną liczbę adresów e-mail, numerów (SMS) lub adresów URL elementu webhook — uprawnia do zawsze, gdy alert jest aktywny odwołujących się do grupy akcji wszystkich odbiorcy powiadomienia. Dzięki temu można ponownie użyć Grupa odbiorców (na przykład na wywołania odtwarzania listy) przez wiele obiektów alertu. Obecnie tylko alerty dziennik aktywności wykorzystać grupy akcji, ale pracy przy użyciu grup akcji, jak również kilka inne Azure typy alertów.

Grupy akcji powiadomienia są obsługiwane przez publikowanie do adresu URL elementu webhook oprócz adresy e-mail i SMS cyfry. Dzięki temu automatyzacji i korygowania, na przykład przy użyciu:
    - Element Runbook usługi Azure Automation
    - Azure — funkcja
    - Aplikacja logiki platformy Azure
    - usługi innej firmy

Niemal w czasie rzeczywistym Metryka alertów (wersja zapoznawcza) i dziennika aktywności alerty używać grup akcji.

Alerty metryki jeszcze nie należy używać grup akcji. Na poszczególnych metryki alertu można skonfigurować powiadomienia do:
* Wysyłania powiadomień e-mail do administratora usługi, współadministratorów lub adresy e-mail dodatkowych, które określisz.
* Wywołaj element webhook, co pozwala na uruchamianie automatyzacji dodatkowe akcje.

## <a name="next-steps"></a>Kolejne kroki
Uzyskaj informacje o regułach alertów i konfigurowanie ich za pomocą:

* Dowiedz się więcej o [metryk](monitoring-overview-metrics.md)
* Skonfiguruj [Metryka alertów za pośrednictwem portalu Azure](insights-alerts-portal.md)
* Skonfiguruj [Metryka alerty programu PowerShell](insights-alerts-powershell.md)
* Skonfiguruj [interfejsu Metryka alerty z wiersza polecenia (CLI)](insights-alerts-command-line-interface.md)
* Skonfiguruj [Metryka alerty monitora Azure interfejsu API REST](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Dowiedz się więcej o [dziennik aktywności](monitoring-overview-activity-logs.md)
* Skonfiguruj [alerty dziennika aktywności za pośrednictwem portalu Azure](monitoring-activity-log-alerts.md)
* Skonfiguruj [alerty dziennika aktywności za pomocą Menedżera zasobów](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Przegląd [schemat alertu elementu webhook dziennika aktywności](monitoring-activity-log-alerts-webhook.md)
* Dowiedz się więcej o [niemal w czasie rzeczywistym Metryka alertów](monitoring-near-real-time-metric-alerts.md)
* Dowiedz się więcej o [powiadomień usługi](monitoring-service-notifications.md)
* Dowiedz się więcej o [grupy akcji](monitoring-action-groups.md)
* Skonfiguruj [alertów alerty (wersja zapoznawcza)](monitor-alerts-unified-usage.md)
