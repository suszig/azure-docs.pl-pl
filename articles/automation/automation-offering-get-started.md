--- 
title: "Rozpoczynanie pracy z usługą Azure Automation | Microsoft Docs"
description: "Ten artykuł zawiera omówienie usługi Azure Automation oparte na przeglądzie szczegółów projektu i implementacji w ramach przygotowania do dołączenia oferty z platformy Azure Marketplace."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: 7ef31d7d72844c0ed3be0701549e49e26aac9abf
ms.contentlocale: pl-pl
ms.lasthandoff: 07/24/2017

---

# <a name="getting-started-with-azure-automation"></a>Rozpoczynanie pracy z usługą Azure Automation

Ten przewodnik Wprowadzenie opisuje podstawowe pojęcia związane z wdrażaniem usługi Azure Automation. Jeśli usługa Automation na platformie Azure nie była wcześniej używana lub masz doświadczenie z oprogramowaniem do automatyzacji przepływu pracy, takim jak System Center Orchestrator, ten przewodnik ułatwi Ci zrozumienie procedur przygotowywania i dołączania usługi Automation.  Po zakończeniu będzie można rozpocząć tworzenie elementów runbook w celu zaspokojenia potrzeb związanych z automatyzacją procesów. 


## <a name="automation-architecture-overview"></a>Omówienie architektury usługi Automation

![Omówienie usługi Azure Automation](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Usługa Azure Automation to aplikacja oprogramowania jako usługi (SaaS), która zapewnia skalowalne i niezawodne wielodostępne środowisko do automatyzacji procesów za pomocą elementów Runbook i zarządzania zmianami konfiguracji w systemach Windows i Linux przy użyciu konfiguracji żądanego stanu (DSC) na platformie Azure, w innych usługach w chmurze lub lokalnie. Jednostki zawarte na Twoim koncie usługi Automation, takie jak elementy Runbook, zasoby, konta Uruchom jako, są izolowane od innych kont usługi Automation w ramach Twojej subskrypcji i innych subskrypcji.  

Elementy Runbook, które uruchamiasz na platformie Azure, są wykonywane w piaskownicach usługi Automation hostowanych na platformie Azure w postaci platformy jako usługi (PaaS) dla maszyn wirtualnych.  Piaskownice usługi Automation zapewniają izolację dzierżawy dla wszystkich aspektów wykonywania elementów Runbook — modułów, magazynu, pamięci, komunikacji sieciowej, strumieni zadań itd. Ta rola jest zarządzana przez usługę i nie jest dostępna do sterowania z Twojej platformy Azure ani konta usługi Azure Automation.         

Aby zautomatyzować wdrażanie zasobów i zarządzanie nimi w lokalnym centrum danych lub w innych usługach w chmurze, po utworzeniu konta usługi Automation możesz wyznaczyć jedną lub większą liczbę maszyn do uruchamiania roli [hybrydowego procesu roboczego elementu runbook (HRW, Hybrid Runbook Worker)](automation-hybrid-runbook-worker.md).  Każdy proces HRW wymaga agenta Microsoft Management Agent mającego połączenie z obszarem roboczym usługi Log Analytics i kontem usługi Automation.  Usługa Log Analytics służy do uruchamiania instalacji, obsługi agenta Microsoft Management Agent i monitorowania funkcjonalności procesu HRW.  Dostarczanie elementów Runbook i instrukcji ich uruchamiania jest realizowane przez usługę Azure Automation.

Możesz wdrożyć wiele procesów HRW, aby zapewnić wysoką dostępność swoich elementów Runbook, zrównoważenie obciążenia zadań elementów Runbook, a w niektórych przypadkach dedykowanie ich dla konkretnych obciążeń lub środowisk.  Agent Microsoft Monitoring Agent w procesie HRW inicjuje komunikację z usługą Automation za pośrednictwem portu TCP 443 i nie ma żadnych wymagań dotyczących zapory dla ruchu przychodzącego.  Gdy element runbook działa w ramach procesu HRW w obrębie Twojego środowiska i chcesz, aby element runbook wykonywał zadania zarządzania względem innych maszyn lub usług w ramach tego środowiska, element runbook może wymagać dostępu również do innych portów.  Jeśli Twoje informatyczne zasady zabezpieczeń nie pozwalają komputerom w Twojej sieci na łączenie się z Internetem, zapoznaj się z artykułem [Brama pakietu OMS](../log-analytics/log-analytics-oms-gateway.md). Taka brama działa jak serwer proxy dla procesu HRW w celu zbierania stanu zadania i odbierania informacji o konfiguracji z Twojego konta usługi Automation.

Elementy Runbook działające w oparciu o proces HRW działają w kontekście lokalnego konta systemowego na komputerze, który jest zalecanym kontekstem zabezpieczeń podczas wykonywania zadań administracyjnych na komputerze lokalnym z systemem Windows. Jeśli element Runbook ma uruchamiać zadania dotyczące zasobów poza komputerem lokalnym, może być konieczne zdefiniowanie na koncie usługi Automation bezpiecznych zasobów poświadczeń, do których możesz uzyskać dostęp z elementu Runbook i których możesz użyć do uwierzytelniania za pomocą zewnętrznego zasobu. Zasobów [Poświadczenie](automation-credentials.md), [Certyfikat](automation-certificates.md) i [Połączenie](automation-connections.md) w Twoim elemencie Runbook możesz użyć za pomocą poleceń cmdlet, które pozwalają określić poświadczenia, więc możesz je uwierzytelnić.

Konfiguracje DSC przechowywane w usłudze Azure Automation mogą być stosowane bezpośrednio do maszyn wirtualnych platformy Azure. Inne maszyny fizyczne i wirtualne mogą żądać dostępu do konfiguracji z serwera ściągania usługi Azure Automation DSC.  Do zarządzania konfiguracjami lokalnych fizycznych lub wirtualnych systemów Windows i Linux nie potrzebujesz wdrażać żadnej infrastruktury do obsługi serwera ściągania usługi Automation DSC, a tylko wychodzącego dostępu do Internetu zarządzanego przez usługę Automation DSC komunikującego się z pakietem OMS przez port 443 protokołu TCP.   

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="automation-dsc"></a>Usługa Automation DSC
Usługa Azure Automation DSC może służyć do zarządzania różnymi maszynami:

* Maszynami wirtualnymi platformy Azure (klasycznymi) z systemem Windows lub Linux
* Maszynami wirtualnymi platformy Azure z systemem Windows lub Linux
* Maszynami wirtualnymi usługi Amazon Web Services (AWS) z systemem Windows lub Linux
* Fizycznymi/wirtualnymi lokalnymi komputerami z systemem Windows lub w chmurze innej niż platformy Azure lub usług AWS
* Fizycznymi/wirtualnymi lokalnymi komputerami z systemem Linux lub w chmurze innej niż platformy Azure lub usług AWS

Aby system Windows mógł się komunikować z usługą Azure Automation, musi być zainstalowana najnowsza wersja WMF 5 dla agenta rozszerzenia DSC programu PowerShell. Aby system Linux mógł komunikować się z usługą Azure Automation, musi być zainstalowana najnowsza wersja [agenta rozszerzenia DSC programu PowerShell dla systemu Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150).

### <a name="hybrid-runbook-worker"></a>Hybrydowy proces roboczy elementu Runbook  
Podczas wyznaczania komputera do uruchamiania zadań hybrydowych elementów Runbook należy wziąć pod uwagę istnienie następujących elementów:

* Windows Server 2012 lub nowszy
* Windows PowerShell 4.0 lub nowszy.  Zaleca się zainstalowanie na komputerze programu Windows PowerShell 5.0 w celu zwiększenia niezawodności. Nową wersję możesz pobrać w [Centrum pobierania Microsoft](https://www.microsoft.com/download/details.aspx?id=50395)
* Co najmniej dwa rdzenie
* Co najmniej 4 GB pamięci RAM

### <a name="permissions-required-to-create-automation-account"></a>Uprawnienia wymagane do utworzenia konta usługi Automation
Aby utworzyć lub zaktualizować konto usługi Automation, musisz mieć następujące określone uprawnienia i uprawnienia wymagane do wykonania zadań opisanych w tym temacie.   
 
* Przed utworzeniem konta usługi Automation konto użytkownika usługi AD musi zostać dodane do roli z uprawnieniami odpowiadającym roli współautora dla zasobów Microsoft.Automation w sposób opisany w artykule [Kontrola dostępu oparta na rolach w usłudze Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).  
* Użytkownicy inni niż administratorzy w dzierżawie usługi Azure AD mogą [rejestrować aplikacje usługi AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions), jeśli wartością ustawienia rejestracji aplikacji jest **Tak**.  Jeśli wartością ustawienia rejestracji aplikacji jest **Nie**, użytkownik wykonujący tę akcję musi być administratorem globalnym w usłudze Azure AD. 

Jeśli nie jesteś członkiem wystąpienia usługi Active Directory subskrypcji, przed dodaniem do roli administratora globalnego/współadministratora subskrypcji, dodamy Cię do usługi Active Directory jako gościa. W tej usłudze zobaczysz ostrzeżenie „Nie masz uprawnień do utworzenia...” w bloku **Dodawanie konta usługi Automation**. Użytkownicy dodani do roli administratora globalnego/współadministratora najpierw mogą zostać usunięci z wystąpienia usługi Active Directory dla subskrypcji i ponownie dodani, aby zostać pełnymi użytkownikami w usłudze Active Directory. Aby zweryfikować tę sytuację, w okienku **Azure Active Directory** w witrynie Azure Portal wybierz kolejno pozycje **Użytkownicy i grupy** i **Wszyscy użytkownicy**, a po wybraniu określonego użytkownika wybierz pozycję **Profil**. Wartość atrybutu **Typ użytkownika** na liście profilów użytkowników nie powinna być równa **Gość**.

## <a name="authentication-planning"></a>Planowanie uwierzytelniania
Usługa Azure Automation pozwala na zautomatyzowanie zadań w odniesieniu do zasobów platformy Azure, a także zasobów lokalnych oraz pochodzących od innych dostawców chmury.  Aby element Runbook mógł wykonać żądane działania, musi mieć uprawnienia do bezpiecznego dostępu do zasobów z minimalnymi prawami wymaganymi w ramach subskrypcji.  

### <a name="what-is-an-automation-account"></a>Co to jest konto usługi Automation 
Wszystkie zadania automatyzacji, które wykonujesz w odniesieniu do zasobów przy użyciu poleceń cmdlet platformy Azure w usłudze Azure Automation, są uwierzytelniane na platformie Azure przy użyciu funkcji uwierzytelniania opartego na poświadczeniu tożsamości organizacyjnej w usłudze Azure Active Directory.  Konto usługi Automation jest oddzielone od konta, którego używasz do logowania się do portalu w celu skonfigurowania i używania zasobów platformy Azure.  Zasoby usługi Automation uwzględnione na koncie to:

* **Certyfikaty** — zawiera certyfikat używany do uwierzytelniania z poziomu elementu runbook lub konfiguracji DSC lub ich dodawania.
* **Połączenia** — zawiera informacje o uwierzytelnianiu wymagane do połączenia z zewnętrzną usługą lub aplikacją z poziomu elementu runbook lub konfiguracji DSC.
* **Poświadczenia** — obiekt PSCredential zawierający poświadczenia zabezpieczeń, takie jak nazwa użytkownika i hasło, wymagane do uwierzytelniania z poziomu elementu runbook lub konfiguracji DSC.
* **Moduły integracji** — moduły programu PowerShell uwzględnione na koncie usługi Azure Automation w celu korzystania z poleceń cmdlet w obrębie elementów runbook i konfiguracji DSC.
* **Harmonogramy** — zawiera harmonogramy, które uruchamiają lub zatrzymują element runbook o określonej godzinie (w tym z częstotliwością cykliczną).
* **Zmienne** — zawiera wartości dostępne z poziomu elementu runbook lub konfiguracji DSC.
* **Konfiguracja DSC** — to skrypty programu PowerShell opisujące sposób konfigurowania funkcji lub ustawienia systemu operacyjnego bądź instalowania aplikacji na komputerze z systemem Windows lub Linux.  
* **Elementy Runbook** — to zestaw zadań opartych na programie PowerShell wykonujących niektóre zautomatyzowane procesy w usłudze Azure Automation.    

Zasoby usługi Automation na poszczególnych kontach są skojarzone z pojedynczym regionem świadczenia usługi Azure, ale za pomocą kont usługi Automation można zarządzać wszystkimi zasobami w ramach subskrypcji. Utwórz konta usługi Automation w różnych regionach, jeśli masz zasady, które wymagają, aby dane i zasoby były izolowane w określonym regionie.

> [!NOTE]
> Utworzone w witrynie Azure Portal konta usługi Automation i zawarte w nich zasoby nie są dostępne w klasycznym portalu Azure. Aby zarządzać tymi kontami lub ich zasobami za pomocą programu Windows PowerShell, należy skorzystać z modułów usługi Azure Resource Manager.
> 

Podczas tworzenia konta usługi Automation w witrynie Azure Portal automatycznie utworzysz dwie jednostki uwierzytelniania:

* Konto Uruchom jako. To konto służy do tworzenia nazwy głównej usługi w usłudze Azure Active Directory (Azure AD) oraz certyfikatu. Umożliwia ono również przypisywanie kontroli dostępu opartej na rolach (RBAC) typu Współautor, która zarządza zasobami usługi Resource Manager przy użyciu elementów runbook.
* Klasyczne konto Uruchom jako. To konto służy do przekazywania certyfikatu zarządzania, który jest używany do zarządzania klasycznymi zasobami przy użyciu elementów Runbook.

Dostępna w usłudze Azure Resource Manager kontrola dostępu oparta na rolach umożliwia przypisywanie praw wykonywania dozwolonych akcji do konta użytkownika usługi Azure AD i konta Uruchom jako oraz pozwala na uwierzytelnianie tego obiektu głównego usługi.  Dalsze informacje pomagające w tworzeniu modelu zarządzania uprawnieniami w usłudze Automation można znaleźć w artykule [Kontrola dostępu oparta na rolach w usłudze Azure Automation](automation-role-based-access-control.md).  

#### <a name="authentication-methods"></a>Metody uwierzytelniania
Poniższa tabela zawiera zestawienie różnych metod uwierzytelniania dla środowisk obsługiwanych przez usługę Azure Automation.

| Metoda | Środowisko 
| --- | --- | 
| Konto Uruchom jako platformy Azure i klasyczne konto Uruchom jako |Wdrożenie usługi Azure Resource Manager i klasycznej platformy Azure |  
| Konto użytkownika usługi Azure AD |Wdrożenie usługi Azure Resource Manager i klasycznej platformy Azure |  
| Uwierzytelnianie systemu Windows |Lokalne centrum danych lub inny dostawca usług w chmurze z wykorzystaniem hybrydowego procesu roboczego elementu Runbook |  
| Poświadczenia AWS |Amazon Web Services |  

W sekcji **Instrukcje\Uwierzytelnianie i zabezpieczenia** znajdują się pomocne artykuły zawierające omówienie i kroki implementacji umożliwiające skonfigurowanie uwierzytelniania dla tych środowisk przy użyciu istniejącego albo nowego konta przeznaczonego dla danego środowiska.  W przypadku konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako temat [Aktualizacja konta Uruchom jako usługi Automation](automation-create-runas-account.md) opisuje sposób aktualizacji istniejącego konta usługi Automation za pomocą kont Uruchom jako w portalu lub przy użyciu programu PowerShell, jeśli nie zostało ono pierwotnie skonfigurowane przy użyciu konta Uruchom jako lub klasycznego konta Uruchom jako. Jeśli chcesz utworzyć konto Uruchom jako i klasyczne konto Uruchom jako, używając certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa, zapoznaj się z tym artykułem, aby dowiedzieć się, jak utworzyć konta przy użyciu tej konfiguracji.     
 
## <a name="network-planning"></a>Planowanie sieci
Aby hybrydowy proces roboczy elementu Runbook mógł nawiązać połączenie i zarejestrować się za pomocą pakietu Microsoft Operations Management Suite (OMS), musi on mieć dostęp do numeru portu i opisanych poniżej adresów URL.  Jest to uzupełnienie [portów i adresów URL wymaganych przez agenta Microsoft Monitoring Agent](../log-analytics/log-analytics-windows-agents.md#network) do nawiązania połączenia z pakietem OMS. Jeśli na potrzeby komunikacji między agentem i pakietem OMS używasz serwera proxy, konieczne będzie zapewnienie dostępności odpowiednich zasobów. W przypadku ograniczania dostępu do Internetu za pomocą zapory musisz skonfigurować w zaporze zezwalanie na dostęp.

Poniższe informacje zawierają listę portów i adresów URL, które są wymagane przez hybrydowy proces roboczy elementu Runbook do komunikacji z usługą Automation.

* Port: Dla wychodzącego dostępu do Internetu wymagany jest tylko port 443 protokołu TCP
* Globalny adres URL: *.azure-automation.net

Jeśli masz konto usługi Automation zdefiniowane dla określonego regionu i chcesz ograniczyć komunikację z tym regionalnym centrum danych, w poniższej tabeli znajdziesz rekord DNS dla każdego regionu.

| **Region** | **Rekord DNS** |
| --- | --- |
| Środkowo-południowe stany USA |scus-jobruntimedata-prod-su1.azure-automation.net |
| Wschodnie stany USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Środkowo-zachodnie stany USA | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Europa Zachodnia |we-jobruntimedata-prod-su1.azure-automation.net |
| Europa Północna |ne-jobruntimedata-prod-su1.azure-automation.net |
| Kanada Środkowa |cc-jobruntimedata-prod-su1.azure-automation.net |
| Azja Południowo-Wschodnia |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indie Środkowe |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonia Wschodnia |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australia Południowo-Wschodnia |ase-jobruntimedata-prod-su1.azure-automation.net |
| Południowe Zjednoczone Królestwo | uks-jobruntimedata-prod-su1.azure-automation.net |
| Administracja USA — Wirginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Aby zamiast nazw uzyskać listę adresów IP, pobierz plik xml [Azure Datacenter IP address](https://www.microsoft.com/download/details.aspx?id=41653) (Adres IP centrum danych platformy Azure) z Centrum pobierania Microsoft i przejrzyj jego zawartość. 

> [!NOTE]
> Ten plik zawiera zakresy adresów IP (w tym zakresy zasobów obliczeniowych, bazy danych SQL i magazynu) używane w centrach danych platformy Microsoft Azure. Co tydzień jest publikowany zaktualizowany plik odzwierciedlający aktualnie wdrożone zakresy i wszystkie nadchodzące zmiany w zakresach adresów IP. Nowe zakresy pojawiające się w pliku nie będą używane w centrach danych przez co najmniej tydzień. Nowy plik xml należy pobierać co tydzień i wykonywać niezbędne zmiany w witrynie, aby prawidłowo identyfikować usługi uruchomione na platformie Azure. Użytkownicy usługi ExpressRoute mogą zwrócić uwagę na to, że wcześniej ten plik aktualizował anonsowania BGP platformy Azure w pierwszym tygodniu każdego miesiąca. 
> 

## <a name="creating-an-automation-account"></a>Tworzenie konta usługi Automation

Istnieją różne sposoby tworzenia konta usługi Automation w witrynie Azure Portal.  Poniższa tabela opisuje każdy typ wdrożenia i różnice między nimi.  

|Metoda | Opis |
|-------|-------------|
| Na platformie Marketplace wybierz pozycję Automatyzacja i kontrola | Oferta, która tworzy zarówno konto usługi Automation, jak i obszar roboczy OMS związane ze sobą w tej samej grupie zasobów i regionie.  Integracja z pakietem OMS obejmuje również korzyści związane z używaniem usługi Log Analytics do monitorowania i analizowania strumieni zadań i stanu zadań elementów runbook w czasie oraz do wykorzystywania zaawansowanych funkcji w celu eskalowania lub badania problemów. Oferta wdraża też rozwiązania do śledzenia zmian i zarządzania aktualizacjami, które są domyślnie włączone. |
| Na platformie Marketplace wybierz pozycję Automatyzacja | Tworzy konto usługi Automation w nowej lub istniejącej grupie zasobów, która nie jest połączona z obszarem roboczym OMS i nie zawiera żadnych dostępnych rozwiązań z oferty automatyzacji i kontroli. To jest podstawowa konfiguracja, która stanowi wprowadzenie do usługi Automation i ułatwia zapoznanie się ze sposobem pisania elementów Runbook, konfiguracji DSC i korzystania z możliwości usługi. |
| Wybrane rozwiązania do zarządzania | W przypadku wybrania rozwiązania — **[Zarządzanie aktualizacjami](../operations-management-suite/oms-solution-update-management.md)**, **[Uruchamianie i zatrzymywanie maszyn wirtualnych poza godzinami pracy](automation-solution-vm-management.md)** lub **[Śledzenie zmian](../log-analytics/log-analytics-change-tracking.md)** — monitują one o wybranie istniejącej usługi Automation i obszaru roboczego OMS lub umożliwiają utworzenie ich obu zgodnie z wymaganiami dla rozwiązania, które ma zostać wdrożone w ramach Twojej subskrypcji. |

Ten temat poprowadzi Cię przez proces tworzenia konta usługi Automation i obszaru roboczego OMS przez dołączenie oferty automatyzacji i kontroli.  Aby uzyskać informacje dotyczące tworzenia autonomicznego konta usługi Automation do testowania lub przeglądania usługi, zapoznaj się z następującym artykułem: [Tworzenie autonomicznego konta usługi Automation](automation-create-standalone-account.md).  

### <a name="create-automation-account-integrated-with-oms"></a>Tworzenie konta usługi Automation zintegrowanego z pakietem OMS
Zalecana metoda uwzględnienia usługi Automation polega na wybraniu oferty automatyzacji i kontroli z platformy Marketplace.  Spowoduje to zarówno utworzenie konta usługi Automation, jak i ustanowienie integracji z obszarem roboczym OMS, a w tym możliwość instalowania rozwiązań do zarządzania, które są dostępne wraz z ofertą.  

1. Zaloguj się do witryny Azure Portal przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.

2. Kliknij przycisk **Nowy**.<br><br> ![Wybieranie opcji Nowy w witrynie Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Wyszukaj termin **Automation**, a następnie w wynikach wyszukiwania wybierz pozycję **Automatyzacja i kontrola***.<br><br> ![Na platformie Marketplace wyszukaj i wybierz pozycję Automatyzacja i kontrola](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   

4. Po przeczytaniu opisu oferty kliknij przycisk **Utwórz**.  

5. W bloku ustawień **Automatyzacja i kontrola** wybierz pozycję **Obszar roboczy OMS**.  W bloku **Obszary robocze OMS** wybierz obszar roboczy OMS połączony z tą samą subskrypcją platformy Azure, w której znajduje się konto usługi Automation, lub utwórz obszar roboczy OMS.  Jeśli nie masz obszaru roboczego OMS, wybierz pozycję **Utwórz nowy obszar roboczy** i wykonaj następujące czynności w bloku **Obszar roboczy OMS**: 
   - Określ nazwę dla nowego **Obszaru roboczego OMS**.
   - Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
   - W pozycji **Grupa zasobów** możesz utworzyć grupę zasobów lub wybrać istniejącą grupę zasobów.  
   - Wybierz **lokalizację**.  Obecnie jedynymi dostępnymi lokalizacjami są: **Australia Południowo-Wschodnia**, **Wschodnie stany USA**, **Azja Południowo-Wschodnia**, **Środkowo-zachodnie stany USA** oraz **Europa Zachodnia**.
   - Wybierz **warstwę cenową**.  Rozwiązanie jest oferowane w dwóch warstwach: bezpłatnej i warstwie na węzeł (OMS).  W warstwie bezpłatnej obowiązuje dzienny limit ilości zbieranych danych, a także limit okresu przechowywania oraz minut czasu wykonywania zadania elementu Runbook.  W warstwie na węzeł (OMS) nie stosuje się dziennego limitu ilości zbieranych danych.  
   - Wybierz pozycję **Konto usługi Automation**.  Jeśli tworzysz nowy obszar roboczy OMS, wymagane będzie również utworzenie konta usługi Automation, które jest skojarzone z nowym wcześniej określonym obszarem roboczym OMS, włącznie z subskrypcją platformy Azure, grupą zasobów i regionem.  Możesz wybrać pozycję **Utwórz konto usługi Automation** i podać następujące dane w bloku **Konto usługi Automation**: 
  - W polu **Nazwa** wprowadź nazwę konta usługi Automation.

    Wszystkie inne opcje są automatycznie uzupełniane w oparciu o wybrany obszar roboczy OMS. Nie można zmodyfikować tych opcji.  Konto Uruchom jako platformy Azure jest domyślną metodą uwierzytelniania dla oferty.  Gdy klikniesz przycisk **OK**, opcje konfiguracji zostaną sprawdzone, a konto usługi Automation zostanie utworzone.  Postęp możesz śledzić w sekcji **Powiadomienia** z poziomu menu. 

    W przeciwnym razie wybierz istniejące konto Uruchom jako usługi Automation.  Wybrane konto nie może być już połączone z innym obszarem roboczym OMS. W przeciwnym razie w bloku zostanie wyświetlony komunikat powiadomienia.  Jeśli konto jest już połączone, musisz wybrać inne konto Uruchom jako usługi Automation lub utworzyć konto.

    Po podaniu wymaganych informacji kliknij przycisk **Utwórz**.  Informacje są weryfikowane, a następnie jest tworzone konto usługi Automation oraz konta Uruchom jako.  Nastąpi automatyczny powrót do bloku **Obszar roboczy OMS**.  

6. Po podaniu wymaganych informacji w bloku **Obszar roboczy OMS** kliknij przycisk **Utwórz**.  Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu.  Nastąpi powrót do bloku **Dodawanie rozwiązania**.  

7. W bloku ustawień **Automatyzacja i kontrola** potwierdź chęć zainstalowania zalecanych wcześniej wybranych rozwiązań. Jeśli usuniesz wybór któregokolwiek z nich, możesz zainstalować je oddzielnie później.  

8. Kliknij przycisk **Utwórz**, aby kontynuować dołączanie usługi Automation i obszaru roboczego OMS. Wszystkie ustawienia są sprawdzane, a następnie jest podejmowana próba wdrożenia oferty w Twojej subskrypcji.  Ten proces może potrwać kilka sekund. Możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

Po dołączeniu tej oferty możesz rozpocząć tworzenie elementów runbook, pracować z włączonymi przez Ciebie rozwiązaniami do zarządzania, wdrożyć rolę [procesu roboczego hybrydowego elementu runbook](automation-hybrid-runbook-worker.md) lub rozpocząć pracę z usługą [Log Analytics](https://docs.microsoft.com/azure/log-analytics), aby zebrać dane wygenerowane przez zasoby w środowiskach w chmurze lub lokalnych.   

## <a name="next-steps"></a>Następne kroki
* Możesz potwierdzić, że Twoje nowe konto usługi Automation może uwierzytelniać względem zasobów platformy Azure, sprawdzając [test uwierzytelniania konta Uruchom jako usługi Azure Automation](automation-verify-runas-authentication.md).
* Przed rozpoczęciem tworzenia elementów runbook należy najpierw poznać obsługiwane [typy elementów runbook usługi Automation](automation-runbook-types.md) oraz powiązane zagadnienia.



