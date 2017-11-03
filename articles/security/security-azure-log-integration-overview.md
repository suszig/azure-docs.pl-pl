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
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6c3a2ac18fdb7a7a722447af720b9dee28adef08
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Wprowadzenie do integracji dziennika Microsoft Azure
Więcej informacji na temat integracji Azure dziennika, jego kluczowych możliwości i jak działa.

## <a name="overview"></a>Omówienie

Integracja dzienników Azure jest bezpłatna rozwiązania, które pozwala na integrowanie nowych dzienników z zasobów platformy Azure w lokalnych systemów Security Information and Event Management (SIEM).

Integracja dzienników Azure zdarzeń systemu Windows zbiera dzienniki Podglądu zdarzeń systemu Windows, [Dzienniki aktywności Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [alerty Centrum zabezpieczeń Azure](../security-center/security-center-intro.md), i [dzienników diagnostycznych Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z zasobów platformy Azure. Integracja ta pomaga zapewnić jednolity pulpit nawigacyjny dla wszystkich zasobów rozwiązania SIEM, lokalnymi lub w chmurze, dzięki czemu można zagregować skorelowania, analizowanie i alertów zdarzeń zabezpieczeń.

>[!NOTE]
W tej chwili tylko chmury obsługiwane są handlowych Azure oraz Azure dla instytucji rządowych. Inne chmury nie są obsługiwane.

![Integracja dzienników platformy Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować?
Azure tworzy szczegółowe rejestrowanie dla każdej usługi Azure. Te dzienniki stanowią trzy typy dzienników:

* **Dzienniki sterowania i zarządzania nimi** zapewniają wgląd w [usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operacji tworzenia, AKTUALIZOWANIA lub usuwania. Przykładem tego typu dziennika jest Dzienniki aktywności platformy Azure.
* **Dane płaszczyzny dzienniki** zapewniają wgląd w zdarzenia wygenerowane jako część użycia zasobów platformy Azure. Przykładem tego typu dziennika jest Podgląd zdarzeń systemu Windows **systemu**, **zabezpieczeń**, i **aplikacji** kanałów na maszynie wirtualnej systemu Windows. Innym przykładem jest rejestrowania diagnostyki skonfigurowane za pośrednictwem Monitora Azure
* **Przetworzonych zdarzeń** przeanalizowane zdarzeń i informacji o alertach przetwarzane w Twoim imieniu. Przykładem tego typu zdarzenia jest Azure Security Center alertów, gdzie Centrum zabezpieczeń Azure jest przetwarzane i przeanalizowane subskrypcji do alertów istotne dla bieżącej strukturę bezpieczeństwa podaj.

Integracja z usługą Azure dziennika obsługuje ArcSight, QRadar i Splunk. W każdej sytuacji skontaktuj się z dostawcą SIEM w celu oceny, czy mają one natywnego łącznika. W niektórych przypadkach należy nie korzystać z integracji dziennika Azure, jeśli natywnego łączniki nie są dostępne. Dodatkowe informacje na temat obsługiwane typy dziennika należy odwiedziny często zadawane pytania.

>[!NOTE]
Podczas integracji dziennika Azure to rozwiązanie bezpłatne, istnieją magazynu Azure kosztów wynikających z magazynu informacji o pliku dziennika.

Społeczność pomoc jest dostępna za pośrednictwem [Forum MSDN integracji dziennika Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Forum umożliwia społeczności AzLog obsługuje siebie z pytania, odpowiedzi, porady i wskazówki dotyczące sposobu maksymalne wykorzystanie integracji dziennika Azure. Ponadto zespół Azure dziennika integracji monitoruje tym forum i zawierają informacje pomocne przy każdym możemy.

Można również otworzyć [żądania obsługi](../azure-supportability/how-to-create-azure-support-request.md). Aby to zrobić, wybierz **integracji dziennika** jako usługa żądania pomocy technicznej.

## <a name="next-steps"></a>Następne kroki
W tym dokumencie zostały wprowadzone do integracji dzienników Azure. Aby dowiedzieć się więcej na temat integracji Azure dziennika i typów dzienników obsługiwane, zobacz następujące tematy:

* [Microsoft Azure dziennika integracji](https://www.microsoft.com/download/details.aspx?id=53324) — Centrum pobierania, aby uzyskać szczegółowe informacje, wymagania systemowe i zainstalować instrukcje dotyczące integracji dzienników Azure.
* [Wprowadzenie do integracji dzienników Azure](security-azure-log-integration-get-started.md) — w tym samouczku przeprowadzi Cię przez proces instalacji integracji dzienników Azure i integrowanie dzienniki z magazynu Azure WAD, dzienniki aktywności platformy Azure, alerty Centrum zabezpieczeń Azure i usługi Azure Active Directory dzienniki inspekcji.
* [Czynności konfiguracyjnych partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) — ten wpis w blogu przedstawiono sposób konfigurowania integracji dzienników Azure do pracy z rozwiązań partnerskich Splunk HP ArcSight i IBM QRadar. Ten blog reprezentuje nasze bieżące położenie na temat konfigurowania rozwiązań partnerskich. We wszystkich przypadkach zapoznaj się z dokumentacją rozwiązań partnerskich najpierw.
* [Działanie i ASC alertów za pośrednictwem syslog do QRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) — ten wpis w blogu zawiera opis kroków do wysłania do QRadar syslog alerty Centrum zabezpieczeń Azure i działania
* [Dzienników Azure — często zadawane pytania (FAQ) integracji](security-azure-log-integration-faq.md) — to często zadawane pytania dotyczące odpowiedzi na pytania dotyczące integracji dzienników Azure.
* [Integrowanie Centrum zabezpieczeń alertów z usługi Azure dziennika integracji](../security-center/security-center-integrating-alerts-with-log-integration.md) — tym dokumencie przedstawiono sposób synchronizować alerty Centrum zabezpieczeń Azure z integracją dziennika Azure.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
