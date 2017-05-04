---
title: "Rozwiązanie do zarządzania aktualizacjami w usłudze OMS | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomaga zrozumieć, jak używać tego rozwiązania do zarządzania aktualizacjami komputerów z systemami Windows i Linux."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: b0624bde9ab53231768beb0c832a4a49b21a8975
ms.lasthandoff: 04/20/2017


---
# <a name="update-management-solution-in-oms"></a>Rozwiązanie do zarządzania aktualizacjami w usłudze OMS
Rozwiązanie do zarządzania aktualizacjami w usłudze OMS pozwala na zarządzanie aktualizacjami dla komputerów z systemami Windows i Linux.  Umożliwia ono szybką ocenę stanu dostępnych aktualizacji na wszystkich komputerach z agentami i zainicjowanie procesu instalacji wymaganych aktualizacji serwerów. 

## <a name="solution-components"></a>Składniki rozwiązania

Komputery zarządzane przez usługi OMS używają następujących elementów do wykonywania oceny i aktualizacji wdrożeń: 

* Agent usługi OMS dla systemu Windows lub Linux
* Platforma PowerShell Desired State Configuration (DSC) dla systemu Linux 
* Hybrydowy proces roboczy elementu runbook usługi Automation 
* Usługa Microsoft Update lub Windows Server Update Services dla komputerów z systemem Windows

Na poniższych diagramach przedstawiono koncepcyjny widok działania i przepływu danych dotyczący oceniania i stosowania aktualizacji dla wszystkich podłączonych komputerów z systemami Windows Server i Linux w obszarze roboczym.    

#### <a name="windows-server"></a>Oprogramowanie Windows Server
![Przepływ procesu zarządzania aktualizacjami dla systemu Windows Server](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Przepływ procesu zarządzania aktualizacjami dla systemu Linux](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Gdy komputer przeprowadzi skanowanie pod kątem zgodności aktualizacji, agent usługi OMS przekazuje zbiorczo informacje do usługi OMS. Na komputerze z systemem Windows skanowanie pod kątem zgodności jest domyślnie przeprowadzane co 12 godzin.  Skanowanie pod kątem zgodności aktualizacji, oprócz tego, że jest przeprowadzane zgodnie z harmonogramem, jest także inicjowane w ciągu 15 minut po ponownym uruchomieniu programu Microsoft Monitoring Agent (MMA), przed instalacją aktualizacji i po zainstalowaniu aktualizacji.  Na komputerze z systemem Linux skanowanie pod kątem zgodności jest domyślnie przeprowadzane co 3 godziny, a także inicjowane w ciągu 15 minut po ponownym uruchomieniu agenta programu MMA.  

Informacje o zgodności są następnie przetwarzane i podsumowywane na pulpitach nawigacyjnych zawartych w rozwiązaniu oraz można je przeszukiwać za pomocą zapytań zdefiniowanych wstępnie lub przez użytkownika.  Rozwiązanie informuje, jak aktualne jest oprogramowanie na komputerze, bazując na źródle skonfigurowanym na potrzeby synchronizacji.  Jeśli komputer z systemem Windows jest skonfigurowany do raportowania do usługi WSUS, to w zależności od tego, kiedy usługa WSUS była ostatnio synchronizowana z usługą Microsoft Update, wyniki mogą się różnić od tych pokazywanych przez usługę Microsoft Updates.  To samo dotyczy komputerów z systemem Linux skonfigurowanych do raportowania do repozytorium lokalnego i repozytorium publicznego.   

Aktualizacje oprogramowania można wdrożyć i zainstalować na komputerach, które ich wymagają, tworząc zaplanowane wdrożenie.  Aktualizacje sklasyfikowane jako *Opcjonalne* są poza zakresem wdrożenia dla komputerów z systemem Windows. W zakresie tym są tylko aktualizacje wymagane.  Zaplanowane wdrożenie definiuje, które komputery docelowe otrzymają odpowiednie aktualizacje. Odbędzie się to jawnie, przez określenie komputerów, albo wybranie [grupy komputerów](../log-analytics/log-analytics-computer-groups.md) opartej na przeszukiwaniu dzienników w określonej grupie komputerów.  Można również określić harmonogram zatwierdzania i wyznaczyć okres, w którym można instalować aktualizacje.  Aktualizacje są instalowane przez elementy runbook w usłudze Azure Automation.  Nie można wyświetlić tych elementów runbook i nie wymagają one żadnej konfiguracji.  Utworzenie wdrożenia aktualizacji powoduje utworzenie harmonogramu, który uruchamia główny element runbook aktualizacji w określonym czasie na uwzględnionych komputerach.  Ten główny element runbook uruchamia podrzędny element runbook na każdym agencie, który przeprowadza instalację wymaganych aktualizacji.       

W dniu i o godzinie określonych we wdrożeniu aktualizacji komputery docelowe wykonują równolegle wdrożenie.  Najpierw jest wykonywane skanowanie, aby sprawdzić, czy aktualizacje są ciągle wymagane. Dopiero wtedy są one instalowane.  Należy zauważyć, że w przypadku komputerów klienckich usługi WSUS wdrażanie aktualizacji niezatwierdzonych w usłudze WSUS nie powiedzie się.  Wyniki zastosowania aktualizacji są przekazywane do usługi OMS do przetworzenia i podsumowania na pulpitach nawigacyjnych lub przez wyszukiwanie zdarzeń.     

## <a name="prerequisites"></a>Wymagania wstępne
* Rozwiązanie obsługuje przeprowadzanie ocen aktualizacji dla systemu Windows Server 2008 lub nowszych i wdrożeń aktualizacji systemu Windows Server 2012 lub nowszych.  Opcje instalacji Server Core i Nano Server nie są obsługiwane.
* Systemy operacyjne klienta systemu Windows nie są obsługiwane.  
* Agenci dla systemu Windows muszą być skonfigurowani do komunikowania się z serwerem Windows Server Update Services (WSUS) albo mieć dostęp do usługi Microsoft Update.  
  
    > [!NOTE]
    > Agent dla systemu Windows nie może być zarządzany współbieżnie przez program System Center Configuration Manager.  
    >
* CentOS 6 (x86/x64) i 7 (x64)
* Red Hat Enterprise 6 (x86/x64) i 7 (x64)
* SUSE Linux Enterprise Server 11 (x86/x64) i 12 (x64)
* Ubuntu 12.04 LTS i nowsze x86/x64  
* Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.  

    > [!NOTE]
    > Agent usługi OMS dla systemu Linux skonfigurowany pod kątem raportowania do wielu obszarów roboczych usługi OMS nie jest obsługiwany przez to rozwiązanie.  
    > 

Aby uzyskać dodatkowe informacje na temat instalowania agenta usługi OMS dla systemu Linux i pobierania najnowszej wersji, zapoznaj się z tematem [Operations Management Suite Agent for Linux](https://github.com/microsoft/oms-agent-for-linux) (Agent usługi Operations Management Suite dla systemu Linux).  Aby uzyskać informacje na temat sposobu instalowania agenta usługi OMS dla systemu Windows, przejrzyj temat [Operations Management Suite Agent for Windows](../log-analytics/log-analytics-windows-agents.md) (Agent usługi Operations Management Suite dla systemu Windows).  

## <a name="solution-components"></a>Składniki rozwiązania
To rozwiązanie składa się z następujących zasobów, które są dodawane do Twojego konta usługi Automation, i bezpośrednio połączonych agentów lub grupy zarządzania połączonej z programem Operations Manager. 

### <a name="management-packs"></a>Pakiety administracyjne
Jeśli grupa zarządzania programu System Center Operations Manager jest połączona z obszarem roboczym usługi OMS, to następujące pakiety administracyjne są instalowane w programie Operations Manager.  Te pakiety administracyjne są również instalowane na bezpośrednio połączonych komputerach z systemem Windows po dodaniu tego rozwiązania. W przypadku tych pakietów administracyjnych nie trzeba niczego konfigurować ani niczym zarządzać. 

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pakiet administracyjny wdrożenia aktualizacji

Aby uzyskać więcej informacji na temat aktualizowania pakietów administracyjnych rozwiązania, zobacz artykuł [Connect Operations Manager to Log Analytics](../log-analytics/log-analytics-om-agents.md) (Połączenie programu Operations Manager z usługą Log Analytics).

### <a name="hybrid-worker-groups"></a>Grupy hybrydowych procesów roboczych
Włączenie tego rozwiązania powoduje automatyczne skonfigurowanie każdego komputera z systemem Windows połączonego z obszarem roboczym usługi OMS jako hybrydowego procesu roboczego elementu runbook w celu obsługi elementów runbook należących do tego rozwiązania.  Każdy komputer z systemem Windows zarządzany przez to rozwiązanie będzie wyświetlany w bloku Grupy hybrydowych procesów roboczych elementu runbook konta usługi Automation. Konwencja nazw będzie następująca: *Nazwa_hosta FQDN_GUID*.  Te grupy nie mogą być celami dla elementów runbook na Twoim koncie, w przeciwnym razie działanie tych elementów zakończy się niepowodzeniem. Te grupy są przeznaczone wyłącznie do obsługi rozwiązania do zarządzania.   

Możesz jednak dodać komputery z systemem Windows do grupy hybrydowych procesów roboczych elementów runbook na Twoim koncie usługi Automation w celu obsługi elementów runbook usługi Automation, o ile używasz tego samego konta zarówno dla tego rozwiązania, jak i dla członkostwa w grupie hybrydowych procesów roboczych elementów runbook.  Ta funkcjonalność została dodana do wersji 7.2.12024.0 hybrydowego procesu roboczego elementu Runbook.  

## <a name="configuration"></a>Konfiguracja
Wykonaj poniższe kroki, aby dodać rozwiązanie do zarządzania aktualizacjami do swojego obszaru roboczego usługi OMS, a także potwierdzić, że agenci wykonują raportowanie. Agenci systemu Windows są dodawani automatycznie bez dodatkowej konfiguracji. 

Istnieją trzy sposoby na dodanie tego rozwiązania: z witryny Azure Marketplace w witrynie Azure Portal, przez wybranie oferty Automation & Control lub rozwiązania Zarządzanie aktualizacjami, lub z galerii rozwiązań usługi OMS w obszarze roboczym usługi OMS.  Jeśli masz już konto usługi Automation i obszar roboczy usługi OMS połączone ze sobą w tej samej grupie zasobów i w tym samym regionie, wybranie pozycji Automation & Control spowoduje sprawdzenie Twojej konfiguracji i zainstalowanie samego rozwiązania oraz skonfigurowanie go w obu usługach.  Wybranie rozwiązania Zarządzanie aktualizacjami z witryny Azure Marketplace ma taki sam skutek.  Jeśli którejś z usług nie masz wdrożonej w swojej subskrypcji, wykonaj kroki w bloku **Tworzenie nowego rozwiązania** i potwierdź, że chcesz zainstalować inne wstępnie wybrane, zalecane rozwiązania.  Opcjonalnie możesz dodać rozwiązanie Zarządzanie aktualizacjami do swojego obszaru roboczego usługi OMS przy użyciu kroków opisanych w artykule [Add OMS solutions](../log-analytics/log-analytics-add-solutions.md) (Dodawanie rozwiązań usługi OMS) z galerii rozwiązań.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-to-oms"></a>Potwierdzanie, że agenci usługi OMS i grupa zarządzania programu Operations Manager są połączone z usługą OMS

Aby potwierdzić, że bezpośrednio połączeni agenci usługi OMS dla systemów Linux i Windows komunikują się z usługą OMS, po kilku minutach możesz uruchomić następujące wyszukiwanie w dzienniku:

* Linux — `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows — `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Na komputerze z systemem Windows możesz przejrzeć następujące informacje, aby sprawdzić połączenie agenta z usługą OMS:

1.  Otwórz program Microsoft Monitoring Agent w Panelu sterowania. Na karcie **Azure Log Analytics (OMS)** agent wyświetla komunikat z następującą informacją: **Program Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite**.   
2.  Otwórz Dziennik zdarzeń systemu Windows, przejdź do pozycji **Dzienniki aplikacji i usług\Operacje** i wyszukaj identyfikatory zdarzeń 3000 i 5002 ze źródła Service Connector.  Te zdarzenia wskazują, że komputer został zarejestrowany w obszarze roboczym usługi OMS i odbiera konfigurację.  

Jeśli agent nie może komunikować się z usługą OMS i jest skonfigurowany do komunikacji z Internetem przez zaporę lub serwer proxy, potwierdź, że zapora lub serwer proxy ma prawidłową konfigurację, przeglądając artykuł [Konfigurowanie ustawień serwera proxy i zapory w usłudze Log Analytics](../log-analytics/log-analytics-proxy-firewall.md).
  
Po przeprowadzeniu oceny nowo dodani agenci systemu Linux będą mieć stan **Zaktualizowane**.  Ten proces może potrwać do 6 godzin. 

Aby potwierdzić, że grupa zarządzania programu Operations Manager komunikuje się z usługą OMS, zobacz [Validate Operations Manager Integration with OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms) (Weryfikowanie integracji programu Operations Manager z usługą OMS).

## <a name="data-collection"></a>Zbieranie danych
### <a name="supported-agents"></a>Obsługiwani agenci
W poniższej tabeli opisano połączone źródła, które obsługuje to rozwiązanie.

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Windows i inicjuje instalowanie wymaganych aktualizacji. |
| Agenci dla systemu Linux |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Linux i inicjuje instalowanie wymaganych aktualizacji w obsługiwanych dystrybucjach. |
| Grupa zarządzania programu Operations Manager |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów w połączonej grupie zarządzania.<br>Bezpośrednie połączenie agenta programu Operations Manager z usługą Log Analytics nie jest wymagane. Dane są przekazywane z grupy zarządzania do repozytorium usługi OMS. |
| Konto magazynu Azure |Nie |Magazyn Azure nie zawiera informacji o aktualizacjach systemu. |

### <a name="collection-frequency"></a>Częstotliwość zbierania
Skanowanie każdego zarządzanego komputera z systemem Windows odbywa się dwa razy dziennie. Co 15 minut wywoływany jest interfejs API systemu Windows, aby wykonać zapytanie o czas ostatniej aktualizacji w celu sprawdzenia, czy stan się zmienił, a jeśli tak, inicjowane jest skanowanie pod kątem zgodności.  Skanowanie każdego zarządzanego komputera z systemem Linux odbywa się co 3 godziny. 

Wyświetlenie zaktualizowanych danych z zarządzanych komputerów na pulpicie nawigacyjnym może potrwać od 30 minut do 6 godzin.   

## <a name="using-the-solution"></a>Użycie rozwiązania
Po dodaniu do obszaru roboczego OMS rozwiązania do zarządzania aktualizacjami na pulpicie nawigacyjnym usługi OMS pojawi się kafelek **Zarządzanie aktualizacjami**. Ten kafelek zawiera liczbę oraz graficzną reprezentację liczby komputerów w środowisku wraz z informacjami o ich zgodności aktualizacji.<br><br>
![Kafelek podsumowujący zarządzanie aktualizacjami](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Wyświetlanie ocen aktualizacji
Kliknij kafelek **Zarządzanie aktualizacjami**, aby otworzyć pulpit nawigacyjny **Zarządzanie aktualizacjami**.<br><br> ![Pulpit nawigacyjny podsumowujący zarządzanie aktualizacjami](./media/oms-solution-update-management/update-management-dashboard.png)<br> 

Ten pulpit nawigacyjny zawiera szczegółowy podział stanu aktualizacji według typu systemu operacyjnego i klasyfikacji aktualizacji — krytyczna, bezpieczeństwa lub inna (taka jak aktualizacja definicji). Wybranie kafelka **Wdrożenia aktualizacji** powoduje przekierowanie na stronę Wdrożenia aktualizacji, na której można obejrzeć harmonogramy, aktualnie uruchomione wdrożenia i ukończone wdrożenia albo zaplanować nowe wdrożenie.  

Klikając konkretny kafelek, możesz uruchomić przeszukiwanie dziennika, które zwróci wszystkie rekordy. Możesz też uruchomić zapytanie o określoną kategorię z wcześniej zdefiniowanymi kryteriami, wybierając jedno z zapytań z listy dostępnej w kolumnie **Typowe zapytania dotyczące aktualizacji**.    

## <a name="installing-updates"></a>Instalowanie aktualizacji
Gdy aktualizacje zostaną ocenione dla wszystkich komputerów z systemami Windows i Linux w obszarze roboczym, możesz utworzyć *wdrożenie aktualizacji* w celu zainstalowania wymaganych aktualizacji.  Wdrożenie aktualizacji to zaplanowana instalacja wymaganych aktualizacji na co najmniej jednym komputerze.  Należy określić datę i godzinę wdrożenia, a także komputer lub grupę komputerów, które mają zostać uwzględnione w zakresie wdrożenia.  Aby dowiedzieć się więcej na temat grup komputerów, zobacz [Computer groups in Log Analytics](../log-analytics/log-analytics-computer-groups.md) (Grupy komputerów w usłudze Log Analytics).  Gdy uwzględnisz grupy komputerów we wdrożeniu aktualizacji, członkostwo w grupie jest oceniane tylko raz, w chwili tworzenia harmonogramu.  Późniejsze zmiany w grupie nie są odzwierciedlane.  Aby obejść ten problem, usuń zaplanowane wdrożenie aktualizacji, a następnie utwórz je ponownie. 

> [!NOTE]
> Maszyny wirtualne z systemem Windows wdrożone z witryny Azure Marketplace są domyślnie ustawione do automatycznego odbierania aktualizacji z usługi Windows Update.  To zachowanie nie zmienia się po dodaniu tego rozwiązania lub maszyny wirtualnej z systemem Windows do obszaru roboczego.  Jeśli nie będziesz aktywnie zarządzać aktualizacjami za pomocą tego rozwiązania, będzie miało zastosowanie zachowanie domyślne (czyli automatyczne stosowanie aktualizacji).  

W przypadku maszyn wirtualnych utworzonych na podstawie z obrazów systemu Red Hat Enterprise Linux (RHEL) na żądanie dostępnych w witrynie Azure Marketplace są one rejestrowane w celu uzyskiwania dostępu do [infrastruktury aktualizacji systemu Red Hat (RHUI)](../virtual-machines/linux/update-infrastructure-redhat.md) wdrożonej na platformie Azure.  Inne dystrybucje systemu Linux należy aktualizować przy użyciu repozytorium plików online dystrybucji i odpowiadających im metod.  

### <a name="viewing-update-deployments"></a>Wyświetlanie wdrożeń aktualizacji
Kliknij kafelek **Wdrożenia aktualizacji**, aby wyświetlić listę istniejących wdrożeń aktualizacji.  Są one pogrupowane według stanu — **Zaplanowano**, **Uruchomiono** i **Ukończono**.<br><br> ![Strona harmonogramu wdrożenia aktualizacji](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Właściwości wyświetlane w każdym wdrożeniu aktualizacji są opisane w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Nazwa |Nazwa wdrożenia aktualizacji. |
| Harmonogram |Typ harmonogramu.  Dostępne opcje to *Jednorazowo*, *Co tydzień* i *Co miesiąc*. |
| Godzina rozpoczęcia |Data i godzina zaplanowanego uruchomienia wdrożenia aktualizacji. |
| Czas trwania |Liczba minut, przez jaką może działać wdrożenie aktualizacji.  Jeśli wszystkie aktualizacje nie zostaną zainstalowane przed upływem tego czasu, pozostałe aktualizacje zostaną odroczone do następnego wdrażania aktualizacji. |
| Serwery |Liczba komputerów, których dotyczy wdrożenie aktualizacji.  |
| Stan |Stan wdrożenia aktualizacji.<br><br>Możliwe wartości:<br>- Nie uruchomiono<br>- Uruchomiono<br>- Ukończono |

Wybierz ukończone wdrożenie aktualizacji, aby wyświetlić ekran szczegółów, który zawiera kolumny podane w tabeli poniżej.  Kolumny te będą niewypełnione, jeśli wdrożenie aktualizacji nie zostało jeszcze uruchomione.<br><br> ![Przegląd wyników wdrożenia aktualizacji](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Kolumna | Opis |
| --- | --- |
| **Widok komputerów** | |
| Komputery z systemem Windows |Wyświetla liczbę komputerów z systemem Windows objętych wdrażaniem aktualizacji według ich stanu.  Kliknij stan, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie rekordy aktualizacji o tym stanie wdrażania aktualizacji. |
| Komputery z systemem Linux |Wyświetla liczbę komputerów z systemem Linux objętych wdrażaniem aktualizacji według ich stanu.  Kliknij stan, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie rekordy aktualizacji o tym stanie wdrażania aktualizacji. |
| Stan instalacji komputera |Wyświetla listę komputerów objętych wdrożeniem aktualizacji i procent pomyślnie zainstalowanych aktualizacji. Kliknij jeden z wpisów, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie brakujące aktualizacje krytyczne. |
| **Widok aktualizacji** | |
| Aktualizacje systemu Windows |Wyświetla listę aktualizacji systemu Windows uwzględnionych we wdrożeniu aktualizacji i stan instalacji każdej z nich.  Wybierz aktualizację, aby uruchomić przeszukiwanie dziennika zwracające wszystkie rekordy aktualizacji dla tej konkretnej aktualizacji, albo kliknij stan, aby uruchomić przeszukiwanie dziennika zwracające wszystkie rekordy aktualizacji dla wdrożenia. | 
| Aktualizacje systemu Linux |Wyświetla listę aktualizacji systemu Linux uwzględnionych we wdrożeniu aktualizacji i stan instalacji każdej z nich.  Wybierz aktualizację, aby uruchomić przeszukiwanie dziennika zwracające wszystkie rekordy aktualizacji dla tej konkretnej aktualizacji, albo kliknij stan, aby uruchomić przeszukiwanie dziennika zwracające wszystkie rekordy aktualizacji dla wdrożenia. | 

### <a name="creating-an-update-deployment"></a>Tworzenie wdrożenia aktualizacji
Aby utworzyć nowe wdrożenie aktualizacji, kliknij przycisk **Dodaj** na górze ekranu. Zostanie otwarta strona **Nowe wdrożenie aktualizacji**.  Należy podać wartości właściwości zawartych w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Nazwa |Unikatowa nazwa identyfikującą wdrożenie aktualizacji. |
| Strefa czasowa |Strefa czasowa służąca do określenia godziny rozpoczęcia. |
| Typ harmonogramu | Typ harmonogramu.  Dostępne opcje to *Jednorazowo*, *Co tydzień* i *Co miesiąc*.  
| Godzina rozpoczęcia |Data i godzina rozpoczęcia wdrażania aktualizacji. **Uwaga:** Jeśli konieczne jest natychmiastowe wdrożenie, można je uruchomić najszybciej po 30 minutach od bieżącej godziny. |
| Czas trwania |Liczba minut, przez jaką może działać wdrożenie aktualizacji.  Jeśli wszystkie aktualizacje nie zostaną zainstalowane przed upływem tego czasu, pozostałe aktualizacje zostaną odroczone do następnego wdrażania aktualizacji. |
| Komputery |Nazwy komputerów lub grup komputerów do uwzględnienia i wybrania jako cel podczas wdrażania aktualizacji.  Wybierz co najmniej jeden wpis z listy rozwijanej. |

<br><br> ![Strona nowego wdrożenia aktualizacji](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Przedział czasu
Domyślnie zakres danych przeanalizowanych w rozwiązaniu do zarządzania aktualizacjami obejmuje wszystkie połączone grupy zarządzania wygenerowane w ciągu ostatniego dnia. 

Aby zmienić okres danych, wybierz pozycję **Dane oparte na** u góry pulpitu nawigacyjnego. Możesz wybrać rekordy utworzone lub zaktualizowane w ciągu ostatnich 7 dni, 1 dnia lub 6 godzin. Możesz też wybrać opcję **Niestandardowy** i określić niestandardowy zakres dat.

## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics
Rozwiązanie do zarządzania aktualizacjami tworzy dwa typy rekordów w repozytorium OMS.

### <a name="update-records"></a>Rekordy Update (Aktualizacja)
Rekord o typie **Update** (Aktualizacja) jest tworzony dla każdej aktualizacji, która jest zainstalowana lub wymagana na poszczególnych komputerach. Rekordy Update (Aktualizacja) mają właściwości podane w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Typ |*Aktualizacja* |
| SourceSystem |Źródło zatwierdzonych instalacji aktualizacji.<br>Możliwe wartości:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Linux Servers (serwery systemu Linux pobrane z menedżerów pakietów) |
| Approved (Zatwierdzono) |Określa, czy aktualizacja została zatwierdzona do instalacji.<br> W przypadku serwerów z systemem Linux jest to obecnie opcjonalne, ponieważ poprawki nie są zarządzane przez usługę OMS. |
| Klasyfikacja dla systemu Windows |Klasyfikacja aktualizacji.<br>Możliwe wartości:<br>- Applications (Aplikacje)<br>- Critical Updates (Aktualizacje krytyczne)<br>- Definition Updates (Aktualizacje definicji)<br>- Feature Packs (Pakiety funkcji)<br>- Security Updates (Aktualizacje zabezpieczeń)<br>- Service Packs (Dodatki Service Pack)<br>- Update Rollups (Pakiety zbiorcze aktualizacji)<br>- Updates (Aktualizacje) |
| Klasyfikacja dla systemu Linux |Klasyfikacja aktualizacji.<br>Możliwe wartości:<br>- Critical Updates (Aktualizacje krytyczne)<br>- Security Updates (Aktualizacje zabezpieczeń)<br>- Other Updates (Inne aktualizacje) |
| Computer (Komputer) |Nazwa komputera. |
| InstallTimeAvailable |Określa, czy inni agenci, którzy zainstalowali tę samą aktualizację, mają dostęp do czasu instalacji. |
| InstallTimePredictionSeconds |Czas instalacji w sekundach, szacowany na podstawie innych agentów, którzy zainstalowali tę samą aktualizację. |
| KBID |Identyfikator artykułu w bazie wiedzy (KB), który opisuje tę aktualizację. |
| ManagementGroupName |Nazwa grupy zarządzania agentów SCOM.  W przypadku innych agentów jest to AOI-<workspace ID>. |
| MSRCBulletinID |Identyfikator biuletynu zabezpieczeń firmy Microsoft zawierającego opis aktualizacji. |
| MSRCSeverity |Ważność biuletynu zabezpieczeń firmy Microsoft.<br>Możliwe wartości:<br>- Critical (Krytyczny)<br>- Important (Ważny)<br>- Moderate (Średni) |
| Optional (Opcjonalność) |Określa, czy aktualizacja jest opcjonalna. |
| Product (Produkt) |Nazwa produktu, którego dotyczy aktualizacja.  Kliknij przycisk **Wyświetl**, aby otworzyć artykuł w przeglądarce. |
| PackageSeverity |Ważności luki w zabezpieczeniach naprawionej w ramach tej aktualizacji, określona przez dostawców dystrybucji systemu Linux. |
| PublishDate |Data i godzina zainstalowania aktualizacji. |
| RebootBehavior |Określa, czy aktualizacja wymusza ponowne uruchomienie komputera.<br>Możliwe wartości:<br>- canrequestreboot (może wymagać ponownego uruchomienia)<br>- neverreboots (nigdy nie uruchamia ponownie) |
| RevisionNumber |Numer poprawki aktualizacji. |
| SourceComputerId |Identyfikator GUID służący do unikatowej identyfikacji komputera. |
| TimeGenerated |Data i godzina ostatniej aktualizacji rekordu. |
| Tytuł |Tytuł aktualizacji. |
| UpdateID |Identyfikator GUID służący do unikatowego identyfikowania aktualizacji. |
| UpdateState |Określa, czy aktualizacja jest zainstalowana na tym komputerze.<br>Możliwe wartości:<br>- Installed — aktualizacja jest zainstalowana na tym komputerze.<br>- Needed — aktualizacja nie jest zainstalowana i jest wymagana na tym komputerze. |

Jeśli wykonujesz dowolne wyszukiwanie w dzienniku, które zwraca rekordy w typu **Update** (Aktualizacja), możesz wybrać widok **Aktualizacje**, który wyświetla zestaw kafelków z podsumowaniem aktualizacji zwracanych przez wyszukiwanie. Możesz kliknąć pozycje na kafelkach **Aktualizacje brakujące i zainstalowane** i **Aktualizacje wymagane i opcjonalne**, aby ograniczyć zakres widoku do tego zestawu aktualizacji. Wybierz widok **Lista** lub **Tabela**, aby przywrócić widok poszczególnych rekordów.<br> 

![Widok aktualizacji wyszukiwania w dzienniku z aktualizacją typu rekord](./media/oms-solution-update-management/update-la-view-updates.png)  

W widoku **Tabela** możesz kliknąć identyfikator **KBID** dowolnego rekordu, aby otworzyć przeglądarkę z artykułem bazy wiedzy (KB). Dzięki temu można szybko zapoznać się ze szczegółami danej aktualizacji.<br> 

![Widok tabeli wyszukiwania w dzienniku z kafelkami aktualizacji typu rekord](./media/oms-solution-update-management/update-la-view-table.png)

W widoku **Lista** kliknij link **Wyświetl** obok identyfikatora KBID, aby otworzyć artykuł bazy wiedzy (KB).<br>

![Widok listy wyszukiwania w dzienniki z kafelkami aktualizacji typu rekord](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>Rekordy UpdateSummary
Rekord **UpdateSummary** jest tworzony dla każdego komputera agenta systemu Windows. Jest on aktualizowany przy każdym skanowaniu komputera w poszukiwaniu aktualizacji. Rekordy **UpdateSummary** mają właściwości podane w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Typ |UpdateSummary |
| SourceSystem |OpsManager |
| Computer (Komputer) |Nazwa komputera. |
| CriticalUpdatesMissing |Liczba aktualizacji krytycznych, których brakuje na komputerze. |
| ManagementGroupName |Nazwa grupy zarządzania agentów SCOM. W przypadku innych agentów jest to AOI-<workspace ID>. |
| NETRuntimeVersion |Wersja środowiska uruchomieniowego .NET zainstalowanego na komputerze. |
| OldestMissingSecurityUpdateBucket |Przedział służący do kategoryzowania czasu od opublikowania najstarszej aktualizacji zabezpieczeń, której brakuje na tym komputerze.<br>Możliwe wartości:<br>- OIder (Starsze)<br>- 180 days ago (180 dni temu)<br>- 150 days ago (150 dni temu)<br>- 120 days ago (120 dni temu)<br>- 90 days ago (90 dni temu)<br>- 60 days ago (60 dni temu)<br>- 30 days ago (30 dni temu)<br>- Recent (Najnowsze) |
| OldestMissingSecurityUpdateInDays |Liczba dni od czasu opublikowania najstarszej aktualizacji zabezpieczeń, której brakuje na tym komputerze. |
| OsVersion |Wersja systemu operacyjnego zainstalowanego na komputerze. |
| OtherUpdatesMissing |Liczba innych aktualizacji, których brakuje na komputerze. |
| SecurityUpdatesMissing |Liczba aktualizacji zabezpieczeń, których brakuje na komputerze. |
| SourceComputerId |Identyfikator GUID służący do unikatowej identyfikacji komputera. |
| TimeGenerated |Data i godzina ostatniej aktualizacji rekordu. |
| TotalUpdatesMissing |Liczba wszystkich aktualizacji, których brakuje na komputerze. |
| WindowsUpdateAgentVersion |Numer wersji agenta usługi Windows Update na komputerze. |
| WindowsUpdateSetting |Ustawienie sposobu instalowania ważnych aktualizacji na komputerze.<br>Możliwe wartości:<br>- Disabled (Wyłączono)<br>- Notify before installation (Powiadom przed rozpoczęciem instalacji)<br>- Scheduled installation (Instalacja zaplanowana) |
| WSUSServer |Adres URL serwera WSUS, jeśli komputer jest skonfigurowany do korzystania z tego serwera. |

## <a name="sample-log-searches"></a>Przykładowe wyszukiwania dzienników
Poniższa tabela zawiera przykładowe wyszukiwania w dzienniku dotyczące rekordów aktualizacji zbieranych przez to rozwiązanie. 

| Zapytanie | Opis |
| --- | --- |
|Serwery z systemem Windows wymagające aktualizacji |`Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false | measure count() by Computer` |
|Serwery z systemem Linux wymagające aktualizacji | `Type:Update OSType=Linux UpdateState!="Not needed" | measure count() by Computer` |
| Wszystkie komputery z brakującymi aktualizacjami |`Type=Update UpdateState=Needed Optional=false | select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Brakujące aktualizacje dla konkretnego komputera (zastąp wartość własną nazwą komputera) |`Type=Update UpdateState=Needed Optional=false Computer="KOMPUTER01.contoso.com" | select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate` |
| Wszystkie komputery z brakującymi aktualizacjami krytycznymi i zabezpieczeń |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates"`) |
| Aktualizacje krytyczne lub zabezpieczeń wymagane przez komputery, na których aktualizacje są stosowane ręcznie |`Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | Distinct Computer} | Distinct KBID` |
| Zdarzenia błędu dotyczące komputerów, na których brakuje wymaganych aktualizacji krytycznych lub zabezpieczeń |`Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false | Distinct Computer}` |
| Wszystkie komputery z brakującymi pakietami zbiorczymi aktualizacji |`Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed| select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate` |
| Różne brakujące aktualizacje na wszystkich komputerach |`Type=Update UpdateState=Needed Optional=false | Distinct Title` |
| Serwer z systemem Windows z aktualizacjami, które nie powiodły się podczas przebiegu aktualizacji | `Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Title, UpdateRunName` |
| Serwer z systemem Linux z aktualizacjami, które nie powiodły się podczas przebiegu aktualizacji |`Type:UpdateRunProgress InstallationStatus=failed | measure count() by Computer, Product, UpdateRunName` |
| Korzystanie przez komputer z usług WSUS |`Type=UpdateSummary | measure count() by WSUSServer` |
| Konfiguracja automatycznej aktualizacji |`Type=UpdateSummary | measure count() by WindowsUpdateSetting` |
| Komputery z wyłączoną automatyczną aktualizacją |`Type=UpdateSummary WindowsUpdateSetting=Manual` |
| Lista wszystkich komputerów z systemem Linux, które mają dostępne aktualizacje pakietu |`Type=Update and OSType=Linux and UpdateState!="Not needed" | measure count() by Computer` |
| Lista wszystkich komputerów z systemem Linux, dla których jest dostępna aktualizacja pakietu o znaczeniu krytycznym lub dotycząca luki w zabezpieczeniach |`Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") | measure count() by Computer` |
| Lista wszystkich pakietów, które mają dostępną aktualizację |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Lista wszystkich pakietów, dla których jest dostępna aktualizacja o znaczeniu krytycznym lub dotycząca luki w zabezpieczeniach |`Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates")` |
| Lista wdrożeń aktualizacji, które zmodyfikowały komputery |`Type:UpdateRunProgress | measure Count() by UpdateRunName` |
|Komputery, które zostały zaktualizowane w ramach tego przebiegu aktualizacji (zastąp wartość nazwą własnego wdrożenia aktualizacji) |`Type:UpdateRunProgress UpdateRunName="Nazwa_wdrożenia" | measure Count() by Computer` |
| Listę wszystkich komputerów z systemem Ubuntu, dla których są dostępne dowolne aktualizacje |`Type=Update and OSType=Linux and OSName = Ubuntu &| measure count() by Computer` |

## <a name="troubleshooting"></a>Rozwiązywanie problemów 

Ta sekcja zawiera informacje ułatwiające rozwiązywanie problemów z rozwiązaniem Zarządzanie aktualizacjami.  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Jak rozwiązywać problemy z wdrożeniami aktualizacji?
Wyniki elementu runbook odpowiedzialnego za wdrożenie aktualizacji zawartych w zaplanowanym wdrożeniu aktualizacji możesz obejrzeć w bloku Zadania swojego konta usługi Automation, które jest połączone z obszarem roboczym usługi OMS obsługującym to rozwiązanie.  Element runbook **Patch-MicrosoftOMSComputer** to podrzędny element runbook, który jest nakierowany na konkretny komputer zarządzany. Przejrzenie pełnych informacji ze strumienia pozwala uzyskać szczegółowe informacje o tym wdrożeniu.  W danych wyjściowych będzie widać, które wymagane aktualizacje mają zastosowanie, jaki jest ich stan pobierania i instalacji, a także inne informacje.<br><br> ![Stan zadania wdrożenia aktualizacji](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Aby uzyskać więcej informacji, zobacz [Automation runbook output and messages](../automation/automation-runbook-output-and-messages.md) (Dane wyjściowe i komunikaty elementu runbook usługi Automation).   
  
## <a name="next-steps"></a>Następne kroki
* Korzystanie z wyszukiwania w dzienniku usługi [Log Analytics](../log-analytics/log-analytics-log-searches.md) w celu wyświetlania szczegółowych danych aktualizacji.
* [Tworzenie własnych pulpitów nawigacyjnych](../log-analytics/log-analytics-dashboards.md) przedstawiających zgodność aktualizacji na zarządzanych komputerach.
* [Tworzenie alertów](../log-analytics/log-analytics-alerts.md) po wykryciu braku aktualizacji krytycznych na komputerach lub komputera z wyłączonymi aktualizacjami automatycznymi.  


