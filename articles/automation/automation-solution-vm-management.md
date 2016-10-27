<properties
    pageTitle="Rozwiązanie umożliwiające uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza] | Microsoft Azure"
    description="Rozwiązania z zakresu zarządzania maszynami wirtualnymi uruchamiają i zatrzymują maszyny wirtualne usługi Azure Resource Manager zgodnie z harmonogramem i aktywnie monitorują działanie z poziomu usługi Log Analytics."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/07/2016"
    ms.author="magoedte"/>


# Rozwiązanie umożliwiające uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza] w usłudze Automation

Rozwiązanie umożliwiające uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza] uruchamia i zatrzymuje maszyny wirtualne usługi Azure Resource Manager zgodnie z harmonogramem zdefiniowanym przez użytkownika i zapewnia wgląd w powodzenie zadań usługi Automation, które uruchamiają i zatrzymują maszyny wirtualne, za pomocą usługi OMS Log Analytics.  

## Wymagania wstępne

- Elementy Runbook działają przy użyciu [konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).  Konto Uruchom jako jest preferowaną metodą uwierzytelniania, ponieważ używa certyfikatu zamiast hasła, które może wygasać lub ulegać częstym zmianom.  

- To rozwiązanie może zarządzać tylko maszynami wirtualnymi, które znajdują się w tej samej subskrypcji i grupie zasobów co konto usługi Automation.  

- To rozwiązanie może być wdrożone tylko w następujących regionach platformy Azure: Australia Południowo-Wschodnia, Wschodnie stany USA, Azja Południowo-Wschodnia oraz Europa Zachodnia.  Elementy Runbook, które zarządzają harmonogramem maszyny wirtualnej, mogą kierować działania na maszyny wirtualne w dowolnym regionie.  

- Aby wysyłać powiadomienia e-mail w momencie zakończenia działania elementów Runbook uruchamiających lub zatrzymujących maszyny wirtualne, wymagana jest subskrypcja klasy biznesowej usługi Office 365.  

## Składniki rozwiązania

To rozwiązanie składa się z następujących zasobów, które zostaną zaimportowane i dodane do konta usługi Automation.

### Elementy Runbook

Element Runbook | Opis|
--------|------------|
CleanSolution-MS-Mgmt-VM | Ten element Runbook spowoduje usunięcie wszystkich zasobów i harmonogramów po rozpoczęciu operacji usuwania rozwiązania z subskrypcji.|  
SendMailO365-MS-Mgmt | Ten element Runbook wysyła wiadomość e-mail za pośrednictwem programu Exchange w usłudze Office 365.|
StartByResourceGroup-MS-Mgmt-VM | Ten element Runbook jest przeznaczony do uruchamiania maszyn wirtualnych (opartych na modelu klasycznym oraz maszyn wirtualnych opartych na usłudze ARM), które znajdują się na danej liście grup zasobów platformy Azure.
StopByResourceGroup-MS-Mgmt-VM | Ten element Runbook jest przeznaczony do zatrzymywania maszyn wirtualnych (opartych na modelu klasycznym oraz maszyn wirtualnych opartych na usłudze ARM), które znajdują się na danej liście grup zasobów platformy Azure.|
<br>

### Zmienne

Zmienna | Opis|
---------|------------|
Element Runbook **SendMailO365-MS-Mgmt** ||
SendMailO365-IsSendEmail-MS-Mgmt | Określa, czy elementy Runbook StartByResourceGroup-MS-Mgmt-VM i StopByResourceGroup-MS-Mgmt-VM mogą wysyłać powiadomienia e-mail po zakończeniu działania.  Wybierz wartość **True**, aby włączyć, lub **False**, aby wyłączyć alerty e-mail. Wartość domyślna to **False**.| 
Element Runbook **StartByResourceGroup-MS-Mgmt-VM** ||
StartByResourceGroup-ExcludeList-MS-Mgmt-VM | Wprowadź nazwy maszyn wirtualnych, które będą wykluczone z operacji zarządzania. Oddziel nazwy przy użyciu średnika (;). W wartościach jest rozróżniana wielkość liter. Dopuszcza się użycie symbolu wieloznacznego (gwiazdki).|
StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Tekst, który można dołączyć do początku treści wiadomości e-mail.|
StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Określa nazwę konta usługi Automation, które zawiera element Runbook poczty e-mail.  **Nie należy modyfikować tej zmiennej.**|
StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt | Określa nazwę elementu Runbook poczty e-mail.  Ta zmienna jest używana przez elementy Runbook StartByResourceGroup-MS-Mgmt-VM i StopByResourceGroup-MS-Mgmt-VM do wysyłania wiadomości e-mail.  **Nie należy modyfikować tej zmiennej.**|
StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Określa nazwę grupy zasobów, która zawiera element Runbook poczty e-mail.  **Nie należy modyfikować tej zmiennej.**|
StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Określa tekst dla wiersza tematu wiadomości e-mail.|  
StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Określa adresatów wiadomości e-mail.  Wprowadź oddzielne nazwy przy użyciu średnika (;).|
StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Wprowadź nazwy maszyn wirtualnych, które będą wykluczone z operacji zarządzania. Oddziel nazwy przy użyciu średnika (;). W wartościach jest rozróżniana wielkość liter. Dopuszcza się użycie symbolu wieloznacznego (gwiazdki).  Wartość domyślna (gwiazdka) będzie obejmować wszystkie grupy zasobów w subskrypcji.|
StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Określa subskrypcję zawierającą maszyny wirtualne, które będą zarządzane przez to rozwiązanie.  Musi to być ta sama subskrypcja, w której znajduje się konto usługi Automation tego rozwiązania.|
Element Runbook **StopByResourceGroup-MS-Mgmt-VM** ||
StopByResourceGroup-ExcludeList-MS-Mgmt-VM | Wprowadź nazwy maszyn wirtualnych, które będą wykluczone z operacji zarządzania. Oddziel nazwy przy użyciu średnika (;). W wartościach jest rozróżniana wielkość liter. Dopuszcza się użycie symbolu wieloznacznego (gwiazdki).|
StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Tekst, który można dołączyć do początku treści wiadomości e-mail.|
StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Określa nazwę konta usługi Automation, które zawiera element Runbook poczty e-mail.  **Nie należy modyfikować tej zmiennej.**|
StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Określa nazwę grupy zasobów, która zawiera element Runbook poczty e-mail.  **Nie należy modyfikować tej zmiennej.**|
StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Określa tekst dla wiersza tematu wiadomości e-mail.|  
StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Określa adresatów wiadomości e-mail.  Wprowadź oddzielne nazwy przy użyciu średnika (;).|
StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Wprowadź nazwy maszyn wirtualnych, które będą wykluczone z operacji zarządzania. Oddziel nazwy przy użyciu średnika (;). W wartościach jest rozróżniana wielkość liter. Dopuszcza się użycie symbolu wieloznacznego (gwiazdki).  Wartość domyślna (gwiazdka) będzie obejmować wszystkie grupy zasobów w subskrypcji.|
StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Określa subskrypcję zawierającą maszyny wirtualne, które będą zarządzane przez to rozwiązanie.  Musi to być ta sama subskrypcja, w której znajduje się konto usługi Automation tego rozwiązania.|  
<br>

### Harmonogramy

Harmonogram | Opis|
---------|------------|
StartByResourceGroup-Schedule-MS-Mgmt | Harmonogram elementu Runbook StartByResourceGroup, który wykonuje uruchamianie maszyn wirtualnych zarządzanych przez to rozwiązanie.|
StopByResourceGroup-Schedule-MS-Mgmt | Harmonogram elementu Runbook StopByResourceGroup, który wykonuje zatrzymywanie maszyn wirtualnych zarządzanych przez to rozwiązanie.|

### Poświadczenia

Poświadczenie | Opis|
-----------|------------|
O365Credential | Określa prawidłowe konto użytkownika usługi Office 365 do wysłania wiadomości e-mail.  Wymagane tylko wtedy, gdy zmienna SendMailO365-IsSendEmail-MS-Mgmt jest ustawiona na wartość **True**.

## Konfiguracja

Wykonaj poniższe kroki, aby dodać rozwiązanie umożliwiające uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza] do konta usługi Automation, a następnie skonfiguruj zmienne, aby dostosować rozwiązanie.

1. Na ekranie głównym w witrynie Azure Portal wybierz kafelek **Marketplace**.  Jeśli kafelek nie jest już przypięty do ekranu głównego, wybierz opcję **Nowy** w lewym okienku nawigacyjnym.  
2. W bloku Marketplace wpisz **Uruchamianie maszyn wirtualnych** w polu wyszukiwania, a następnie wybierz rozwiązanie **Uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza]** z wyników wyszukiwania.  
3. W bloku **Uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza]** dla wybranego rozwiązania sprawdź informacje w podsumowaniu, a następnie kliknij przycisk **Utwórz**.  
4. Blok **Dodawanie rozwiązania** pojawi się, gdy zostanie wyświetlony monit o skonfigurowanie rozwiązania przed jego zaimportowaniem do subskrypcji usługi Automation.<br><br> ![Zarządzanie maszynami wirtualnymi — blok Dodawanie rozwiązania](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5.  W bloku **Dodawanie rozwiązania** wybierz pozycję **Obszar roboczy**. W tym miejscu możesz wybrać obszar roboczy OMS połączony z tą samą subskrypcją platformy Azure, w której znajduje się konto usługi Automation, lub utworzyć nowy obszar roboczy OMS.  Jeśli nie masz obszaru roboczego OMS, możesz wybrać opcję **Utwórz nowy obszar roboczy** i wykonać następujące czynności w bloku **Obszar roboczy OMS**: 
   - Określ nazwę dla nowego **Obszaru roboczego OMS**.
   - Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
   - W pozycji **Grupa zasobów** możesz utworzyć nową grupę zasobów lub wybrać istniejącą grupę zasobów.  
   - Wybierz **lokalizację**.  Obecnie można wybrać tylko spośród następujących lokalizacji: **Australia Południowo-Wschodnia**, **Wschodnie stany USA**, **Azja Południowo-Wschodnia** oraz **Europa Zachodnia**.
   - Wybierz **warstwę cenową**.  Rozwiązanie jest oferowane w dwóch warstwach: bezpłatnej i płatnej warstwie pakietu OMS.  W warstwie bezpłatnej obowiązuje dzienny limit ilości zbieranych danych, a także limit okresu przechowywania oraz minut czasu wykonywania zadania elementu Runbook.  W warstwie płatnej usługi OMS nie stosuje się dziennego limitu ilości zbieranych danych.  

        > [AZURE.NOTE]
        > Autonomiczna warstwa płatna jest wyświetlana jako opcja, ale nie ma tu zastosowania.  Jeśli wybierzesz tę opcję i spróbujesz kontynuować tworzenie tego rozwiązania, operacja nie powiedzie się.  Ten problem zostanie rozwiązany po oficjalnym wydaniu tego rozwiązania.<br>Jeśli używasz tego rozwiązania, będzie ono używać wyłącznie minut zadania automatyzacji oraz pozyskiwania dziennika.  Rozwiązanie nie dodaje kolejnych węzłów pakietu OMS do środowiska.  

6. Po podaniu wymaganych informacji w bloku **Obszar roboczy OMS** kliknij przycisk **Utwórz**.  Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu.  Nastąpi powrót do bloku **Dodawanie rozwiązania**.  
7. W bloku **Dodawanie rozwiązania** wybierz opcję **Konto usługi Automation**.  Jeśli tworzysz nowy obszar roboczy OMS, wymagane będzie również utworzenie nowego konta usługi Automation, które będzie skojarzone z nowym wcześniej określonym obszarem roboczym OMS, włącznie z subskrypcją platformy Azure, grupą zasobów i regionem.  Możesz wybrać opcję **Utwórz konto usługi Automation** i podać następujące dane w bloku **Dodawanie konta usługi Automation**: 
  - W polu **Nazwa** wprowadź nazwę konta usługi Automation.

    Wszystkie inne opcje są automatycznie uzupełniane w oparciu o wybrany obszar roboczy OMS. Nie można zmodyfikować tych opcji.  Konto Uruchom jako platformy Azure jest domyślną metodą uwierzytelniania dla elementów Runbook zawartych w tym rozwiązaniu.  Gdy klikniesz przycisk **OK**, opcje konfiguracji zostaną sprawdzone, a konto usługi Automation zostanie utworzone.  Postęp możesz śledzić w sekcji **Powiadomienia** z poziomu menu. 

    W przeciwnym razie możesz wybrać istniejące konto Uruchom jako usługi Automation.  Pamiętaj, że wybrane konto nie może być już połączone z innym obszarem roboczym OMS. W przeciwnym razie w bloku zostanie wyświetlony komunikat z odpowiednią informacją dla użytkownika.  Jeśli konto jest już połączone, należy wybrać inne konto Uruchom jako usługi Automation lub utworzyć nowe konto.<br><br> ![Konto usługi Automation, które jest już połączone z obszarem roboczym OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Na koniec w bloku **Dodawanie rozwiązania** wybierz opcję **Konfiguracja**. Zostanie wyświetlony blok **Parametry**.  W bloku **Parametry** zostanie wyświetlony monit o:  
   - Określenie **Nazwy docelowej grupy zasobów**, tj. nazwy grupy zasobów zawierającej maszyny wirtualne, które będą zarządzane przez to rozwiązanie.  Wprowadzić możesz kilka nazw i oddzielić je przy użyciu średnika (w wartościach jest rozróżniana wielkość liter).  Użycie symbolu wieloznacznego jest obsługiwane. Możesz skorzystać z tej opcji, jeśli chcesz uwzględnić maszyny wirtualne we wszystkich grupach zasobów w subskrypcji.
   - Wybranie **Harmonogramu** czyli cyklicznej daty i godziny uruchamiania oraz zatrzymywania maszyn wirtualnych w docelowych grupach zasobów.  

10. Po zakończeniu konfigurowania ustawień początkowych wymaganych dla rozwiązania wybierz opcję **Utwórz**.  Wszystkie ustawienia zostaną sprawdzone, a następnie zostanie podjęta próba wdrożenia rozwiązania w subskrypcji.  Ten proces może potrwać kilka sekund. Możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## Częstotliwość zbierania

Dziennik zadań usługi Automation oraz dane strumienia zadań są zbierane w repozytorium OMS co pięć minut.  

## Użycie rozwiązania

Po dodaniu rozwiązania do zarządzania maszynami wirtualnymi w obszarze roboczym OMS do pulpitu nawigacyjnego OMS zostanie dodany kafelek **Widok StartStopVM**.  Ten kafelek wyświetla liczbę i graficzne przedstawienie zadań elementów Runbook dla rozwiązania, które zostały uruchomione i zakończyły pracę z powodzeniem.<br><br> ![Zarządzanie maszynami wirtualnymi — kafelek Widok StartStopVM](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Na koncie usługi Automation możesz uzyskać dostęp do rozwiązania i zarządzać nim poprzez wybranie kafelka **Rozwiązania**, a następnie wybranie w bloku **Rozwiązania** opcji **Start-Stop-VM[Obszar roboczy]** z lity.<br><br> ![Lista rozwiązań usługi Automation](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Wybranie rozwiązania spowoduje wyświetlenie bloku rozwiązania **Start-Stop-VM[Obszar roboczy]**. W tym miejscu możesz sprawdzić ważne szczegóły, np. kafelek **StartStopVM**, tak jak w obszarze roboczym OMS, przedstawiający liczbę i graficzną reprezentację zadań elementów Runbook dla rozwiązania, które zostały uruchomione i zakończyły pracę z powodzeniem.<br><br> ![Blok rozwiązania maszyny wirtualnej w usłudze Automation](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

W tym miejscu możesz także otworzyć obszar roboczy OMS i wykonać dalszą analizę rekordów zadania.  Po prostu kliknij pozycję **Wszystkie ustawienia** i blok **Ustawienia**, wybierz opcję **Szybki start** i następnie w bloku **Szybki start** wybierz pozycję **Portal pakietu OMS**.   To spowoduje otwarcie nowej karty lub nowej sesji przeglądarki i wyświetlenie obszaru roboczego OMS skojarzonego z Twoim kontem i subskrypcją usługi Automation.  


### Konfigurowanie powiadomień e-mail

Aby włączyć powiadomienia e-mail wysyłane w przypadku zakończenia pracy elementów Runbook uruchamiających i zatrzymujących maszyny wirtualne, należy zmodyfikować poświadczenie **O365Credential** oraz co najmniej następujące zmienne:

 - SendMailO365-IsSendEmail-MS-Mgmt
 - StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
 - StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

Aby skonfigurować poświadczenie **O365Credential**, wykonaj następujące czynności:

1. Na Twoim koncie usługi Automation kliknij przycisk **Wszystkie ustawienia** w górnej części okna. 
2. W bloku **Ustawienia** w sekcji **Zasoby usługi Automation** wybierz opcję **Zasoby**. 
3. W bloku **Zasoby** wybierz kafelek **Poświadczenie**, a następnie w bloku **Poświadczenie** wybierz pozycję **O365Credential**.  
4. Wprowadź prawidłową nazwę użytkownika oraz hasło usługi Office 365, a następnie kliknij przycisk **Zapisz**, aby zapisać zmiany.  

Aby skonfigurować wcześniej wyróżnione zmienne, wykonaj następujące czynności:

1. Na Twoim koncie usługi Automation kliknij przycisk **Wszystkie ustawienia** w górnej części okna. 
2. W bloku **Ustawienia** w sekcji **Zasoby usługi Automation** wybierz opcję **Zasoby**. 
3. W bloku **Zasoby** wybierz kafelek **Zmienne**, a następnie w bloku **Zmienne** wybierz zmienną wymienioną powyżej. Zmodyfikuj wartość zmiennej zgodnie z opisem określonym wcześniej w sekcji [Zmienne](##variables).  
4. Kliknij przycisk **Zapisz**, aby zapisać zmiany wprowadzone w zmiennej.   

### Modyfikowanie harmonogramu uruchamiania i zamykania

Zarządzanie harmonogramem uruchamiania i zamykania w tym rozwiązaniu obejmuje te same kroki co przedstawione w temacie [Planowanie elementu Runbook w usłudze Azure Automation](automation-scheduling-a-runbook.md).  Należy pamiętać, że nie można zmodyfikować konfiguracji harmonogramu.  Należy wyłączyć istniejący harmonogram, utworzyć nowy i następnie połączyć go z elementem Runbook **StartByResourceGroup-MS-Mgmt-VM** lub **StopByResourceGroup-MS-Mgmt-VM**, dla którego ma zostać zastosowany harmonogram.   

## Rekordy usługi Log Analytics

Usługa Automation tworzy dwa typy rekordów w repozytorium OMS.

### Dzienniki zadań

Właściwość | Opis|
----------|----------|
Obiekt wywołujący |  Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań.|
Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobLogs.|
CorrelationId | Identyfikator GUID, który jest identyfikatorem korelacji zadania elementu Runbook.|
JobId | Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook.|
operationName | Określa typ operacji wykonywanej na platformie Azure.  W przypadku usługi Automation wartością jest Job.|
resourceId | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
ResourceGroup | Określa nazwę grupy zasobów zadania elementu Runbook.|
ResourceProvider | Określa nazwę usługi platformy Azure, która zapewnia zasoby do wdrożenia i zarządzania.  W przypadku usługi Automation wartością jest Azure Automation.|
ResourceType | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
resultType | Stan zadania elementu Runbook.  Możliwe wartości:<br>— Uruchomione<br>— Zatrzymane<br>— Wstrzymane<br>— Nie powiodło się<br>— Powiodło się|
resultDescription | Opisuje stan wyniku zadania elementu Runbook.  Możliwe wartości:<br>— Zadanie jest uruchomione<br>— Zadanie nie powiodło się<br>— Zadanie zostało ukończone|
RunbookName | Określa nazwę elementu Runbook.|
SourceSystem | Określa system źródłowy dla przesłanych danych.  W przypadku usługi Automation wartością jest :OpsManager|
StreamType | Określa typ zdarzenia. Możliwe wartości:<br>— Pełne<br>— Dane wyjściowe<br>— Błąd<br>— Ostrzeżenie|
SubscriptionId | Określa identyfikator subskrypcji zadania.
Time | Data i godzina dla wykonania zadania elementu Runbook.|


### Strumienie zadania

Właściwość | Opis|
----------|----------|
Obiekt wywołujący |  Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań.|
Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobStreams.|
JobId | Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook.|
operationName | Określa typ operacji wykonywanej na platformie Azure.  W przypadku usługi Automation wartością jest Job.|
ResourceGroup | Określa nazwę grupy zasobów zadania elementu Runbook.|
resourceId | Określa identyfikator zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
ResourceProvider | Określa nazwę usługi platformy Azure, która zapewnia zasoby do wdrożenia i zarządzania.  W przypadku usługi Automation wartością jest Azure Automation.|
ResourceType | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
resultType | Wynik zadania elementu Runbook w czasie wygenerowania zdarzenia.  Możliwe wartości:<br>— W toku|
resultDescription | Obejmuje strumień wyjściowy z elementu Runbook.|
RunbookName | Nazwa elementu Runbook.|
SourceSystem | Określa system źródłowy dla przesłanych danych.  W przypadku usługi Automation wartością jest OpsManager|
StreamType | Typ strumienia zadania. Możliwe wartości:<br>— Postęp<br>— Dane wyjściowe<br>— Ostrzeżenie<br>— Błąd<br>— Debugowanie<br>— Pełne|
Time | Data i godzina dla wykonania zadania elementu Runbook.|

Jeśli wykonujesz dowolne wyszukiwanie dzienników, które zwraca rekordy w kategorii **JobLogs** lub **JobStreams**, możesz wybrać widok **JobLogs** lub **JobStreams**, który zawiera zestaw kafelków z podsumowaniem aktualizacji zwracanych przez wyszukiwanie.

## Przykładowe wyszukiwania dzienników

Poniższa tabela zawiera przykładowe wyszukiwania dzienników dla rekordów dzienników zbieranych przez to rozwiązanie. 

Zapytanie | Opis|
----------|----------|
Znajdź zadania dla elementu Runbook StartVM, które zostały zakończone powodzeniem | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; measure count() by JobId_g|
Znajdź zadania dla elementu Runbook StopVM, które zostały zakończone powodzeniem | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; measure count() by JobId_g
Pokaż stan zadania w czasie dla elementów Runbook StartVM i StopVM | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") | measure Count() by ResultType interval 1day|

## Następne kroki

- Aby dowiedzieć się więcej o sposobie tworzenia różnych zapytań wyszukiwania i sprawdzaniu dzienników zadań usługi Automation przy użyciu usługi Log Analytics, zobacz [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md) (Wyszukiwanie dzienników w usłudze Log Analytics)
- Aby dowiedzieć się więcej o wykonywaniu elementów Runbook, sposobie monitorowania zadań elementów Runbook i innych szczegółach technicznych, zobacz [Track a runbook job](automation-runbook-execution.md) (Śledzenie zadania elementu Runbook)
- Aby dowiedzieć się więcej o usłudze OMS Log Analytics i źródłach zbierania danych, zobacz [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md) (Zbieranie danych magazynu platformy Azure w usłudze Log Analytics — omówienie)






   




<!--HONumber=Oct16_HO3-->


