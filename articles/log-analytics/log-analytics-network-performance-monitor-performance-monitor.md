---
title: "Funkcję Monitor wydajności w rozwiązaniu monitora wydajności sieci w Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Możliwość monitora wydajności w Monitorze wydajności sieci pomaga monitorować łączność sieciową między różnymi punktami w sieci, takich jak wdrożenia chmury i lokalizacji lokalnej, wielu centrów danych oraz biura oddziałów krytycznym znaczeniu wielowarstwowe aplikacje/micro-services."
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
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Sieci rozwiązania monitora wydajności — monitorowanie wydajności

Możliwość monitora wydajności w [monitora wydajności sieci](log-analytics-network-performance-monitor.md) pomaga monitorować łączność sieciową między różnymi punktów w sieci, takich jak wdrożenia chmury i lokalizacji lokalnej, wielu centrów danych i biura oddziałów misji wielowarstwowe aplikacje/micro-obsługi usług krytycznych. Zanim użytkownicy skarżą się z Monitora wydajności można wykryć problemy z siecią. Dostępne są następujące kluczowe korzyści: 

- Monitorowanie utraty i opóźnienia za pośrednictwem różnych podsieci i Ustaw alerty 
- Monitorowanie wszystkich ścieżek (w tym nadmiarowe ścieżki) w sieci 
- Przejściowa & w momencie Rozwiązywanie problemów z sieci, które są trudne do replikacji 
- Określić określonego segmentu w sieci, która jest odpowiedzialna za pogorszenie wydajności 
- Monitorowanie kondycji sieci, bez konieczności SNMP 


![Monitor wydajności sieci](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Konfigurowanie
Aby otworzyć konfigurację monitora wydajności w sieci, otwórz [rozwiązania monitora wydajności sieci](log-analytics-network-performance-monitor.md) i kliknij przycisk **Konfiguruj** przycisku.

![Konfigurowanie monitora wydajności sieci](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Tworzenie nowych sieci

Sieć w NPM jest kontenerem logicznym podsieci. Pomaga organizować Monitorowanie infrastruktury sieci, zgodnie z potrzebami monitorowania. Można utworzyć sieci o przyjaznej nazwie i dodać podsieci do niego według logiki biznesowej. Na przykład można utworzyć sieci o nazwie Londynie i dodać wszystkie podsieci w Londynie centrum danych lub sieć o nazwie *ContosoFrontEnd* i dodać wszystkie podsieci obsługujących fronton aplikacji o nazwie Contoso z tą siecią. Rozwiązanie automatycznie tworzy domyślnej sieci, zawierającą wszystkie podsieci wykrytych w środowisku. Gdy utworzysz sieć Dodaj podsieć do niej i podsieci jest usuwany z domyślnej sieci. Jeśli usuniesz sieci, wszystkie jego podsieci automatycznie są zwracane do domyślnej sieci. W związku z tym domyślnej sieci działa jako kontener dla wszystkich podsieci, które nie znajdują się w dowolnej sieci zdefiniowanej przez użytkownika. Nie można edytować ani usunąć domyślnej sieci. Zawsze pozostaje w systemie. Jednak można utworzyć dowolną liczbę sieci niestandardowe. W większości przypadków podsieci w organizacji są rozmieszczone w więcej niż jedną sieć i należy utworzyć co najmniej jednej sieci, aby zgrupować podsieci dla logiki biznesowej.

Aby utworzyć nową sieć:


1. Polecenie **kartę sieci**.
1. Kliknij przycisk **sieci Dodaj** , a następnie wpisz nazwę sieciową i opis. 
2. Wybierz co najmniej jednej podsieci, a następnie kliknij przycisk **Dodaj**. 
3. Kliknij przycisk **zapisać** Aby zapisać konfigurację. 


### <a name="create-monitoring-rules"></a>Tworzenie reguły monitorowania 

Monitor wydajności generuje zdarzenia kondycji, gdy naruszenia progu wydajności połączeń sieciowych między dwoma podsieci lub między dwiema sieciami. Progi te mogą być rozpoznawane automatycznie przez system lub zapewniają progami niestandardowymi. System automatycznie tworzy regułę domyślną, która generuje zdarzenie kondycji, w przypadku utraty lub opóźnienia między jakiejkolwiek parze sieć/podsieć łączy naruszeń progu rozpoznawane przez system. Pomaga to rozwiązanie Monitorowanie infrastruktury sieci, dopóki nie utworzono żadnych reguł monitorowania jawnie. Jeśli **domyślną regułę** jest włączona, wszystkie węzły wysyłania transakcji syntetycznych do wszystkich węzłów, które mają włączone do monitorowania. Domyślna reguła jest przydatne w przypadku małych sieciach, na przykład w sytuacji, gdy masz niewielkiej liczby serwerów z systemem mikrousługi i chce mieć pewność, że wszystkie serwery mają łączność między sobą. 

>[!NOTE]
> Zdecydowanie zaleca się wyłączenie **domyślną regułę** i utworzyć niestandardowe reguły monitorowania, szczególnie w dużych sieciach, gdy używasz dużą liczbę węzłów do monitorowania. Pozwala to ograniczyć ruch generowany przez rozwiązanie i pomoc można organizować monitorowania sieci. 

Tworzenie reguł monitorowania zgodnie z logiki biznesowej. Na przykład jeśli chcesz monitorować wydajność łączności sieciowej z dwóch lokacjach office siedzibę główną, następnie grupy wszystkich podsieci w site1 pakietu office w sieci O1 wszystkich podsieci w witryna2 pakietu office w sieci O2 i wszystkich podsieci w sieci H. siedzibę główną Tworzenie reguł-do-jednego między O1 i H, a druga między O2 i H. monitorowania 2 

Aby utworzyć niestandardowe reguły monitorowania:

1. Kliknij przycisk **Dodaj regułę** w **Monitor** i wprowadzić nazwę reguły i opis. 
2. Wybierz pary sieci lub podsieć łącza do monitorowania za pomocą list. 
3. Najpierw wybierz sieć, w którym znajduje się pierwszy podsieć/s zainteresowań z listy rozwijanej sieci, a następnie wybierz podsieć/s z listy rozwijanej odpowiednich podsieci. Wybierz **wszystkie podsieci** Jeśli chcesz monitorować wszystkich podsieci w połączenia sieciowego. Podobnie wybierz inne podsieć/s zainteresowań. I kliknięcie **dodać wyjątek** do wykluczenia z monitorowania łączy określonej podsieci z zaznaczenia zostały wprowadzone. 
4. Wybór między ICMP i TCP protokołów wykonania transakcji syntetycznych. 
5. Jeśli nie chcesz utworzyć zdarzenia kondycji dla elementów jest zaznaczony, następnie wyczyść **Włącz monitorowanie kondycji w łączach objęte regułą**. 
6. Wybierz warunki monitorowania. Można ustawić progami niestandardowymi kondycji zdarzenie generacji, wpisując wartości progowe. Zawsze, gdy wartość warunku wykraczają poza jego próg dla pary wybranego sieć/podsieć, jest generowane zdarzenie kondycji. 
7. Kliknij przycisk **zapisać** Aby zapisać konfigurację. 

Po zapisaniu reguły monitorowania tej reguły można zintegrować z zarządzania alertami, klikając **Tworzenie alertów**. Reguła alertu są tworzone przy użyciu zapytania wyszukiwania i innych wymaganych parametrów wypełniane automatycznie. Reguła alertu może otrzymywać pocztą e-mail alertów, oprócz istniejące alerty w ramach programu NPM. Alerty można również uruchomić czynności zaradczych z elementami runbook i ich można zintegrować z istniejącymi rozwiązaniami zarządzania usługi za pomocą elementów webhook. Możesz kliknąć **Zarządzanie Alert** Aby edytować ustawienia alertu. 

Możesz teraz utworzyć więcej reguł monitora wydajności lub przejdź do pulpitu nawigacyjnego rozwiązania, aby rozpocząć korzystanie z możliwości 

### <a name="choose-the-protocol"></a>Wybierz protokół

Monitor wydajności sieci (NPM) używa transakcji syntetycznych do obliczania metryki wydajności sieci, takich jak opóźnienie utratą i łącze pakietów. Aby lepiej zrozumieć to, należy wziąć pod uwagę połączony agenta NPM jeden koniec połączenia sieciowego. Ten agent NPM wysyła pakiety sondy do agenta programu NPM drugiego połączenia w innym celu sieci. Druga odpowiedź agenta z pakietów odpowiedzi. Ten proces jest powtarzany kilka razy. Mierząc kod odpowiedzi oraz czas potrzebny na odebranie odpowiedź pierwszego agenta NPM ocenia czas oczekiwania linku oraz operacjach porzucania pakietów. 

Format, rozmiar i kolejność tych pakietów jest określana przez protokół, który można wybrać podczas tworzenia reguł monitorowania. Na podstawie protokołu pakietów, pośrednich sieci urządzenia (routery, przełączniki itd.) może różnie te pakiety. W rezultacie wybranego protokołu ma wpływ na dokładność wyników. I wybór protokołu określa, czy wszystkie wymagane ręczne wykonanie czynności należy wykonać po wdrożeniu rozwiązania NPM. 

NPM umożliwia wybór między protokołów ICMP i TCP wykonania transakcji syntetycznych. Jeśli wybierzesz protokołu ICMP, podczas tworzenia reguły transakcji syntetycznych, agenci NPM umożliwia obliczenie opóźnienie sieci i utraty pakietów komunikatów ECHA protokołu ICMP. Ten sam komunikat wysyłany przez narzędzie konwencjonalnej korzysta z ECHA protokołu ICMP. Użycie jako protokołu TCP, NPM agenci wysyłają pakietu TCP SYN za pośrednictwem sieci. Następuje uzgadniania TCP uzupełniania i usunięcie połączenia przy użyciu RST pakietów. 

Aby wybrać protokół do użycia, należy wziąć pod uwagę następujące informacje: 

**Odnajdywanie wiele tras sieciowych.**  Protokół TCP jest dokładniejsze podczas rozpoznawania wiele tras i wymaga mniejszej liczby agentów w każdej podsieci. Na przykład jeden lub dwaj agenci za pomocą protokołu TCP umożliwia odnalezienie wszystkich nadmiarowe ścieżki między podsieciami. Należy jednak kilka czynników, aby to zrobić za pomocą protokołu ICMP. Za pomocą protokołu ICMP, jeśli masz wiele tras między dwiema podsieciami potrzebujesz więcej niż 5 n agentów w podsieci źródłowe lub docelowe. 

**Dokładność wyników.** Routery i przełączniki zwykle można przypisać niższy priorytet pakietów ECHA protokołu ICMP w porównaniu do pakietów TCP. W niektórych sytuacjach silnie ładowane urządzeń sieciowych, dane uzyskiwane przez protokół TCP dokładniejsze odzwierciedla utraty i opóźnienia wystąpił przez aplikacje. Dzieje się tak, ponieważ większość aplikacji ruch przepływa za pośrednictwem protokołu TCP. W takich przypadkach ICMP zawiera mniej dokładne wyniki w porównaniu do TCP. 

**Konfiguracja zapory.** Protokół TCP wymaga, aby TCP pakiety są wysyłane do portu docelowego. Domyślny port używany przez agentów NPM jest 8084, ale można go zmienić po skonfigurowaniu agentów. Dlatego należy się upewnić, czy z zapory sieciowe lub reguły NSG (na platformie Azure) są zezwala na ruch na porcie. Należy również upewnić, że zapory lokalnej na komputerach, na których zainstalowano agentów jest skonfigurowane zezwalająca na ruch na tym porcie. Można skonfigurować reguł zapory na komputerach z systemem Windows, jednak należy ręcznie skonfigurować zapory sieciowej, można użyć skryptów środowiska PowerShell. Z kolei w protokołu ICMP przy użyciu portu nie będzie działać. W większości przypadków enterprise ruch protokołu ICMP jest dozwolone za pośrednictwem zapór umożliwiają używanie narzędzia diagnostyczne sieci, takie jak narzędzie Ping. Tak, jeśli jeden komputer z innej można wywołać, można użyć protokołu ICMP, bez konieczności ręcznego konfigurowania zapory. 

>[!NOTE] 
> Niektóre zapory można zablokować protokołu ICMP, co może prowadzić do retransmisji powodujące dużej liczby zdarzeń w systemie zarządzania informacji i zdarzeń zabezpieczeń. Upewnij się, że protokół, który wybrano nie jest blokowane przez zaporę sieci/NSG, w przeciwnym razie NPM nie jest możliwość monitorowania segmentu sieci. W związku z tym zaleca się używać protokołu TCP do monitorowania. Należy używać protokołu ICMP w scenariuszach, w którym nie będą używać protokołu TCP, takie jak czas: 
>
> - Używasz węzłów opartą na kliencie systemu Windows, ponieważ gniazd TCP nie są dozwolone w kliencie systemu Windows
> - Bloki zapory/NSG Twojej sieci TCP
> - Jak przełączać protokołu 

Jeśli została wybrana opcja do wykorzystania podczas wdrażania protokołu ICMP, możesz przełączyć się do TCP w dowolnym momencie poprzez edycję domyślna reguła monitorowania:

1. Przejdź do **wydajność sieci** > **Monitor** > **skonfigurować**   >  **Monitor** , a następnie kliknij przycisk **domyślną regułę**. 
2. Przewiń do **protokołu** sekcji, a następnie wybierz protokół, którego chcesz używać. 
3. Kliknij przycisk **zapisać** Aby zastosować ustawienia. 

Nawet jeśli domyślna reguła jest przy użyciu określonego protokołu, można utworzyć nowe reguły z innego protokołu. Nawet tworzenia różnych zasad gdzie Użyj reguł ICMP i używa innego protokołu TCP. 

## <a name="walkthrough"></a>Przewodnik 

Teraz, gdy Monitor wydajności zostały przedstawione, Przyjrzyjmy się proste badanie przyczynę kondycji zdarzenie.  

Na pulpicie nawigacyjnym rozwiązania można zauważyć, że istnieje zdarzenie kondycji — połączenia sieciowego jest w złej kondycji. Chcesz zbadać problem, a następnie kliknij polecenie **łączy sieciowych monitorowanych** kafelka.

Na stronie rozwijania szczegółów, sprawdź który **DMZ2 DMZ1** połączenia sieciowego jest w złej kondycji. Możesz kliknąć **wyświetlanie łączy podsieci** link dla tego połączenia sieciowego. 


Na stronie przechodzenia znajdują się wszystkich łączy podsieci w **DMZ2 DMZ1** połączenia sieciowego. Można zauważyć, że dla obu łączy podsieci opóźnienie przekroczyło wartość progową, tworzenia połączenia sieciowego złej kondycji. Można również sprawdzić czas oczekiwania trendów łączy podsieci. Wybór czasu można użyć formantu na wykresie, aby skoncentrować się na zakres wymagany czas. Widać porę dnia, gdy czas oczekiwania został osiągnięty Szczyt. Dzienniki dla tego okresu do badania problemu można będzie później wyszukiwać. Kliknij przycisk **wyświetlanie łączy węzłów** aby przejść dalej. 
 
 ![Łącza podsieci](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Podobnie do poprzedniej strony, strony Przechodzenie do szczegółów dla łącza określonej podsieci wyświetlane w jego łączy węzłów składowych. Można wykonywać działania podobne tutaj tak samo jak w poprzednim kroku. Kliknij przycisk **widoku topologii** do wyświetlania topologii między dwoma węzłami. 
 
 ![Łączy węzłów](media/log-analytics-network-performance-monitor/node-links.png) 

Wszystkie ścieżki między dwoma węzłami wybranego na wykresie mapy topologii. Można zwizualizować topologii przeskoku przeskoku tras między dwa węzły na mapie topologii. Umożliwia danych istnieje jak wiele tras między dwa węzły i co ścieżek trwa pakietów danych. Wąskich gardeł wydajności sieci są oznaczone kolorem czerwonym. Połączenie sieciowe uszkodzony lub uszkodzenia urządzenia sieciowego można znaleźć analizując red kolorowe elementy na mapie topologii. 

 ![Topologia pulpitu nawigacyjnego](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Utrata, opóźnienia i liczby przeskoków w poszczególnych ścieżek można przeglądać w **akcji** okienka. Umożliwia wyświetlenie szczegółów tych ścieżek zła pasek przewijania. Użyj filtrów, aby zaznaczyć ścieżek o złej kondycji przeskoku, dzięki czemu są kreślone topologii dla wybranej ścieżki. Można użyć kółka myszy, aby powiększyć lub pomniejszyć mapy topologii. 

W poniżej obrazu, analizując ścieżki i przeskoków kolorem czerwonym wyraźnie widzi głównej przyczyny problemów do określonej sekcji sieci. Kliknięcie w węźle mapy topologii ujawnia właściwości węzła, w tym nazwę FQDN i adres IP. Kliknięcie skoku zawiera adres IP przeskoku. 
 
![Topologia pulpitu nawigacyjnego](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Kolejne kroki
* [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlania rekordów danych wydajności szczegółowe sieci.
