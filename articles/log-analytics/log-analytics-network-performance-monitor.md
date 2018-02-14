---
title: "Sieci monitora wydajności rozwiązania Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Monitor wydajności w Azure Log Analytics pomaga monitorować wydajność Twojej sieci w pobliżu real-jednorazowej — aby wykrywać sieci i Znajdź wąskich gardeł wydajności sieci."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: magoedte
ms.openlocfilehash: 5fc2477e566fdea76294b62a738c0e18facbe629
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Monitor wydajności rozwiązania analizy dzienników sieci

![Symbol monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Ten dokument zawiera opis sposobu ustawiania i Użyj Monitora wydajności sieci rozwiązania analizy dzienników, która pomaga monitorować wydajność Twojej sieci w pobliżu real-jednorazowej do wykrywania i zlokalizuj wąskich gardeł wydajności sieci. Dzięki rozwiązaniu do monitora wydajności sieci można monitorować utraty i opóźnienia między dwiema sieciami podsieci lub serwerów. Monitor wydajności sieci wykrywa, takie jak blackholing ruchu, błędy routingu i problemów, które metody monitorowania z konwencjonalnej sieci nie są w stanie wykryć problemy z siecią. Monitor wydajności sieci generuje alerty i powiadamia, gdy naruszenia progu dla połączenia sieciowego. Progi te mogą być rozpoznawane automatycznie przez system, lub można skonfigurować je, aby użyć niestandardowej reguły alertów. Monitor wydajności sieci zapewnia szybkie wykrycie problemów z wydajnością sieci i lokalizuje źródłem problemu konkretnym segmencie sieci lub urządzenia.

Można wykrywać problemy z siecią z pulpitu nawigacyjnego rozwiązania. Wyświetla informacje podsumowania dotyczące sieci, w tym ostatnie zdarzenia kondycji sieci, łączy sieciowych w złej kondycji i łączy podsieci, stoją utraty pakietów wysokiej i opóźnień. Użytkownik może przejść do połączenia sieciowego, aby wyświetlić bieżący stan kondycji łączy podsieci, a także łącza między węzłami. Można również wyświetlić trendów historycznych utraty oraz czas oczekiwania na poziomie sieci, podsieci i węzła do węzła. Można wykrywać przejściowe problemy z siecią przez wyświetlanie trendów historycznych wykresów do utraty pakietów i opóźnienia i Znajdź wąskich gardeł sieci w formie mapy topologii. Wykres interaktywny topologii umożliwia wizualizację tras sieciowych przeskoku przeskoku i określeniem źródła problemu. Na przykład innych rozwiązań dziennik wyszukiwania różne wymagania analytics można używać do tworzenia niestandardowych raportów opartych na danych zbieranych przez Monitor wydajności sieci.

W tym rozwiązaniu zastosowano transakcji syntetycznych jako podstawowy mechanizm do wykrywania błędów sieci. Tak można go używać bez względu na urządzenie sieciowe określonego dostawcy lub modelu. Działa ona za pośrednictwem lokalnego, w chmurze (IaaS) i środowisk hybrydowych. Rozwiązanie automatycznie odnajduje topologii sieci i różne trasy w sieci.

Produkty monitorowania sieci typowe skupić się na monitorowanie kondycji urządzeń (routery, przełączniki itd.) w sieci, ale nie zapewniają wgląd w rzeczywista jakość połączenie sieciowe między dwoma punktami, które w Monitorze wydajności sieci.

### <a name="using-the-solution-standalone"></a>Przy użyciu rozwiązania autonomiczne
Jeśli chcesz monitorować jakość połączenia sieciowe między ich krytycznych obciążeń, sieci, centrów danych lub witryny pakietu office, a następnie możesz umożliwia rozwiązanie monitora wydajności sieci samodzielnie monitorowanie kondycji łączności między:

* wiele centrów danych lub office witryn, które są połączone za pomocą sieci publicznych lub prywatnych
* krytycznych obciążeń uruchomionych aplikacji — biznesowych
* usługi w chmurze publicznej, takich jak Microsoft Azure lub Amazon Web Services (AWS) i sieci lokalnej, jeśli masz IaaS (VM) dostępne i masz bram skonfigurowany tak, aby umożliwić komunikację między sieciami lokalnymi i sieciach w chmurze
* Sieci platformy Azure i lokalnego używania Express Route

### <a name="using-the-solution-with-other-networking-tools"></a>Za pomocą rozwiązania przy użyciu innych narzędzi sieci
Jeśli chcesz monitorować aplikacji biznesowych z umożliwia rozwiązanie monitora wydajności sieci jako rozwiązaniem pomocnika innych narzędzi sieci. Wolnej sieci może prowadzić do aplikacji powolne i Monitor wydajności sieci może ułatwić badać problemy wydajności aplikacji, które są spowodowane przez problemy z siecią podstawowej. Ponieważ rozwiązanie nie wymaga dostępu do urządzeń sieciowych, administrator aplikacji nie musi ufać sieci zespołu, aby podać informacje o sposobie sieci wpływa na aplikacje.

Ponadto jeśli już inwestycji w inne narzędzia do monitorowania sieci, następnie rozwiązania można uzupełnić te narzędzia ponieważ najbardziej tradycyjnych rozwiązań monitorowania sieci nie zapewniają wgląd w sieci end-to-end metryki wydajności, takich jak utraty i opóźnienia.  Rozwiązanie monitora wydajności sieci może pomóc wypełnienia tego odstępu.

## <a name="installing-and-configuring-agents-for-the-solution"></a>Instalowanie i konfigurowanie agentów dla rozwiązania
Należy zainstalować agentów na podstawowy proces [połączyć komputery do analizy dzienników](log-analytics-windows-agent.md) i [połączenie programu Operations Manager do analizy dzienników](log-analytics-om-agents.md).

> [!NOTE]
> Należy zainstalować co najmniej 2 agentów w celu ma za mało danych do odnajdywania i monitorowania zasobów sieciowych. W przeciwnym razie rozwiązania pozostaje w stanie konfigurowanie do momentu zainstalowania i skonfigurowania dodatkowych czynników.
>
>

### <a name="where-to-install-the-agents"></a>Gdzie chcesz zainstalować agentów
Przed zainstalowaniem agentów należy rozważyć topologię sieci i które części sieci, który chcesz monitorować. Zaleca się zainstalowanie więcej niż jednego agenta dla każdej podsieci, które chcesz monitorować. Innymi słowy dla każdej podsieci, która ma być monitorowany, wybierz co najmniej dwa serwery lub maszyn wirtualnych i zainstalować agenta na nich.

Jeśli nie wiesz o topologii sieci, należy zainstalować agentów na serwerach z krytycznych obciążeń, które chcesz monitorować wydajność sieci. Można na przykład informacje o połączenie sieciowe między serwerem sieci Web i serwer z programem SQL Server. W tym przykładzie należy zainstalować agenta na obu serwerach.

Agenci monitorują łączność sieciową (linki) między hostami — nie hosty samodzielnie. Tak aby monitorować połączenia sieciowego, należy zainstalować agentów na oba punkty końcowe tego łącza.

### <a name="configure-agents"></a>Konfiguruj agentów

Jeśli zamierzasz używać protokołu ICMP dla transakcji syntetycznych, należy włączyć następujące reguły zapory dla niezawodnego przy użyciu protokołu ICMP:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Jeśli zamierzasz korzystać z protokołu TCP, należy otworzyć porty zapory dla komputerów upewnić się, że agenci mogą się komunikować. Pobierz i uruchom [skrypt programu PowerShell EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez żadnych parametrów w oknie programu PowerShell z uprawnieniami administracyjnymi.

Skrypt tworzy kluczy rejestru wymaganych przez Monitor wydajności sieci i tworzy reguł zapory systemu Windows umożliwiają agentów do utworzenia połączenia TCP ze sobą. Klucze rejestru utworzony przez skrypt Określ również, czy do rejestrowania dzienników debugowania i ścieżkę do plików dzienników. Definiuje również port TCP agent używany do komunikacji. Wartości te klucze są automatycznie ustawiane za pomocą skryptu, dlatego nie należy ręcznie zmienić te klucze.

Otworzyć domyślny port to 8084. Można użyć portu niestandardowego, podając parametr `portNumber` do skryptu. Jednak tego samego portu należy używać na wszystkich komputerach, na którym skrypt jest uruchamiane.

> [!NOTE]
> Skrypt EnableRules.ps1 umożliwia skonfigurowanie reguł zapory systemu Windows tylko na komputerze, na którym skrypt jest uruchamiany. Jeśli masz zapory sieciowej, należy upewnić się, że umożliwia ruch kierowany do portu TCP używany przez Monitor wydajności sieci.
>
>

## <a name="configuring-the-solution"></a>Trwa konfigurowanie rozwiązania
Skorzystaj z poniższych informacji, aby zainstalować i skonfigurować rozwiązania.

1. Rozwiązanie monitora wydajności sieci uzyskuje dane z komputerów z systemem Windows Server 2008 z dodatkiem SP 1 lub nowszym lub Windows 7 z dodatkiem SP1 lub nowszym, które są te same wymagania jako Microsoft Monitoring Agent (MMA). Agenci programu NPM można również uruchomić na pulpicie/klienckie systemy operacyjne Windows (Windows 10, Windows 8.1, Windows 8 i Windows 7).
    >[!NOTE]
    >Agentów dla systemów operacyjnych Windows server obsługuje zarówno TCP, jak i protokołu ICMP jako protokoły dla transakcji syntetycznych. Agentów dla systemów operacyjnych Windows klient obsługuje jednak tylko ICMP jako protokół dla transakcji syntetycznych.

2. Dodaj rozwiązanie sieci monitora wydajności do swojego obszaru roboczego z [witrynę Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) lub przy użyciu procesu opisanego w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md).<br><br> ![Symbol monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-symbol.png)  
3. W portalu OMS, zostanie wyświetlony nowy Kafelek zatytułowany **monitora wydajności sieci** z komunikatem *rozwiązanie wymaga dodatkowej konfiguracji*. Kliknij Kafelek, aby przejść do **wdrożenia** i wybierz protokół używany do wybierania transakcji syntetycznych monitorowania sieci.  Przegląd [wybierz prawa ICMP protokołu lub TCP](#choose-the-right-protocol-icmp-or-tcp) pomaga wybrać odpowiedni protokół nadaje się do sieci.<br><br> ![rozwiązanie wymaga wybór protokołu](media/log-analytics-network-performance-monitor/log-analytics-netmon-perf-welcome.png)<br><br>

4. Po wybraniu protokołu, nastąpi przekierowanie do **omówienie OMS** strony. Gdy rozwiązanie agregują dane z sieci, kafelka przeglądu monitora wydajności sieci wyświetla komunikat informujący *agregacja danych w toku*.<br><br> ![rozwiązanie jest agregowanie danych](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-01.png)<br><br>
5. Gdy dane są zbierane i indeksowane zmiany Kafelek omówienie wskazująca, że należy wykonać dodatkowe czynności konfiguracyjne.<br><br> ![Kafelek rozwiązanie wymaga dodatkowej konfiguracji](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-02.png)<br><br>
6. Kliknij Kafelek i rozpocząć konfigurowanie rozwiązanie, wykonując następujące kroki.

### <a name="create-new-networks"></a>Tworzenie nowych sieci
Sieć w Monitorze wydajności w sieci jest kontenerem logicznym podsieci. Można utworzyć sieci o przyjaznej nazwie i dodać podsieci do niego według logiki biznesowej. Na przykład można utworzyć sieci o nazwie *Londyn* i dodać wszystkie podsieci w Londynie centrum danych lub w sieci o nazwie *ContosoFrontEnd* i dodać wszystkie podsieci obsługujących fronton aplikacji o nazwie Contoso z tą siecią.
Na stronie Konfiguracja Zobacz sieci o nazwie **domyślne** na karcie sieci. Jeśli nie utworzono żadnych sieci, wszystkie automatycznie odnalezione podsieci są umieszczane w domyślnej sieci.
Gdy utworzysz sieć Dodaj podsieć do niej i podsieci jest usuwany z domyślnej sieci. Jeśli usuniesz sieci, wszystkie jego podsieci automatycznie są zwracane do domyślnej sieci.
W związku z tym domyślnej sieci działa jako kontener dla wszystkich podsieci, które nie znajdują się w dowolnej sieci zdefiniowanej przez użytkownika. Nie można edytować ani usunąć domyślnej sieci. Zawsze pozostaje w systemie. Jednak można utworzyć dowolną liczbę sieci niestandardowe.
W większości przypadków podsieci w organizacji są rozmieszczone w więcej niż jedną sieć i należy utworzyć co najmniej jednej sieci, aby zgrupować podsieci zgodnie z harmonogramem logiki biznesowej

#### <a name="to-create-a-new-network"></a>Aby utworzyć nową sieć
1. Kliknij przycisk **sieci Dodaj** , a następnie wpisz nazwę sieciową i opis.
2. Wybierz co najmniej jednej podsieci, a następnie kliknij przycisk **Dodaj**.
3. Kliknij przycisk **zapisać** Aby zapisać konfigurację.<br><br> ![Dodawanie sieci](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Poczekaj, aż agregacja danych
Po zapisaniu konfiguracji po raz pierwszy, rozpoczyna się rozwiązania pobierania sieci pakietów utraty i opóźnienia między węzłami, na których zostali zainstalowani agenci. Ten proces może trochę potrwać, czasami 30 minut. W tym stanie kafelka monitora wydajności sieci na stronie przeglądu wyświetla komunikat z informacją *agregacja danych w procesie*.

![agregacja danych w toku](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Dane zostały przekazane, kafelka monitora wydajności sieci zostaje zaktualizowany dane są wyświetlane.

![Kafelek monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-tile.png)

Kliknij Kafelek, aby wyświetlić pulpit nawigacyjny monitora wydajności sieci.

![Pulpit nawigacyjny sieci monitora wydajności](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Edytuj ustawienia monitorowania dla podsieci
Wszystkie podsieci, gdy co najmniej jeden agent został zainstalowany są wyświetlane na **podsieci** karcie na stronie konfiguracji.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Aby włączyć lub wyłączyć monitorowanie dla określonej podsieci
1. Zaznacz lub usuń zaznaczenie pola obok **podsieci Identyfikatora** , a następnie upewnij się, że **na użytek monitorowania** jest wybrany lub wyczyszczone, odpowiednio. Można zaznaczyć lub wyczyścić wielu podsieci. Po wyłączeniu podsieciami, które nie są monitorowane, jak agenci zostaną zaktualizowane, aby zatrzymać polecenie ping innych agentów.
2. Wybierz węzły, które mają być monitorowane dla określonej podsieci, wybierając podsieci z listy i przenoszenie wymagane węzłów między listami zawierającego węzły niemonitorowane i monitorowane.
   Możesz dodać niestandardowe **opis** do podsieci.
3. Kliknij przycisk **zapisać** Aby zapisać konfigurację.<br><br> ![Edytuj podsieci](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Wybierz węzły do monitorowania
Wszystkie węzły, które zainstalowano na nich agenta są wymienione w **węzłów** kartę.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Aby włączyć lub wyłączyć monitorowanie dla węzłów
1. Zaznacz lub usuń węzły, które chcesz monitorować lub zatrzymać monitorowanie.
2. Kliknij przycisk **na użytek monitorowania**, lub wyczyść, zależnie od potrzeb.
3. Kliknij pozycję **Zapisz**.<br><br> ![Aby włączyć monitorowanie węzła](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Ustaw reguły monitorowania
Monitor wydajności sieci generuje zdarzenia kondycji, gdy naruszenia progu wydajności połączeń sieciowych między dwoma podsieci lub między dwiema sieciami. Progi te mogą być rozpoznawane automatycznie przez system lub zapewniają progami niestandardowymi.

System automatycznie tworzy domyślną regułę. Ta reguła umożliwia wygenerowanie zdarzenie kondycji, w przypadku utraty lub opóźnienia między jakiejkolwiek parze łączy sieć/podsieć breaches próg rozpoznawane przez system. Pomaga to rozwiązanie Monitorowanie infrastruktury sieci, dopóki nie utworzono żadnych reguł monitorowania jawnie. Jeśli domyślna reguła jest włączona, wszystkie węzły wysłać transakcji syntetycznych do wszystkich węzłów, które mają włączone do monitorowania. Domyślna reguła jest przydatne w małych sieciach. Na przykład w sytuacji, gdy masz niewielkiej liczby serwerów z systemem mikrousługi i chcesz upewnij się, że wszystkie serwery mają łączność między sobą.

>[!NOTE]
>Zdecydowanie zaleca się wyłączyć zasadę domyślną, a następnie utworzyć niestandardowe reguły monitorowania, szczególnie w przypadku dużych sieci, gdy używasz dużą liczbę węzłów do monitorowania. Pozwala to ograniczyć ruch generowany przez rozwiązanie i pomoc można organizować monitorowania sieci.

Tworzenie niestandardowych reguł monitorowania zgodnie z logiki biznesowej. Na przykład jeśli chcesz monitorować wydajność łączności sieciowej dwóch lokacji office z centralą grupie wszystkich podsieci w site1 pakietu office w sieci O1 wszystkich podsieci w witryna2 pakietu office w sieci O2 i wszystkich podsieci w sieci H. siedzibę główną Utwórz dwie reguły-do-jednego między O1 i H, a druga między O2 i H. monitorowania


#### <a name="to-create-custom-monitoring-rules"></a>Aby utworzyć niestandardowe reguły monitorowania
1. Kliknij przycisk **Dodaj regułę** w **Monitor** i wprowadzić nazwę reguły i opis.
2. Wybierz pary sieci lub podsieć łącza do monitorowania za pomocą list.
3. Najpierw wybierz sieć, w którym znajduje się pierwszy podsieć/s zainteresowań z listy rozwijanej sieci, a następnie wybierz podsieć/s z listy rozwijanej odpowiednich podsieci.
   Wybierz **wszystkie podsieci** Jeśli chcesz monitorować wszystkich podsieci w połączenia sieciowego. Podobnie wybierz inne podsieć/s zainteresowań. I kliknięcie **dodać wyjątek** do wykluczenia z monitorowania łączy określonej podsieci z zaznaczenia zostały wprowadzone.
4. [Wybór między protokołów ICMP i TCP](#choose-the-right-protocol-icmp-or-tcp) wykonania transakcji syntetycznych.
5. Jeśli nie chcesz utworzyć zdarzenia kondycji dla elementów jest zaznaczony, następnie wyczyść **Włącz monitorowanie kondycji w łączach objęte regułą**.
6. Wybierz warunki monitorowania.
   Można ustawić progami niestandardowymi kondycji zdarzenie generacji, wpisując wartości progowe. Zawsze, gdy wartość warunku wykraczają poza jego próg dla pary wybranego sieć/podsieć, jest generowane zdarzenie kondycji.
7. Kliknij przycisk **zapisać** Aby zapisać konfigurację.<br><br> ![Utwórz regułę niestandardową monitorowania](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Po zapisaniu reguły monitorowania tej reguły można zintegrować z zarządzania alertami, klikając **Tworzenie alertów**. Reguła alertu są tworzone przy użyciu zapytania wyszukiwania i innych wymaganych parametrów wypełniane automatycznie. Reguła alertu może otrzymywać pocztą e-mail alertów, oprócz istniejące alerty w ramach programu NPM. Alerty można również uruchomić czynności zaradczych z elementami runbook i ich można zintegrować z istniejącymi rozwiązaniami zarządzania usługi za pomocą elementów webhook. Możesz kliknąć **Zarządzanie Alert** Aby edytować ustawienia alertu.

### <a name="choose-the-right-protocol-icmp-or-tcp"></a>Wybierz prawa ICMP protokołu lub TCP

Monitor wydajności sieci (NPM) używa transakcji syntetycznych do obliczania metryki wydajności sieci, takich jak opóźnienie utratą i łącze pakietów. Aby lepiej zrozumieć to, należy wziąć pod uwagę połączony agenta NPM jeden koniec połączenia sieciowego. Ten agent NPM wysyła pakiety sondy do agenta programu NPM drugiego połączenia w innym celu sieci. Druga odpowiedź agenta z pakietów odpowiedzi. Ten proces jest powtarzany kilka razy. Mierząc kod odpowiedzi oraz czas potrzebny na odebranie odpowiedź pierwszego agenta NPM ocenia czas oczekiwania linku oraz operacjach porzucania pakietów.

Format, rozmiar i kolejność tych pakietów jest określana przez protokół, który można wybrać podczas tworzenia reguł monitorowania. Na podstawie protokołu pakietów, pośrednich sieci urządzenia (routery, przełączniki itd.) może różnie te pakiety. W rezultacie wybranego protokołu ma wpływ na dokładność wyników. I wybór protokołu określa, czy wszystkie wymagane ręczne wykonanie czynności należy wykonać po wdrożeniu rozwiązania NPM.

NPM umożliwia wybór między protokołów ICMP i TCP wykonania transakcji syntetycznych.
Jeśli wybierzesz protokołu ICMP, podczas tworzenia reguły transakcji syntetycznych, agenci NPM umożliwia obliczenie opóźnienie sieci i utraty pakietów komunikatów ECHA protokołu ICMP. Ten sam komunikat wysyłany przez narzędzie konwencjonalnej korzysta z ECHA protokołu ICMP. Użycie jako protokołu TCP, NPM agenci wysyłają pakietu TCP SYN za pośrednictwem sieci. Następuje uzgadniania TCP uzupełniania i usunięcie połączenia przy użyciu RST pakietów.

#### <a name="points-to-consider-before-choosing-the-protocol"></a>Kwestie do rozważenia przed wybraniem protokołu
Aby wybrać protokół do użycia, należy wziąć pod uwagę następujące informacje:

##### <a name="discovering-multiple-network-routes"></a>Odnajdywanie wiele tras z sieci
Protokół TCP jest dokładniejsze podczas rozpoznawania wiele tras i wymaga mniejszej liczby agentów w każdej podsieci. Na przykład jeden lub dwaj agenci za pomocą protokołu TCP umożliwia odnalezienie wszystkich nadmiarowe ścieżki między podsieciami. Należy jednak kilka czynników, aby to zrobić za pomocą protokołu ICMP. Za pomocą protokołu ICMP, jeśli masz *N* liczba tras między dwiema podsieciami, potrzebujesz więcej niż 5*N* agentów w podsieci źródłowe lub docelowe.

##### <a name="accuracy-of-results"></a>Dokładność wyników
Routery i przełączniki zwykle można przypisać niższy priorytet pakietów ECHA protokołu ICMP w porównaniu do pakietów TCP. W niektórych sytuacjach silnie ładowane urządzeń sieciowych, dane uzyskiwane przez protokół TCP dokładniejsze odzwierciedla utraty i opóźnienia wystąpił przez aplikacje. Dzieje się tak, ponieważ większość aplikacji ruch przepływa za pośrednictwem protokołu TCP. W takich przypadkach ICMP zawiera mniej dokładne wyniki w porównaniu do TCP.

##### <a name="firewall-configuration"></a>Konfiguracja zapory
Protokół TCP wymaga, aby TCP pakiety są wysyłane do portu docelowego. Domyślny port używany przez agentów NPM jest 8084, ale można go zmienić po skonfigurowaniu agentów. Dlatego należy się upewnić, czy z zapory sieciowe lub reguły NSG (na platformie Azure) są zezwala na ruch na porcie. Należy również upewnić, że zapory lokalnej na komputerach, na których zainstalowano agentów jest skonfigurowane zezwalająca na ruch na tym porcie.

Można skonfigurować reguł zapory na komputerach z systemem Windows, jednak należy ręcznie skonfigurować zapory sieciowej, można użyć skryptów środowiska PowerShell.

Z kolei w protokołu ICMP przy użyciu portu nie będzie działać. W większości przypadków enterprise ruch protokołu ICMP jest dozwolone za pośrednictwem zapór umożliwiają używanie narzędzia diagnostyczne sieci, takie jak narzędzie Ping. Tak, jeśli jeden komputer z innej można wywołać, można użyć protokołu ICMP, bez konieczności ręcznego konfigurowania zapory.

> [!NOTE]
> Niektóre zapory można zablokować protokołu ICMP, co może prowadzić do retransmisji powodujące dużej liczby zdarzeń w systemie zarządzania informacji i zdarzeń zabezpieczeń. Upewnij się, że protokół, który wybrano nie jest blokowane przez zaporę sieci/NSG, w przeciwnym razie NPM nie będzie monitorować segmencie sieci.  W związku z tym zaleca się używać protokołu TCP do monitorowania.
> Należy używać protokołu ICMP w scenariuszach, w którym nie będą używać protokołu TCP, takie jak czas:
> * Używasz węzłów opartą na kliencie systemu Windows, ponieważ gniazd TCP nie są dozwolone w kliencie systemu Windows
> * Bloki zapory/NSG Twojej sieci TCP


#### <a name="how-to-switch-the-protocol"></a>Jak przełączać protokołu

Jeśli została wybrana opcja do wykorzystania podczas wdrażania protokołu ICMP, możesz przełączyć TCP, aby w dowolnym momencie poprzez edycję domyślna reguła monitorowania.

##### <a name="to-edit-the-default-monitoring-rule"></a>Aby edytować wartość domyślna reguła monitorowania
1.  Przejdź do **wydajność sieci** > **Monitor** > **skonfigurować** > **Monitor** a następnie kliknij przycisk **domyślną regułę**.
2.  Przewiń do **protokołu** sekcji, a następnie wybierz protokół, którego chcesz używać.
3.  Kliknij przycisk **zapisać** Aby zastosować ustawienia.

Nawet jeśli domyślna reguła jest przy użyciu określonego protokołu, można utworzyć nowe reguły z innego protokołu. Nawet tworzenia różnych zasad gdzie Użyj reguł ICMP i używa innego protokołu TCP.


## <a name="data-collection-details"></a>Szczegóły danych kolekcji
Monitor wydajności sieci używa pakietów uzgadnianie TCP SYN SYNACK potwierdzenie po wybraniu TCP i odpowiedzi ECHA ICMP po wybraniu ICMP jako protokół zbierać informacje o utracie i opóźnienia. Traceroute jest również używany do pobierania informacji o topologii.

W poniższej tabeli przedstawiono metody zbierania danych i inne szczegółowe informacje o jak dane są zbierane dla monitora wydajności sieci.

| Platformy | Bezpośrednie agenta | Agenta programu SCOM | Azure Storage | SCOM wymagane? | Dane agenta programu SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość kolekcji |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Komunikaty ECHA ICMP/uzgodnienia TCP co 5 sekund danych wysyłane co 3 minuty |

W tym rozwiązaniu zastosowano transakcji syntetycznych do oceny kondycji sieci. Agenci OMS zainstalowany w różnych miejscach pakiety TCP exchange sieci lub echa protokołu ICMP (w zależności od protokołu wybrany do monitorowania) ze sobą. W procesie agentów Dowiedz się obustronne straty czasu i pakietów, jeśli istnieje. Okresowo każdy agent wykonuje także śledzenie trasy do innych agentów można znaleźć w sieci, należy sprawdzić wszystkie różne trasy. Przy użyciu tych danych, agentów można wywnioskować opóźnienia sieci i rysunki utraty pakietów. Testy są powtarzane co pięć sekund i dane są agregowane dla trzy minuty przez agentów przed przekazaniem go z usługą analizy dzienników.

> [!NOTE]
> Mimo że agentów komunikują się ze sobą często, nie generują one dużą ilość ruchu sieciowego podczas przeprowadzania testów. Agentów polegać tylko na TCP SYN SYNACK potwierdzenie uzgadnianie pakietów do określenia utraty i opóźnienia — żadne dane, które są wymieniane pakietów. W trakcie tego procesu agentów komunikują się ze sobą tylko w razie potrzeby, a topologia komunikacja agenta jest optymalizowane w celu zmniejszenie ruchu w sieci.
>
>

## <a name="using-the-solution"></a>Użycie rozwiązania
W tej sekcji opisano pulpitu nawigacyjnego funkcje i sposobu ich używania.

### <a name="solution-overview-tile"></a>Kafelka przeglądu rozwiązania
Po włączeniu rozwiązania monitora wydajności sieci kafelka rozwiązania na stronie Przegląd OMS zapewnia szybki przegląd kondycji sieci. Wyświetla wykres przedstawiający liczbę łączy podsieci w dobrej kondycji i złej kondycji. Po kliknięciu kafelka otwiera pulpitu nawigacyjnego rozwiązania.

![Kafelek monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Pulpit nawigacyjny rozwiązania monitora wydajności sieci
**Podsumowania sieci** obszaru zawiera podsumowanie informacji o sieci wraz z ich rozmiar względny. Następuje to Kafelki przedstawiający łączna liczba łączy sieciowych, łącza podsieci i ścieżki w systemie (ścieżka składa się z adresów IP dwa hosty z agentami i wszystkich przeskokach między nimi).

**Zdarzenia kondycji sieci górnej** obszaru zawiera listę najnowszych zdarzeń, kondycji i alertów w systemie i czasu, ponieważ zdarzenie zostało active. Zdarzenie kondycji lub alert jest generowany utraty pakietów lub opóźnienia sieci lub podsieć łącza przekracza próg.

**Łączy sieciowych w złej kondycji górnej** obszaru zawiera listę łączy sieciowych w złej kondycji. Są to łączy sieciowych, który ma co najmniej jednego zdarzenia kondycji niekorzystny ich w tej chwili.

**Łączy podsieci Top najbardziej utraty** i **łączy podsieci z opóźnieniem najbardziej** obszarów Wyświetl łączy podsieci top przez utraty pakietów i odpowiednio góry łączy podsieci opóźnienie. Duże opóźnienie lub niektóre ilości utraty pakietów mogą znajdować się na niektórych łączy sieciowych. Takie łącza są wyświetlane na liście dziesięć top, ale nie jest oznaczony jako niepoprawny.

**Typowych kwerend** obszaru zawiera zestaw zapytania wyszukiwania, które pobierają sieci nieprzetworzone dane monitorowania bezpośrednio. Te zapytania można użyć jako punktu wyjścia do tworzenia zapytań niestandardowych raportowania.

![Pulpit nawigacyjny sieci monitora wydajności](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Przechodzenie do głębokość
Możesz kliknąć linki różnych na pulpicie nawigacyjnym rozwiązania, aby przejść do dół głębiej do wszystkie obszary zainteresowań. Na przykład gdy zostanie wyświetlony alert lub łączy sieciowych w złej kondycji, są wyświetlane na pulpicie nawigacyjnym, kliknięcie go, aby zbadać dokładnie. Zostaje wyświetlona strona, która zawiera listę wszystkich łączy podsieci dla określonej sieci łącza. Można wyświetlić utraty, opóźnienia i kondycji stanu każdego łącza podsieci i szybko dowiedzieć łączy podsieci, które są przyczyną problemu. Następnie możesz kliknąć **wyświetlanie łączy węzłów** wyświetlić wszystkich łączy węzłów łącza podsieci w złej kondycji. Następnie możesz wyświetlić poszczególnych łączy węzła do węzła i łącza węzłów w złej kondycji.

Możesz kliknąć **widoku topologii** do wyświetlania topologii przeskoku przeskoku tras między węzły źródłowe i docelowe. Zła tras lub przeskoków są wyświetlane na czerwono, dzięki czemu można szybko zidentyfikować problem do określonej części sieci.

![drążenie danych](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Rejestrator stanu sieci

Każdy widok wyświetla migawkę kondycji Twojej sieci w określonym punkcie w czasie. Domyślnie wyświetlany jest stan ostatniej. Na pasku w górnej części strony wyświetlany punkt w czasie, dla którego stan jest wyświetlany. Możesz przejść wstecz w czasie i wyświetlić migawki kondycji Twojej sieci, kliknij na pasku **akcje**. Można również włączyć lub wyłączyć automatyczne odświeżanie na każdej stronie podczas przeglądania najnowszy stan.

![Stan sieci](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Trend wykresów
Na każdym poziomie Drąż w dół można wyświetlić trend utraty oraz czas oczekiwania dla połączenia sieciowego. Wykresy trendu są również dostępne dla łączy podsieci i węzła. Można zmienić interwał wykresu do wykreślenia za pomocą formantu czasu w górnej części wykresu.

Wykresy trendu Pokaż historycznych perspektywy wydajność połączenia sieciowego. Niektóre problemy dotyczące sieci są charakter przejściowy oraz będzie trudne do catch tylko na podstawie bieżącego stanu sieci. Jest to spowodowane problemy można szybko surface i zniknąć przed każdy powiadomienia, tylko do pojawić się ponownie w późniejszym czasie, w czasie. Takie przejściowych problemów również może być trudne dla administratorów aplikacji, ponieważ te problemy często powierzchni wraz ze wzrostem niewyjaśniony czas odpowiedzi aplikacji, nawet jeśli wszystkie składniki aplikacji są wyświetlane bezproblemowe działanie.

Tego rodzaju problemy mogą łatwo wykryć, analizując wykresu trendu, gdy problem pojawi się nagłym kolekcji w opóźnieniem sieci lub utraty pakietów.

![wykresu trendu](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>mapy topologii przeskoku przeskoku
Monitor wydajności sieci przedstawia topologię przeskoku przeskoku tras między dwa węzły na mapie interakcyjne topologii. Mapy topologii można wyświetlić, wybierając węzeł łącze, a następnie klikając polecenie **widoku topologii**. Ponadto można wyświetlić mapy topologii, klikając **ścieżki** kafelka na pulpicie nawigacyjnym. Po kliknięciu **ścieżki** na pulpicie nawigacyjnym masz wybierz węzeł źródłowy i docelowy z lewego panelu, a następnie kliknij przycisk **kreślenia** do wykreślenia tras między dwoma węzłami.

Wyświetla mapy topologii, ile trasy są między dwoma węzłami i co ścieżek zająć pakietów danych. Wąskich gardeł wydajności sieci są oznaczone na czerwono na mapy topologii. Połączenie sieciowe uszkodzony lub uszkodzenia urządzenia sieciowego można znaleźć analizując red kolorowe elementy na mapie topologii.

Po kliknięciu węzła lub kursor nad nim mapy topologii, można znaleźć we właściwościach węzła, takie jak adres IP i nazwy FQDN. Kliknij przycisk przeskoków, aby wyświetlić jego adresu IP. Istnieje możliwość filtrowania tras określonego za pomocą filtrów w okienku akcji zwijane. I topologii sieci można również uprościć, ukrywając przeskoków za pomocą suwaka, w okienku akcji. Możesz powiększania lub out mapy topologii za pomocą kółka myszy.

Pamiętaj, że topologii pokazano mapy topologii warstwy 3 i nie zawiera warstwy 2 urządzenia i połączeń.

![mapy topologii przeskoku przeskoku](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizacja usterek
Monitor wydajności sieci jest w stanie znaleźć wąskie gardła sieci bez połączenia z urządzeniami sieciowymi. Na podstawie danych zebranych z sieci i stosując zaawansowane algorytmy na wykresie sieci, Monitor wydajności sieci sprawia, że prawdopodobieństwa oszacowanie części sieci, które są najbardziej prawdopodobną przyczyną problemu.

Takie rozwiązanie jest przydatne do określenia wąskich gardeł sieci, gdy dostęp do przeskoków jest niedostępna, ponieważ nie wymaga żadnych danych do zebrania z urządzeń sieciowych, takich jak routery i przełączniki. Jest to przydatne także wtedy, gdy liczba przeskoków między dwoma węzłami nie znajdują się w sieci kontrolę administracyjną. Na przykład przeskoków może być routery usługodawców internetowych.

### <a name="log-analytics-search"></a>Wyszukaj analizy dzienników
Wszystkie dane, które jest dostępne w postaci graficznej za pośrednictwem Monitora wydajności sieci pulpitu nawigacyjnego i przechodzenie do strony jest również dostępna natywnie w wyszukiwaniu analizy dzienników. Można wysyłać zapytania o dane przy użyciu języka zapytań wyszukiwania i twórz raporty niestandardowe przez eksportowanie danych do programu Excel lub Power BI. **Typowych kwerend** obszar na pulpicie nawigacyjnym ma niektóre przydatne zapytań, które służy jako punkt początkowy do tworzenia własnych kwerendy i raporty.

![zapytania wyszukiwania](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-the-root-cause-of-a-health-alert"></a>Znajdź jego główną przyczynę alertu health
Teraz, gdy zostały przedstawione monitora wydajności sieci, Przyjrzyjmy się proste badanie przyczynę kondycji zdarzenie.

1. Na stronie Przegląd otrzymasz szybkiej migawki kondycji sieci, obserwując **monitora wydajności sieci** kafelka. Należy zauważyć, że poza 6 podsieć łączy monitorowane, jest w złej kondycji 2. Gwarantuje to dochodzenia. Kliknij Kafelek, aby wyświetlić pulpit nawigacyjny rozwiązania.<br><br> ![Kafelek monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-investigation01.png)  
2. Na poniższej ilustracji Zwróć uwagę, że istnieje zdarzenie kondycji połączenia sieciowego, który jest nieprawidłowy. Chcesz zbadać problem, a następnie kliknij polecenie **DMZ2 DMZ1** połączenia sieciowego, aby sprawdzić przyczyny problemu.<br><br> ![przykład łączy sieciowych w złej kondycji](./media/log-analytics-network-performance-monitor/npm-investigation02.png)  
3. Przechodzenie do strony pokazuje wszystkich łączy podsieci w **DMZ2 DMZ1** połączenia sieciowego. Można zauważyć, że dla obu łączy podsieci opóźnienie przekroczyło wartość progową, tworzenia połączenia sieciowego złej kondycji. Można również sprawdzić czas oczekiwania trendów łączy podsieci. Wybór czasu można użyć formantu na wykresie, aby skoncentrować się na zakres wymagany czas. Widać porę dnia, gdy czas oczekiwania został osiągnięty Szczyt. Dzienniki dla tego okresu do badania problemu można będzie później wyszukiwać. Kliknij przycisk **wyświetlanie łączy węzłów** aby przejść dalej.<br><br> ![przykład łączy podsieci w złej kondycji](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Podobnie do poprzedniej strony, przechodzenie strony dla określonej podsieci łącza wyświetlane w jego łączy węzłów składników. Można wykonywać działania podobne tutaj tak samo jak w poprzednim kroku. Kliknij przycisk **widoku topologii** do wyświetlania topologii między węzłami 2.<br><br> ![przykład łączy węzłów w złej kondycji](./media/log-analytics-network-performance-monitor/npm-investigation04.png)  
5. Wszystkie ścieżki między 2 wybrane węzły na wykresie mapy topologii. Można zwizualizować topologii przeskoku przeskoku tras między dwa węzły na mapie topologii. Umożliwia danych istnieje jak wiele tras między dwa węzły i co ścieżek trwa pakietów danych. Wąskich gardeł wydajności sieci są oznaczone kolorem czerwonym. Połączenie sieciowe uszkodzony lub uszkodzenia urządzenia sieciowego można znaleźć analizując red kolorowe elementy na mapie topologii.<br><br> ![przykład widoku topologii złej kondycji](./media/log-analytics-network-performance-monitor/npm-investigation05.png)  
6. Utrata, opóźnienia i liczby przeskoków w poszczególnych ścieżek można przeglądać w **akcji** okienka. Umożliwia wyświetlenie szczegółów tych ścieżek zła pasek przewijania.  Użyj filtrów, aby zaznaczyć ścieżek o złej kondycji przeskoku, dzięki czemu są kreślone topologii dla wybranej ścieżki. Można użyć kółka myszy, aby powiększyć lub pomniejszyć mapy topologii.

   W poniżej obrazu analizując ścieżki i przeskoków kolorem czerwonym wyraźnie widzi głównej przyczyny problemów do określonej sekcji sieci. Kliknięcie w węźle mapy topologii ujawnia właściwości węzła, w tym nazwę FQDN i adres IP. Kliknięcie skoku zawiera adres IP przeskoku.<br><br> ![Zła topologia — przykład szczegóły ścieżki](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Przekazywanie opinii

- **UserVoice** — możesz zamieścić pomysłów funkcji Monitora wydajności sieci, które chcesz pracować nad. Można znaleźć w naszych [strony UserVoice](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Dołącz do naszych kohorty** — NAS interesuje zawsze o nowych klientów, Dołącz do naszych kohorty. W ramach go uzyskać wczesny dostęp do nowych funkcji i pomóc nam w ulepszaniu monitora wydajności sieci. Jeśli interesuje Cię dołączenie, wypełnienie wychodzący to [szybkie ankiety](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Kolejne kroki
* [Wyszukaj dzienniki](log-analytics-log-searches.md) do wyświetlania rekordów danych wydajności szczegółowe sieci.
