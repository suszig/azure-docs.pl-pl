---
title: Korzystanie z usługi Azure Security Center w celu reagowania na zdarzenia | Microsoft Docs
description: Ten dokument przedstawia sposób korzystania z usługi Azure Security Center w scenariuszach reagowania na zdarzenia.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2016
ms.author: yurid

---
# Korzystanie z usługi Azure Security Center w celu reagowania na zdarzenia
Wiele organizacji uczy się reagowania na zdarzenia zabezpieczeń dopiero wtedy, gdy zostaną celem ataku. W celu zmniejszenia kosztów i strat ważne jest, aby dysponować planem reagowania na takie zdarzenia jeszcze przed wystąpieniem ataku. Usługa Azure Security Center jest przydatna na różnych etapach reagowania na zdarzenia.

## Reagowanie na zdarzenia
Efektywny plan zależy od trzech podstawowych możliwości: ochrony, wykrywania zagrożeń i reagowania na nie. Ochrona oznacza zapobieganie zdarzeniom, wykrywanie polega na ich wczesnej identyfikacji, a reagowanie oznacza udaremnienie próby ataku i przywrócenie systemów w celu zminimalizowania skutków naruszenia zabezpieczeń. 

W tym artykule zastosowano etapy reagowania na zdarzenie zabezpieczeń opisane w artykule [Microsoft Azure Security Response in the Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) (Reagowanie na zdarzenia zabezpieczeń platformy Microsoft Azure w chmurze), zgodnie z diagramem poniżej:

![Cykl życia reakcji na zdarzenie](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Z usługi Security Center można skorzystać na etapach wykrywania, oceny i diagnostyki. Aby dowiedzieć się więcej o każdym z tych etapów, zapoznaj się z przykładem wykorzystania usługi Security Center na każdym z trzech początkowych etapów reagowania na zdarzenie:

* **Wykrywanie**: pierwszy sygnał badania zdarzenia
  * Przykład: wstępne potwierdzenie, że na pulpicie nawigacyjnym Security Center został zgłoszony alert zabezpieczeń o wysokim priorytecie.
* **Ocenianie**: wykonaj wstępną ocenę, aby uzyskać więcej informacji na temat podejrzanego działania.
  * Przykład: uzyskiwanie dodatkowych informacji na temat alertu zabezpieczeń.
* **Diagnozowanie**: przeprowadź badanie techniczne oraz określ strategię ograniczania ataku, łagodzenia jego skutków oraz możliwych obejść
  * Przykład: wykonanie czynności naprawczych zaproponowanych w usłudze Security Center dla danego alertu zabezpieczeń.

Opisany niżej scenariusz pokazuje, jak korzystać z usługi Security Center na etapach wykrywania, oceny i diagnozowania/reagowania w przypadku wystąpienia zdarzenia zabezpieczeń.  W usłudze Security Center [zdarzenie zabezpieczeń](security-center-incident.md) to agregacja wszystkich alertów dotyczących zasobu, które są zgodne ze wzorcami ataku cybernetycznego typu [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/). Zdarzenia są wyświetlane na kafelku i w bloku [Alerty zabezpieczeń](security-center-managing-and-responding-alerts.md). Zdarzenie wyświetli listę powiązanych alertów, co pozwala uzyskać więcej informacji na temat każdego wystąpienia. Usługa Security Center generuje też autonomiczne alerty zabezpieczeń, które ułatwiają śledzenie podejrzanych działań.

## Scenariusz
Firma Contoso dokonała niedawno ostatnio migracji niektórych lokalnych zasobów na platformę Azure. Dotyczyło to między innymi niektórych baz danych SQL i obciążeń biznesowych na maszynach wirtualnych. Obecnie zespół reagowania na zdarzenia zabezpieczeń firmy Contoso (CSIRT, Computer Security Incident Response Team) ma za zadanie zbadać problemy dotyczące zabezpieczeń. Jest ono utrudnione przez brak funkcji analizy zabezpieczeń zintegrowanych z ich obecnymi narzędziami do reagowania na zdarzenia. Brak integracji powoduje trudności na etapach wykrywania (zbyt wiele fałszywych alarmów), oceny i diagnostyki. W ramach tej migracji zespół postanowił skorzystać z usługi Security Center, aby rozwiązać ten problem. 

Pierwsza faza migracji zakończyła się po wprowadzeniu wszystkich zasobów i zastosowaniu się do zaleceń dotyczących bezpieczeństwa przedstawionych w usłudze Security Center. Zespół Contoso CSIRT stanowi centralny punkt rozwiązywania problemów związanych ze zdarzeniami bezpieczeństwa komputerowego. Zespół składa się z grupy osób odpowiedzialnych za zajmowanie się wszystkimi zdarzeniami zabezpieczeń. Członkowie zespołu mają jasno określone obowiązki obejmujące wszystkie obszary reagowania. 

Na potrzeby tego scenariusza skoncentrujemy się na rolach następujących członków zespołu Contoso CSIRT:

![Cykl życia reakcji na zdarzenie](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Julia należy do zespołu operacyjnego ds. zabezpieczeń i do jej obowiązków należą:

* Stałe monitorowanie zagrożeń bezpieczeństwa i reagowanie na nie
* Eskalacja problemu do właściciela obciążeń chmurowych lub analityka zabezpieczeń w razie potrzeby

Szymon jest analitykiem zabezpieczeń i do jego obowiązków należą:

* Badanie ataków
* Korygowanie działań na podstawie alertów 
* Współpraca z właścicielami obciążeń w celu określania i stosowania rozwiązań

Jak widać, Julia i Szymon mają różne obowiązki i muszą współpracować, dzieląc się informacjami uzyskanymi w usłudze Security Center. 

## Zalecane rozwiązanie
Ponieważ Julia i Szymon pełnią różne role, będą korzystać z różnych obszarów usługi Security Center w celu uzyskiwania informacje ważnych dla ich codziennych obowiązków. Julia używa alertów zabezpieczeń w ramach codziennego monitorowania. 

![Alert zabezpieczeń](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Julia używa alertów zabezpieczeń na etapach wykrywania i oceny. Po zakończeniu początkowej oceny może przekazać sprawę Szymonowi, jeśli wymaga ona dalszego zbadania. Na tym etapie Szymon korzysta z informacji pochodzących z usługi Security Center, a czasem również z innych źródeł danych, aby przejść do etapu diagnozy.

## Sposób implementacji rozwiązania
Aby zobaczyć, jak skorzystać z usługi Azure Security Center w scenariuszu reagowania na zdarzenie, prześledzimy czynności Julii na etapie wykrywania i oceny, a następnie czynności Szymona na etapie diagnostyki. 

### Etapy wykrywania i oceny w ramach reagowania na zdarzenie
Julia zalogowała się w witrynie Azure Portal i otworzyła konsolę Security Center. W ramach codziennego monitorowania zaczęła przeglądać alerty zabezpieczeń o wysokim priorytecie, wykonując następujące czynności:

1. Kliknij kafelek **Alerty zabezpieczeń** i przejdź do bloku **Alerty zabezpieczeń**.
    ![Blok Alerty zabezpieczeń](./media/security-center-incident-response/security-center-incident-response-fig4.png)
   
   > [!NOTE]
   > Na potrzeby tego scenariusza Julia zamierza przeprowadzić ocenę alertu dotyczącego złośliwego działania SQL, jak widać na rysunku powyżej. 
   > 
   > 
2. Kliknij alert **Malicious SQL activity** (Złośliwe działanie SQL) i przejrzyj zaatakowane zasoby w bloku **Malicious SQL activity** (Złośliwe działanie SQL):  ![Szczegóły zdarzenia](./media/security-center-incident-response/security-center-incident-response-fig5.png)
   
    W tym bloku Julia może zanotować zaatakowane zasoby, liczbę ataków i czas ich wykrycia.
3. Kliknij pozycję **Zaatakowany zasób**, aby uzyskać więcej informacji na temat ataku. 

Po przeczytaniu opisu Julia jest przekonana, że nie jest to fałszywy alarm, i postanawia eskalować sprawę do Szymona.

### Etap diagnozy w ramach reagowania na zdarzenie
Szymon otrzymuje zgłoszenie od Julii i zaczyna przeglądanie czynności zaradczych zasugerowanych przez usługę Security Center.

![Cykl życia reakcji na zdarzenie](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### Dodatkowe zasoby
Zespół ds. reagowania na zdarzenia może również korzystać z funkcji [Security Center Power BI](security-center-powerbi.md) w celu wyświetlania różnych typów raportów, które pomagają w dalszych badaniach dzięki wizualizacji, analizie i filtrowaniu zaleceń i alertów zabezpieczeń. Firmy, które korzystają z rozwiązania do zarządzania informacjami i zdarzeniami dotyczącymi bezpieczeństwa (SIEM), mogą również [zintegrować usługę Security Center z używanym rozwiązaniem](security-center-integrating-alerts-with-log-integration.md). Dzienniki inspekcji platformy Azure i zdarzenia zabezpieczeń dotyczące maszyn wirtualnych można również zintegrować przy użyciu [narzędzia do integracji dziennika Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). Te informacje można wykorzystać w połączeniu z danymi dostarczonych przez usługę Security Center w celu badania ataków.

## Podsumowanie
Stworzenie zespołu przed wystąpieniem ataku jest bardzo istotne dla organizacji i pozytywnie wpływa na sposób reagowania zdarzenia dotyczące bezpieczeństwa. Odpowiednie narzędzia do monitorowania zasobów mogą ułatwić zespołowi podjęcie właściwych kroków w celu usunięcia skutków zdarzenia zabezpieczeń. [Funkcje wykrywania](security-center-detection-capabilities.md) usługi Security Center ułatwią zespołowi IT szybkie reagowanie na zdarzenia zabezpieczeń i rozwiązywanie problemów dotyczących bezpieczeństwa.

<!--HONumber=Sep16_HO3-->


