---
title: "Wprowadzenie do Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
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
ms.date: 09/26/2017
ms.author: terrylan
ms.openlocfilehash: 21415af0d449d639d000e07afdb4de3680a64774
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-security-center"></a>Wprowadzenie do Centrum zabezpieczeń Azure
Uzyskaj informacje na temat Centrum zabezpieczeń Azure, jego kluczowych możliwości i sposobu działania.

## <a name="what-is-azure-security-center"></a>Co to jest Centrum zabezpieczeń Azure?
Centrum zabezpieczeń Azure zapewnia ujednoliconą funkcję zarządzania zabezpieczeniami i advanced threat ochronę obciążeń działających na platformie Azure i lokalnego, a w innych chmur.  Zapewnia wgląd i większą kontrolę nad obciążeń chmury hybrydowej, aktywne zabezpieczenia, które obniżyć poziom narażenia na zagrożenia i inteligentnego wykrywania, aby sprostać szybko rozwijających się cyberattacks.

Omówienie Centrum zabezpieczeń zapewnia zapewnia szybki wgląd w stan zabezpieczeń platformy Azure i obciążeń innych niż Azure, co pozwala na potrzeby odnajdywania i ocenę zabezpieczeń obciążeń i identyfikacji i ograniczenia ryzyka.

![Omówienie](./media/security-center-intro/security-center-intro-fig1.png)

## <a name="why-use-security-center"></a>Dlaczego warto korzystać z usługi Security Center?

**Ujednolicone wgląd i większą kontrolę**

- **Zrozumienie stanu zabezpieczeń między obciążeń hybrydowego**. Zarządzanie zabezpieczeniami we wszystkich hybrydowe chmury obciążeń — lokalnie, Azure i innych platform chmury — w jednej konsoli. Pulpity nawigacyjne wbudowane zapewniają błyskawiczny wgląd w problemy z zabezpieczeniami, które wymagają uwagi.
- **Wgląd w chmurze obciążeń**. Aby dostosować je do szybko ulegają zmianie obciążeń chmury. Automatycznego wykrywania i dodać nowe zasoby utworzone w Twojej subskrypcji platformy Azure.
- **Scentralizowane zarządzanie zasadami**. Centralne zarządzanie zasadami zabezpieczeń we wszystkich obciążeń chmury hybrydowej zapewnienia zgodności z firmy lub wymagania dotyczące zabezpieczeń przepisami.
- **Zabezpieczenia danych z wielu źródeł**. Zbieranie, wyszukiwanie i analizować dane z różnych źródeł, takich jak połączonych rozwiązań partnerskich, takich jak zapory sieciowe i innych usług firmy Microsoft. 
- **Integracja z istniejącą zabezpieczeń przepływy pracy**. Dostęp, integracji i analizowania informacji o zabezpieczeniach, które łączą się z istniejących narzędzi i procesów przy użyciu interfejsów API REST.
- **Raportowanie zgodności**. Umożliwia zabezpieczeń danych i informacje pokazują zgodności oraz łatwe generowanie dowody dla audytorów.

**Zapobieganie adaptacyjną zagrożeń**

- **Ocena ciągłego zabezpieczeń**. Monitorowanie zabezpieczeń maszyny, sieci i usług Azure przy użyciu setki oceny zabezpieczeń lub Utwórz swój własny. Określ oprogramowanie i konfiguracje, które są narażone na atak.
- **Praktyczne zalecenia**. Skoryguj luk w zabezpieczeniach, można było wykorzystać przez osoby atakujące zalecenia dotyczące zabezpieczeń z określonymi priorytetami, można wykonać i playbooks automatyzację przez wbudowany.
- **Formanty adaptacyjną aplikacji**. Blok przed złośliwym oprogramowaniem i innymi aplikacjami niechciane przy zastosowaniu zaleceń listę dozwolonych podobnej dostosowane do określonych obciążeń Azure i obsługiwane przez usługę uczenie maszynowe. 
- **Zabezpieczenia dostępu do sieci**. Zmniejsz sieci ataku just in time, kontrolowany dostęp do zarządzania porty na maszynach wirtualnych Azure, co znacznie zmniejsza podatność na ataki siłowe i inne ataki sieciowe.

**Wykrywanie zagrożeń inteligentnego i odpowiedzi**

- **Analizy zagrożeń najszerszych branży**. Wybierz do firmy Microsoft inteligentnego zabezpieczeń wykresu, którego używa bilionów sygnały z usług firmy Microsoft i systemów na całym świecie do identyfikowania nowych i eliminowaniem nowych zagrożeń bezpieczeństwa.
- **Zaawansowane wykrywanie zagrożeń**. Użyj wbudowanego analizy behawioralnej i uczenia do identyfikowania ataków i luk w zabezpieczeniach dzień zero maszynowego. Monitoruj sieci, maszyny i usługi w chmurze pod kątem przychodzących ataków i działań po naruszeniu zabezpieczeń.
- **Priorytety alertów i zdarzeń**. Skoncentruj się na najważniejszych zagrożenia najpierw z alertów zabezpieczeń uporządkowanych według priorytetu i zdarzenia, które mapują alerty o różnych typach do kampanii ataku. Tworzenie własnych niestandardowych alerty zabezpieczeń również.
- **Uproszczenie dochodzenia**. Szybko ocenić zakresu i skutków ataku przy użyciu środowiska visual, interakcyjne. Użyj wstępnie zdefiniowanych lub zapytań ad hoc dla bardziej Eksploracja danych zabezpieczeń. 
- **Analizy zagrożeń kontekstowe**. Wizualizuj źródła ataków na mapie world interaktywnego. Za pomocą wbudowanych zagrożeń analizy raportów cenne wgląd w technik i cele znanych złośliwych osób.

## <a name="get-started"></a>Rozpoczęcie pracy
Aby rozpocząć pracę z Centrum zabezpieczeń, musisz mieć subskrypcję, do systemu Microsoft Azure. Centrum zabezpieczeń jest włączone w ramach subskrypcji platformy Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). 

[Wprowadzenie do Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-get-started) szybko przeprowadzi Cię przez składniki monitorowania zabezpieczeń i zarządzanie zasadami Centrum zabezpieczeń. 


## <a name="next-steps"></a>Następne kroki
Ten dokument zawiera wprowadzenie do Centrum zabezpieczeń, jego najważniejszych możliwości, a także informacje o sposobie rozpoczęcia pracy. Aby dowiedzieć się więcej, zobacz następujące zasoby:

* [Przewodnik dotyczący planowania i operacji Centrum zabezpieczeń Azure](security-center-planning-and-operations-guide.md) — Dowiedz się, jak zoptymalizować korzystanie z Centrum zabezpieczeń, w oparciu o wymagania dotyczące zabezpieczeń oraz modelu zarządzania chmurą w organizacji.
* [Ustawianie zasad zabezpieczeń](https://docs.microsoft.com/azure/security-center/security-center-policies) — informacje o sposobie konfigurowania zasad zabezpieczeń dla subskrypcji platformy Azure i grup zasobów.
* [Zarządzanie zaleceniami dotyczącymi zabezpieczeń](https://docs.microsoft.com/azure/security-center/security-center-recommendations) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów innych niż Azure oraz Azure.
* [Monitorowanie kondycji zabezpieczeń](https://docs.microsoft.com/azure/security-center/security-center-monitoring) — informacje o sposobie monitorowania kondycji Azure i zasobów innych niż Azure.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) — Dowiedz się, jak reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Monitorowanie rozwiązań partnerskich](https://docs.microsoft.com/azure/security-center/security-center-partner-solutions) — informacje o sposobie monitorowania stanu kondycji rozwiązań partnerskich.
* [Często zadawane pytania dotyczące Centrum zabezpieczeń](https://docs.microsoft.com/azure/security-center/security-center-faq) — Znajdź często zadawane pytania dotyczące korzystania z Centrum zabezpieczeń.


