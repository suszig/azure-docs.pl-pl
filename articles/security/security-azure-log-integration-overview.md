---
title: "Integrowanie dzienników z zasobów platformy Azure z systemów SIEM | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat integracji Azure dziennika, jego kluczowych możliwości i jak działa."
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
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Wprowadzenie do integracji dziennika Microsoft Azure

Integracja dzienników Azure pozwala na integrowanie nowych dzienników z zasobów platformy Azure z lokalnymi systemami Security Information and Event Management (SIEM) w przypadku gdy łącznika do [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) nie jest jeszcze dostępna z Dostawca SIEM.

Preferowaną metodą integrowanie dzienników Azure jest za pomocą łącznika Azure Monitor dostawcą SIEM i wykonując te [instrukcje](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). Jednak jeśli dostawcą SIEM nie łącznika do monitorowania Azure, można korzystać z usługi Azure dziennika integracji jako rozwiązanie tymczasowe (Jeśli system SIEM jest obsługiwana przez integrację dziennika Azure) do czasu udostępnienia łącznika programu.

>[!IMPORTANT]
>Jeśli zainteresowanie podstawowego do zbierania dzienników maszyny wirtualnej, większość dostawców rozwiązań SIEM dołączyć to ich rozwiązania. Przy użyciu SIEM dostawcy łącznik powinien zawsze mieć preferowanych alternatywnej.

Integracja dzienników Azure zdarzeń systemu Windows zbiera dzienniki Podglądu zdarzeń systemu Windows, [Dzienniki aktywności Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [alerty Centrum zabezpieczeń Azure](../security-center/security-center-intro.md), i [dzienników diagnostycznych Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z zasobów platformy Azure. Integracja ta pomaga zapewnić jednolity pulpit nawigacyjny dla wszystkich zasobów rozwiązania SIEM, lokalnymi lub w chmurze, dzięki czemu można zagregować skorelowania, analizowanie i alertów zdarzeń zabezpieczeń.

>[!NOTE]
W tej chwili tylko chmury obsługiwane są handlowych Azure oraz Azure dla instytucji rządowych. Inne chmury nie są obsługiwane.

![Integracja dzienników platformy Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować?

Azure tworzy szczegółowe rejestrowanie dla każdej usługi Azure. Te dzienniki stanowią trzy typy dzienników:

* **Dzienniki sterowania i zarządzania nimi** zapewniają wgląd w [usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operacji tworzenia, AKTUALIZOWANIA lub usuwania. Przykładem tego typu dziennika jest Dzienniki aktywności platformy Azure.
* **Dane płaszczyzny dzienniki** zapewniają wgląd w zdarzenia wygenerowane jako część użycia zasobów platformy Azure. Przykładem tego typu dziennika jest Podgląd zdarzeń systemu Windows **systemu**, **zabezpieczeń**, i **aplikacji** kanałów na maszynie wirtualnej systemu Windows. Innym przykładem jest rejestrowania diagnostyki skonfigurowane za pośrednictwem Monitora Azure
* **Przetworzonych zdarzeń** przeanalizowane zdarzeń i informacji o alertach przetwarzane w Twoim imieniu. Przykładem tego typu zdarzenia jest Azure Security Center alertów, gdzie Centrum zabezpieczeń Azure jest przetwarzane i przeanalizowane subskrypcji do alertów istotne dla bieżącej strukturę bezpieczeństwa podaj.

Integracja z usługą Azure dziennika obsługuje ArcSight, QRadar i Splunk. We wszystkich przypadkach skontaktować z dostawcą SIEM do oceny, czy mają one natywnego łącznika. Nie należy używać Azure dziennika integracji dostępnych łączników macierzystego.

Jeśli nie są dostępne nie inne opcje, można rozważyć integracji dziennika Azure. W poniższej tabeli przedstawiono Nasze zalecenia.

|**SIEM** | **Już za pomocą integratora dziennika klienta** | **Badanie opcji integracji SIEM klienta**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Rozpocząć migrację do [dodatek Azure monitora dla Splunk](https://splunkbase.splunk.com/app/3534/) | Użyj [SPLUNK łącznika](https://splunkbase.splunk.com/app/3534/) |
|**IBM QRADAR** | Migracja do i rozpocząć korzystanie z łącznika QRadar udokumentowane w końcu http://aka.ms/azmoneventhub | Korzystania z łącznika QRadar udokumentowane w końcu http://aka.ms/azmoneventhub  |
|**ARCSIGHT** | Nadal używać Integrator dziennika, aż do łącznika jest dostępny, a następnie przeprowadzić migrację do rozwiązania opartego na łącznika.  | Alternatywnie, należy wziąć pod uwagę Analiza dzienników Azure. Czy nie dołączyć do integracji dziennika Azure, chyba że chcesz przejść przez proces migracji, po udostępnieniu łącznika. |

>[!NOTE]
>Podczas integracji dziennika Azure to rozwiązanie bezpłatne, istnieją magazynu Azure kosztów wynikających z magazynu informacji o pliku dziennika.

Jeśli potrzebujesz pomocy można otworzyć [żądania obsługi](../azure-supportability/how-to-create-azure-support-request.md). Aby to zrobić, wybierz **integracji dziennika** jako usługa żądania pomocy technicznej.

## <a name="next-steps"></a>Kolejne kroki

W tym dokumencie zostały wprowadzone do integracji dzienników Azure. Aby dowiedzieć się więcej na temat integracji Azure dziennika i typów dzienników obsługiwane, zobacz następujące tematy:

* [Wprowadzenie do integracji dzienników Azure](security-azure-log-integration-get-started.md) — w tym samouczku przeprowadzi Cię przez proces instalacji integracji dzienników Azure i integrowanie dzienniki z magazynu Azure WAD, dzienniki aktywności platformy Azure, alerty Centrum zabezpieczeń Azure i usługi Azure Active Directory dzienniki inspekcji.
* [Dzienników Azure — często zadawane pytania (FAQ) integracji](security-azure-log-integration-faq.md) — to często zadawane pytania dotyczące odpowiedzi na pytania dotyczące integracji dzienników Azure.
* [Azure strumienia danych do Centrum zdarzeń do użycia przez monitorowania przez narzędzie zewnętrzne](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
