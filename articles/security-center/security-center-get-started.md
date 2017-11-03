---
title: "Przewodnik Szybki start dotyczący Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomaga szybko rozpocząć pracę z usługą Azure Security Center. Omówiono w nim składniki monitorowania zabezpieczeń i zarządzania zasadami oraz podano linki do następnych kroków."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: c28f92af96f31d1c386cf072f83fc142b9a7f588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Przewodnik Szybki start dotyczący usługi Azure Security Center
Ten artykuł pomaga szybko rozpocząć pracę z usługą Azure Security Center. Omówiono w nim składniki monitorowania zabezpieczeń i zarządzania zasadami usługi Security Center.

## <a name="prerequisites"></a>Wymagania wstępne
Do rozpoczęcia korzystania z usługi Security Center wymagana jest subskrypcja usługi Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/).

Warstwa bezpłatna Centrum zabezpieczeń jest automatycznie włączona dla wszystkich subskrypcji platformy Azure i zawiera zasady zabezpieczeń, oceny ciągłego zabezpieczeń i zalecenia dotyczące zabezpieczeń można wykonać, aby ułatwić ochronę zasobów platformy Azure.

Usługa Security Center jest dostępna w witrynie [Azure Portal](https://azure.microsoft.com/features/azure-portal/). Aby dowiedzieć się więcej o witrynie Azure Portal, zobacz [dokumentację portalu](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Uprawnienia
W Centrum zabezpieczeń widoczne są tylko informacje związane z zasobem, jeśli przypisano rolę właściciela, współautora lub czytelnika subskrypcji lub grupy zasobów, do której należy zasób. Zobacz [uprawnienia w Centrum zabezpieczeń Azure](security-center-permissions.md) Aby dowiedzieć się więcej o rolach i akcji dozwolonych w Centrum zabezpieczeń.

## <a name="data-collection"></a>Zbieranie danych
Centrum zabezpieczeń zbiera dane z maszyn wirtualnych platformy Azure (maszyny wirtualne) i komputerów z systemem innym niż Azure monitorowanie luk w zabezpieczeniach i zagrożeń. Dane są zbierane za pomocą programu Microsoft Monitoring Agent, która odczytuje różnych konfiguracji związanych z zabezpieczeniami i dzienniki zdarzeń z komputera i kopiuje dane do swojego obszaru roboczego do analizy. Centrum zabezpieczeń przepisy programu Microsoft Monitoring Agent na wszystkich istniejących obsługiwane maszynach wirtualnych platformy Azure i nowe pliki, które są tworzone. Zobacz [Włącz zbieranie danych](security-center-enable-data-collection.md) Aby dowiedzieć się więcej na temat działania zbierania danych.

Automatyczne udostępnianie zdecydowanie zaleca się i jest wymagany dla subskrypcji w warstwie standardowa Centrum zabezpieczeń. Wyłączanie automatycznego inicjowania obsługi administracyjnej limitów monitorowania zabezpieczeń dla zasobów.

Zobacz [cennik Centrum zabezpieczeń](security-center-pricing.md) Aby dowiedzieć się więcej o wolnym i Standard warstw cenowych.

Poniższe kroki zawierają opis sposobu uzyskania dostępu do usługi Security Center i używania jej składników.

> [!NOTE]
> Informacje na temat usługi przedstawiono w tym artykule za pomocą przykładowego wdrożenia. Ten artykuł nie jest przewodnikiem krok po kroku.
>
>

## <a name="access-security-center"></a>Dostęp do usługi Security Center
Aby uzyskać dostęp do usługi Security Center, wykonaj następujące kroki w portalu:

1. W menu platformy **Microsoft Azure** wybierz pozycję **Security Center**.

   ![Azure menu][1]
2. Jeśli po raz pierwszy uzyskujesz dostęp do usługi Security Center, zostanie otwarty blok **Witamy**. Wybierz **Centrum zabezpieczeń uruchamiania** otworzyć **Centrum zabezpieczeń**.
   ![Ekran powitalny][10]
3. Po po uruchomieniu Centrum zabezpieczeń z bloku powitalnej lub Centrum zabezpieczeń, wybierz z menu Microsoft Azure, **Centrum zabezpieczeń** otwiera. Aby mieć w przyszłości łatwiejszy dostęp do bloku **Security Center**, wybierz opcję **Przypnij blok do pulpitu nawigacyjnego** (w prawym górnym rogu).
   ![Opcja Przypnij blok do pulpitu nawigacyjnego][2]

## <a name="use-security-center"></a>Korzystanie z usługi Security Center
Skonfigurować można zasady zabezpieczeń dla subskrypcji i grup zasobów platformy Azure. Skonfigurujmy zasady zabezpieczeń dla subskrypcji:

1. W menu głównym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**.
2. W obszarze **Centrum zabezpieczeń — zasady zabezpieczeń**, wybierz subskrypcję.
3. W obszarze **zasady zabezpieczeń — zbieranie danych**, **automatyczne udostępnianie** jest włączona. Centrum zabezpieczeń przepisy programu Microsoft Monitoring Agent na wszystkich istniejących obsługiwane maszynach wirtualnych platformy Azure i nowe pliki, które są tworzone.

    ![Zasady zabezpieczeń][12]

4. Na **zasady zabezpieczeń** bloku, wybierz składnik zasad **zasady zabezpieczeń**.

     ![Zasady zabezpieczeń][11]

5. W obszarze **Pokaż zalecenia dotyczące**, Włącz zaleceń, które mają być wyświetlane jako część zasad zabezpieczeń. Przykłady:

   * Ustawienie **aktualizacji systemu** do **na** skanowanie wszystkich obsługiwanych maszyn wirtualnych dla brakujących aktualizacji systemu operacyjnego.
   * Ustawienie **luk w zabezpieczeniach systemu operacyjnego** do **na** skanowanie wszystkich obsługiwanych maszyn wirtualnych, aby zidentyfikować konfiguracji systemu operacyjnego, które maszyna wirtualna jest bardziej narażony na ataki.

6. Wybierz pozycję **Zapisz**.

### <a name="view-recommendations"></a>Wyświetlanie zaleceń
1. Wróć do bloku **Security Center** i wybierz kafelek **Zalecenia**. Usługa Security Center okresowo analizuje stan zabezpieczeń zasobów platformy Azure. Po znalezieniu przez usługę Security Center potencjalnych luk w zabezpieczeniach wyświetla ono zalecenia w bloku **Zalecenia**.
   ![Zalecenia w usłudze Azure Security Center][5]
2. W bloku **Zalecenia** wybierz zalecenie, aby wyświetlić więcej informacji lub podjąć działania w celu rozwiązania problemu.

### <a name="view-the-security-state-of-your-resources"></a>Wyświetl stan zabezpieczeń zasobów
1. Wróć do bloku **Security Center**. **Zapobiegania** pulpitu nawigacyjnego zawiera wskaźniki stanu zabezpieczeń dla maszyn wirtualnych, sieci, danych i aplikacji.
2. Wybierz **obliczeniowe** Aby wyświetlić więcej informacji. **Obliczeniowe** zostanie otwarty blok przedstawiający trzy karty:

  - **Omówienie** — zawiera, monitorowania i zalecenia dotyczące maszyny Wirtualnej.
  - **Maszyny wirtualne i komputerach** -Wyświetla bieżący stan zabezpieczeń wszystkich maszyn wirtualnych i komputerów.
  - **Usługi w chmurze** — zawiera listę ról sieci web i proces roboczy, które są monitorowane przez Centrum zabezpieczeń.

    ![Obliczenia bazy danych kondycji zabezpieczeń][6]

3. Na **omówienie** , a następnie wybierz zalecenie poniżej, aby wyświetlić więcej informacji lub podjąć działania w celu skonfigurowania niezbędnych opcji.
4. Na **maszyn wirtualnych i komputery** , a następnie wybierz zasób w celu wyświetlenia dodatkowych szczegółów.

### <a name="view-security-alerts"></a>Wyświetlanie alertów zabezpieczeń
1. Wróć do bloku **ecurity Center** i wybierz kafelek **Alerty zabezpieczeń**. Zostanie otwarty blok **Alerty zabezpieczeń** z listą alertów. Te alerty są generowane na podstawie analizy usługi Security Center uwzględniającej dzienniki zabezpieczeń i aktywność sieciową. Uwzględniane są alerty zintegrowanych rozwiązań partnerskich.
   ![Alerty zabezpieczeń w usłudze Azure Security Center][7]

2. Wybierz alert, aby wyświetlić dodatkowe informacje. W tym przykładzie załóżmy wybierz **zmodyfikować systemowym pliku binarnym odnalezione w filtrze zrzutu**. Spowoduje to otwarcie bloków udostępniających dodatkowe szczegóły dotyczące alertu.
   ![Szczegóły alertów zabezpieczeń w usłudze Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Wyświetlanie informacji o kondycji rozwiązań partnerskich
1. Wróć do bloku **Security Center**. **Rozwiązań zabezpieczeń** kafelka umożliwia monitorowanie, w skrócie, stan kondycji rozwiązań partnerskich zintegrowanych z subskrypcją platformy Azure.
2. Wybierz **rozwiązań zabezpieczeń** kafelka. Zostanie otwarty blok zawierający listę rozwiązań partnerskich połączonych z usługą Security Center.
   ![Rozwiązania partnerskie][9]
3. Wybierz rozwiązanie partnerskie. Zostanie otwarty blok pokazujący stan rozwiązania partnerskiego oraz zasoby skojarzone z rozwiązaniem. Wybierz opcję **Konsola rozwiązań**, aby otworzyć narzędzie do zarządzania rozwiązaniem partnerskim.

   ![Rozwiązania partnerskie][13]

## <a name="next-steps"></a>Następne kroki
W tym artykule zostały opisane składniki monitorowania zabezpieczeń i zarządzania zasadami w usłudze Security Center. Teraz, kiedy znasz już usługę Security Center, spróbuj wykonać następujące czynności:

* Konfigurowanie zasad zabezpieczeń dla subskrypcji platformy Azure, zobacz [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md).
* Użyj zaleceń w Centrum zabezpieczeń, aby ułatwić ochronę zasobów platformy Azure, zobacz [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md).
* Przejrzyj i zarządzanie bieżące alerty zabezpieczeń, zobacz [reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi](security-center-managing-and-responding-alerts.md).
- Dowiedz się więcej o integracji z partnerami, aby zwiększyć poziom bezpieczeństwa ogólnej, zobacz [partnera i integracja z rozwiązań](security-center-partner-integration.md).
- Dowiedz się, jak zarządzanie danymi i chronione w Centrum zabezpieczeń, zobacz [bezpieczeństwo danych w Centrum zabezpieczeń Azure](security-center-data-security.md).
* Dowiedz się więcej o [zaawansowanych funkcjach wykrywania zagrożeń](security-center-detection-capabilities.md) pochodzących z [warstwy standardowej](security-center-pricing.md) usługi Security Center. Warstwa Standardowa jest oferowana za darmo przez pierwsze 60 dni.
* Jeśli masz pytania dotyczące korzystania z usługi Security Center, zobacz [Często zadawane pytania dotyczące usługi Azure Security Center](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
[11]: ./media/security-center-get-started/show-recommendations-for.png
[12]: ./media/security-center-get-started/automatic-provisioning.png
[13]: ./media/security-center-get-started/partner-solutions-detail.png
