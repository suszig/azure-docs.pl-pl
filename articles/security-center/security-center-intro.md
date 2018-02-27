---
title: Co to jest Azure Security Center? | Microsoft Docs
description: "Uzyskaj informacje na temat Centrum zabezpieczeń Azure, jego kluczowych możliwości i sposobu działania."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="what-is-azure-security-center"></a>Co to jest Centrum zabezpieczeń Azure?
Usługa Azure Security Center zapewnia ujednolicone zarządzanie zabezpieczeniami i zaawansowaną ochronę przed zagrożeniami na potrzeby różnych obciążeń chmury hybrydowej. Usługa Security Center umożliwia stosowanie zasad zabezpieczeń do różnych obciążeń, ograniczanie podatności na zagrożenia i wykrywanie ataków oraz reagowanie na nie.

Dlaczego warto korzystać z usługi Security Center?

- **Scentralizowane zarządzanie zasadami** — zapewnianie zgodności z firmowymi lub prawnymi wymaganiami dotyczącymi zabezpieczeń przez centralne zarządzanie zasadami zabezpieczeń we wszystkich obciążeniach chmury hybrydowej.
- **Ciągła ocena zabezpieczeń** — monitorowanie zabezpieczeń maszyn, sieci, magazynu i usług danych oraz aplikacji pod kątem potencjalnych problemów z zabezpieczeniami.
- **Praktyczne zalecenia** — korygowanie luk w zabezpieczeniach, zanim zostaną wykorzystane przez osoby atakujące, dzięki zaleceniom dotyczącym zabezpieczeń z określonymi priorytetami i możliwością wykonywania akcji.
- **Zaawansowane zabezpieczenia chmury** — ograniczenie zagrożeń dzięki dostępowi dokładnie na czas do portów zarządzania i listy dozwolonych na potrzeby kontroli aplikacji działających na maszynach wirtualnych.
- **Alerty i zdarzenia uszeregowane według priorytetów** — zdarzenia i alerty zabezpieczeń uszeregowane według priorytetów pozwalają koncentrować się najpierw na krytycznych zagrożeniach.
- **Zintegrowane rozwiązania w zakresie zabezpieczeń** — zbieranie, wyszukiwanie i analizowanie danych zabezpieczeń z różnych źródeł, w tym połączonych rozwiązań partnerskich.

Okno **Security Center — Przegląd** zapewnia szybki wgląd w stan zabezpieczeń platformy Azure i obciążeń spoza platformy Azure, dzięki czemu można odnajdywać i oceniać zabezpieczenia obciążeń oraz wykrywać i ograniczać ryzyko. Wbudowany pulpit nawigacyjny zapewnia błyskawiczny wgląd w alerty zabezpieczeń i luki w zabezpieczeniach, które wymagają uwagi.

![Omówienie][1]

## <a name="centralized-policy-management"></a>Scentralizowane zarządzanie zasadami
Zasady zabezpieczeń definiują pożądaną konfigurację Twoich obciążeń oraz pomagają zapewnić zgodność z wymaganiami dotyczącymi zabezpieczeń określonymi przez firmę lub przepisy. Usługa Security Center umożliwia definiowanie zasad i dostosowywanie ich do danego typu obciążenia lub poziomu poufności danych.

Zasady usługi Security Center zawierają następujące składniki:

- **Zbieranie danych**: umożliwia określenie ustawień aprowizacji agenta i [zbierania danych](security-center-enable-data-collection.md) zabezpieczeń.
- **Zasady zabezpieczeń**: umożliwia określenie kontrolek monitorowanych i zalecanych przez usługę Security Center przez edytowanie [zasad zabezpieczeń](security-center-policies.md).
- **Wiadomości e-mail z powiadomieniami**: umożliwia określenie ustawień kontaktów i [powiadomień e-mail](security-center-provide-security-contact-details.md) dotyczących zabezpieczeń.
- **Warstwa cenowa**: umożliwia zdefiniowanie [warstwy cenowej](security-center-pricing.md) Bezpłatna lub Standardowa. Wybrana warstwa określa, które funkcje usługi Security Center są dostępne dla zasobów w zakresie.

![Zasady zabezpieczeń][2]

Aby uzyskać więcej informacji, zobacz [Omówienie zasad zabezpieczeń](security-center-policies-overview.md).

## <a name="continuous-security-assessment"></a>Ciągła ocena zabezpieczeń
Usługa Security Center analizuje stan zabezpieczeń zasobów obliczeniowych, sieci wirtualnych, usług magazynu i danych oraz aplikacji. Ciągła ocena ułatwia wykrywanie potencjalnych problemów z zabezpieczeniami, takich jak systemy bez aktualizacji zabezpieczeń lub narażone porty sieciowe. Wybierz kafelek w sekcji Zapobieganie, aby wyświetlić więcej informacji, łącznie z listą zasobów i wykrytych usterek.

![Monitorowanie kondycji zabezpieczeń][3]

Aby uzyskać więcej informacji, zobacz [Monitorowanie kondycji zabezpieczeń](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Zalecenia z możliwością wykonywania akcji
Usługa Security Center analizuje stan zabezpieczeń zasobów platformy Azure i spoza niej w celu identyfikowania potencjalnych luk w zabezpieczeniach. Lista zaleceń dotyczących zabezpieczeń z określonymi priorytetami przeprowadzi Cię przez proces rozwiązywania problemów z zabezpieczeniami.

![Zalecenia][4]

Aby uzyskać więcej informacji, zobacz [Zarządzanie zaleceniami dotyczącymi zabezpieczeń](security-center-recommendations.md).

## <a name="just-in-time-vm-access"></a>Dostęp do maszyny wirtualnej dokładnie na czas
Zmniejsz obszar narażony na ataki sieciowe dzięki kontrolowanemu dostęp dokładnie na czas do portów zarządzania na maszynach wirtualnych platformy Azure, aby znacząco ograniczyć narażenie na ataki metodą pełnego przeglądu i inne ataki sieciowe.

![Dostęp do maszyny wirtualnej dokładnie na czas][5]

Możesz określać reguły dotyczące sposobu łączenia się użytkowników z maszynami wirtualnymi. W razie potrzeby możesz żądać dostępu za pomocą usługi Security Center lub programu PowerShell. Dopóki żądanie jest zgodne z zasadami, dostęp jest udzielany automatycznie na żądany czas.

Aby uzyskać więcej informacji, zobacz [Manage virtual machine access using just in time](security-center-just-in-time.md) (Zarządzanie dostępem maszyny wirtualnej przy użyciu funkcji „dokładnie na czas”).

## <a name="adaptive-application-controls"></a>Funkcje adaptacyjnego sterowania aplikacjami
Blokuj złośliwe oprogramowanie i inne niechciane aplikacje, stosując zalecenia dotyczące listy dozwolonych dostosowane do konkretnych obciążeń platformy Azure i obsługiwane przez uczenie maszynowe.

![Funkcje adaptacyjnego sterowania aplikacjami][6]

Przejrzyj i kliknij, aby zastosować zalecane reguły listy dozwolonych aplikacji wygenerowane przez usługę Security Center lub edytować skonfigurowane już reguły.

Aby uzyskać więcej informacji, zobacz [Funkcje adaptacyjnego sterowania aplikacjami](security-center-adaptive-application.md).

## <a name="prioritized-alerts-and-incidents"></a>Alerty i zdarzenia uszeregowane według priorytetów
Usługa Security Center wykrywa przychodzące ataki i działania po naruszeniu zabezpieczeń za pomocą zaawansowanych funkcji analizy i globalnej analizy zagrożeń. Alerty są uszeregowane według priorytetów i pogrupowane według zdarzeń, aby ułatwić skoncentrowanie się najpierw na najbardziej krytycznych zagrożeniach. Możesz też tworzyć własne niestandardowe alerty zabezpieczeń.

![Alerty i zdarzenia uszeregowane według priorytetów][7]

Możesz szybko ocenić zakres i wpływ ataku przy użyciu wizualnego i interakcyjnego środowiska badawczego oraz użyć wstępnie zdefiniowanych zapytań lub zapytań ad hoc na potrzeby bardziej zaawansowanej eksploracji danych zabezpieczeń.

Aby uzyskać więcej informacji, zobacz [Reagowanie na alerty zabezpieczeń i zarządzanie nimi](security-center-managing-and-responding-alerts.md).

## <a name="integrate-your-security-solutions"></a>Integrowanie rozwiązań w zakresie zabezpieczeń
W usłudze Security Center możesz zbierać, wyszukiwać i analizować dane zabezpieczeń z różnych źródeł, w tym połączonych rozwiązań partnerskich, takich jak zapory sieciowe i inne usługi firmy Microsoft.

![Integrowanie rozwiązań w zakresie zabezpieczeń][8]

Aby uzyskać więcej informacji, zobacz [Integrowanie rozwiązań w zakresie zabezpieczeń](security-center-partner-integration.md).

## <a name="next-steps"></a>Następne kroki

- Do rozpoczęcia pracy z usługą Security Center jest wymagana subskrypcja platformy Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).
- Usługa Security Center w warstwie cenowej Bezpłatna jest włączona w ramach subskrypcji platformy Azure. Aby móc korzystać z zaawansowanych możliwości wykrywania zagrożeń i zarządzania zabezpieczeniami, musisz przeprowadzić uaktualnienie do warstwy cenowej Standardowa. Korzystanie z warstwy Standardowa jest bezpłatne przez pierwszych 60 dni. Aby uzyskać więcej informacji, zobacz [stronę z cennikiem usługi Security Center](https://azure.microsoft.com/pricing/details/security-center/).
- Jeśli możesz już włączyć usługę Security Center w warstwie Standardowa, skorzystaj z przewodnika [Szybki start: dołączanie subskrypcji platformy Azure do standardowej usługi Security Center](security-center-get-started.md), który przeprowadzi Cię przez ten proces.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
