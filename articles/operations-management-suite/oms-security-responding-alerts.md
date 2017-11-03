---
title: "Monitorowanie i reagowanie na alerty zabezpieczeń w Operations Management Suite zabezpieczeń i inspekcji rozwiązania | Dokumentacja firmy Microsoft"
description: "Ten dokument ułatwia użycie opcji analizy zagrożeń dostępnych w OMS zabezpieczeń i inspekcji do monitorowania i reagowania na alerty zabezpieczeń."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: df82afab2c38431e134146143524edc080ee38f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Monitorowanie i reagowanie na alerty zabezpieczeń w Operations Management Suite zabezpieczeń i rozwiązanie inspekcji
Ten dokument ułatwia użyć opcji analizy zagrożeń dostępnych w OMS zabezpieczeń i inspekcji do monitorowania i reagowania na alerty zabezpieczeń.

## <a name="what-is-oms"></a>Co to jest pakiet OMS?
Microsoft Operations Management Suite (OMS) to rozwiązanie do zarządzania IT, które pomaga zarządzać i chronić lokalnej i w chmurze infrastruktury opartej na chmurze firmy Microsoft. Aby uzyskać więcej informacji na temat pakietu OMS, przeczytaj artykuł [Omówienie pakietu Operations Management Suite (OMS)](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Analiza zagrożeń
W środowisku firmowym, których użytkownicy mają szerokiego dostępu do sieci i korzystają z różnych urządzeń do firmowych danych konieczne jest można aktywne monitorowanie zasobów i szybkiego reagowania na zagrożenia bezpieczeństwa. Jest to szczególnie istotne z punktu widzenia zabezpieczeń cyklu życia, ponieważ niektóre bezpieczeństwa, które nie mogą zgłaszać zagrożenia alerty lub podejrzanych działań, które mogą zostać zidentyfikowane przez kontrolę techniczną tradycyjne zabezpieczenia. 

Za pomocą **analizy zagrożeń** opcja dostępna w OMS zabezpieczeń i inspekcji, Administratorzy IT można zidentyfikować zagrożenia bezpieczeństwa w środowisku, na przykład, ustalić, czy na określonym komputerze jest częścią [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Komputery mogą stać się węzłami w botnecie, gdy osoby atakujące bezprawnie zainstalują złośliwe oprogramowanie, które potajemnie łączy ten komputer z centrum sterowania. Mogą też wskazać potencjalnych zagrożeń pochodzące z kanałów komunikacyjnych podziemnych, takich jak [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Aby można było skompilować ten analizy zagrożeń, rozwiązanie OMS zabezpieczeń i inspekcji używa danych pochodzących z wielu źródeł w ramach firmy Microsoft. Te dane do identyfikowania potencjalnych zagrożeń dla środowiska będzie korzystać z zabezpieczeń OMS i inspekcji.

Okienko analizy zagrożeń zawiera trzy główne opcje:

* Serwery z wychodzącym ruchem złośliwego oprogramowania
* Typy wykrytych zagrożeń
* Mapa analizy zagrożeń

> [!NOTE]
> Omówienie tych opcji, należy przeczytać [wprowadzenie Operations Management Suite zabezpieczeń i rozwiązanie inspekcji](oms-security-getting-started.md).
> 
> 

### <a name="responding-to-security-alerts"></a>Reagowanie na alerty zabezpieczeń
Jeden z kroków z [odpowiedzi na zdarzenia zabezpieczeń](https://technet.microsoft.com/library/cc512623.aspx) procesu jest zidentyfikowanie ważność systemy naruszenia. Na tym etapie należy wykonać następujące zadania:

* Określenie natury ataku
* Określenie źródła ataku
* Określenie celu ataku Czy atak miał charakter przypadkowy czy był jawnie skierowany przeciwko Twojej organizacji w celu uzyskania konkretnych informacji?
* Zidentyfikowanie systemów, których zabezpieczenia zostały naruszone
* Identyfikowanie plików, które zostały dostęp i określenie czułości tych plików

Można wykorzystać **analizy zagrożeń** informacji w rozwiązaniu OMS zabezpieczeń i inspekcji ułatwiające te zadania. Wykonaj poniższe kroki, aby przejść do nich **analizy zagrożeń** opcje:

1. Na głównym pulpicie nawigacyjnym pakietu **Microsoft Operations Management Suite** kliknij kafelek **Zabezpieczenia i inspekcja**.
   
    ![Bezpieczeństwo i inspekcji](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. W **zabezpieczeń i inspekcji** pulpitu nawigacyjnego, zobaczysz **analizy zagrożeń** opcje w prawo, jak pokazano poniżej:
   
    ![Intel zagrożeń](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Te trzy Kafelki zawiera przegląd bieżącego zagrożeń. W **serwera z wychodzącym ruchem złośliwego oprogramowania** będzie mógł zidentyfikować, jeśli komputer, na którym są monitorowania (wewnątrz lub na zewnątrz sieci) która wysyła szkodliwy ruch do Internetu. 

**Wykryto typy zagrożeń** kafelka zawiera podsumowanie zagrożenia, które są aktualne "w symbole", po kliknięciu tego kafelka więcej informacji o tych zagrożeniach zostanie wyświetlone jako wyświetlane poniżej:

![Typy wykrytych zagrożeń](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Można wyodrębnić więcej informacji na temat każde zagrożenie, klikając go. W poniższym przykładzie przedstawiono szczegółowe informacje o Botnet:

![więcej informacji na temat zagrożenie](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Zgodnie z opisem na początku tej sekcji, informacja ta może być bardzo przydatne w przypadku odpowiedzi na zdarzenia. Można również go ważne podczas postępowania śledczego, gdy trzeba znaleźć źródło ataku, który system został naruszony oraz osi czasu. W tym raporcie można łatwo zidentyfikować niektóre najważniejsze szczegóły dotyczące ataku, takich jak: Źródło ataku, lokalny adres IP, którego bezpieczeństwo zostało naruszone i stan bieżącej sesji połączenia. 

**Mapy analizy zagrożeń** pomoże zidentyfikować bieżącej lokalizacji na całym świecie, które mają szkodliwy ruch. Kolor pomarańczowy są (operacja przychodząca) i czerwone (wychodzące) strzałki na tej mapie identyfikujące kierunek ruchu, jeśli klikniesz przycisk w jednym z tych strzałki, zostanie wyświetlona typu zagrożeń i kierunek ruchu, jak pokazano poniżej:

![Mapa analizy zagrożeń](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Można wyświetlić pokaz na temat sposobu używania tej funkcji w procesie odpowiedzi na zdarzenia obserwując prezentacji [ograniczenia zagrożeń zabezpieczeń w centrum danych z przewodnikiem postępowaniu przy użyciu usługi Operations Management Suite](https://myignite.microsoft.com/videos/5000) dostarczone w Ignite firmy Microsoft.
> 

### <a name="responding-to-distinct-malicious-ip-accessed"></a>Odpowiada na różne złośliwe IP dostępne
W niektórych scenariuszach możesz zauważyć potencjalnie złośliwy adres IP, do którego uzyskano dostęp z jednego z monitorowanych komputerów:

![Mapa analizy zagrożeń](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.PNG)

Ten alert i inne w tej samej kategorii są generowane przez rozwiązanie OMS Security z wykorzystaniem [funkcji analizy zagrożeń firmy Microsoft](https://youtu.be/O4WtxgUrDc8). Dane funkcji analizy zagrożeń są gromadzone przez firmę Microsoft, a także kupowane od czołowych dostawców analizy zagrożeń. Te dane są często aktualizowane i adaptowane do szybko zmieniających się zagrożeń. Ze względu na ich charakter podczas [badania](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) alertu zabezpieczeń powinny być one łączone z innymi źródłami informacji o zabezpieczeniach. 

## <a name="customize-alerts-received-via-e-mail"></a>Dostosowywanie alertów odebranych za pośrednictwem poczty e-mail

Można dostosować, którzy użytkownicy w organizacji zostanie powiadomiony, gdy alerty zabezpieczeń są wyzwalane przez zabezpieczenia OMS. Ta opcja jest dostępna w obszarze Przegląd / ustawienia na pulpicie nawigacyjnym OMS:

![Adres e-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono sposób używania **analizy zagrożeń** opcję reagowania na alerty zabezpieczeń w rozwiązaniu OMS zabezpieczeń i inspekcji. Więcej informacji na temat zabezpieczeń w pakiecie OMS zawierają następujące artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Wprowadzenie do programu Operations Management Suite zabezpieczeń i rozwiązanie inspekcji](oms-security-getting-started.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)

