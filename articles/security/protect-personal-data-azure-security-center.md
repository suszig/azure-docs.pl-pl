---
title: "Ochrona danych osobowych z Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Użyj Centrum zabezpieczeń do ochrony danych osobowych. To pozwala na działaniach zgodne z ogólnych danych ochrony rozporządzenia (GDPR)"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 118752a030a1c300306aa3e05571bb9b624a2c20
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Ochrona danych osobowych z atakami i naruszeń: Centrum zabezpieczeń Azure

Ten artykuł pomoże Ci zrozumieć sposób korzystania z Centrum zabezpieczeń Azure do ochrony danych osobowych przed naruszeniami i ataków. Informacje zawarte w tym pomoże Ci w wysiłków w celu zachowania zgodności z ogólnych danych ochrony rozporządzenia (GDPR).

## <a name="scenario"></a>Scenariusz 

Rejs duże przedsiębiorstwo, siedzibą w Stanach Zjednoczonych rozwija operacjach oferowanie trasy w DS i Bałtyckiego mórz oraz brytyjskich. Aby pomóc w tych działań, ustawił mniejszych rejs wiersze na podstawie we Włoszech, Niemczech, Dania i Zjednoczone Królestwo

Przez firmę Microsoft Azure do przechowywania danych firmowych w chmurze. Dotyczy to również dane osobowe, takich jak nazwy, adresy, numery telefonów i informacje o karcie kredytowej. Zawiera również zasoby ludzkie informacji takich jak:

- Adresy
- Numery telefonów
- Numery identyfikacyjne podatku
- Inne informacje

Wiersz rejs zachowuje również dużej bazy danych elementów członkowskich programu osób trzecich i lojalność. Pracownikom firmy dostępu do sieci w oddziałach firmy i podróży agentów na całym świecie mają dostęp do niektórych zasobów firmy.
Dane osobowe są przesyłane w sieci między te lokalizacje i centrum danych firmy Microsoft.

## <a name="problem-statement"></a>Opis problemu

Firma jest zajmującym się zagrożenie atakami ich zasobów platformy Azure. Chcą wyeliminowania zagrożenia pracowników i klientów danych osobowych do osoby nieupoważnione. Chcą, aby uzyskać wskazówki dotyczące zarówno związanych z zapobieganiem i odpowiedzi/korygowania, jak i efektywny sposób monitorowania bieżących bezpieczeństwa zasobów w chmurze.
Muszą one silne linię obrony przed atakami współczesnych zaawansowane i organizowane.

## <a name="company-goal"></a>Celem firmy

Jest jednym z celów firmy można zapewnić poufności danych osobowych pracowników i klientów, aby chronić go przed zagrożeniami. Jest jednym z celów ich natychmiast odpowiadać na znaki zmniejszyć skuteczność naruszenia. Wymaga to sposób ocenić bieżący stan zabezpieczeń, identyfikowanie narażone konfiguracje i usuwać z nich.

## <a name="solutions"></a>Rozwiązania

Microsoft Azure Security Center (ASC) zapewnia zabezpieczenia zintegrowane monitorowanie i zasady rozwiązania do zarządzania. Zapewnia łatwy w użyciu i skuteczne zapobieganie, wykrywanie i odpowiedzi możliwości.

### <a name="prevention"></a>Zapobieganie

ASC pomaga zapobiegać naruszeń, należy włączyć ustawienie zasad zabezpieczeń, dostęp w czasie i zaimplementować zalecenia dotyczące zabezpieczeń.

Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, zalecane dla zasobów w określonej subskrypcji. Tylko w czasie może służyć do blokowania ruchu przychodzącego na maszynach wirtualnych platformy Azure, ograniczenia narażenia na ataki. Zalecenia dotyczące zabezpieczeń są tworzone przez ASC po przeanalizowaniu stan zabezpieczeń zasobów platformy Azure.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Jak ustawić zasady zabezpieczeń w ASC?

Zasady zabezpieczeń można skonfigurować dla każdej subskrypcji. Aby zmodyfikować zasady zabezpieczeń, musisz być właścicielem lub współautorem subskrypcji. W portalu Azure wykonaj następujące czynności:

1. Wybierz **zasad** na pulpicie nawigacyjnym ASC.

2. Wybierz subskrypcję, na którym chcesz włączyć zasady.

3. Wybierz **zasady zapobiegania** skonfigurować zasady dla subskrypcji. **Zbieranie danych z maszyn wirtualnych** powinien być ustawiony na **na.**

4. W **zasady zapobiegania** opcji wybierz **na** Aby włączyć zalecenia dotyczące zabezpieczeń, które są istotne dla subskrypcji.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Aby uzyskać szczegółowe instrukcje i informacje o wszystkich zaleceń dotyczących zasad, które mogą być włączone, zobacz [ustawić zasady zabezpieczeń w Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Jak skonfigurować tylko w czasie dostępu (JIT)?

Po włączeniu JIT Centrum zabezpieczeń blokuje ruch przychodzący na maszynach wirtualnych platformy Azure przez tworzenie reguły NSG. Należy wybrać porty na maszynie Wirtualnej, do którego będzie można zablokować ruch przychodzący. Aby korzystać z dostępu JIT, wykonaj następujące czynności:

1. Wybierz **bezpośrednio w kafelku dostęp maszyny Wirtualnej czasu** w bloku ASC.

2. Wybierz **zalecane** kartę.

3. W obszarze **maszyn wirtualnych**, wybierz maszyny wirtualne, które chcesz włączyć. Spowoduje to umieszczenie znacznik wyboru obok maszyny Wirtualnej. 
4. Wybierz **włączyć JIT** na maszynach wirtualnych.
5. Wybierz pozycję **Zapisz**.

Następnie widać domyślnych portów, które ASC zaleca włączana na potrzeby JIT. Można także dodać i skonfigurować nowy port, na którym chcesz włączyć tylko w rozwiązaniu czasu. **Bezpośrednio w dostęp do maszyny Wirtualnej czasu** kafelka w Centrum zabezpieczeń jest wyświetlana liczba maszyn wirtualnych skonfigurowana dla dostępu JIT. Ponadto liczba żądań zatwierdzonych dostępu w ostatnim tygodniu.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

W jaki sposób to zrobić oraz dodatkowe informacje na temat tylko w czasie dostępu, zobacz [zarządzanie dostępem do maszyny wirtualnej przy użyciu tylko w czasie.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Jak wdrożyć ASC zalecenia dotyczące zabezpieczeń?

Po znalezieniu potencjalnych luk w zabezpieczeniach usługa Security Center tworzy odpowiednie zalecenia. Przewodnik dotyczący zaleceń prowadzi użytkownika przez proces konfigurowania wymaganych kontrolek. 
1. Wybierz **zalecenia** kafelka na pulpicie nawigacyjnym ASC.

2. Wyświetl zalecenia, które są wyświetlane w formacie tabeli, w której każdy wiersz zawiera jeden zalecenia.

3. Filtr zaleceń, wybierz opcję **filtru** i wybierz ważność i stan wartości, które chcesz wyświetlić.

4. Aby odrzucić rekomendację, która nie ma zastosowania, kliknij prawym przyciskiem myszy i wybierz **odrzucenia.**

5. Należy ocenić, które zalecenie powinny być stosowane najpierw.

6. Stosować zalecenia w kolejności priorytetów.

Listę możliwych zaleceń i przewodników dotyczących stosowania każdego zawiera [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Wykrywanie i odpowiedzi

Wykrywanie i odpowiedzi go razem, jak należy jak najszybciej po wykryciu zagrożenia.
Wykrywanie zagrożeń ASC polega na automatyczne zbieranie informacji o zabezpieczeniach z zasobów platformy Azure, sieci i rozwiązań partnerskich połączonych. ASC szybko może aktualizować algorytmy wykrywania, jak osoby atakujące wersji nowy, coraz bardziej zaawansowany luki w zabezpieczeniach. Aby uzyskać szczegółowe informacje dotyczące sposobu działania wykrywanie zagrożeń ASC firmy, zobacz [funkcji wykrywania Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a>Jak zarządzać i reagowania na alerty zabezpieczeń?

Lista alertów zabezpieczeń uporządkowanych według priorytetu jest wyświetlana w Centrum zabezpieczeń oraz informacje potrzebne do szybkiego analizowania problemu. Centrum zabezpieczeń zawiera także zalecenia dotyczące skorygować atak. Aby zarządzać alerty zabezpieczeń, wykonaj następujące czynności:

1. Wybierz **alerty zabezpieczeń** kafelka na pulpicie nawigacyjnym ASC. To przedstawia szczegółowe informacje o każdym alercie.

2. Aby filtrować alerty na podstawie daty, stanu i ważności, wybierz **filtru** , a następnie wybierz wartości, które mają być wyświetlane.

3. Aby odpowiedzieć alertu, zaznacz go i przejrzyj informacje, a następnie wybierz zaatakowany zasób.

4. W **opis** pola, zobaczysz szczegółowe informacje, łącznie z zalecanych czynności naprawczych.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Aby uzyskać szczegółowe instrukcje dotyczące reagowanie na alerty zabezpieczeń, zobacz [reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

Aby uzyskać dalszą pomoc w badania alertów zabezpieczeń firmy można zintegrować ASC alerty z własnego rozwiązania SIEM, za pomocą [integracji dziennika Azure](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Jak zarządzać przypadki naruszenia zabezpieczeń?

Zdarzenia zabezpieczeń ASC, to agregacji wszystkich alertów dla zasobu, które są wyrównane z wzorców łańcucha kill. Zdarzenie wyświetli listę powiązanych alertów, co pozwala uzyskać więcej informacji na temat każdego wystąpienia. Zdarzenia są wyświetlane w kafelku alerty zabezpieczeń i bloku.

Aby przejrzeć i zarządzania incydentami zabezpieczeń, wykonaj następujące czynności:

1. Wybierz **alerty zabezpieczeń** kafelka. w przypadku wykrycia zdarzenia zabezpieczeń będą wyświetlane w obszarze wykresu alertów zabezpieczeń. Jej ikonę, która różni się od innych alertów.

2. Wybierz zdarzenie, aby wyświetlić więcej szczegółów dotyczących tego zdarzenia zabezpieczeń. Dodatkowe szczegóły obejmują jego pełny opis, jego ważność, jego bieżący stan, zaatakowanych zasobów, czynności korygujące zdarzenia i alerty, które wchodzą w skład tego zdarzenia.

Można filtrować, aby wyświetlić **tylko zdarzenia**, **alerty tylko**, lub **zarówno**.

#### <a name="how-do-i-access-the-threat-intelligence-report"></a>Jak uzyskać dostępu do raportu analizy zagrożeń

ASC analizuje informacje z wielu źródeł do identyfikowania zagrożeń. Aby pomóc odpowiedzi na zdarzenia zespoły zbadać i eliminowanie zagrożeń, Centrum zabezpieczeń zawiera raport analizy zagrożeń, który zawiera informacje dotyczące zagrożenia, które zostało wykryte.

Centrum zabezpieczeń zawiera trzy raporty zagrożenia, które mogą się różnić na atak.
Dostępne raporty:

- Raport grupy działań: zawiera głębokie dives do osoby atakujące, ich cele i taktyk.

- Raport kampanii: koncentruje się na szczegóły ataku określonej kampanii.

- Raport podsumowania zagrożeń: obejmuje wszystkie elementy w poprzednich dwóch raportów.

Informacje tego typu jest bardzo przydatny podczas procesu odpowiedzi na zdarzenia w przypadku, gdy istnieje trwającą dochodzenia zrozumienie źródło ataku, motywacji osoby atakującej i co zrobić, aby zminimalizować ten problem, przenoszenie do przodu.

1. Aby uzyskać dostęp do raportu analizy zagrożeń, wykonaj następujące czynności:

2. Wybierz **alerty zabezpieczeń** kafelka na pulpicie nawigacyjnym ASC.

3. Wybierz alert zabezpieczeń, dla którego chcesz uzyskać więcej informacji.

4. W **raporty** kliknij łącze do raportu analizy zagrożeń.

5. Spowoduje to otwarcie pliku PDF, który można pobrać.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

Aby uzyskać dodatkowe informacje na temat raportu analizy zagrożeń ASC, zobacz [raport analizy zagrożeń z Centrum zabezpieczeń Azure.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Ocena

Aby pomóc w testowania, oceny i ocenie strukturę bezpieczeństwa, ASC zapewnia oceny luk w zabezpieczeniach zintegrowane z Qualys agentów chmurze jako część jej składników zalecenia dotyczące maszyny wirtualnej.

Qualys agent raporty Qualys platformy zarządzania, który następnie wysyła luki w zabezpieczeniach i kondycji danych monitorowania z powrotem do ASC danych luki w zabezpieczeniach. Zalecenie, aby dodać rozwiązanie do oceny luk w zabezpieczeniach jest wyświetlany w **zalecenia** bloku na pulpicie nawigacyjnym ASC.

Po zainstalowaniu rozwiązania do oceny luk w zabezpieczeniach na docelowej maszynie wirtualnej usługa Security Center skanuje maszynę wirtualną w celu wykrycia i zidentyfikowania luk w zabezpieczeniach systemu i aplikacji. Wykryte problemy zostaną wyświetlone w obszarze **Virtual Machines Recommendations** (Zalecenia dotyczące maszyn wirtualnych).

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Jak wdrożyć rozwiązanie do oceny luk w zabezpieczeniach? 

Jeśli maszyna wirtualna nie ma już wdrożone rozwiązanie do oceny luk w zabezpieczeniach zintegrowanego, Centrum zabezpieczeń zaleca się ona zainstalowana.

1. Na pulpicie nawigacyjnym ASC na **zalecenia** bloku, wybierz opcję **Dodaj rozwiązanie do oceny luk w zabezpieczeniach.**

2. Wybierz maszyny wirtualne, które chcesz zainstalować rozwiązanie do oceny luk w zabezpieczeniach.

3. Polecenie **zainstalowania na maszynach wirtualnych [numer].**

4. Wybierz rozwiązanie partnerskie w portalu Azure Marketplace, lub na podstawie **użyć istniejącego rozwiązania,** wybierz **Qualys.**

5. Można włączyć ustawienia automatycznej aktualizacji lub wyłączyć **rozwiązań partnerskich** bloku.

Aby uzyskać dalsze instrukcje dotyczące implementowania rozwiązania oceny luk w zabezpieczeniach, zobacz [oceny luk w zabezpieczeniach w Centrum zabezpieczeń Azure.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Kolejne kroki

- [Przewodnik Szybki start dotyczący Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Wprowadzenie do usługi Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Integrowanie z integracją dzienników Azure alerty Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Zwiększenie wydajności Centrum zabezpieczeń Azure z oceny luk w zabezpieczeniach zintegrowane](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
