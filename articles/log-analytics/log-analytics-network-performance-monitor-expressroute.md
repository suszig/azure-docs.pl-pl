---
title: "Sieci monitora wydajności rozwiązania Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Możliwość Menedżera usługi ExpressRoute w Monitorze wydajności sieci umożliwia monitorować wydajność między oddziałach i Azure i na trasie łączności za pośrednictwem usługi Azure ExpressRoute."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>ExpressRoute Manager

Funkcja Menedżera usługi ExpressRoute w [monitora wydajności sieci](log-analytics-network-performance-monitor.md) pozwala monitorować wydajność między oddziałach i Azure i na trasie łączności za pośrednictwem usługi Azure ExpressRoute. Dostępne są następujące kluczowe korzyści: 

- Automatyczne wykrywanie skojarzonych z Twoją subskrypcją obwody usługi ExpressRoute 
- Śledzenie wykorzystania przepustowości, utrata i czas oczekiwania na poziomie obwód, równorzędna i sieci wirtualnej dla programu ExpressRoute 
- Odnajdywanie topologii sieci Twojej obwody usługi ExpressRoute 

![Monitor usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Konfigurowanie 
Aby otworzyć konfigurację monitora wydajności w sieci, otwórz [rozwiązania monitora wydajności sieci](log-analytics-network-performance-monitor.md) i kliknij przycisk **Konfiguruj** przycisku.

### <a name="configure-nsg-rules"></a>Skonfiguruj reguły NSG 
Dla serwerów na platformie Azure, które są używane do monitorowania za pomocą Menedżera NPM należy skonfigurować reguły grupa zabezpieczeń sieci zezwalająca na ruch TCP na porcie używanym przez NPM dla transakcji syntetycznych. Domyślny port to 8084. Dzięki temu agent pakietu OMS zainstalowana na maszynie Wirtualnej platformy Azure do komunikowania się z lokalnymi agenta monitorowania. 

Aby uzyskać więcej informacji na temat grupy NSG, zobacz [grup zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Upewnij się, czy po zainstalowaniu agentów (agent serwera lokalnego i agent serwera Azure) i uruchomiono skrypt programu PowerShell EnableRules.ps1 przed kontynuowaniem w ramach tego kroku. 

 
### <a name="discover-expressroute-peering-connections"></a>Odnajdywanie połączenia komunikacji równorzędnej ExpressRoute 
 
1. Polecenie **komunikacji równorzędnych ExpressRoute** widoku.  
2. Polecenie **odnajdywanie teraz** przycisk, aby odnaleźć wszystkie ExpressRoute prywatnej komunikacji równorzędnych, które są podłączone do sieci wirtualnych w subskrypcji platformy Azure są połączone z tym obszarem roboczym analizy dzienników.  

>[!NOTE]  
> Rozwiązanie umożliwia odnalezienie obecnie tylko ExpressRoute prywatnej komunikacji równorzędnych. 

>[!NOTE]  
> Tylko te prywatnej komunikacji równorzędnych są odnajdywane, które są podłączone do sieci wirtualnych, skojarzone z subskrypcją połączone z tym obszarem roboczym analizy dzienników. Twoje ExpressRoute jest podłączony do sieci wirtualnych poza subskrypcji połączone z tym obszarem roboczym, należy utworzyć obszaru roboczego analizy dzienników w tych subskrypcjach i monitorowanie tych komunikacji równorzędnych za pomocą programu NPM. 

 ![Konfigurowanie monitora usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Po zakończeniu odnajdywania odnalezionych prywatnej komunikacji równorzędnej połączeń są wyświetlane w tabeli. Do monitorowania tych komunikacji równorzędnych początkowo będzie w stanie wyłączenia. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Aby włączyć monitorowanie połączeń komunikacji równorzędnej ExpressRoute 

1. Kliknięcie prywatnej komunikacji równorzędnej łączącego są zainteresowani monitorowania.  
2. W okienku RHS kliknij pole wyboru **monitorować tej komunikacji równorzędnej**. 
3. Jeżeli chcesz utworzyć zdarzenia kondycji dla tego połączenia, sprawdź **Włącz monitorowanie kondycji dla komunikacji równorzędnej**. 
4. Wybierz warunki monitorowania. Można ustawić progami niestandardowymi kondycji zdarzenie generacji, wpisując wartości progowe. Zawsze, gdy wartość warunku wykraczają poza jego próg dla połączenia komunikacji równorzędnej, jest generowane zdarzenie kondycji. 
5. Polecenie **dodać agentów** możliwość monitorowania agentów, które mają być używane do monitorowania tego połączenia komunikacji równorzędnej. Należy się upewnić, możesz dodać agentów na obu końców połączenia, co najmniej jednego agenta w sieci Wirtualnej Azure podłączone do komunikacji równorzędnej, a także co najmniej jednego agenta lokalnie podłączone do komunikacji równorzędnej. 
6. Kliknij przycisk **zapisać** Aby zapisać konfigurację. 

![Konfigurowanie monitora usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Po włączenie zasad i wybraniu wartości i agenci mają być monitorowane, jest Poczekaj około 30 – 60 minut dla wartości rozpocząć wypełniania i **ExpressRoute monitorowania** Kafelki, które stają się dostępne. Gdy zobaczysz Kafelki monitorowania obwody usługi ExpressRoute i siecią połączenia są monitorowane przez NPM. 

>[!NOTE]
> Ta funkcja działa niezawodnej na obszary robocze, które zostały uaktualnione do nowego języka zapytań.  

## <a name="walkthrough"></a>Przewodnik 

Pulpit nawigacyjny monitorowania wydajności sieci zawiera przegląd kondycji obwody usługi ExpressRoute i połączenia komunikacji równorzędnej. 

![NPM ExpressRoute pulpitu nawigacyjnego](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Obwody listy 

Aby wyświetlić listę wszystkich monitorowanych obwody usługi ExpressRoute, kliknij Kafelek obwody usługi ExpressRoute. Możesz wybrać obwód i wyświetlić jej stan kondycji, wykresy trendu do utraty pakietów, użycie przepustowości i opóźnień. Wykresy są interaktywne. Możesz wybrać okno czasu niestandardowych do kreślenia wykresy. Na wykresie powiększanie i zobaczyć punkty szczegółowych danych można przeciągnąć myszy nad obszarem. 

![Lista obwody usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Trend utraty, opóźnienia i przepływności 

Wykresy przepustowości, opóźnienia i utratę są interaktywne. W dowolnej sekcji wykresy te można powiększyć za pomocą formantów myszy. Można również zauważyć, że przepustowość, opóźnienia i utratę danych dla innych interwałów, klikając daty/godziny, znajdujący się poniżej przycisk akcje w lewym górnym rogu. 

![Opóźnienie ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Lista komunikacji równorzędnych 

Kliknięcie **prywatnej komunikacji równorzędnych** kafelka na pulpicie nawigacyjnym powoduje wyświetlenie listy wszystkich połączeń do sieci wirtualnych za pośrednictwem prywatnej komunikacji równorzędnej. W tym miejscu można wybrać wirtualnej połączenie sieciowe i wyświetlić jej stan kondycji, wykresy trendu do utraty pakietów, użycie przepustowości i opóźnień. 

![Komunikacji równorzędnych usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Topologia obwodu 

Aby wyświetlić topologii obwód, kliknij **topologii** kafelka. Powoduje to przejście do wybranego widoku topologii obwodu lub komunikacji równorzędnej. Diagram topologii udostępnia czas oczekiwania na każdy z segmentów w sieci i każdego przeskoku warstwy 3 jest reprezentowany przez węzeł diagramu. Kliknięcie przeskoku ujawnia więcej szczegółów na temat przeskoku. Można zwiększyć poziom widoczności uwzględnienie przeskoków lokalnymi Przesuń suwak poniżej **filtry**. Przenoszenie suwak w lewo lub w prawo, zwiększa/zmniejsza liczbę przeskoków na wykresie topologii. Czas oczekiwania w każdym segmencie jest widoczny, która umożliwia szybsze izolacji segmentów duże opóźnienie w sieci. 

![Topologia usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Widok szczegółowy topologii obwodu 

Ten widok przedstawia połączeń sieci wirtualnej. 

![Sieć wirtualna ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnostyka 

Monitor wydajności sieci ułatwia diagnozowanie problemów z łącznością kilka obwodu. Niektóre problemy są wymienione poniżej 

**Obwód jest wyłączony.** NPM powiadamia jak łączność między lokalnymi zasobami i sieciami wirtualnymi platformy Azure zostaną utracone. Dzięki temu można podjąć aktywne działania przed otrzymaniem za użytkownika i ograniczyć czas przestoju 

![Obwód usługi Expressroute w dół](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Ruch nie przepływają danego obwodu.** Zawsze, gdy ruch nie jest nieoczekiwanie przepływających przez danego obwodu ExpressRoute może powiadomić NPM. Może to nastąpić, jeśli obwód nie działa, a ruch jest przepływających przez trasy kopii zapasowej lub jeśli wystąpi problem routingu. Te informacje pomagają aktywnie zarządzać problemów dotyczących konfiguracji w zasad routingu i upewnij się, że używana jest najbardziej optymalnego i bezpiecznego trasa. 

 

**Ruch nie przepływają głównej obwodu.** Możliwość powiadamia użytkownika, gdy ruch jest przepływających przez dodatkowej obwodu usługi expressroute. Mimo że w takim przypadku nie będzie działać wszelkie problemy z łącznością, ale aktywne rozwiązywania problemów problemy z podstawowym obwodu udoskonalić należy przygotować. 

 
![Przepływ ruchu usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Pogorszenia się z powodu szczytowego wykorzystania.** Można skorelować trend wykorzystania przepustowości z panującym trendem opóźnienia, aby określić, czy degradacji Azure obciążenie jest spowodowane szczytowego użycia przepustowości i podejmij odpowiednie działanie.  

![Monitor usługi ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Kolejne kroki
* [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlania rekordów danych wydajności szczegółowe sieci.
