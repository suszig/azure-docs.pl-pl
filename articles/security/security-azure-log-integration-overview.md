---
title: Integracja dzienników z zasobów platformy Azure z systemów SIEM | Dokumentacja firmy Microsoft
description: Więcej informacji na temat integracji dziennika Azure, jego kluczowych możliwości i jak działa.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6d91692a64a4d3def80990a439fe0a0898bf2f09
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-azure-log-integration"></a>Wprowadzenie do integracji dzienników Azure

Integracja dziennika Azure umożliwia integrowanie nowych dzienników z zasobów platformy Azure z lokalnymi systemami Security Information and Event Management (SIEM). Użyj integracji dziennika Azure tylko wtedy, gdy łącznik do [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) nie jest dostępny od dostawcy rozwiązania SIEM.

Preferowaną metodą całkującej dzienników Azure jest za pomocą łącznika Azure Monitor dostawcą SIEM. Do korzystania z łącznika, postępuj zgodnie z instrukcjami [Monitor strumienia danych monitorowania usługi event hubs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). 

Jednak jeśli dostawcą SIEM nie łącznika do monitorowania Azure, można korzystać z integracji dziennika Azure jako rozwiązanie tymczasowe, aż łącznik jest dostępna. Integracja z usługą Azure dziennika to opcja tylko w przypadku integracji dziennika Azure obsługuje system SIEM.

> [!IMPORTANT]
> Większość dostawców rozwiązań SIEM zainteresowanie podstawowy jest zbierania dzienników maszyny wirtualnej, dołączyć tę opcję w ich rozwiązania. Przy użyciu SIEM dostawcy łącznik jest zawsze preferowanych alternatywnej.

Integracja z usługą Azure dziennika zdarzeń systemu Windows zbiera dzienniki Podglądu zdarzeń systemu Windows, [Dzienniki aktywności Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [alerty Centrum zabezpieczeń Azure](../security-center/security-center-intro.md), i [dzienników diagnostyki Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z Zasoby platformy Azure. Integracja ułatwia rozwiązania SIEM, podaj jednolity pulpit nawigacyjny dla wszystkich zasobów, czy lokalnie lub w chmurze. Pulpit nawigacyjny służy do odbierania, agregacji skorelowania i analizować alerty dla zdarzeń zabezpieczeń.

> [!NOTE]
> Obecnie integracji dziennika Azure obsługuje tylko Azure handlowych i chmury Azure dla instytucji rządowych. Inne chmury nie są obsługiwane.

![Diagram procesu integracji dziennika Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować

Azure tworzy szczegółowe rejestrowanie dla poszczególnych usług Azure. Dzienniki reprezentują trzy typy dziennika:

* **Dzienniki sterowania i zarządzania nimi**: zapewniają wgląd w [usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operacji tworzenia, AKTUALIZOWANIA lub usuwania. Przykładem tego typu dziennika jest dziennik aktywności platformy Azure.
* **Dane płaszczyzny dzienniki**: zapewniają wgląd w zdarzenia, które są wywoływane, gdy używasz zasobów platformy Azure. Przykładem tego typu dziennika jest Podgląd zdarzeń systemu Windows **systemu**, **zabezpieczeń**, i **aplikacji** kanałów na maszynie wirtualnej systemu Windows. Innym przykładem jest rejestrowanie, należy skonfigurować za pośrednictwem Monitora Azure diagnostyki Azure.
* **Przetworzonych zdarzeń**: Podaj przeanalizowane zdarzeń i informacji o alertach, które są przetwarzane. Przykładem tego typu zdarzenia jest alerty Centrum zabezpieczeń Azure. Centrum zabezpieczeń Azure przetwarza i analizuje subskrypcji do alertów, które mają zastosowanie do bieżącego strukturę bezpieczeństwa podaj.

Integracja z usługą Azure dziennika obsługuje ArcSight, QRadar i Splunk. Skontaktuj się z dostawcą SIEM w celu oceny, czy dostawca posiada natywnego łącznika. Nie używaj integracji dziennika Azure, jeśli natywnego łącznika jest dostępna.

Jeśli nie są dostępne żadne inne opcje, należy wziąć pod uwagę przy użyciu integracji z dzienników Azure. W poniższej tabeli przedstawiono Nasze zalecenia:

|SIEM | Klient używa już integrator dzienników Azure | Klient bada opcji integracji SIEM|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Rozpocząć migrację do [dodatek Azure Monitor dla Splunk](https://splunkbase.splunk.com/app/3534/). | Użyj [łącznik Splunk](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Migracja do i rozpocząć korzystanie z łącznika QRadar udokumentowane w ostatniej sekcji [Azure strumienia danych do Centrum zdarzeń do użycia przez monitorowania przez narzędzie zewnętrzne](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). | Korzystania z łącznika QRadar udokumentowane w ostatniej sekcji [Azure strumienia danych do Centrum zdarzeń do użycia przez monitorowania przez narzędzie zewnętrzne](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Nadal używać integrator dzienników Azure, aż łącznik jest dostępna. Następnie przeprowadź migrację do rozwiązania opartego na łącznika.  | Rozważ użycie zamiast Analiza dzienników Azure. Nie są podłączone do integracji dziennika Azure, chyba że chcesz przejść przez proces migracji, po udostępnieniu łącznika. |

> [!NOTE]
> Chociaż integracji dziennika Azure to rozwiązanie bezpłatne, istnieją magazynu Azure koszty związane z magazynu informacji o pliku dziennika.

Jeśli potrzebujesz pomocy, możesz utworzyć [żądania obsługi](../azure-supportability/how-to-create-azure-support-request.md). Dla usługi, wybierz **integracji dziennika**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule przedstawiono integracji dziennika Azure. Aby dowiedzieć się więcej na temat integracji dziennika Azure i typy dzienników, które są obsługiwane, zobacz następujące artykuły:

* [Wprowadzenie do integracji dziennika Azure](security-azure-log-integration-get-started.md). Ten samouczek przeprowadzi Cię przez instalację integracji dziennika Azure. Zawiera również opis integrowanie dzienników z magazynu systemu Windows Azure Diagnostics (WAD), dzienniki aktywności platformy Azure, alerty Centrum zabezpieczeń Azure i dzienników inspekcji usługi Azure Active Directory.
* [Integracja dziennika Azure — często zadawane pytania (FAQ)](security-azure-log-integration-faq.md). Często zadawane pytania odpowiedzi na często zadawane pytania dotyczące integracji dziennika Azure.
* Dowiedz się więcej o sposobie [strumienia Azure monitorowania danych do Centrum zdarzeń do użycia przez narzędzie zewnętrzne](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
