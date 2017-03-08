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
ms.date: 02/28/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: fa9b427afff2c12babde30aa354e59d31c8f5b2c
ms.openlocfilehash: 219fe64481df2c5c5cbfe622afdab11dcc1b7100
ms.lasthandoff: 02/28/2017


---
# <a name="update-management-solution-in-oms"></a>Rozwiązanie do zarządzania aktualizacjami w usłudze OMS
Rozwiązanie do zarządzania aktualizacjami w usłudze OMS pozwala na zarządzanie aktualizacjami dla komputerów z systemami Windows i Linux.  Umożliwia ono szybką ocenę stanu dostępnych aktualizacji na wszystkich komputerach z agentami i zainicjowanie procesu instalacji wymaganych aktualizacji serwerów. 

## <a name="prerequisites"></a>Wymagania wstępne
* Rozwiązanie obsługuje tylko przeprowadzanie ocen aktualizacji dla systemu Windows Server 2008 lub nowszych i wdrożeń aktualizacji systemu Windows Server 2012 lub nowszych.  Opcje instalacji Server Core i Nano Server nie są obsługiwane.
* Systemy operacyjne klienta systemu Windows nie są obsługiwane.  
* Agenci dla systemu Windows muszą być skonfigurowani do komunikowania się z serwerem Windows Server Update Services (WSUS) albo mieć dostęp do usługi Microsoft Update.  
  
  > [!NOTE]
  > Agent dla systemu Windows nie może być zarządzany współbieżnie przez program System Center Configuration Manager.  
  > 
  > 
* Agenci dla systemu Linux muszą mieć dostęp do repozytorium aktualizacji.  Agenta usługi OMS dla systemu Linux można pobrać z witryny [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfiguracja
Wykonaj poniższe kroki, aby dodać rozwiązanie do zarządzania aktualizacjami do swojego obszaru roboczego usługi OMS, a także dodać agentów systemu Linux. Agenci systemu Windows są dodawani automatycznie bez dodatkowej konfiguracji.

> [!NOTE]
> Włączenie tego rozwiązania powoduje automatyczne skonfigurowanie każdego komputera z systemem Windows połączonego z obszarem roboczym pakietu OMS jako hybrydowego procesu roboczego elementu Runbook w celu obsługi elementów Runbook należących do tego rozwiązania.  Nie jest ono jednak zarejestrowane w żadnych grupach hybrydowych procesów roboczych, które mogły już zostać przez Ciebie zdefiniowane na Twoim koncie usługi Automation.  Możesz je dodać do grupy hybrydowych procesów roboczych elementów Runbook na Twoim koncie usługi Automation w celu obsługi elementów Runbook usługi Automation, o ile używasz tego samego konta zarówno dla tego rozwiązania, jak i dla członkostwa w grupie hybrydowych procesów roboczych elementów Runbook.  Ta funkcjonalność została dodana do wersji 7.2.12024.0 hybrydowego procesu roboczego elementu Runbook.   

1. Dodaj rozwiązanie do zarządzania aktualizacjami do swojego obszaru roboczego usługi OMS przy użyciu procesu opisanego w artykule [Add OMS solutions](../log-analytics/log-analytics-add-solutions.md) (Dodawanie rozwiązań OMS) z galerii rozwiązań.  
2. W portalu usługi OMS wybierz pozycję **Ustawienia**, a następnie pozycję **Połączone źródła**.  Zanotuj **identyfikator obszaru roboczego** i **klucz podstawowy** albo **klucz pomocniczy**.
3. Wykonaj następujące czynności na każdym komputerze z systemem Linux.
   
   a.    Zainstaluj najnowszą wersję agenta usługi OMS dla systemu Linux, uruchamiając następujące polecenia.  Zastąp parametr <Workspace ID> identyfikatorem obszaru roboczego, a parametr <Key> kluczem podstawowym lub pomocniczym.
   
        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh  
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

   b. Aby usunąć agenta, uruchom następujące polecenie:
   
        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Pakiety administracyjne
Jeśli grupa zarządzania programu System Center Operations Manager jest połączona z obszarem roboczym usługi OMS, to następujące pakiety administracyjne zostaną zainstalowane w programie Operations Manager po dodaniu tego rozwiązania. Nie jest wymagana żadna konfiguracja ani obsługa tych pakietów administracyjnych. 

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pakiet administracyjny wdrożenia aktualizacji

Aby uzyskać więcej informacji na temat aktualizowania pakietów administracyjnych rozwiązania, zobacz artykuł [Connect Operations Manager to Log Analytics](../log-analytics/log-analytics-om-agents.md) (Połączenie programu Operations Manager z usługą Log Analytics).

## <a name="data-collection"></a>Zbieranie danych
### <a name="supported-agents"></a>Obsługiwani agenci
W poniższej tabeli opisano połączone źródła, które obsługuje to rozwiązanie.

| Połączone źródło | Obsługiwane | Opis |
| --- | --- | --- |
| Agenci dla systemu Windows |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Windows i inicjuje instalowanie wymaganych aktualizacji. |
| Agenci dla systemu Linux |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów dla systemu Linux. |
| Grupa zarządzania programu Operations Manager |Tak |Rozwiązanie zbiera informacje o aktualizacjach systemu z agentów w połączonej grupie zarządzania.<br>Bezpośrednie połączenie agenta programu Operations Manager z usługą Log Analytics nie jest wymagane. Dane są przekazywane z grupy zarządzania do repozytorium usługi OMS. |
| Konto magazynu Azure |Nie |Magazyn Azure nie zawiera informacji o aktualizacjach systemu. |

### <a name="collection-frequency"></a>Częstotliwość zbierania
Skanowanie każdego zarządzanego komputera z systemem Windows odbywa się dwa razy dziennie.  Po zainstalowaniu aktualizacji informacje o niej są aktualizowane w ciągu 15 minut.  

Skanowanie każdego zarządzanego komputera z systemem Linux odbywa się co 3 godziny.  

## <a name="using-the-solution"></a>Użycie rozwiązania
Po dodaniu do obszaru roboczego OMS rozwiązania do zarządzania aktualizacjami na pulpicie nawigacyjnym usługi OMS pojawi się kafelek **Zarządzanie aktualizacjami**. Ten kafelek zawiera liczbę oraz graficzną reprezentację liczby komputerów w środowisku, które obecnie wymagają aktualizacji systemu.<br><br>
![Kafelek podsumowujący zarządzanie aktualizacjami](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Wyświetlanie ocen aktualizacji
Kliknij kafelek **Zarządzanie aktualizacjami**, aby otworzyć pulpit nawigacyjny **Zarządzanie aktualizacjami**. Na pulpicie nawigacyjnym znajdują się kolumny wymienione w poniższej tabeli. Każda kolumna zawiera do dziesięciu elementów spełniających jej kryteria dla podanego zakresu i przedziału czasu. Można uruchomić wyszukiwanie w dzienniku, które zwróci wszystkie rekordy. W tym celu kliknij przycisk **Zobacz wszystko** na dole kolumny lub kliknij nagłówek kolumny.

| Kolumna | Opis |
| --- | --- |
| **Komputery bez aktualizacji** | |
| Aktualizacje krytyczne lub zabezpieczeń |Wyświetla dziesięć pierwszych komputerów, na których brakuje aktualizacji. Komputery są posortowane według liczby brakujących aktualizacji. Kliknij nazwę komputera, aby uruchomić wyszukiwanie w dzienniku, które zwróci wszystkie rekordy aktualizacji dotyczące tego komputera. |
| Aktualizacje krytyczne lub zabezpieczeń starsze niż 30 dni |Wyświetla liczbę komputerów, na których brakuje aktualizacji krytycznych lub zabezpieczeń, grupując je według czasu od momentu opublikowania aktualizacji. Kliknij jeden z wpisów, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie brakujące aktualizacje krytyczne. |
| **Wymagane brakujące aktualizacje** | |
| Aktualizacje krytyczne lub zabezpieczeń |Wyświetla listę klasyfikacji aktualizacji, których brakuje na komputerach, posortowaną według liczby komputerów, na których brakuje aktualizacji z danej kategorii. Kliknij nazwę klasyfikacji, aby uruchomić wyszukiwanie w dzienniku, które zwróci wszystkie rekordy aktualizacji dotyczące tej klasyfikacji. |
| **Wdrożenia aktualizacji** | |
| Wdrożenia aktualizacji |Liczba obecnie zaplanowanych wdrożeń aktualizacji i czas do następnego zaplanowanego uruchomienia.  Kliknij kafelek, aby wyświetlić harmonogramy oraz aktualnie uruchomione i ukończone aktualizacje lub aby zaplanować nowe wdrożenie. |

<br>  
![Pulpit nawigacyjny podsumowujący zarządzanie aktualizacjami](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Widok komputerów pulpitu nawigacyjnego zarządzania aktualizacjami](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Widok pakietów pulpitu nawigacyjnego zarządzania aktualizacjami](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Instalowanie aktualizacji
Gdy aktualizacje zostaną przeanalizowane dla wszystkich komputerów z systemem Windows w środowisku, konieczne może być utworzenie *wdrożenia aktualizacji* w celu zainstalowania wymaganych aktualizacji.  Wdrożenie aktualizacji to zaplanowana instalacja wymaganych aktualizacji na co najmniej jednym komputerze z systemem Windows.  Należy określić datę i godzinę wdrożenia, a także komputer lub grupę komputerów, które mają zostać uwzględnione.  

Aktualizacje są instalowane przez elementy runbook w usłudze Azure Automation.  Nie można wyświetlić tych elementów runbook i nie wymagają one żadnej konfiguracji.  Utworzenie wdrożenia aktualizacji powoduje utworzenie harmonogramu, który uruchamia główny element runbook aktualizacji w określonym czasie na uwzględnionych komputerach.  Ten główny element runbook uruchamia podrzędny element runbook na każdym agencie systemu Windows, który przeprowadza instalację wymaganych aktualizacji.  

W przypadku maszyn wirtualnych utworzonych na podstawie z obrazów systemu Red Hat Enterprise Linux (RHEL) na żądanie dostępnych w witrynie Azure Marketplace są one rejestrowane w celu uzyskiwania dostępu do [infrastruktury aktualizacji systemu Red Hat (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) wdrożonej na platformie Azure.  Inne dystrybucje systemu Linux należy aktualizować przy użyciu repozytorium plików online dystrybucji i odpowiadających im metod.  

### <a name="viewing-update-deployments"></a>Wyświetlanie wdrożeń aktualizacji
Kliknij kafelek **Wdrożenia aktualizacji**, aby wyświetlić listę istniejących wdrożeń aktualizacji.  Są one pogrupowane według stanu — **Zaplanowano**, **Uruchomiono** i **Ukończono**.<br><br> ![Strona harmonogramu wdrożenia aktualizacji](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Właściwości wyświetlane w każdym wdrożeniu aktualizacji są opisane w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Nazwa |Nazwa wdrożenia aktualizacji. |
| Harmonogram |Typ harmonogramu.  Obecnie jedyną możliwą wartością jest *OneTime* (Jednorazowy). |
| Godzina rozpoczęcia |Data i godzina zaplanowanego uruchomienia wdrożenia aktualizacji. |
| Czas trwania |Liczba minut, przez jaką może działać wdrożenie aktualizacji.  Jeśli wszystkie aktualizacje nie zostaną zainstalowane przed upływem tego czasu, pozostałe aktualizacje zostaną odroczone do następnego wdrażania aktualizacji. |
| Serwery |Liczba komputerów, których dotyczy wdrożenie aktualizacji. |
| Stan |Stan wdrożenia aktualizacji.<br><br>Możliwe wartości:<br>- Nie uruchomiono<br>- Uruchomiono<br>- Ukończono |

Kliknij wdrożenie aktualizacji, aby wyświetlić ekran szczegółów, który zawiera kolumny podane w tabeli poniżej.  Kolumny te będą niewypełnione, jeśli wdrożenie aktualizacji nie zostało jeszcze uruchomione.<br>

| Kolumna | Opis |
| --- | --- |
| **Wyniki komputera** | |
| Ukończono pomyślnie |Wyświetla liczbę komputerów objętych wdrażaniem aktualizacji według ich stanu.  Kliknij stan, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie rekordy aktualizacji o tym stanie wdrażania aktualizacji. |
| Stan instalacji komputera |Wyświetla listę komputerów objętych wdrożeniem aktualizacji i procent pomyślnie zainstalowanych aktualizacji. Kliknij jeden z wpisów, aby uruchomić wyszukiwanie w dzienniku, które zwraca wszystkie brakujące aktualizacje krytyczne. |
| **Aktualizowanie wyników wystąpienia** | |
| Stan instalacji wystąpienia |Wyświetla listę klasyfikacji aktualizacji, których brakuje na komputerach, posortowaną według liczby komputerów, na których brakuje aktualizacji z danej kategorii. Kliknij nazwę komputera, aby uruchomić wyszukiwanie w dzienniku, które zwróci wszystkie rekordy aktualizacji dotyczące tego komputera. |

<br><br> ![Przegląd wyników wdrożenia aktualizacji](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Tworzenie wdrożenia aktualizacji
Aby utworzyć nowe wdrożenie aktualizacji, kliknij przycisk **Dodaj** na górze ekranu. Zostanie otwarta strona **Nowe wdrożenie aktualizacji**.  Należy podać wartości właściwości zawartych w poniższej tabeli.

| Właściwość | Opis |
| --- | --- |
| Nazwa |Unikatowa nazwa identyfikującą wdrożenie aktualizacji. |
| Strefa czasowa |Strefa czasowa służąca do określenia godziny rozpoczęcia. |
| Godzina rozpoczęcia |Data i godzina rozpoczęcia wdrażania aktualizacji. |
| Czas trwania |Liczba minut, przez jaką może działać wdrożenie aktualizacji.  Jeśli wszystkie aktualizacje nie zostaną zainstalowane przed upływem tego czasu, pozostałe aktualizacje zostaną odroczone do następnego wdrażania aktualizacji. |
| Komputery |Nazwy komputerów lub grup komputerów do uwzględnienia podczas wdrożenia aktualizacji.  Wybierz co najmniej jeden wpis z listy rozwijanej. |

<br><br> ![Strona nowego wdrożenia aktualizacji](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Przedział czasu
Domyślnie zakres danych przeanalizowanych w rozwiązaniu do zarządzania aktualizacjami obejmuje wszystkie połączone grupy zarządzania wygenerowane w ciągu ostatniego dnia. 

Aby zmienić okres danych, wybierz pozycję **Dane oparte na** u góry pulpitu nawigacyjnego. Możesz wybrać rekordy utworzone lub zaktualizowane w ciągu ostatnich 7 dni, 1 dnia lub 6 godzin. Możesz też wybrać opcję **Niestandardowy** i określić niestandardowy zakres dat.<br><br> ![Opcja niestandardowego przedziału czasu](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

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

<br>
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
| Wszystkie komputery z brakującymi aktualizacjami |Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Brakujące aktualizacje na komputerze „COMPUTER01.contoso.com” (zastąp to nazwą własnego komputera) |Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |
| Wszystkie komputery z brakującymi aktualizacjami krytycznymi i zabezpieczeń |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |
| Aktualizacje krytyczne lub zabezpieczeń wymagane przez komputery, na których aktualizacje są stosowane ręcznie |Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |
| Zdarzenia błędu dotyczące komputerów, na których brakuje wymaganych aktualizacji krytycznych lub zabezpieczeń |Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |
| Wszystkie komputery z brakującymi pakietami zbiorczymi aktualizacji |Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |
| Różne brakujące aktualizacje na wszystkich komputerach |Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |
| Korzystanie przez komputer z usług WSUS |Type=UpdateSummary &#124; measure count() by WSUSServer |
| Konfiguracja automatycznej aktualizacji |Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |
| Komputery z wyłączoną automatyczną aktualizacją |Type=UpdateSummary WindowsUpdateSetting=Manual |
| Lista wszystkich komputerów z systemem Linux, które mają dostępne aktualizacje pakietu |Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |
| Lista wszystkich komputerów z systemem Linux, dla których jest dostępna aktualizacja pakietu o znaczeniu krytycznym lub dotycząca luki w zabezpieczeniach |Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |
| Lista wszystkich pakietów, które mają dostępną aktualizację |Type=Update and OSType=Linux and UpdateState!="Not needed" |
| Lista wszystkich pakietów, dla których jest dostępna aktualizacja o znaczeniu krytycznym lub dotycząca luki w zabezpieczeniach |Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |
| Listę wszystkich komputerów z systemem Ubuntu, dla których są dostępne dowolne aktualizacje |Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |

## <a name="next-steps"></a>Następne kroki
* Korzystanie z wyszukiwania w dzienniku usługi [Log Analytics](../log-analytics/log-analytics-log-searches.md) w celu wyświetlania szczegółowych danych aktualizacji.
* [Tworzenie własnych pulpitów nawigacyjnych](../log-analytics/log-analytics-dashboards.md) przedstawiających zgodność aktualizacji na zarządzanych komputerach.
* [Tworzenie alertów](../log-analytics/log-analytics-alerts.md) po wykryciu braku aktualizacji krytycznych na komputerach lub komputera z wyłączonymi aktualizacjami automatycznymi.  


