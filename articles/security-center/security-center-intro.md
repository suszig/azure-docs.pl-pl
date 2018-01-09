---
title: "Co to jest Centrum zabezpieczeń Azure? | Dokumentacja firmy Microsoft"
description: "Uzyskaj informacje na temat Centrum zabezpieczeń Azure, jego kluczowych możliwości i sposobu działania."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 50a54b8d2a73807aa9a0217f7ccf971b8c516494
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="what-is-azure-security-center"></a>Co to jest Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure zapewnia ujednoliconą funkcję zarządzania zabezpieczeniami i advanced threat protection między obciążeń chmury hybrydowej. Z Centrum zabezpieczeń można stosować zasady zabezpieczeń w obciążeń, ograniczyć poziom narażenia na zagrożenia i wykrywania i odpowiadać na ataki.

Dlaczego warto korzystać z usługi Security Center?

- **Scentralizowane zarządzanie zasadami** — zapewnienia zgodności z firmy lub wymagania dotyczące zabezpieczeń przepisami centralne zarządzanie zasadami zabezpieczeń we wszystkich obciążeń chmury hybrydowej.
- **Ocena ciągłego zabezpieczeń** — monitorowanie zabezpieczeń maszyny, sieci, magazynu i danych usługi i aplikacje w celu odnalezienia potencjalnych problemów z zabezpieczeniami.
- **Praktyczne zalecenia** — korygowanie luk w zabezpieczeniach, można było wykorzystać przez osoby atakujące z zalecenia dotyczące zabezpieczeń z określonymi priorytetami i możliwością wykonania akcji.
- **Zaawansowane zabezpieczenia chmury** — zmniejszenie zagrożeń z tylko w czasie dostępu do portów zarządzania i listę dozwolonych podobnej do kontroli aplikacji działających na maszyny wirtualne.
- **Priorytety alertów i zdarzeń** — koncentrowanie się na najważniejszych zagrożenia najpierw z priorytety alerty zabezpieczeń i zdarzeń.
- **Zintegrowanych rozwiązań zabezpieczeń** — zbieranie, wyszukiwania i analizować dane z różnych źródeł, takich jak połączonych rozwiązań partnerskich.

**Centrum zabezpieczeń — omówienie** zapewnia szybki wgląd w stan zabezpieczeń platformy Azure i obciążeń innych niż Azure, co pozwala na potrzeby odnajdywania i ocenę zabezpieczeń obciążeń i identyfikacji i ograniczenia ryzyka. Wbudowane pulpit nawigacyjny zapewnia błyskawiczny wgląd w alerty zabezpieczeń i luk w zabezpieczeniach, które wymagają uwagi.

![Przegląd][1]

## <a name="centralized-policy-management"></a>Scentralizowane zarządzanie
Zasady zabezpieczeń definiują pożądaną konfigurację Twoich obciążeń oraz pomagają zapewnić zgodność z wymaganiami dotyczącymi zabezpieczeń określonymi przez firmę lub przepisy. Centrum zabezpieczeń służy do definiowania zasad i dostosować je do danego typu obciążenia i wrażliwość danych.

Zasady usługi Security Center zawierają następujące składniki:

- **Zbieranie danych**: Określa agenta inicjowania obsługi administracyjnej i zabezpieczeń [zbierania danych](security-center-enable-data-collection.md) ustawienia.
- **Zasady zabezpieczeń**: Określa monitorów Centrum zabezpieczeń oraz zaleca edytując [zasady zabezpieczeń](security-center-policies.md).
- **Wiadomości e-mail z powiadomieniami**: Określa zabezpieczeń kontaktów i [powiadomień e-mail](security-center-provide-security-contact-details.md) ustawienia.
- **Warstwa cenowa**: definiuje wolne lub Standard [cennik wybór](security-center-pricing.md). Wybranej warstwie określa funkcji Centrum zabezpieczeń, które są dostępne dla zasobów w zakresie.

![Zasady zabezpieczeń][2]

Zobacz [Omówienie zasad zabezpieczeń](security-center-policies-overview.md) Aby uzyskać więcej informacji.

## <a name="continuous-security-assessment"></a>Ocena ciągłego zabezpieczeń
Centrum zabezpieczeń analizuje stan zabezpieczeń zasobów obliczeniowych, sieci wirtualnych, magazynu i usług danych i aplikacji. Ciągłej oceny ułatwia odnajdywanie potencjalnych problemów z zabezpieczeniami, takich jak systemy z brakującymi aktualizacjami zabezpieczeń lub narażonych porty. Wybierz Kafelek w sekcji zapobiegania, aby wyświetlić więcej informacji, łącznie z listą zasobów i wszelkie usterki, które zostały zidentyfikowane.

![Monitorowanie kondycji zabezpieczeń][3]

Zobacz [monitorowanie kondycji zabezpieczeń](security-center-monitoring.md) Aby uzyskać więcej informacji.

## <a name="actionable-recommendations"></a>Praktyczne zalecenia
Centrum zabezpieczeń analizuje stan zabezpieczeń platformy Azure i zasobów innych niż Azure do identyfikowania potencjalnych luk w zabezpieczeniach. Lista zaleceń dotyczących zabezpieczeń z określonymi priorytetami prowadzi użytkownika przez proces adresowania problemy z zabezpieczeniami.

![Zalecenia][4]

Zobacz [Zarządzanie zaleceniami dotyczącymi zabezpieczeń](security-center-recommendations.md) Aby uzyskać więcej informacji.

## <a name="just-in-time-vm-access"></a>Dostęp just in time do maszyny wirtualnej
Zmniejsz podatność na ataki sieciowe z tylko w czasie, kontroli dostępu do portów zarządzania na maszynach wirtualnych Azure, co znacznie zmniejsza podatność na ataki siłowe i inne ataki sieciowe.

![Dostęp just in time do maszyny wirtualnej][5]

Określ reguły dla jak użytkownicy mogą łączyć się maszyny wirtualne. W razie potrzeby można żądać dostępu, Centrum zabezpieczeń lub za pomocą programu PowerShell. Jak długo żądanie jest zgodne z zasadami, automatycznie dostęp do żądanego czasu.

Zobacz [zarządzanie dostępem do maszyny wirtualnej przy użyciu tylko w czasie](security-center-just-in-time.md) Aby uzyskać więcej informacji.

## <a name="adaptive-application-controls"></a>Funkcje adaptacyjnego sterowania aplikacjami
Blok przed złośliwym oprogramowaniem i innymi aplikacjami niechciane przy zastosowaniu zaleceń listę dozwolonych podobnej dostosowane do określonych obciążeń Azure i obsługiwane przez usługę uczenie maszynowe.

![Funkcje adaptacyjnego sterowania aplikacjami][6]

Przejrzyj i kliknij, aby zastosować zasady listę dozwolonych podobnej zalecane aplikacji wygenerowane przez Centrum zabezpieczeń lub Edytuj reguły już skonfigurowane.

Zobacz [formanty adaptacyjną aplikacji](security-center-adaptive-application.md) Aby uzyskać więcej informacji.

## <a name="prioritized-alerts-and-incidents"></a>Alertów uszeregowanych według priorytetów i zdarzeń
Centrum zabezpieczeń używa zaawansowane analizy i globalnej analizy zagrożeń wykrywania ataków przychodzące i po naruszyć działania. Alerty są priorytety i zgrupowane w zdarzeń, w myśl można skupić się na najważniejszych zagrożenia najpierw. Możesz utworzyć własne niestandardowe alerty zabezpieczeń również.

![Alertów uszeregowanych według priorytetów i zdarzeń][7]

Możesz szybko oceny zakresu i skutków ataku przy użyciu środowiska visual, interaktywnych dochodzenia i użyć wstępnie zdefiniowanych lub ad hoc zapytań dla bardziej Eksploracja danych zabezpieczeń.

Zobacz [reagowanie na alerty zabezpieczeń i zarządzanie nimi](security-center-managing-and-responding-alerts.md) Aby uzyskać więcej informacji.

## <a name="integrate-your-security-solutions"></a>Integrowanie rozwiązań zabezpieczeń
Można zbierać, wyszukiwanie i analizować dane z różnych źródeł, takich jak połączonych rozwiązań partnerskich, takich jak zapory sieciowe i innych usług firmy Microsoft w Centrum zabezpieczeń.

![Zintegrowanie rozwiązań zabezpieczeń][8]

Zobacz [zintegrowanie rozwiązań zabezpieczeń](security-center-partner-integration.md) Aby uzyskać więcej informacji.

## <a name="next-steps"></a>Kolejne kroki

- Aby rozpocząć pracę z Centrum zabezpieczeń, musisz mieć subskrypcję, do systemu Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).
- Warstwy cenowej bezpłatna Centrum zabezpieczeń jest włączone w Twojej subskrypcji platformy Azure. Aby móc korzystać z zabezpieczeniami zaawansowanymi zarządzania i możliwości wykrywania zagrożeń, musisz najpierw uaktualnić do warstwa cenowa standardowa. Korzystanie z warstwy Standardowa jest bezpłatne przez pierwsze 60 dni. Zobacz [Centrum zabezpieczeń, na stronie dotyczącej cen](https://azure.microsoft.com/pricing/details/security-center/) Aby uzyskać więcej informacji.
- Jeśli wszystko jest gotowe włączyć teraz Security Center Standard [Szybki Start: dołączyć subskrypcją Azure Security Center Standard](security-center-get-started.md) przeprowadzi Cię przez kolejne kroki.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
