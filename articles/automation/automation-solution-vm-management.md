---
title: "Maszyny wirtualne uruchamiania i zatrzymywania podczas rozwiązania poza godzinami szczytu | Dokumentacja firmy Microsoft"
description: "Rozwiązania z zakresu zarządzania maszynami wirtualnymi uruchamiają i zatrzymują maszyny wirtualne usługi Azure Resource Manager zgodnie z harmonogramem i aktywnie monitorują działanie z poziomu usługi Log Analytics."
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: magoedte
ms.openlocfilehash: 2ff2208f62c24c460c9d17533e28fd007549828b
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Maszyny wirtualne uruchamiania i zatrzymywania podczas rozwiązania poza godzinami szczytu w automatyzacji Azure

Maszyny wirtualne uruchamiania i zatrzymywania godzinami rozwiązania rozpoczyna się zatrzymuje maszynach wirtualnych Azure harmonogramów zdefiniowane przez użytkownika, zawiera informacje na temat technologii za pośrednictwem analizy dzienników i wysyła wiadomości e-mail opcjonalne dzięki wykorzystaniu [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Obsługuje ona zarówno usługi Azure Resource Manager i klasycznych maszyn wirtualnych dla większości scenariuszy. 

To rozwiązanie zapewnia możliwość zdecentralizowane automatyzacji dla klientów, którzy mają zmniejszyć koszty wykorzystaniu zasobów niekorzystającą, niskich kosztów. Funkcje obejmują:

* Zaplanuj maszyn wirtualnych do uruchamiania i zatrzymywania
* Zaplanuj maszyn wirtualnych do uruchamiania/zatrzymywania rosnąco przy użyciu tagów Azure (nieobsługiwane w przypadku klasycznych maszyn wirtualnych)
* Automatycznie zatrzymania maszyny wirtualne oparte na niski użycie procesora CPU

## <a name="prerequisites"></a>Wymagania wstępne

- Elementy Runbook działają przy użyciu [konta Uruchom jako platformy Azure](automation-offering-get-started.md#authentication-methods).  Konto Uruchom jako jest preferowaną metodą uwierzytelniania, ponieważ używa certyfikatu zamiast hasła, które może wygasać lub ulegać częstym zmianom.  

- To rozwiązanie może zarządzać tylko maszyn wirtualnych, które znajdują się w tej samej subskrypcji co gdzie znajduje się konto automatyzacji.  

- To rozwiązanie wdraża tylko następujące regiony platformy Azure - Australia Południowo-Wschodnia, Kanada centralnej, Indie środkowe, wschodnie stany USA, Japonia Wschodnia, Azja południowo-wschodnia, Wielka Brytania Południowa i Europa Zachodnia.  
    
    > [!NOTE]
    > Elementy runbook, zarządzanie harmonogramem maszyny Wirtualnej można kierować maszyn wirtualnych w dowolnym regionie.  

- Do wysyłania powiadomień e-mail po ukończeniu uruchamianie i zatrzymywanie elementów runbook maszyny Wirtualnej, podczas dołączania z portalu Azure Marketplace, musisz wybrać Wybierz **tak** SendGrid wdrażania. 

    > [!IMPORTANT]
    > SendGrid to usługa innej firmy, pomocy technicznej sendgrid skontaktuj się z [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Ograniczenia sendgrid są następujące:

    * Maksymalna liczba jednego konta SendGrid dla użytkownika na subskrypcję
    * Maksymalnie dwóch kont SendGrid na subskrypcję

Jeśli wdrożono poprzedniej wersji tego rozwiązania, należy najpierw usunąć go z Twojego konta, przed wdrożeniem tej wersji.  

## <a name="solution-components"></a>Składniki rozwiązania

To rozwiązanie obejmuje wstępnie skonfigurowane elementów runbook, harmonogramów i integracja z usługą analizy dzienników, która pozwala na dostosowanie uruchamiania i wyłączania maszyn wirtualnych do zestawu firmy wymaga. 

### <a name="runbooks"></a>Elementy Runbook

Następująca tabela zawiera listę elementów runbook, wdrożone na koncie automatyzacji.  Nie zaleca się wprowadzać zmian w kodzie elementu runbook, ale raczej napisać własny elementu runbook dla nowych funkcji.

> [!NOTE]
> Bezpośrednio nie uruchamiaj dowolnym elemencie runbook o nazwie "Podrzędny" dołączany na końcu jego nazwie.
>

Zawiera wszystkie nadrzędne elementy runbook *WhatIf* parametr służący ustawioną **True**, obsługuje określające zachowanie dokładne wykonuje element runbook, gdy uruchamiane bez *WhatIf* parametr i sprawdza poprawność poprawny docelowej maszyn wirtualnych.  Elementy Runbook tylko wykonuje działania zdefiniowane podczas *WhatIf* ustawiono parametr **False**. 

|**Element Runbook** | **Parametry** | **Opis**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Wywołuje element runbook nadrzędnej. Tworzy alerty na podstawie ciągu zasobu dla scenariusza AutoStop.| 
|AutoStop_CreateAlert_Parent | WhatIf: Wartość PRAWDA lub FAŁSZ <br> VMList | Tworzy lub aktualizuje Azure reguł alertów na maszynach wirtualnych w grupach docelowych subskrypcja lub zasób. <br> VMList: Rozdzielana przecinkami lista maszyn wirtualnych.  Na przykład *vm1 maszyny vm2, vm3*| 
|AutoStop_Disable | brak | Wyłącz alerty AutoStop i domyślnego harmonogramu.| 
|AutoStop_StopVM_Child | WebHookData | Wywołana tylko nadrzędny element runbook. Reguły alertów wywołanie tego elementu runbook i jego wykonuje pracę zatrzymanie maszyny Wirtualnej.|  
|Bootstrap_Main | brak | Używany jeden raz w celu konfiguracji ładowania początkowego konfiguracje, takie jak webhookURI, która zwykle jest niedostępny z usługi Azure Resource Manager. Ten element runbook zostanie usunięta automatycznie, jeśli wdrożenia został pomyślnie usunięty.|  
|ScheduledStartStop_Child | VMName <br> Akcja: Zatrzymywania lub uruchamiania <br> Grupy zasobów o nazwie | Wywołana tylko nadrzędny element runbook. Nie rzeczywiste wykonywanie zatrzymanie lub uruchomienie na zatrzymanie zaplanowane.|  
|ScheduledStartStop_Parent | Akcja: Zatrzymywania lub uruchamiania <br> WhatIf: Wartość PRAWDA lub FAŁSZ | To zostaną wprowadzone na wszystkich maszynach wirtualnych w subskrypcji bez modyfikowania **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupNames** której będzie go można wykonać tylko na te ograniczyć grup zasobów obiektu docelowego. Można też wykluczyć określone maszyn wirtualnych, aktualizując **External_ExcludeVMNames** zmiennej. WhatIf działa tak samo jak inne elementy runbook.|  
|SequencedStartStop_Parent | Akcja: Zatrzymywania lub uruchamiania <br> WhatIf: Wartość PRAWDA lub FAŁSZ | Utworzyć tag o nazwie **SequenceStart** i innego tagu o nazwie **SequenceStop** na każdej maszynie Wirtualnej, do rozpoczęcia sekwencji\\zatrzymać działanie. Wartość tagu powinna być dodatnią liczbą całkowitą (1,2,3) umożliwiająca kolejność chcesz uruchomić\\zatrzymano w kolejności rosnącej. WhatIf działa tak samo jak inne elementy runbook. <br> **Uwaga: Maszyn wirtualnych musi być w grupach zasobów zdefiniowanych External_Start_ResourceGroupNames External_Stop_ResourceGroupNames i External_ExcludeVMNames w automatyzacji Azure zmiennych i mieć odpowiednie tagi dla akcji, które zostały wprowadzone.**|

### <a name="variables"></a>Zmienne

Poniższa tabela zawiera listę zmiennych utworzone na Twoim koncie automatyzacji.  Zalecane jest, modyfikować tylko zmienne prefiksem **zewnętrznych**, modyfikowanie zmiennych prefiksem **wewnętrzne** spowoduje, że niepożądane skutki.  

|**Zmienna** | **Opis**|
---------|------------|
|External_AutoStop_Condition | Jest to operator warunkowy wymagane do skonfigurowania warunek przed wyzwolenie alertu. Dopuszczalne wartości to **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, **LessThanOrEqual**.|  
|External_AutoStop_Description | Alert zatrzymania maszyny Wirtualnej, jeśli procent użycia procesora CPU przekracza wartość progową.|  
|External_AutoStop_MetricName | Nazwa metryki wydajności reguły alertu Azure jest konfigurowana dla.| 
|External_AutoStop_Threshold | Próg alertu Azure reguły określone w zmiennej *External_AutoStop_MetricName*. Wartości procentowe mogą należeć do zakresu od 1 do 100.|  
|External_AutoStop_TimeAggregationOperator | Operator agregacji czasu, które mają zostać zastosowane do rozmiaru wybranego okna Próba sprawdzenia warunku. Dopuszczalne wartości to **średni**, **Minimum**, **maksymalna**, **całkowita**, **ostatniego**.|  
|External_AutoStop_TimeWindow | Rozmiar okna, w którym Azure będzie analizować wybrana Metryka służącą do wyzwalania alertu. Ten parametr akceptuje dane wejściowe w formacie timespan. Możliwe wartości to od 5 do sześciu godzin.|  
|External_EmailFromAddress | Określa nadawcy wiadomości e-mail.|  
|External_EmailSubject | Określa tekst dla wiersza tematu wiadomości e-mail.|  
|External_EmailToAddress | Określa adresatów wiadomości e-mail. Rozdziel nazwy, użyj przecinka.|  
|External_ExcludeVMNames | Wprowadź nazw maszyn wirtualnych, które mają zostać wykluczone, oddzielając nazwy, użyj przecinka nie może zawierać spacji.|  
|External_IsSendEmail | Określa opcje do wysyłania powiadomień e-mail po zakończeniu.  Określ **tak** lub **nr** nie wysłać wiadomości e-mail.  Ta opcja powinna być **nr** Jeśli SendGrid nie może utworzyć podczas pierwszego wdrożenia.|  
|External_Start_ResourceGroupNames | Określa co najmniej jedną grupę zasobów, oddzielanie wartości przecinkami, przeznaczony dla działania uruchamiania.|  
|External_Stop_ResourceGroupNames | Określa jedną lub więcej grup zasobów, oddzielając wartości przecinkami, przeznaczony dla akcji zatrzymania.|  
|Internal_AutomationAccountName | Określa nazwę konta automatyzacji.|  
|Internal_AutoSnooze_WebhookUri | Określa, że identyfikator URI elementu Webhook wywołana dla tego scenariusza AutoStop.|  
|Internal_AzureSubscriptionId | Określa identyfikator subskrypcji platformy Azure.|  
|Internal_ResourceGroupName | Określa nazwę grupy zasobów konta usługi Automatyzacja Azure.|  
|Internal_SendGridAccountName | Określa nazwę konta SendGrid.|  
|Internal_SendGridPassword | Określa hasło konta SendGrid.|  

<br>

We wszystkich scenariuszach **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, i **External_ExcludeVMNames** zmienne są wymagane przeznaczony do maszyn wirtualnych z wyjątkiem dostarczanie przecinkami lista maszyn wirtualnych dla rozdzielonych **AutoStop_CreateAlert_Parent** elementu runbook. Oznacza to maszyny wirtualne muszą znajdować się w grup zasobów obiektu docelowego dla działania uruchamiania/zatrzymywania występuje. Logika działa nieco jak Azure zasad można docelowego w subskrypcji lub grupy zasobów i akcji dziedziczone przez nowo utworzony maszyn wirtualnych. Takie podejście pozwala uniknąć konieczności Obsługa oddzielne harmonogramu dla każdej maszyny Wirtualnej i zarządzanie nimi uruchamiania/zatrzymywania w skali.

### <a name="schedules"></a>Harmonogramy

Poniższa tabela zawiera listę domyślnych planów utworzone na Twoim koncie automatyzacji.  Można je zmodyfikować lub utworzyć własne niestandardowe harmonogramy.  Domyślnie każdy te harmonogramy wyłączone z wyjątkiem **Scheduled_StartVM** i **StopVM zaplanowane**.

Nie zaleca się włączyć wszystkie harmonogramy, jak to można utworzyć nakładanie się jako do harmonogramu, który wykonuje akcję.  Zamiast byłoby najlepiej określić optymalizacji, które chcesz wykonać i zmodyfikuj odpowiednio.  Zobacz przykładowe scenariusze, w sekcji Przegląd, aby uzyskać dokładniejsze objaśnienie.   

|**ScheduleName** | **Częstotliwość** | **Opis**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Co 8 godzin | Uruchamia element runbook AutoStop_CreateAlert_Parent co 8 godzin, które z kolei spowoduje zatrzymanie maszyny Wirtualnej na podstawie wartości w "External_Start_ResourceGroupNames", "External_Stop_ResourceGroupNames" i "External_ExcludeVMNames" w zmiennych automatyzacji Azure.  Alternatywnie można określić rozdzielana przecinkami lista maszyn wirtualnych za pomocą parametru "VMList".|  
|Scheduled_StopVM | Zdefiniowane przez użytkownika, każdego dnia | Uruchamia element runbook Scheduled_Parent z parametrem "Stop" każdego dnia o określonej godzinie.  Automatycznie spowoduje zatrzymanie wszystkich maszyn wirtualnych spełniających reguły zdefiniowane za pomocą zmiennych zasobów.  Zaleca się włączenie siostrzanej zaplanować StartVM zaplanowane.|  
|Scheduled_StartVM | Zdefiniowane przez użytkownika, każdego dnia | Uruchamia element runbook Scheduled_Parent z parametrem *Start* każdego dnia o określonej godzinie.  Zostaną automatycznie uruchomione wszystkie maszyny Wirtualnej spełniający zdefiniowanych reguł i część odpowiednich zmiennych.  Zalecane jest, Włącz siostrzanej harmonogramu, **StopVM zaplanowane**.|
|StopVM sekwencjonowania | 1:00:00 (UTC), każdy piątek | Uruchamia element runbook Sequenced_Parent z parametrem *zatrzymać* każdy piątek o określonej godzinie.  Zostanie sekwencyjnie (rosnąco) zatrzymanie wszystkich maszyn wirtualnych przez za pomocą tagów z **SequenceStop** zdefiniowane i należą do odpowiednich zmiennych.  Zapoznaj się z rozdziałem elementów Runbook uzyskać więcej informacji dotyczących zasobów zmiennych i wartości tagów.  Jest zalecane, Włącz siostrzanej harmonogramu, **Sequenced StartVM**.|
|StartVM sekwencjonowania | 1:00 PM (UTC), każdy poniedziałek | Uruchamia element runbook Sequenced_Parent z parametrem *Start* w każdy poniedziałek o określonej godzinie.  Zostaną kolejno start (malejąco) do wszystkich maszyn wirtualnych przy użyciu tagu z **SequenceStart** zdefiniowane i należą do odpowiednich zmiennych.  Zapoznaj się z rozdziałem elementów Runbook uzyskać więcej informacji dotyczących zasobów zmiennych i wartości tagów.  Zalecane jest, Włącz siostrzanej harmonogramu, **Sequenced StopVM**.|

<br>

## <a name="configuration"></a>Konfiguracja

Wykonaj poniższe kroki, aby dodać rozwiązanie umożliwiające uruchamianie/zatrzymywanie maszyn wirtualnych poza godzinami szczytu [wersja zapoznawcza] do konta usługi Automation, a następnie skonfiguruj zmienne, aby dostosować rozwiązanie.

1. W witrynie Azure Portal kliknij pozycję **Nowy**.<br> ![Witryna Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. W okienku Marketplace wpisz **uruchamianie maszyny Wirtualnej**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **uruchamiania/zatrzymywania maszyn wirtualnych w godzinach [Podgląd]** w wynikach wyszukiwania.  
3. W **uruchamiania/zatrzymywania maszyn wirtualnych w godzinach [Podgląd]** okienko dla wybranego rozwiązania, sprawdź informacje, a następnie kliknij przycisk **Utwórz**.  
4. **Dodaj rozwiązanie** okienko jest wyświetlane, gdy zostanie wyświetlony monit o skonfigurowanie rozwiązania, przed zaimportowaniem go w ramach subskrypcji automatyzacji.<br><br> ![Zarządzanie maszynami wirtualnymi — blok Dodawanie rozwiązania](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Na **Dodaj rozwiązanie** bloku, wybierz opcję **obszaru roboczego** i tutaj wybierz obszar roboczy OMS, który jest połączony z tej samej subskrypcji Azure, która jest konto usługi Automatyzacja lub Utwórz nowy obszar roboczy.  Jeśli nie masz obszaru roboczego, możesz wybrać **Utwórz nowy obszar roboczy** i na **obszarem roboczym pakietu OMS** okienko, wykonaj następujące czynności: 
   - Określ nazwę dla nowego **Obszaru roboczego OMS**.
   - Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
   - W pozycji **Grupa zasobów** możesz utworzyć nową grupę zasobów lub wybrać istniejącą grupę zasobów.  
   - Wybierz **lokalizację**.  Obecnie jedynymi lokalizacjami, dostarczone do wyboru są **południowo-wschodnia Australia**, **Kanada centralnej**, **Indie środkowe**, **wschodnie stany USA**, **Japonia Wschodnia**, **Azja południowo-wschodnia**, **Wielka Brytania Południowa**, i **Europa**.
   - Wybierz **warstwę cenową**.  Rozwiązanie jest oferowane w dwóch warstwach: bezpłatnej i płatnej warstwie pakietu OMS.  W warstwie bezpłatnej obowiązuje dzienny limit ilości zbieranych danych, a także limit okresu przechowywania oraz minut czasu wykonywania zadania elementu Runbook.  W warstwie płatnej usługi OMS nie stosuje się dziennego limitu ilości zbieranych danych.  

        > [!NOTE]
        > Gdy warstwy na GB (autonomiczna) płatnej jest wyświetlany jako opcja, nie ma zastosowania.  Jeśli wybierzesz tę opcję i spróbujesz kontynuować tworzenie tego rozwiązania, operacja nie powiedzie się.  Ten problem zostanie rozwiązany po oficjalnym wydaniu tego rozwiązania.<br>Jeśli używasz tego rozwiązania, będzie ono używać wyłącznie minut zadania automatyzacji oraz pozyskiwania dziennika.  Rozwiązanie nie dodaje kolejnych węzłów pakietu OMS do środowiska.  

6. Po podaniu wymaganych informacji w bloku **Obszar roboczy OMS** kliknij przycisk **Utwórz**.  Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu.  Nastąpi powrót do bloku **Dodawanie rozwiązania**.  
7. W bloku **Dodawanie rozwiązania** wybierz opcję **Konto usługi Automation**.  Jeśli tworzysz nowy obszar roboczy OMS, wymagane będzie również utworzenie nowego konta usługi Automation, które będzie skojarzone z nowym wcześniej określonym obszarem roboczym OMS, włącznie z subskrypcją platformy Azure, grupą zasobów i regionem.  Możesz wybrać opcję **Utwórz konto usługi Automation** i podać następujące dane w bloku **Dodawanie konta usługi Automation**: 
  - W polu **Nazwa** wprowadź nazwę konta usługi Automation.

    Wszystkie inne opcje są automatycznie uzupełniane w oparciu o wybrany obszar roboczy OMS. Nie można zmodyfikować tych opcji.  Konto Uruchom jako platformy Azure jest domyślną metodą uwierzytelniania dla elementów Runbook zawartych w tym rozwiązaniu.  Gdy klikniesz przycisk **OK**, opcje konfiguracji zostaną sprawdzone, a konto usługi Automation zostanie utworzone.  Postęp możesz śledzić w sekcji **Powiadomienia** z poziomu menu. 

    W przeciwnym razie możesz wybrać istniejące konto Uruchom jako usługi Automation.  Pamiętaj, że wybrane konto nie może być już połączone z innym obszarem roboczym OMS. W przeciwnym razie w bloku zostanie wyświetlony komunikat z odpowiednią informacją dla użytkownika.  Jeśli konto jest już połączone, należy wybrać inne konto Uruchom jako usługi Automation lub utworzyć nowe konto.<br><br> ![Konto usługi Automation, które jest już połączone z obszarem roboczym OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Na koniec w bloku **Dodawanie rozwiązania** wybierz opcję **Konfiguracja**. Zostanie wyświetlony blok **Parametry**.<br><br> ![Okienko parametrów dla rozwiązania](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  W bloku **Parametry** zostanie wyświetlony monit o:  
   - Określenie **Nazwy docelowej grupy zasobów**, tj. nazwy grupy zasobów zawierającej maszyny wirtualne, które będą zarządzane przez to rozwiązanie.  Można wprowadzić więcej niż jedną nazwę i oddzielić od siebie użyj przecinka (wartości nie są z uwzględnieniem wielkości liter).  Użycie symbolu wieloznacznego jest obsługiwane. Możesz skorzystać z tej opcji, jeśli chcesz uwzględnić maszyny wirtualne we wszystkich grupach zasobów w subskrypcji.
   - Specity **listę wykluczyć maszyny Wirtualnej (ciąg)**, czyli na nazwę lub więcej maszyn wirtualnych, docelowa grupa zasobów.  Można wprowadzić więcej niż jedną nazwę i oddzielić od siebie użyj przecinka (wartości nie są z uwzględnieniem wielkości liter).  Przy użyciu symboli wieloznacznych jest obsługiwana.
   - Wybranie **Harmonogramu** czyli cyklicznej daty i godziny uruchamiania oraz zatrzymywania maszyn wirtualnych w docelowych grupach zasobów.  Domyślnie harmonogramu jest skonfigurowany zgodnie ze strefą czasową UTC i wybrać inny region nie jest dostępna.  Jeśli chcesz skonfigurować harmonogram określonej strefy czasowej po skonfigurowaniu rozwiązania, zobacz [modyfikowanie harmonogramu uruchamiania i wyłączania](#modifying-the-startup-and-shutdown-schedule) poniżej.
   - Aby otrzymywać **wiadomości E-mail z powiadomieniami** z SendGrid, zaakceptuj wartość domyślną **tak** i podaj prawidłowy adres e-mail.  W przypadku wybrania **nr** i później zdecydujesz, aby otrzymywać powiadomienia pocztą e-mail, należy ponownie wdrożyć rozwiązanie ponownie z portalu Azure marketplace.  

10. Po zakończeniu konfigurowania ustawień początkowych wymaganych dla rozwiązania wybierz opcję **Utwórz**.  Wszystkie ustawienia zostaną sprawdzone, a następnie zostanie podjęta próba wdrożenia rozwiązania w subskrypcji.  Ten proces może potrwać kilka sekund. Możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="collection-frequency"></a>Częstotliwość zbierania

Automatyzacja zadania dziennika i zadania strumienia danych jest pozyskanych w repozytorium analizy dzienników co pięć minut.  

## <a name="using-the-solution"></a>Użycie rozwiązania

Po dodaniu rozwiązania do zarządzania maszyny Wirtualnej, w obszarze roboczym z portalu Azure Log Analytics **widoku StartStopVM** kafelka jest dodawany do pulpitu nawigacyjnego.  Ten Kafelek Wyświetla graficzną reprezentację zadania elementów runbook dla rozwiązania, które zostały uruchomione i została ukończona pomyślnie i liczba.<br><br> ![Zarządzanie maszynami wirtualnymi — kafelek Widok StartStopVM](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Na koncie automatyzacji można uzyskać dostęp i zarządzać nimi rozwiązania, wybierając **obszaru roboczego** opcji i na stronie analizy dzienników wybierz **rozwiązań** w lewym okienku.  Na **rozwiązań** wybierz rozwiązanie **Start-Stop-VM [obszaru roboczego]** z listy.<br><br> ![Lista rozwiązań w analizy dzienników](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Wybranie rozwiązania spowoduje wyświetlenie **Start-Stop-VM [obszaru roboczego]** bloku rozwiązania, w którym można przejrzeć ważne informacje takie jak **StartStopVM** Kafelek tak samo, jak w obszarze roboczym analizy dzienników, która przedstawia liczbę i graficzną reprezentację zadań elementu runbook dla rozwiązania, które zostały uruchomione i zostały ukończone pomyślnie.<br><br> ![Strona rozwiązania Automatyzacja zarządzania aktualizacjami](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

W tym miejscu można również wykonywać dalszej analizy rekordów zadań, klikając Kafelek pierścień i z poziomu pulpitu nawigacyjnego rozwiązania zawiera historię zadania, dziennik wstępnie zdefiniowane zapytania wyszukiwania, i przełącznika do portalu zaawansowane analizy dziennika, aby wyszukać oparte na zapytań wyszukiwania.  

Zawiera wszystkie nadrzędne elementy runbook *WhatIf* parametr służący ustawioną **True**, obsługuje określające zachowanie dokładne wykonuje element runbook, gdy uruchamiane bez *WhatIf* parametr i sprawdza poprawność poprawny docelowej maszyn wirtualnych.  Elementy Runbook tylko wykonuje działania zdefiniowane podczas *WhatIf* ustawiono parametr **False**.  


### <a name="scenario-1-daily-stopstart-vms-across-a-subscription-or-target-resource-groups"></a>Scenariusz 1: Codziennie stop/start maszyn wirtualnych w ramach subskrypcji lub docelowych grup zasobów 

To jest konfiguracja domyślna najpierw wdrażania rozwiązania.  Na przykład można skonfigurować jego zatrzymanie wszystkich maszyn wirtualnych między subskrypcjami wieczorem podczas pozostaw pracy i uruchom je w nocy, podczas pracy w biurze. Po skonfigurowaniu harmonogramy ** zaplanowane-StartVM "i **StopVM zaplanowane** podczas wdrażania, uruchomić i zatrzymać docelowych maszyn wirtualnych.
>[!NOTE]
>Strefa czasowa jest bieżącej strefy czasowej po skonfigurowaniu harmonogramu parametru czasu; jednak jest on przechowywany w formacie UTC automatyzacji Azure.  Nie trzeba wykonywać żadnych konwersji strefy czasowej odbywa się to podczas wdrażania.

Możesz kontrolować, które maszyny wirtualne są w zakresie przez skonfigurowanie dwóch zmiennych - **External_Start_ResourceGroupNames**, ** External_Stop_ResourceGroupNames, i **External_ExcludeVMNames**.  

>[!NOTE]
> Wartość zmiennej **nazw grupa zasobów docelowych**"jest przechowywana jako wartość dla obu **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupNames** zmienne. Dla dalszego szczegółowości można zmodyfikować każdą z tych zmiennych pod kątem różnych grup zasobów.  Do rozpoczęcia akcji użycia **External_Start_ResourceGroupNames** i stosowania akcji zatrzymania **External_Stop_ResourceGroupNames** zamiast tego. Nowe maszyny wirtualne są automatycznie dodawane do uruchomienia i zatrzymania harmonogramów.

Aby przetestować i sprawdzić poprawności konfiguracji, należy ręcznie uruchomić **ScheduledStartStop_Parent** runbook i ustaw *akcji* parametr **start** lub **zatrzymania**  i *WhatIf* parametr **true**.<br><br> ![Konfiguruj parametry elementu runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Dzięki temu można wyświetlić podgląd akcję, która będzie odbywać się i wprowadź zmiany w razie potrzeby, przed wdrożeniem względem maszyn wirtualnych w środowisku produkcyjnym.  Po potrafisz, można ręcznie wykonania elementu runbook z parametrem ustawioną **false** lub programowi Harmonogram automatyzacji **harmonogram-StartVM** i **harmonogram-StopVM**Uruchom automatycznie następującą określonego harmonogramu.

### <a name="scenario-2-sequence-the-stopstart-vms-across-a-subscription-using-tags"></a>Scenariusz 2: Sekwencji stop/uruchamiania maszyn wirtualnych w subskrypcji przy użyciu tagów
W środowisku zawierającym co najmniej dwóch składników na wiele maszyn wirtualnych, obsługa obciążeń rozproszonych ważne jest Obsługa sekwencji, które składniki są uruchomione/zatrzymany w kolejności.  Można to zrobić, wykonując następujące kroki.

1. Dodawanie **SequenceStart** i **SequenceStop** tag z dodatnią liczbę całkowitą do maszyn wirtualnych w subskrypcji, które są stosowane w **External_Start_ResourceGroupNames**i  **External_Stop*ResourceGroupNames** zmiennych.  Uruchamianie i zatrzymywanie działań zostanie wykonane w kolejności rosnącej.  Aby dowiedzieć się, jak znacznik maszyny Wirtualnej, zobacz [tagu maszyny wirtualnej systemu Windows na platformie Azure](../virtual-machines/windows/tag.md) i [tagu maszyny wirtualnej systemu Linux na platformie Azure](../virtual-machines/linux/tag.md)
2. Modyfikuj harmonogramy **Sequenced-StartVM** i **Sequenced StopVM** do daty i godziny zgodnie z wymaganiami i włączony harmonogram.  

Aby przetestować i sprawdzić poprawności konfiguracji, należy ręcznie uruchomić **SequencedStartStop_Parent** runbook i ustaw *akcji* parametr **start** lub **zatrzymania**  i *WhatIf* parametr **True**.<br><br> ![Konfiguruj parametry elementu runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Dzięki temu można wyświetlić podgląd akcję, która będzie odbywać się i wprowadź zmiany w razie potrzeby, przed wdrożeniem względem maszyn wirtualnych w środowisku produkcyjnym.  Po potrafisz, można ręcznie wykonania elementu runbook z parametrem ustawioną **false** lub programowi Harmonogram automatyzacji **Sequenced-StartVM** i **Sequenced-StopVM**Uruchom automatycznie następującą określonego harmonogramu.  

### <a name="scenario-3-auto-stopstart-vms-across-a-subscription-based-on-cpu-utilization"></a>Scenariusz 3: Stop/Autostart maszyn wirtualnych między subskrypcjami, na podstawie wykorzystania procesora CPU
Aby ułatwić zarządzanie kosztów działających maszyn wirtualnych w ramach subskrypcji, to rozwiązanie może pomóc wyniku obliczenia maszynach wirtualnych platformy Azure, które nie są używane w okresach, poza szczytem, takich jak po godzinach i automatycznie zamykać je Jeśli wykorzystanie procesora jest mniejsza niż x %.  

Domyślnie rozwiązania jest wstępnie skonfigurowana, aby ocenić metryki procent procesora CPU i jeśli średnie wykorzystanie wynosi 5% lub mniej.  To jest kontrolowany przez następujące zmienne i może być modyfikowany, jeśli ich wartości domyślne nie spełniają wymagań:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Można włączyć tylko z celem akcji względem subskrypcji i grupy zasobów lub listę maszyn wirtualnych, ale nie oba.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Akcja zatrzymania w odniesieniu do subskrypcji i zasobu grupy docelowe

1. Skonfiguruj **External_Stop_ResourceGroupNames** i **External_ExcludeVMNames** zmienne do określania docelowych maszyn wirtualnych.  
2. Włącz i zaktualizuj **Schedule_AutoStop_CreateAlert_Parent** harmonogramu.
3. Uruchom **AutoStop_CreateAlert_Parent** elementu runbook o *akcji* ustawiona **start** i *WhatIf* ustawiona  **Wartość true,** Aby przejrzeć wprowadzone zmiany.

#### <a name="target-stop-action-by-vm-list"></a>Akcja zatrzymania docelowego przez listę maszyn wirtualnych

1. Uruchom **AutoStop_CreateAlert_Parent** elementu runbook o *akcji* ustawiona **start**, Dodaj rozdzielana przecinkami lista maszyn wirtualnych w *VMList* parametru i *WhatIf* ustawiona **True** Aby przejrzeć wprowadzone zmiany.  
2. Skonfiguruj **External_ExcludeVMNames** parametru za pomocą przecinka przecinkami lista maszyn wirtualnych (VM1, maszyny VM2, VM3).
3. W tym scenariuszu nie honoruje **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupnames** varabies.  W tym scenariuszu należy Utwórz własny harmonogram automatyzacji. Aby uzyskać więcej informacji, zobacz [Planowanie elementu runbook automatyzacji Azure](../automation/automation-schedules.md).

Teraz, gdy masz harmonogram zatrzymywanie maszyn wirtualnych opartych na wykorzystanie procesora CPU na, musisz włączyć jedną z poniższych harmonogramów, aby je uruchomić.

* Docelowy uruchomienie akcji przez subskrypcji i grupy zasobów.  Zobacz kroki w [scenariusz #1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) do testowania i włączenie **StartVM zaplanowane** harmonogramu.
* Docelowy uruchomienie akcji przez subskrypcji, grupy zasobów oraz tagu.  Zobacz kroki w [scenariusza #2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) do testowania i włączenie "Sequenced-StartVM" harmonogramu.


### <a name="configuring-e-mail-notifications"></a>Konfigurowanie powiadomień e-mail

Aby skonfigurować powiadomienia e-mail, po wdrożeniu rozwiązania, można zmodyfikować następujących trzech zmiennych:

* External_EmailFromAddress — Określ adres e-mail nadawcy
* Lista adresów e-mail oddzielonych External_EmailToAddress - przecinka (user@hotmail.com, user@outlook.com) do odbierania powiadomień e-mail
* External_IsSendEmail — Jeśli ustawiono **tak**, będą otrzymywać wiadomości e-mail i aby wyłączyć powiadomienia e-mail, ustaw wartość na **nr**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Modyfikowanie uruchamiania i wyłączania harmonogramów

Zarządzanie uruchamiania i wyłączania harmonogramów w tym rozwiązaniu opisano te same kroki, zgodnie z opisem w [Planowanie elementu runbook automatyzacji Azure](automation-schedules.md).     

## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics

Usługa Automation tworzy dwa typy rekordów w repozytorium OMS.

### <a name="job-logs"></a>Dzienniki zadań

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


### <a name="job-streams"></a>Strumienie zadania

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

## <a name="sample-log-searches"></a>Przykładowe wyszukiwania dzienników

Poniższa tabela zawiera przykładowe wyszukiwania dzienników dla rekordów dzienników zbieranych przez to rozwiązanie. 

Zapytanie | Opis|
----------|----------|
Znajdź zadania dla elementu runbook ScheduledStartStop_Parent, które zostały ukończone pomyślnie | Wyszukiwanie kategorii == "JobLogs" &#124; gdzie (RunbookName_s == "ScheduledStartStop_Parent") &#124; gdzie (ResultType == "Ukończone") &#124; Podsumuj AggregatedValue = count() ResultType, bin (TimeGenerated, 1h) &#124; Sortuj według TimeGenerated desc|
Znajdź zadania dla elementu runbook SequencedStartStop_Parent, które zostały ukończone pomyślnie | Wyszukiwanie kategorii == "JobLogs" &#124; gdzie (RunbookName_s == "SequencedStartStop_Parent") &#124; gdzie (ResultType == "Ukończone") &#124; Podsumuj AggregatedValue = count() ResultType, bin (TimeGenerated, 1h) &#124; Sortuj według TimeGenerated desc

## <a name="removing-the-solution"></a>Usuwanie rozwiązania

Jeśli zdecydujesz, że nie trzeba używać rozwiązania żadnych dalszych, należy ją usunąć z konta automatyzacji.  Usunięcie rozwiązania spowoduje tylko usunięcie elementów runbook, nie spowoduje usunięcia harmonogramy lub zmiennych, które zostały utworzone, gdy rozwiązanie zostało dodane.  Zasoby, które trzeba usunąć ręcznie, jeśli nie używasz je z innymi elementami runbook.  

Aby usunąć rozwiązanie, wykonaj następujące czynności:

1.  Twoje konto usługi Automatyzacja, zaznacz **obszaru roboczego** w lewym okienku.  
2.  Na **rozwiązań** wybierz rozwiązanie **Start-Stop-VM [obszaru roboczego]**.  Na **VMManagementSolution [obszaru roboczego]** strony, kliknij przycisk menu **usunąć**.<br><br> ![Usuwanie maszyny Wirtualnej Mgmt rozwiązania](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  W **Usuń rozwiązanie** okna, upewnij się, aby usunąć rozwiązania.
4.  Informacje została zweryfikowana i rozwiązanie zostanie usunięty, można śledzić postęp w obszarze **powiadomienia** z menu.  Nastąpi powrót do **rozwiązań** strony po rozpoczęciu procesu usunąć rozwiązania.  

Konto usługi Automatyzacja i obszaru roboczego analizy dzienników nie są usuwane w ramach tego procesu.  Jeśli nie chcesz zachować obszaru roboczego analizy dzienników, należy ręcznie je usunąć.  Można to zrobić również z portalu Azure.   Ekranie głównym w portalu Azure, zaznacz **analizy dzienników** , a następnie na **analizy dzienników** bloku, wybierz obszar roboczy i kliknij przycisk **usunąć** z menu Blok ustawień obszaru roboczego.  
      
## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o sposobie tworzenia różnych zapytań wyszukiwania i sprawdzaniu dzienników zadań usługi Automation przy użyciu usługi Log Analytics, zobacz [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md) (Wyszukiwanie dzienników w usłudze Log Analytics)
- Aby dowiedzieć się więcej o wykonywaniu elementów Runbook, sposobie monitorowania zadań elementów Runbook i innych szczegółach technicznych, zobacz [Track a runbook job](automation-runbook-execution.md) (Śledzenie zadania elementu Runbook)
- Aby dowiedzieć się więcej na temat analizy dzienników i źródeł danych kolekcji, zobacz [Azure zbierania danych magazynu w omówieniu analizy dzienników](../log-analytics/log-analytics-azure-storage.md)






   

