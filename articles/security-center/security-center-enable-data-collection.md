---
title: "Zbieranie danych w Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: " Informacje o włączaniu zbierania danych w Centrum zabezpieczeń Azure. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 226fc82abf7aa24a0aa1bd3c21279158e1ce8e95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="data-collection-in-azure-security-center"></a>Zbieranie danych w Centrum zabezpieczeń Azure
Centrum zabezpieczeń zbiera dane z maszyn wirtualnych platformy Azure (maszyny wirtualne) i komputerów z systemem innym niż Azure monitorowanie luk w zabezpieczeniach i zagrożeń. Dane są zbierane za pomocą programu Microsoft Monitoring Agent, która odczytuje różnych konfiguracji związanych z zabezpieczeniami i dzienniki zdarzeń z komputera i kopiuje dane do swojego obszaru roboczego do analizy. Przykłady takich danych to typ systemu operacyjnego i jego wersja, dzienniki systemu operacyjnego (dzienniki zdarzeń systemu Windows), uruchomione procesy, nazwa maszyny, adresy IP, zalogowany użytkownik i identyfikator dzierżawy. Microsoft Monitoring Agent kopiuje pliki zrzutu awaryjnego do swojego obszaru roboczego.

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Włącz automatyczne Inicjowanie obsługi programu Microsoft Monitoring Agent     
Automatyczne inicjowanie obsługi administracyjnej jest włączona, przepisy Centrum zabezpieczeń firmy Microsoft Monitoring Agent na wszystkich obsługiwanych maszyn wirtualnych platformy Azure i nowe pliki, które są tworzone. Automatyczne udostępnianie zdecydowanie zaleca się i jest wymagany dla subskrypcji w warstwie standardowa Centrum zabezpieczeń.

> [!NOTE]
> Wyłączanie automatycznego inicjowania obsługi administracyjnej limitów monitorowania zabezpieczeń dla zasobów. Aby dowiedzieć się więcej, zobacz [Wyłącz automatyczne udostępnianie](security-center-enable-data-collection.md#disable-automatic-provisioning) w tym artykule. Migawki dysków maszyny Wirtualnej i kolekcji artefaktu są włączone, nawet jeśli jest wyłączona, automatyczne udostępnianie.
>
>

Aby włączyć automatyczne Inicjowanie obsługi programu Microsoft Monitoring Agent:
1. W menu głównym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**.
2. Wybierz subskrypcję.
3. W obszarze **zasady zabezpieczeń**, wybierz pozycję **zbierania danych**.
4. W obszarze **dołączania**, wybierz pozycję **na** Aby włączyć automatyczne udostępnianie.
5. Wybierz pozycję **Zapisz**.

![Włącz automatyczne Inicjowanie obsługi][1]

## <a name="default-workspace-configuration"></a>Domyślna konfiguracja obszaru roboczego
Dane zebrane przez Centrum zabezpieczeń jest przechowywany w analizy dzienników obszarów roboczych.  Możesz zdecydować się na dane zbierane z maszyn wirtualnych platformy Azure, przechowywane w obszary robocze tworzone przez Centrum zabezpieczeń lub istniejący obszar roboczy utworzony.

Aby użyć istniejącego obszaru roboczego analizy dzienników:
- Obszar roboczy musi być skojarzony z wybranej subskrypcji Azure.
- Co najmniej użytkownik musi mieć uprawnienia odczytu dostępu do obszaru roboczego.

Aby wybrać istniejący obszar roboczy analizy dzienników:

1. W obszarze **zasady zabezpieczeń — zbieranie danych**, wybierz pozycję **Użyj innego obszaru roboczego**.

   ![Wybierz istniejący obszar roboczy][2]

2. Z menu rozwijanego wybierz obszar roboczy do zapisywania zebranych danych.

> [!NOTE]
> W ściągania menu rozwijane są wyświetlane tylko obszarów roboczych, które mają dostęp do i znajdują się w ramach subskrypcji platformy Azure.
>
>

3. Wybierz pozycję **Zapisz**.
4. Po wybraniu **zapisać**, użytkownik zostanie poproszony, jeśli chcesz monitorować ponownej konfiguracji maszyn wirtualnych.

   - Wybierz **nr** Jeśli chcesz, aby nowe ustawienia obszaru roboczego do zastosowania na tylko nowych maszyn wirtualnych. Nowe ustawienia obszaru roboczego dotyczą tylko nowe instalacje agentów; nowo wykryte maszyny wirtualne, które nie mają zainstalowany program Microsoft Monitoring Agent.
   - Wybierz **tak** Jeśli chcesz, aby nowe ustawienia obszaru roboczego do zastosowania na wszystkich maszynach wirtualnych. Ponadto każdej maszyny Wirtualnej podłączone do Centrum zabezpieczeń, obszar roboczy utworzony ponownie nawiązał połączenie nowy docelowy obszar roboczy.

   > [!NOTE]
   > Jeśli wybierzesz tak, nie można usuwać obszarów roboczych, utworzonych przez Centrum zabezpieczeń, dopóki wszystkie maszyny wirtualne mają został ponownie podłączony do nowego docelowego obszaru roboczego. Ta operacja nie powiedzie się, zbyt wcześnie usuniętego obszaru roboczego.
   >
   >

   - Wybierz **anulować** Aby anulować operację.

   ![Wybierz istniejący obszar roboczy][3]

## <a name="data-collection-tier"></a>Warstwa danych kolekcji
Centrum zabezpieczeń może zmniejszyć ilość danych zdarzeń przy zachowaniu za mało zdarzeń do badania, inspekcji i wykrywanie zagrożeń. Można wybrać prawa filtrowania zasady dla subskrypcji i obszarów roboczych z czterech zestawów zdarzeń mają zostać zebrane przez agenta.

- **Wszystkie zdarzenia** — dla klientów, którzy chcą, aby upewnić się, że wszystkie zdarzenia są zbierane. Jest to wartość domyślna.
- **Typowe** — jest to zestaw zdarzeń spełniający większość klientów i umożliwia ich z wersji próbnej w pełną inspekcji.
- **Minimalny** — mniejszy zestaw zdarzeń dla klientów, którzy minimalizowania woluminu zdarzeń.
- **Brak** — Wyłącz zbieranie zdarzeń zabezpieczeń z dzienniki funkcji AppLocker i zabezpieczeń. Dla klientów, którzy wybierz tę opcję pulpity nawigacyjne ich zabezpieczeń ma tylko dzienniki zapory systemu Windows i oceny aktywnego, takich jak ochrony przed złośliwym kodem, linii bazowej i aktualizacji.

> [!NOTE]
> Te zestawy zaprojektowano w celu rozwiązania typowych scenariuszy. Upewnij się ocenić, która z nich potrzebom użytkownika przed jego wdrożeniem.
>
>

Aby określić zdarzenia, które będą należeć do **typowe** i **minimalnego** zestawów zdarzeń, możemy doświadczenie z klientów i standardy branżowe, aby dowiedzieć się więcej o częstotliwości niefiltrowane każdego zdarzenia i ich użycia. Poniższe wskazówki zostały użyte podczas tego procesu:

- **Minimalny** — upewnij się, że ten zestaw obejmuje tylko zdarzenia, które mogą wskazywać pomyślne naruszenia i ważnych wydarzeń, które mają bardzo niskim poziomie. Na przykład ten zestaw zawiera udane i nieudane logowania użytkownika (zdarzenie 4624 identyfikatorów 4625), ale nie zawiera on wylogowania, co jest ważne w przypadku inspekcji, ale nie ma istotnego znaczenia do wykrywania i ma stosunkowo dużej liczby. Większość ilość danych tego zestawu jest zdarzenia logowania i procesu tworzenia zdarzeń (event ID 4688).
- **Typowe** — Podaj dziennik inspekcji logowania, w tym zestawie. Na przykład ten zestaw zawiera identyfikatory logowania użytkownika i wylogowywania użytkowników (zdarzenie ID 4634). Firma Microsoft obejmują akcje, takie jak zmiany grupy zabezpieczeń, operacje Kerberos kontrolera domeny klucza i inne zdarzenia, które są zalecane przez organizacje z branży inspekcji.

Zdarzenia, które mają bardzo niskim poziomie zostały uwzględnione we wspólnym Ustaw jako głównym celem wybranie wszystkich zdarzeń jest zmniejszyć wolumin, a nie odfiltrowywania określonych zdarzeń.

Poniżej przedstawiono pełną podział zabezpieczeń i funkcji AppLocker identyfikatory zdarzeń dla każdego zestawu:

   ![Identyfikatory zdarzeń][4]

Aby wybrać zasady filtrowania:
1. Na **ustawienia i zasady zabezpieczeń** bloku, wybierz filtrowania zasad w obszarze **zdarzenia zabezpieczeń**.
2. Wybierz pozycję **Zapisz**.

   ![Wybierz zasady filtrowania][5]

## <a name="disable-automatic-provisioning"></a>Wyłącz automatyczne udostępnianie
Możesz wyłączyć automatyczne Inicjowanie obsługi administracyjnej z zasobów w dowolnym momencie przez wyłączenie tego ustawienia w zasadach zabezpieczeń. Automatyczne udostępnianie zdecydowanie zalecane jest aby uzyskać alerty zabezpieczeń i zaleceń dotyczących aktualizacji systemu, luk w zabezpieczeniach systemu operacyjnego i programu endpoint protection.

> [!NOTE]
> Wyłączanie automatycznego inicjowania obsługi administracyjnej nie powoduje usunięcia programu Microsoft Monitoring Agent z maszyn wirtualnych platformy Azure, w którym zainicjowano agenta.
>
>

1. Powróć do menu głównego Centrum zabezpieczeń i wybierz zasady zabezpieczeń.

   ![Wyłącz automatyczne udostępnianie][6]

2. Wybierz subskrypcję, która ma zostać Wyłącz automatyczne udostępnianie.
3. Na **zasady zabezpieczeń — zbieranie danych** bloku, w obszarze **dołączania** wybierz **poza** do Wyłącz automatyczne udostępnianie.
4. Wybierz pozycję **Zapisz**.  

## <a name="next-steps"></a>Następne kroki
W tym artykule pokazano możesz sposób zbierania danych i automatyczne udostępnianie w Centrum zabezpieczeń działa. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Ustawianie zasad zabezpieczeń w usłudze Azure Security Center](security-center-policies.md) — informacje na temat konfigurowania zasad zabezpieczeń dla subskrypcji i grup zasobów na platformie Azure.
* [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
* [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Bezpieczeństwo danych w Centrum zabezpieczeń Azure](security-center-data-security.md) — Dowiedz się, jak dane są zarządzane i w Centrum zabezpieczeń.
* [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — najnowsze informacje na temat zabezpieczeń platformy Azure.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[4]: ./media/security-center-enable-data-collection/event-id.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-automatic-provisioning.png
