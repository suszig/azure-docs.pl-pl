---
title: "Aktualizacja rozwiązania do zarządzania na platformie Azure"
description: "Ten artykuł pomaga zrozumieć, jak używać tego rozwiązania do zarządzania aktualizacjami komputerów z systemami Windows i Linux."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 202c75366477ae3445f607f75d08faf0335de79f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="update-management-solution-in-azure"></a>Aktualizacja rozwiązania do zarządzania na platformie Azure

Rozwiązania do zarządzania aktualizacji w usłudze Automatyzacja Azure umożliwia zarządzanie aktualizacjami zabezpieczeń systemu operacyjnego dla komputerów z systemem Windows i Linux wdrożonych w Azure, środowiskach lokalnych lub innych dostawców chmury. Umożliwia ono szybką ocenę stanu dostępnych aktualizacji na wszystkich komputerach agentów oraz zarządzanie procesem instalacji wymaganych aktualizacji serwerów.

Możesz włączyć zarządzanie aktualizacjami dla maszyn wirtualnych bezpośrednio z poziomu konta usługi [Azure Automation](automation-offering-get-started.md).
Aby dowiedzieć się, jak włączyć zarządzanie aktualizacjami dla maszyn wirtualnych z poziomu konta usługi Automation, zobacz [Zarządzanie aktualizacjami dla wielu maszyn wirtualnych](manage-update-multi.md).

## <a name="solution-overview"></a>Omówienie rozwiązania

Komputery zarządzane przy użyciu zarządzania aktualizacji następujące konfiguracje wykonywania ocena i aktualizacja wdrożenia:

* Program Microsoft Monitoring agent dla systemu Windows lub Linux
* Platforma PowerShell Desired State Configuration (DSC) dla systemu Linux
* Hybrydowy proces roboczy elementu runbook usługi Automation
* Usługa Microsoft Update lub Windows Server Update Services dla komputerów z systemem Windows

Na poniższym diagramie przedstawiono koncepcję zachowanie, a przepływ danych jak rozwiązania ocenia i dotyczy wszystkich aktualizacji systemu Windows Server i komputery z systemem Linux w obszarze roboczym.    

![Aktualizowanie przepływu procesów zarządzania](media/automation-update-management/update-mgmt-updateworkflow.png)

Po komputerze wykonuje skanowanie zgodności aktualizacji, agent przekazuje informacje zbiorcze z analizą dzienników. Na komputerze z systemem Windows skanowanie pod kątem zgodności jest domyślnie przeprowadzane co 12 godzin. Oprócz harmonogram skanowania w poszukiwaniu skanowanie pod kątem zgodności aktualizacji jest inicjowana w ciągu 15 minut po uruchomieniu programu Microsoft Monitoring Agent (MMA), przed instalacją aktualizacji i po zainstalowaniu aktualizacji. Na komputerze z systemem Linux skanowanie pod kątem zgodności jest domyślnie przeprowadzane co 3 godziny, a także inicjowane w ciągu 15 minut po ponownym uruchomieniu agenta programu MMA.

Rozwiązanie informuje, jak aktualne jest oprogramowanie na komputerze, bazując na źródle skonfigurowanym na potrzeby synchronizacji. Jeśli komputer z systemem Windows jest skonfigurowany do raportowania do usługi WSUS, to w zależności od tego, kiedy usługa WSUS była ostatnio synchronizowana z usługą Microsoft Update, wyniki mogą się różnić od tych pokazywanych przez usługę Microsoft Updates. To samo dotyczy komputerów z systemem Linux skonfigurowanych do raportowania do repozytorium lokalnego i repozytorium publicznego.

Aktualizacje oprogramowania można wdrożyć i zainstalować na komputerach, które ich wymagają, tworząc zaplanowane wdrożenie. Aktualizacje sklasyfikowane jako *Opcjonalne* są poza zakresem wdrożenia dla komputerów z systemem Windows. W zakresie tym są tylko aktualizacje wymagane. Zaplanowane wdrożenie definiuje komputerów docelowych odbierania odpowiednich aktualizacji przez jawne określenie komputera, lub wybranie [grupy komputerów](../log-analytics/log-analytics-computer-groups.md) opartego wylogowuje wyszukiwania dziennika w określonej grupie komputerów. Można również określić harmonogram zatwierdzania i wyznaczyć okres, w którym można instalować aktualizacje. Aktualizacje są instalowane przez elementy runbook w usłudze Azure Automation. Nie można wyświetlić tych elementów runbook i nie wymagają one żadnej konfiguracji. Utworzenie wdrożenia aktualizacji powoduje utworzenie harmonogramu, który uruchamia główny element runbook aktualizacji w określonym czasie na uwzględnionych komputerach. Ten główny element runbook uruchamia podrzędny element runbook na każdym agencie, który przeprowadza instalację wymaganych aktualizacji.

W dniu i o godzinie określonych we wdrożeniu aktualizacji komputery docelowe wykonują równolegle wdrożenie. Najpierw jest wykonywane skanowanie, aby sprawdzić, czy aktualizacje są ciągle wymagane. Dopiero wtedy są one instalowane. Dla komputerów klienckich WSUS Jeśli aktualizacje nie zostały zatwierdzone w programie WSUS, wdrożenie aktualizacji kończy się niepowodzeniem.

## <a name="clients"></a>Klienci

### <a name="supported-client-types"></a>Typy obsługiwanych klientów

W poniższej tabeli przedstawiono listę obsługiwanych systemów operacyjnych: 

|System operacyjny  |Uwagi  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Obsługuje tylko zaktualizować ocen         |
|Windows Server 2008 R2 z dodatkiem SP1 lub nowszy     |.NET framework 4.5 i WMF 5.0 lub nowszej, które są potrzebne dla systemu Windows Server 2008 R2 z dodatkiem SP1        |
|CentOS 6 (x86/x64) i 7 (x64)      | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|Red Hat Enterprise 6 (x86/x64) i 7 (x64)     | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|SUSE Linux Enterprise Server 11 (x86/x64) i 12 (x64)     | Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.        |
|Ubuntu 12.04 LTS i nowsze x86/x64       |Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.         |

### <a name="unsupported-client-types"></a>Typy klientów nieobsługiwana

W poniższej tabeli wymieniono systemy operacyjne, które nie są obsługiwane:

|System operacyjny  |Uwagi  |
|---------|---------|
|Klient systemu Windows     | Klienckie systemy operacyjne (Windows 7, Windows 10, itp.) nie są obsługiwane.        |
|Windows Server 2016 Nano Server     | Nieobsługiwane       |

### <a name="client-requirements"></a>Wymagania dotyczące klienta

#### <a name="windows"></a>Windows

Agentów systemu Windows musi być skonfigurowana do komunikowania się z serwerem z systemem Windows Server Update Services (WSUS) lub mają dostęp do usługi Microsoft Update. Agent systemu Windows nie może być także zarządzane jednocześnie przez program System Center Configuration Manager. [Agenta Windows](../log-analytics/log-analytics-agent-windows.md) jest wymagana. Ten agent jest instalowany automatycznie w przypadku przechodzenia do maszyny Wirtualnej platformy Azure.

#### <a name="linux"></a>Linux

Dla systemu Linux komputer musi mieć dostęp do repozytorium aktualizacji, który może być prywatny lub publiczny. Agent pakietu OMS Linux skonfigurowany pod kątem raportowania do wielu obszarów roboczych Log Analytics nie jest obsługiwany w tym rozwiązaniu.

Aby uzyskać więcej informacji na temat instalowania agenta pakietu OMS dla systemu Linux i pobrać najnowszą wersję, zobacz [Operations Management Suite agenta dla systemu Linux](https://github.com/microsoft/oms-agent-for-linux). Aby uzyskać informacje na temat sposobu instalowania agenta usługi OMS dla systemu Windows, przejrzyj temat [Operations Management Suite Agent for Windows](../log-analytics/log-analytics-windows-agent.md) (Agent usługi Operations Management Suite dla systemu Windows).  

## <a name="permissions"></a>Uprawnienia
Aby utworzyć i zarządzać wdrożeniami aktualizacji, musisz mieć uprawnienia określone. Aby dowiedzieć się więcej na temat tych uprawnień można znaleźć [dostępu na podstawie roli — zarządzanie aktualizacjami](automation-role-based-access-control.md#update-management) 

## <a name="solution-components"></a>Składniki rozwiązania
To rozwiązanie składa się z następujących zasobów, które są dodawane do Twojego konta usługi Automation, i bezpośrednio połączonych agentów lub grupy zarządzania połączonej z programem Operations Manager.

### <a name="hybrid-worker-groups"></a>Grupy hybrydowych procesów roboczych
Po włączeniu tego rozwiązania dowolnego komputera z systemem Windows, podłączonych bezpośrednio do obszaru roboczego analizy dzienników jest automatycznie konfigurowany jako hybrydowy proces roboczy elementu Runbook do obsługi elementy runbook zawarte w tym rozwiązaniu. Dla każdego komputera z systemem Windows zarządzanych przez rozwiązanie znajduje się w obszarze strony grupy hybrydowego procesu roboczego w systemie grupy hybrydowych procesów roboczych dla konta automatyzacji następującej konwencji nazewnictwa *Hostname FQDN_GUID*. Te grupy z elementami runbook nie może być przeznaczony na koncie, w przeciwnym razie się one zakończyć niepowodzeniem. Te grupy są przeznaczone wyłącznie do obsługi rozwiązania do zarządzania.

Możesz jednak dodać komputery z systemem Windows do grupy hybrydowych procesów roboczych elementów runbook na Twoim koncie usługi Automation w celu obsługi elementów runbook usługi Automation, o ile używasz tego samego konta zarówno dla tego rozwiązania, jak i dla członkostwa w grupie hybrydowych procesów roboczych elementów runbook. Ta funkcjonalność została dodana do wersji 7.2.12024.0 hybrydowego procesu roboczego elementu Runbook.

### <a name="management-packs"></a>Pakiety administracyjne

Jeśli grupa zarządzania programu System Center Operations Manager jest podłączony do obszaru roboczego analizy dzienników, następujące pakiety administracyjne są zainstalowane w programie Operations Manager. Te pakiety administracyjne są również instalowane na bezpośrednio połączonych komputerach z systemem Windows po dodaniu tego rozwiązania. W przypadku tych pakietów administracyjnych nie trzeba niczego konfigurować ani niczym zarządzać.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pakiet administracyjny wdrożenia aktualizacji

Aby uzyskać więcej informacji na temat aktualizowania pakietów administracyjnych rozwiązania, zobacz artykuł [Connect Operations Manager to Log Analytics](../log-analytics/log-analytics-om-agents.md) (Połączenie programu Operations Manager z usługą Log Analytics).

### <a name="confirm-non-azure-machines-are-onboarded"></a>Upewnij się, że komputery z systemem innym niż Azure są został załadowany

Aby potwierdzić bezpośrednio połączone maszyny komunikują się z analizy dzienników po kilka minut, można wykonać następujące wyszukiwania dziennika:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table`
```

Na komputerze z systemem Windows możesz przejrzeć następujące polecenie, aby sprawdzić połączenie agenta z analizy dzienników:

1.  Otwórz program Microsoft Monitoring Agent w Panelu sterowania. Na karcie **Azure Log Analytics (OMS)** agent wyświetla komunikat z następującą informacją: **Program Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite**.   
2.  Otwórz Dziennik zdarzeń systemu Windows, przejdź do pozycji **Dzienniki aplikacji i usług\Operacje** i wyszukaj identyfikatory zdarzeń 3000 i 5002 ze źródła Service Connector. Te zdarzenia wskazują komputer został zarejestrowany za pomocą obszaru roboczego analizy dzienników i odbiera konfiguracji.  

Jeśli agent nie jest w stanie nawiązać połączenia z analizy dzienników i został on skonfigurowany do komunikowania się z Internetem za pośrednictwem zapory lub serwera proxy, upewnij się, serwer zapory lub serwera proxy jest poprawnie skonfigurowany, przeglądając [konfiguracji sieci Windows agent](../log-analytics/log-analytics-agent-windows.md) lub [konfigurację sieci dla agenta systemu Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Jeśli systemy Linux są skonfigurowane do komunikacji z serwera proxy lub bramy OMS i dołączania tego rozwiązania, aktualizacja *proxy.conf* uprawnień, aby udzielić grupie omiuser uprawnienie do odczytu w pliku, wykonując następujące polecenia:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Po przeprowadzeniu oceny nowo dodani agenci systemu Linux będą mieć stan **Zaktualizowane**. Ten proces może potrwać do 6 godzin.

Aby potwierdzić, grupy zarządzania programu Operations Manager komunikuje się z analizy dzienników, zobacz [zweryfikować integracji programu Operations Manager z usługą OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Zbieranie danych

### <a name="supported-agents"></a>Obsługiwani agenci
W poniższej tabeli opisano połączone źródła, które obsługuje to rozwiązanie.

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows |Yes |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Windows i inicjuje instalowanie wymaganych aktualizacji. |
| Agenci dla systemu Linux |Yes |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Linux i inicjuje instalowanie wymaganych aktualizacji w obsługiwanych dystrybucjach. |
| Grupa zarządzania programu Operations Manager |Yes |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów w połączonej grupie zarządzania.<br>Bezpośrednie połączenie agenta programu Operations Manager z usługą Log Analytics nie jest wymagane. Dane są przesyłane dalej z grupy zarządzania do obszaru roboczego analizy dzienników. |

### <a name="collection-frequency"></a>Częstotliwość zbierania
Skanowanie każdego zarządzanego komputera z systemem Windows odbywa się dwa razy dziennie. Co 15 minut wywoływany jest interfejs API systemu Windows, aby wykonać zapytanie o czas ostatniej aktualizacji w celu sprawdzenia, czy stan się zmienił, a jeśli tak, inicjowane jest skanowanie pod kątem zgodności. Skanowanie każdego zarządzanego komputera z systemem Linux odbywa się co 3 godziny.

Wyświetlenie zaktualizowanych danych z zarządzanych komputerów na pulpicie nawigacyjnym może potrwać od 30 minut do 6 godzin.   

## <a name="viewing-update-assessments"></a>Wyświetlanie ocen aktualizacji
Polecenie **zarządzania aktualizacjami** na Twoje konto usługi Automatyzacja, aby wyświetlić stan maszyny.

Ten widok zawiera informacje na maszynach, brakujące aktualizacje, wdrożenia aktualizacji oraz zaplanowana instalacja aktualizacji wdrożenia.

Możesz uruchomić wyszukiwanie dziennika, które zwraca informacje o maszyny, aktualizacji lub wdrożenia po wybraniu elementu na liście. Spowoduje to otwarcie **wyszukiwania dziennika** strony z zapytaniem dla wybranego elementu.

## <a name="installing-updates"></a>Instalowanie aktualizacji

Gdy aktualizacje zostaną ocenione dla wszystkich komputerów z systemami Windows i Linux w obszarze roboczym, możesz utworzyć *wdrożenie aktualizacji* w celu zainstalowania wymaganych aktualizacji. Wdrażanie aktualizacji jest zaplanowana instalacja aktualizacji wymaganych dla co najmniej jeden komputer. Należy określić datę i godzinę wdrożenia, a także komputer lub grupę komputerów, które mają zostać uwzględnione w zakresie wdrożenia. Aby dowiedzieć się więcej na temat grup komputerów, zobacz [Computer groups in Log Analytics](../log-analytics/log-analytics-computer-groups.md) (Grupy komputerów w usłudze Log Analytics). Po dołączeniu grup komputerów w danym wdrożeniu aktualizacji członkostwa w grupie jest oceniane tylko raz w momencie tworzenia harmonogramu. Późniejsze zmiany w grupie nie są odzwierciedlane. Aby obejść ten problem, usuń zaplanowane wdrożenie aktualizacji, a następnie utwórz je ponownie.

> [!NOTE]
> Maszyny wirtualne z systemem Windows wdrożone z witryny Azure Marketplace są domyślnie ustawione do automatycznego odbierania aktualizacji z usługi Windows Update. To zachowanie nie zmienia się po dodaniu tego rozwiązania lub maszyny wirtualnej z systemem Windows do obszaru roboczego. Jeśli użytkownik nie aktywnie Zarządzaj aktualizacje dotyczące tego rozwiązania, domyślne zachowanie (automatyczne stosowanie aktualizacji) ma zastosowanie.

Aby w przypadku systemu Ubuntu uniknąć stosowania aktualizacji poza oknem obsługi, zmień konfigurację pakietu Unattended-Upgrade tak, aby wyłączyć automatyczne aktualizacje. Aby uzyskać informacje na temat sposobu konfigurowania tego, zobacz [temat poświęcony aktualizacjom automatycznym w podręczniku systemu Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

W przypadku maszyn wirtualnych utworzonych na podstawie z obrazów systemu Red Hat Enterprise Linux (RHEL) na żądanie dostępnych w witrynie Azure Marketplace są one rejestrowane w celu uzyskiwania dostępu do [infrastruktury aktualizacji systemu Red Hat (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) wdrożonej na platformie Azure. Inne dystrybucje systemu Linux należy aktualizować przy użyciu repozytorium plików online dystrybucji i odpowiadających im metod.  

## <a name="viewing-missing-updates"></a>Wyświetlanie brakujących aktualizacji.

Kliknij przycisk **brakujących aktualizacji** Aby wyświetlić listę aktualizacji, które są dostępne na maszynach. Każda aktualizacja jest na liście i wyświetla informacje liczby maszyn, które wymagają aktualizacji, system operacyjny i łącze, aby uzyskać więcej informacji. Można wybrać poszczególnych aktualizacji i **wyszukiwania dziennika** są wyświetlane i zawiera szczegółowe informacje o aktualizacji.

## <a name="viewing-update-deployments"></a>Wyświetlanie wdrożeń aktualizacji

Kliknij przycisk **wdrożenia aktualizacji** w celu wyświetlenia listy istniejących wdrożeń aktualizacji. Kliknięcie dowolnej z wdrożenia aktualizacji na liście otwiera **wdrożenie aktualizacji uruchomione** strony dla tego wdrożenia aktualizacji.

![Przegląd wyników wdrożenia aktualizacji](./media/automation-update-management/update-deployment-run.png)

## <a name="creating-an-update-deployment"></a>Tworzenie wdrożenia aktualizacji

Utwórz nowe wdrożenie aktualizacji, klikając **harmonogram wdrożenia aktualizacji** przycisk w górnej części ekranu, aby otworzyć **nowe wdrożenie aktualizacji** strony. Należy podać wartości właściwości w poniższej tabeli:

| Właściwość | Opis |
| --- | --- |
| Name (Nazwa) |Unikatowa nazwa identyfikującą wdrożenie aktualizacji. |
|System operacyjny| Linux lub Windows|
| Komputery do zaktualizowania |Wybierz wyszukiwanie, Saved lub wybierz komputer z listy rozwijanej i wybierz poszczególne maszyny |
|Klasyfikacja aktualizacji|Wybierz wszystkie klasyfikacje aktualizacji, które są potrzebne|
|Aktualizacje, które mają zostać wykluczone|Wprowadź wszystkie KB/s do wykluczenia bez prefiksu "KB."|
|Ustawienia harmonogramu|Wybierz czas uruchomienia, a następnie wybierz jednorazowo lub cykliczne cyklu|
| Okno obsługi |Liczba minut dla aktualizacji. Wartość może nie być mniej niż 30 minut, a nie więcej niż 6 godzin |

## <a name="search-logs"></a>Dzienniki wyszukiwania

Oprócz szczegółów, które zostały opublikowane w portalu można przed dzienniki wyszukiwania. Z **śledzenia zmian** otwarty, kliknij **analizy dzienników**, spowoduje to otwarcie **wyszukiwania dziennika** strony

### <a name="sample-queries"></a>Przykładowe zapytania

W poniższej tabeli przedstawiono przykładowy dziennik wyszukuje zebrane przez to rozwiązanie rekordów aktualizacji:

| Zapytanie | Opis |
| --- | --- |
|Aktualizacja<br>&#124;gdzie UpdateState == "Wymagane" i opcjonalne == false<br>&#124;Projekt komputera, Title, KBID, klasyfikacja, PublishedDate |Wszystkie komputery z brakującymi aktualizacjami<br>Dodaj jeden z następujących czynności, aby ograniczyć systemu operacyjnego:<br>OSType = "Windows"<br>OSType == "Linux" |
| Aktualizacja<br>&#124;gdzie UpdateState == "Wymagane" i opcjonalne == false<br>&#124;gdy komputer == "ContosoVM1.contoso.com"<br>&#124;Projekt komputera, Title, KBID, produktu, PublishedDate |Brakujące aktualizacje dla konkretnego komputera (zastąp wartość własną nazwą komputera)|
| Wydarzenie<br>&#124;gdzie EventLevelName == "error" i komputera w ((aktualizacja &#124; where (klasyfikacji == "Aktualizacje zabezpieczeń" lub klasyfikacji == "Aktualizacje krytyczne")<br>&#124;gdzie UpdateState == "Wymagane" i opcjonalne == false <br>&#124;DISTINCT Computer)) |Zdarzenia błędu dotyczące komputerów, na których brakuje wymaganych aktualizacji krytycznych lub zabezpieczeń |
| Aktualizacja<br>&#124;gdzie UpdateState == "Wymagane" i opcjonalne == false<br>&#124;różne tytułu |Różne brakujące aktualizacje na wszystkich komputerach | 
| UpdateRunProgress<br>&#124;gdzie InstallationStatus == "nie powiodło się" <br>&#124; summarize AggregatedValue = count() by Computer, Title, UpdateRunName |Komputery z aktualizacjami, których nie powiodła się w przebiegu aktualizacji<br>Dodaj jeden z następujących czynności, aby ograniczyć systemu operacyjnego:<br>OSType = "Windows"<br>OSType == "Linux" | 
| Aktualizacja<br>&#124;gdzie OSType == "Linux"<br>&#124;gdzie UpdateState! = "Nie jest wymagane" i (klasyfikacji == "Aktualizacje krytyczne" lub klasyfikacji == "Aktualizacje zabezpieczeń")<br>&#124; summarize AggregatedValue = count() by Computer |Listę wszystkich maszyn systemu Linux, które mają dostępnych aktualizacji pakietu, która usterki krytyczne lub zabezpieczeń | 
| UpdateRunProgress<br>&#124;gdzie UpdateRunName == "DeploymentName"<br>&#124; summarize AggregatedValue = count() by Computer|Komputery, które zostały zaktualizowane w ramach tego przebiegu aktualizacji (zastąp wartość nazwą własnego wdrożenia aktualizacji) | 

## <a name="integrate-with-system-center-configuration-manager"></a>Integracja z programem System Center Configuration Manager

Klienci, którzy zainwestowali w program System Center Configuration Manager do zarządzania komputerami, serwerami i urządzeniami przenośnymi polegają również na ich sile i dojrzałości w zarządzania aktualizacjami oprogramowania w ramach cyklu zarządzania aktualizacjami oprogramowania (SUM).

Aby dowiedzieć się, jak zintegrować rozwiązania zarządzania aktualizacjami pakietu OMS z System Center Configuration Manager, zobacz [integracji System Center Configuration Manager z zarządzania aktualizacjami pakietu OMS](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patching-linux-machines"></a>Stosowanie poprawek maszyny z systemem Linux

W poniższych sekcjach opisano potencjalne problemy z stosowanie poprawek systemu Linux.

### <a name="package-exclusion"></a>Pakiet wykluczeń

Na niektóre z nich Linux, takich jak Red Hat Enterprise Linux uaktualnień systemu operacyjnego na poziomie może wystąpić przy użyciu pakietów. Może to prowadzić do zarządzania aktualizacjami uruchamia gdzie zmienia numer wersji systemu operacyjnego. Ponieważ zarządzanie aktualizacji pakietów aktualizacji, jak administrator lokalnie na komputerze z systemem Linux za pomocą tych samych metod, to zachowanie jest zamierzone.

Aby uniknąć wersji systemu operacyjnego za pomocą zarządzania aktualizacjami przebiegów aktualizacji, należy użyć **wykluczeń** funkcji.

W systemie Red Hat Enterprise Linux, nazwę pakietu, które mają zostać wykluczone byłoby: redhat-release-server.x86_64

![Pakiety do wykluczenia dla systemu Linux](./media/automation-update-management/linuxpatches.png)

### <a name="security-patches-not-being-applied"></a>Poprawki zabezpieczeń, które nie są stosowane

Podczas wdrażania aktualizacji do maszyny z systemem Linux, można wybrać klasyfikacje aktualizacji. Filtry to aktualizacje, które są stosowane do tych, które spełniają określone kryteria. Ten filtr jest stosowane lokalnie na komputerze po wdrożeniu aktualizacji. Ponieważ zarządzanie zaktualizować wykonuje wzbogacenia aktualizacji w chmurze, niektóre aktualizacje mogą oflagowane w programie zaktualizować zarządzania jako mający wpływ zabezpieczeń, mimo że komputer lokalny nie ma tych informacji. W związku z tym jeśli aktualizacje krytyczne dotyczą maszyny z systemem Linux, może istnieć aktualizacje, które nie są oznaczone jako mający wpływ zabezpieczeń na tym komputerze i nie zostać zastosowane. Jednak aktualizacja może nadal raport zarządzania ten komputer jako są niezgodne, ponieważ ma on dodatkowe informacje o odpowiednich aktualizacji.

Wdrażanie aktualizacji według klasyfikacji aktualizacji może nie działać na openSUSE Linux z powodu innego modelu stosowania poprawek używane.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Ta sekcja zawiera informacje ułatwiające rozwiązywanie problemów z rozwiązaniem Zarządzanie aktualizacjami.

Jeśli wystąpią problemy podczas próby dołączenia rozwiązania lub maszyny wirtualnej, sprawdź dziennik zdarzeń **Application and Services Log\Operations Manager** pod kątem zdarzeń mających identyfikator zdarzenia 4502 i komunikat zdarzenia zawierający ciąg **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**. W poniższej tabeli wymieniono określone komunikaty o błędach i możliwe rozwiązanie dla każdego z nich.  

| Komunikat | Przyczyna | Rozwiązanie |   
|----------|----------|----------|  
| Nie można zarejestrować maszyny na potrzeby zarządzania poprawkami,<br>rejestracja nie powiodła się z powodu wyjątku<br>System.InvalidOperationException: {"Message": "Maszyna jest już<br>zarejestrowana na innym koncie. "} | Maszyna została już dołączona do innego obszaru roboczego na potrzeby zarządzania aktualizacjami | Przeprowadź czyszczenie starych artefaktów, [usuwając grupę hybrydowych elementów runbook](automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Nie można zarejestrować maszyny na potrzeby zarządzania poprawkami,<br>rejestracja nie powiodła się z powodu wyjątku<br>System.Net.Http.HttpRequestException: Wystąpił błąd podczas wysyłania żądania. ---><br>System.Net.WebException: Połączenie podstawowe<br>zostało zamknięte: Wystąpił nieoczekiwany błąd<br>przy odbiorze. ---> System.ComponentModel.Win32Exception:<br>Klient i serwer nie mogą nawiązać komunikacji,<br>ponieważ nie mają wspólnego algorytmu | Serwer proxy/brama/zapora blokuje komunikację | [Przejrzyj wymagania dotyczące sieci](automation-offering-get-started.md#network-planning)|  
| Nie można zarejestrować maszyny na potrzeby zarządzania poprawkami,<br>rejestracja nie powiodła się z powodu wyjątku<br>Newtonsoft.Json.JsonReaderException: Błąd podczas analizowania wartości nieskończoności dodatniej. | Serwer proxy/brama/zapora blokuje komunikację | [Przejrzyj wymagania dotyczące sieci](automation-offering-get-started.md#network-planning)| 
| Certyfikat przedstawiony przez usługę <wsid>.oms.opinsights.azure.com<br>nie został wystawiony przez urząd certyfikacji<br>używany na potrzeby usług firmy Microsoft. Kontakt<br>administratorem sieci, aby sprawdzić, czy jest używany serwer proxy, który przechwytuje<br>komunikację TLS/SSL. |Serwer proxy/brama/zapora blokuje komunikację | [Przejrzyj wymagania dotyczące sieci](automation-offering-get-started.md#network-planning)|  
| Nie można zarejestrować maszyny na potrzeby zarządzania poprawkami,<br>rejestracja nie powiodła się z powodu wyjątku<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Nie można utworzyć certyfikatu z podpisem własnym. ---><br>System.UnauthorizedAccessException: Odmowa dostępu. | Niepowodzenie generowania certyfikatu z podpisem własnym | Sprawdź, czy konto systemowe ma<br>dostęp do odczytu do folderu:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

## <a name="next-steps"></a>Kolejne kroki

Nadal samouczkiem, aby dowiedzieć się, jak zarządzanie aktualizacjami dla maszyn wirtualnych systemu Windows.

> [!div class="nextstepaction"]
> [Zarządzanie aktualizacje i poprawki dla maszyn wirtualnych systemu Windows Azure](automation-tutorial-troubleshoot-changes.md)

* Korzystanie z wyszukiwania w dzienniku usługi [Log Analytics](../log-analytics/log-analytics-log-searches.md) w celu wyświetlania szczegółowych danych aktualizacji.
* [Tworzenie alertów](../log-analytics/log-analytics-alerts.md) po wykryciu braku aktualizacji krytycznych na komputerach lub komputera z wyłączonymi aktualizacjami automatycznymi.
