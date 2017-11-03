---
title: "Korygowanie luk w zabezpieczeniach systemu operacyjnego w Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Ten dokument przedstawia sposób wykonania zalecenia Centrum zabezpieczeń Azure ** luk w zabezpieczeniach ** skorygować systemu operacyjnego."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 39879c22278a55f841e294cda5a89bec2bdf6988
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Korygowanie luk w zabezpieczeniach systemu operacyjnego w Centrum zabezpieczeń Azure
Centrum zabezpieczeń Azure codziennie analizuje systemu operacyjnego (OS), maszynach wirtualnych (VM) i na komputerach w konfiguracji, który można utworzyć maszyny wirtualne i bardziej narażony na ataki. Centrum zabezpieczeń zaleca Rozwiąż luk w zabezpieczeniach w przypadku konfiguracji systemu operacyjnego jest niezgodny z reguł zalecanych konfiguracji i zaleca zmiany konfiguracji, aby rozwiązać te luki w zabezpieczeniach.

> [!NOTE]
> Aby uzyskać więcej informacji o określonych monitorowanych konfiguracji, zobacz [lista reguł zalecanych konfiguracji](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Wykonania zalecenia
Korygowanie systemu operacyjnego luk w zabezpieczeniach zostaje przedstawiony jako zalecenia w Centrum zabezpieczeń. To zalecenie będą wyświetlane w obszarze **zalecenia** i w obszarze **obliczeniowe**.

W tym przykładzie przedstawiono **luk w zabezpieczeniach skorygować systemu operacyjnego (przez firmę Microsoft)** zalecenie, w obszarze **obliczeniowe**.
1. Wybierz **obliczeniowe** w menu głównym Centrum zabezpieczeń.

   ![Koryguj luki w zabezpieczeniach systemu operacyjnego][1]

2. W obszarze **obliczeniowe**, wybierz pozycję **luk w zabezpieczeniach skorygować systemu operacyjnego (przez firmę Microsoft)**. **Niezgodność luk w zabezpieczeniach systemu operacyjnego (przez firmę Microsoft)** otwiera pulpitu nawigacyjnego.

   ![Koryguj luki w zabezpieczeniach systemu operacyjnego][2]

  Udostępnia górnej części pulpitu nawigacyjnego:

  - Całkowita liczba reguł według ważności, że konfiguracja systemu operacyjnego nie powiodła się dla maszyny Wirtualnej, a na komputerze.
  - Całkowita liczba reguł według typu, który Konfiguracja systemu operacyjnego nie powiodła się dla maszyny Wirtualnej, a na komputerze.
  - Całkowita liczba reguł nie powiodło się, za pomocą konfiguracji systemu operacyjnego i konfiguracji systemu operacyjnego Linux.

  Dolnej części pulpitu nawigacyjnego zawiera listę wszystkich reguł nie powiodło się maszyn wirtualnych i komputerów i ważności brakujących aktualizacji. Lista zawiera:

  - **CCEID**: CCE Unikatowy identyfikator reguły. Centrum zabezpieczeń używane typowych konfiguracji wyliczenie CCE () do przypisywania unikatowych identyfikatorów dla reguły konfiguracji.
  - **Nazwa**: Nazwa reguły nie powiodło się
  - **Typ reguły**: klucz rejestru, zasady zabezpieczeń lub zasady inspekcji
  - **LICZBA Maszyny wirtualne i komputerach**: łączna liczba maszyn wirtualnych i komputerów, które wykluczyć błędów ma zastosowanie do
  - **WAŻNOŚĆ reguły**: wartość wagi CCE krytyczne ważne i ostrzeżenia
  - **STAN**: bieżący stan zalecenia:

    - **Otwarte**: nie rozpoczęto jeszcze wykonywania zalecenia
    - **Trwa**: zalecenie jest aktualnie stosowane do tych zasobów i jest wymagana żadna akcja ze strony
    - **Rozwiązane**: zalecenie zostało już zakończone. (Jeśli ten problem został rozwiązany, wygaszone wpis)

3. Wybierz z listy, aby wyświetlić szczegóły reguły nie powiodło się.

   ![Reguły konfiguracji, które nie powiodło się][3]

  Poniższe informacje są przekazywane w tym bloku:

  - Nazwa — Nazwa reguły
  - CCIED — CCE Unikatowy identyfikator reguły
  - Wersja systemu operacyjnego — wersja systemu operacyjnego maszyny Wirtualnej lub komputera
  - WAŻNOŚĆ reguły — Wartość wagi CCE krytyczne ważne i ostrzeżenia
  - PEŁNY opis — Opis reguły
  - Luki w zabezpieczeniach — Opis usterki lub ryzyko, gdy nie jest stosowana reguła
  - POTENCJALNY wpływ — Wpływ na prowadzoną działalność po zastosowaniu reguły
  - PRZECIWDZIAŁANIE — czynności korygujące
  - OCZEKIWANA wartość — Wartość oczekiwana w przypadku Centrum zabezpieczeń analizuje względem reguły konfiguracji systemu operacyjnego maszyny Wirtualnej
  - RZECZYWISTA wartość--Zwracana wartość po analizie względem reguły konfiguracji systemu operacyjnego maszyny Wirtualnej
  - — Reguła zasada operacji używane przez Centrum zabezpieczeń podczas analizy konfiguracji systemu operacyjnego maszyny Wirtualnej względem reguły

4. Wybierz **wyszukiwania** ikona na Wstążce top. Wyszukiwanie Otwiera listę obszarów roboczych, których maszyn wirtualnych i komputerów z wybranych luki w zabezpieczeniach systemu operacyjnego. Tego bloku wybór obszaru roboczego jest wyświetlana tylko w przypadku wybranej reguły dotyczą wiele maszyn wirtualnych, które są podłączone do różnych obszarów roboczych.

  ![Obszary robocze wymienione][4]

5. Wybierz obszar roboczy. Zapytania wyszukiwania analizy dzienników otwiera filtrowane do obszaru roboczego z luk w zabezpieczeniach systemu operacyjnego.

  ![Obszar roboczy z luk w zabezpieczeniach systemu operacyjnego][5]

6. Wybierz komputer z listy, aby uzyskać więcej informacji. Otwiera inny wynik wyszukiwania informacji filtrowane tylko dla tego komputera.

  ![Filtrowane dla tego komputera][6]

## <a name="next-steps"></a>Następne kroki
W tym artykule przedstawiono sposób wykonania zalecenia Centrum zabezpieczeń "Luk Skoryguj systemu operacyjnego". Możesz przejrzeć zestaw reguł konfiguracji [tutaj](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centrum zabezpieczeń używane CCE (typowych konfiguracji wyliczenie) do przypisywania unikatowych identyfikatorów dla reguły konfiguracji. Odwiedź stronę [CCE](https://nvd.nist.gov/cce/index.cfm) witryny, aby uzyskać więcej informacji.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące zasoby:

* [Obsługiwane platformy w Centrum zabezpieczeń Azure](security-center-os-coverage.md) — zawiera listę obsługiwanych systemu Windows i maszyn wirtualnych systemu Linux.
* [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje o sposobie konfigurowania zasad zabezpieczeń dla subskrypcji platformy Azure i grup zasobów.
* [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
* [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje o sposobie monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — informacje o sposobie monitorowania stanu kondycji rozwiązań partnerskich.
* [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — często zadawane pytania dotyczące korzystania z usługi wyszukiwania.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń platformy Azure i zgodności.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
