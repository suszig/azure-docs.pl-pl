---
title: "Ocena internetowej linii bazowej rozwiązania Security and Audit w pakiecie Operations Management Suite | Dokumentacja firmy Microsoft"
description: "W tym dokumencie wyjaśniono, jak korzystać z oceny internetowej linii bazowej w rozwiązaniu Security and Audit pakietu OMS w celu oceny linii bazowej wszystkich monitorowanych serwerów internetowych pod kątem zgodności i zabezpieczeń."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 8be49b182df675fe3235d148b87379e1dff3a384
ms.contentlocale: pl-pl
ms.lasthandoff: 08/01/2017

---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Ocena linii bazowej sieci Web w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite
Ten dokument ułatwia korzystanie z funkcji oceny internetowej linii bazowej rozwiązania Security and Audit w pakiecie OMS w celu uzyskiwania dostępu do zabezpieczonych monitorowanych zasobów.

## <a name="what-is-web-baseline-assessment"></a>Co to jest ocena internetowej linii bazowej?
Obecnie rozwiązanie Zabezpieczenia w pakiecie OMS umożliwia ocenę podstawy linii bazowej zabezpieczeń systemów operacyjnych. Skanuje ono ustawienia systemu operacyjnego serwerów co 24 godziny i zapewnia wgląd w ich ustawienia, które mogą potencjalnie powodować zagrożenie. Przeczytaj artykuł [Ocena linii bazowej w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline), aby uzyskać więcej informacji na ten temat.

Ocena internetowej linii bazowej ma na celu znalezienie ustawień serwera internetowego, które mogą potencjalnie powodować zagrożenie. Trzy podstawowe źródła konfiguracji linii bazowej sieci Web to konfiguracja platformy .NET, platformy ASP.NET i usług IIS.  Podobnie jak w przypadku oceny linii bazowej systemu operacyjnego rozwiązanie Zabezpieczenia w pakiecie OMS skanuje serwery sieci Web co 24 godziny i zapewnia wgląd w ich stan zabezpieczeń.  W przypadku usługi Internet Information Service (IIS) konfiguracje mogą być w znaczącym stopniu dostosowywane, co pozwala na zastępowanie różnych poziomów witryny i aplikacji. Skaner sprawdza ustawienia na każdym poziomie witryny/aplikacji oraz na domyślnym poziomie głównym. Pomaga to w identyfikacji ustawień, które mogą potencjalnie powodować zagrożenie, i szybkim ich korygowaniu zgodnie z naszymi zaleceniami dotyczącymi tych ustawień.


## <a name="web-security-baseline-assessment"></a>Ocena internetowej linii bazowej zabezpieczeń

W przypadku tej wersji zapoznawczej dostęp do tej funkcji można uzyskać za pomocą opcji wyszukiwania pakietu OMS oraz pulpitu nawigacyjnego Security and Audit w pakiecie OMS. Aby uruchomić odpowiednie zapytanie, wykonaj następujące czynności:

1. Na głównym pulpicie nawigacyjnym **pakietu Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
2. Na pulpicie nawigacyjnym **Security and Audit** możesz zobaczyć perspektywę internetowej linii bazowej obok perspektywy linii bazowej systemu operacyjnego.
   
    ![Ocena internetowej linii bazowej w rozwiązaniu Security and Audit w pakiecie OMS](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. W okienku po lewej stronie jest wyświetlana liczba serwerów internetowych, które porównano z linią bazową, średnia wartość procentowa reguł, które pomyślnie przeszły ocenę na wszystkich ocenianych serwerach, oraz lista serwerów, które zostały ocenione.
4. W okienku po prawej stronie są wyświetlane unikatowe reguły, których ocena zakończyła się niepowodzeniem, według kryteriów *Ważność* i *RuleType*. Kliknięcie dowolnej reguły w okienku po prawej stronie spowoduje wyświetlenie szczegółów dotyczących tej reguły. Przykład został przedstawiony na poniższym obrazie. Oceniana reguła znajduje się w obszarze *Ustawienie reguły*. Pole *AzId* zawiera unikatowy identyfikator dla każdej reguły używany przez firmę Microsoft do śledzenia reguł linii bazowej. Oprócz tego użytkownicy mogą zobaczyć *Oczekiwany wynik* (wartość zalecaną przez firmę Microsoft) i inne szczegóły dotyczące wpływu reguły na bezpieczeństwo.
    
    ![Zapytanie](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. W celu przeglądania wyników możesz tworzyć własne zapytania. 

Pierwszym zapytaniem, którego możesz użyć, jest zapytanie **podsumowania oceny linii bazowej sieci Web**. W polu **Rozpocznij wyszukiwanie tutaj** wpisz następujące zapytanie: *Type=SecurityBaselineSummary BaselineType=Web*. Poniżej znajdują się przykładowe dane wyjściowe:

![Wynik zapytania](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>W tym zapytaniu każdy rekord wskazuje podsumowanie oceny na jednym serwerze.

Po otwarciu **przeszukiwania dzienników** możesz wpisywać różne zapytania, aby uzyskać więcej informacji o ocenie linii bazowej sieci Web. Oprócz poprzedniego zapytania w tej wersji zapoznawczej można również użyć poniższych zapytań:

**Ocena reguły linii bazowej sieci Web**: każdy rekord reprezentuje jedną ocenę reguły linii bazowej sieci Web na jednym serwerze. Zawiera ono wszystkie dane dotyczące reguły, której ocena zakończyła się niepowodzeniem, wartość *SitePath*, czyli ścieżkę witryny, na której reguła była oceniana, *Oczekiwany wynik* i *Rzeczywisty wynik*.

Zapytanie: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Wynik zapytania 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Pokaż wszystkie wyniki dla określonego serwera**: to zapytanie pokazuje, jak wyświetlić wyniki dla konkretnego serwera: Zapytanie: *Type=SecurityBaseline BaselineType=Web Computer=BaselineTestVM1*

![Wynik zapytania 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Tych rekordów/zapytań możesz używać do tworzenia własnych pulpitów nawigacyjnych, raportów lub alertów. Oto przykład kontrolki interfejsu użytkownika, którą można dodać do własnego pulpitu nawigacyjnego. Aby dowiedzieć się, jak wizualizować dane przy użyciu projektanta widoku w pakiecie OMS, kliknij [tutaj](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). Na poniższym ekranie pokazano przykładowy wygląd kafelka po wprowadzeniu tego dostosowania.

![pulpit nawigacyjny](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono informacje na temat oceny internetowej linii bazowej w rozwiązaniu Security and Audit w pakiecie OMS. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)


