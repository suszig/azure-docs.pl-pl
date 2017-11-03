---
title: "Monitorowanie zasobów Operations Management Suite zabezpieczeń i inspekcji rozwiązania | Dokumentacja firmy Microsoft"
description: "Ten dokument ułatwia korzystanie z zabezpieczeń OMS i inspekcji możliwości monitorowania zasobów i zidentyfikować problemy z zabezpieczeniami."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 2f73266b65a4eda6c8751a2d56bc3f11bf4e6a57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Monitorowanie zasobów Operations Management Suite zabezpieczeń i rozwiązanie inspekcji
Ten dokument ułatwia używać OMS zabezpieczeń i możliwości inspekcji do monitorowania zasobów i zidentyfikować problemy z zabezpieczeniami.

## <a name="what-is-oms"></a>Co to jest pakiet OMS?
Microsoft Operations Management Suite (OMS) to rozwiązanie do zarządzania IT, które pomaga zarządzać i chronić lokalnej i w chmurze infrastruktury opartej na chmurze firmy Microsoft. Aby uzyskać więcej informacji na temat pakietu OMS, przeczytaj artykuł [Omówienie pakietu Operations Management Suite (OMS)](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Monitorowanie zasobów
Zawsze, gdy jest to możliwe, można zapobiec sytuacji, w pierwszej kolejności zdarzeń zabezpieczeń. Jednak jest niemożliwe uniemożliwić wszystkie zdarzenia zabezpieczeń. Gdy wystąpi zdarzenie zabezpieczeń, konieczne będzie upewnij się, czy wpływu jest zminimalizowany.  Istnieją trzy krytyczne zaleceń, których można użyć, aby zminimalizować liczbę i wpływu naruszenia zabezpieczeń:

* Regularnie oceny luk w zabezpieczeniach w danym środowisku.
* Sprawdź rutynowo wszystkich komputerów i urządzeń sieciowych, aby upewnić się, czy mają one wszystkich najnowszych poprawek zainstalowanych.
* Regularnie sprawdzić wszystkie dzienniki i mechanizmów rejestrowania, w tym dzienniki zdarzeń systemu operacyjnego, określonych Dzienniki aplikacji i dzienniki systemu wykrywania nieautoryzowanego dostępu.

OMS zabezpieczeń oraz umożliwia rozwiązanie inspekcji IT aktywnie monitorowania wszystkich zasobów, które mogą pomóc zminimalizować wpływ zdarzeń zabezpieczeń. Zabezpieczenia OMS i inspekcji ma domen zabezpieczeń, które mogą służyć do monitorowania zasobów. Domen zabezpieczeń zapewnia szybki dostęp do opcji dla następujących domen zostaną objęte bardziej szczegółowe monitorowanie zabezpieczeń:

* oceny złośliwego oprogramowania
* Ocena aktualizacji
* Tożsamość i dostęp

> [!NOTE]
> Omówienie tych opcji, należy przeczytać [wprowadzenie Operations Management Suite zabezpieczeń i rozwiązanie inspekcji](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Monitorowanie systemu ochrony
W bardziej podejście głębokość co warstwę ochrony jest ważne dla ogólny stan zabezpieczeń zawartości. Komputery z wykrytymi zagrożeniami i komputery z niewystarczającą ochroną są wyświetlane na kafelku oceny złośliwego oprogramowania, w obszarze domen zabezpieczeń. Korzystając z informacji na temat oceny złośliwego oprogramowania, należy zidentyfikować planu w celu zastosowania ochrony na serwerach, które go potrzebują. Aby uzyskać dostęp do tej opcji, wykonaj następujące czynności:

1. Na głównym pulpicie nawigacyjnym pakietu **Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
   
    ![Bezpieczeństwo i inspekcji](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. W **zabezpieczeń i inspekcji** pulpitu nawigacyjnego, kliknij przycisk **oceny ochrony przed złośliwym kodem** w obszarze **domen zabezpieczeń**. **Oceny ochrony przed złośliwym kodem** pulpitu nawigacyjnego pojawia się, jak pokazano poniżej:

![oceny złośliwego oprogramowania](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Można użyć **oceny złośliwego oprogramowania** pulpit nawigacyjny, aby zidentyfikować następujące problemy bezpieczeństwa:

* **Zagrożenia Active**: komputery, które zostały naruszone i ma aktywne zagrożenia w systemie.
* **Skorygowane zagrożenia**: komputery, które zostały naruszone, ale zagrożenia zostały skorygowane.
* **Nieaktualny podpis**: komputery, które ma włączoną ochronę przed złośliwym oprogramowaniem, ale sygnatura jest nieaktualny.
* **Brak ochrony w czasie rzeczywistym**: komputery, które nie mają ochrony przed złośliwym kodem, zainstalować.

### <a name="monitoring-updates"></a>monitorowanie aktualizacji
Stosowanie najnowszych aktualizacji zabezpieczeń jest najlepszym rozwiązaniem bezpieczeństwa i powinny zostać włączone w strategii zarządzania aktualizacji. Usługa Microsoft Monitoring Agent (HealthService.exe) odczytuje informacje dotyczące aktualizacji z monitorowanych komputerów, a następnie wysyła ten zaktualizowane informacje do usługę w chmurze do przetwarzania. Usługa Microsoft Monitoring Agent jest skonfigurowana jako Usługa automatyczna i powinna być zawsze uruchomiona w komputerze docelowym.

![monitorowanie aktualizacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logika jest stosowany do aktualizacji danych i usługi w chmurze rejestruje dane. W przypadku znalezienia brakujących aktualizacji są wyświetlane na **aktualizacje** pulpitu nawigacyjnego. Można użyć **aktualizacje** pulpitu nawigacyjnego do pracy z brakującymi aktualizacjami i opracować plan je zastosować do serwerów, które są potrzebne. Wykonaj poniższe kroki, aby uzyskać dostęp **aktualizacje** pulpitu nawigacyjnego:

1. Na głównym pulpicie nawigacyjnym pakietu **Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
2. W **zabezpieczeń i inspekcji** pulpitu nawigacyjnego, kliknij przycisk **oceny aktualizacji** w obszarze **domen zabezpieczeń**. Pulpit nawigacyjny aktualizacji pojawia się, jak pokazano poniżej:

![Oceny aktualizacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Na tym pulpicie nawigacyjnym możesz wykonać oceny aktualizacji zrozumieć bieżący stan swoich komputerów i adresów najważniejszych zagrożeń. Za pomocą **aktualizacje krytyczne lub zabezpieczeń** kafelka, Administratorzy IT będą mogli dostęp do szczegółowych informacji o aktualizacji, które nie są spełnione, jak pokazano poniżej:

![Wynik wyszukiwania](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

W tym raporcie obejmują krytyczne informacje, które mogą służyć do identyfikowania typu zagrożenia, ten system jest podatny na, która obejmuje artykuły bazy wiedzy Microsoft KB skojarzonych z aktualizacji zabezpieczeń i Bulletin MS zawierający więcej szczegółów na temat luki w zabezpieczeniach.

### <a name="monitoring-identity-and-access"></a>Monitorowanie tożsamościami i dostępem
Użytkownikom pracy z dowolnego miejsca, przy użyciu różnych urządzeń i uzyskiwanie dostępu do dużych ilości aplikacji w chmurze i lokalnych konieczne jest ochronę poświadczeń. Ataki kradzieży poświadczeń to takie, w których osoba atakująca uzyska początkowo dostęp do zwykłych użytkowników poświadczeń dostępu do systemu w ramach sieci. Wiele razy atak początkowej jest jedynym sposobem uzyskania dostępu do sieci, kiedy ostatecznym celem jest do odnajdywania kont uprawnień. 

Osoby atakujące, pozostanie w sieci, za pomocą bezpłatnych narzędzi można wyodrębnić poświadczenia z sesji innych kont logowania. W zależności od konfiguracji systemu można wyodrębnić te poświadczenia w postaci skrótów, biletów lub haseł, nawet w postaci zwykłego tekstu.  

> [!NOTE]
> maszyny, które są bezpośrednio widoczne z Internetem zostanie wyświetlone wiele nieudanych prób, które próbują zalogować się przy użyciu wszystkich rodzajów dobrze znane nazwy użytkowników (np. administratora). W większości przypadków jest OK dobrze znane nazwy użytkowników nie są używane i jest wystarczająco silne hasło.
> 
> 

Istnieje możliwość zidentyfikować te intruzów przed ich naruszyć bezpieczeństwo konta uprawnień. Można wykorzystać **OMS zabezpieczeń i rozwiązanie inspekcji** do monitora tożsamościami i dostępem. Wykonaj poniższe kroki, aby uzyskać dostęp **tożsamościami i dostępem** pulpitu nawigacyjnego:

1. W **programu Microsoft Operations Management Suite** głównym pulpicie nawigacyjnym kliknij zabezpieczeń i inspekcji kafelka.
2. W **zabezpieczeń i inspekcji** pulpitu nawigacyjnego, kliknij przycisk **tożsamościami i dostępem** w obszarze **domen zabezpieczeń**. **Tożsamościami i dostępem** pulpitu nawigacyjnego pojawia się, jak pokazano poniżej:

![Tożsamość i dostęp](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

W ramach strategii regularnego monitorowania należy uwzględnić monitorowanie tożsamości. Administrator IT powinien wyglądać w przypadku określonych prawidłowej nazwy użytkownika, który ma wiele prób. Może to sygnalizować albo atakujący nabytej rzeczywistą nazwę użytkownika i spróbuj ataki siłowe lub automatyczne narzędzie tego używa ustalony hasło wygasło.

Włącz ten pulpit nawigacyjny działowi IT szybkie identyfikowanie potencjalnych zagrożeń związanych z tożsamościami i dostępem do zasobów firmy. Jest w szczególności należy także zidentyfikować potencjalne trendy, na przykład na kafelku logowania w czasie widać w czasie, ile razy wykonano nieudane próby logowania. W takim przypadku komputer **plików** Odebrano 35 prób logowania. Więcej informacji na temat tego komputera można sprawdzić, klikając go. 

![więcej informacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Raport wygenerowany dla tego komputera powoduje cenne szczegółów dotyczących tego wzorca. Zauważyć, że **konta** kolumna podaje konta użytkownika, którego użyto do próby uzyskania dostępu do systemu **TIMEGENERATED** kolumna podaje przedział czasu, w którym Wykonano próbę i **LOGONTYPENAME** kolumna podaje lokalizacji, gdzie była wykonywana ta próba. Jeśli te próby były wykonywane lokalnie w systemie przez program, **procesu** kolumny będą wyświetlane nazwa procesu. W scenariuszach, w którym próba logowania pochodzi z programu masz już dostępna nazwa procesu i można teraz wykonywać dalszych badań w systemie docelowym.

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono sposób użycia OMS zabezpieczeń i inspekcji rozwiązanie służące do monitorowania zasobów. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Wprowadzenie do programu Operations Management Suite zabezpieczeń i rozwiązanie inspekcji](oms-security-getting-started.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)

