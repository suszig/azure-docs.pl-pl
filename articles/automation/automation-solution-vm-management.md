---
title: "Maszyny wirtualne uruchamiania i zatrzymywania podczas rozwiązania poza godzinami szczytu (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "To rozwiązanie do zarządzania maszyna wirtualna uruchamia i zatrzymuje maszynach wirtualnych Azure Resource Manager zgodnie z harmonogramem i aktywnie monitoruje z analizy dzienników."
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
ms.date: 12/18/2017
ms.author: magoedte
ms.openlocfilehash: 7ffd424de2a7224b5ac50fa228289c5397092b2e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Maszyny wirtualne uruchamiania i zatrzymywania podczas rozwiązania poza godzinami szczytu (wersja zapoznawcza) w usłudze Automatyzacja Azure

Maszyny wirtualne uruchamiania i zatrzymywania godzinami rozwiązania uruchamia i zatrzymuje maszynach wirtualnych Azure harmonogramów zdefiniowane przez użytkownika, zawiera informacje na temat technologii za pośrednictwem usługi Analiza dzienników Azure i wysyła opcjonalne wiadomości e-mail przy użyciu [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Obsługuje ona zarówno usługi Azure Resource Manager i klasycznych maszyn wirtualnych dla większości scenariuszy. 

To rozwiązanie zapewnia opcję zdecentralizowane automatyzacji dla użytkowników, którzy mają zmniejszyć koszty ich przy użyciu zasobów niekorzystającą, niskich kosztach. W tym rozwiązaniu można:

* Planowanie maszyn wirtualnych, aby uruchomić i zatrzymać.
* Planowanie maszyn wirtualnych, aby uruchomić i zatrzymać w kolejności rosnącej przy użyciu tagów Azure (nieobsługiwane w przypadku klasycznych maszyn wirtualnych).
* Auto, aby zatrzymać maszyn wirtualnych na podstawie niski użycia procesora CPU.

## <a name="prerequisites"></a>Wymagania wstępne

- Elementy Runbook działają przy użyciu [konta Uruchom jako platformy Azure](automation-offering-get-started.md#authentication-methods).  Konto Uruchom jako jest preferowaną metodą uwierzytelniania, ponieważ używa certyfikatu uwierzytelniania zamiast hasła, które może wygaśnie lub często zmieniana.  

- To rozwiązanie umożliwia zarządzanie tylko maszyny wirtualne, które znajdują się w tej samej subskrypcji co konto usługi Automatyzacja Azure.  

- To rozwiązanie jest wdrożyć tylko w następujących regionach platformy Azure: Australia Południowo-Wschodnia, Kanada centralnej, Indie środkowe, wschodnie stany USA, Japonia Wschodnia, Azja południowo-wschodnia, Wielka Brytania Południowa i Europa Zachodnia.  
    
    > [!NOTE]
    > Elementy runbook, zarządzanie harmonogramem maszyny Wirtualnej można kierować maszyn wirtualnych w dowolnym regionie.  

- Do wysyłania powiadomień e-mail po zakończeniu uruchamianie i zatrzymywanie elementów runbook maszyny Wirtualnej, podczas dołączania z portalu Azure Marketplace, wybierz **tak** SendGrid wdrażania. 

    > [!IMPORTANT]
    > SendGrid jest usługi innej firmy. Aby uzyskać pomoc, skontaktuj się z [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Ograniczenia sendgrid są:

    * Maksymalnie jedno konto SendGrid na użytkownika na subskrypcję.
    * Maksymalnie dwóch kont SendGrid na subskrypcję.

Jeśli wdrożono poprzedniej wersji tego rozwiązania, należy najpierw usunąć go z Twojego konta przed wdrożeniem tej wersji.  

## <a name="solution-components"></a>Składniki rozwiązania

To rozwiązanie obejmuje wstępnie skonfigurowane elementów runbook, harmonogramów i integracja z usługą analizy dzienników, więc można dostosować sposób uruchamiania i wyłączania maszyn wirtualnych, w zależności od potrzeb biznesowych. 

### <a name="runbooks"></a>Elementy Runbook

W poniższej tabeli wymieniono elementy runbook wdrożonych na koncie automatyzacji.  Nie należy wprowadzać zmian kod elementu runbook. Zamiast tego należy zapisać własnego elementu runbook dla nowych funkcji.

> [!IMPORTANT]
> Nie należy bezpośrednio uruchamiać żadnych elementów runbook z "podrzędny" dołączonym do jego nazwy.
>

Obejmują wszystkie nadrzędne elementy runbook *WhatIf* parametru. Wartość **True**, *WhatIf* obsługuje określające zachowanie dokładne wykonuje element runbook podczas uruchamiania bez *WhatIf* parametru i sprawdza poprawność poprawny są maszyny wirtualne docelowe.  Element runbook tylko wykonuje działania zdefiniowane podczas *WhatIf* ustawiono parametr **False**. 

|**Runbook** | **Parametry** | **Opis**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Wywołana tylko z nadrzędnego elementu runbook. Tworzy alerty na podstawie ciągu zasobu dla scenariusza AutoStop.| 
|AutoStop_CreateAlert_Parent | WhatIf: Wartość PRAWDA lub FAŁSZ <br> VMList | Tworzy lub aktualizuje Azure reguł alertów na maszynach wirtualnych w grupach docelowych subskrypcja lub zasób. <br> VMList: Rozdzielana przecinkami lista maszyn wirtualnych.  Na przykład *vm1 maszyny vm2, vm3*.| 
|AutoStop_Disable | brak | Wyłącza alerty AutoStop i domyślnego harmonogramu.| 
|AutoStop_StopVM_Child | WebHookData | Wywołana tylko z nadrzędnego elementu runbook. Reguły alertów wywołanie tego elementu runbook można zatrzymać maszyny Wirtualnej.|  
|Bootstrap_Main | brak | Jednorazowo można skonfigurować bootstrap konfiguracje, takie jak webhookURI, które nie są zwykle dostępne z usługi Azure Resource Manager. Ten element runbook zostanie usunięta automatycznie po pomyślnym wdrożeniu.|  
|ScheduledStartStop_Child | VMName <br> Akcja: Zatrzymywania lub uruchamiania <br> ResourceGroupName | Wywołana tylko z nadrzędnego elementu runbook. Wykonuje zatrzymanie lub uruchomienie na zatrzymanie zaplanowane.|  
|ScheduledStartStop_Parent | Akcja: Zatrzymywania lub uruchamiania <br> WhatIf: Wartość PRAWDA lub FAŁSZ | Ma to wpływ na wszystkie maszyny wirtualne w subskrypcji. Edytuj **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupNames** można wykonać tylko na tych grup zasobów obiektu docelowego. Można też wykluczyć określone maszyn wirtualnych, aktualizując **External_ExcludeVMNames** zmiennej. *WhatIf* działa tak samo jak inne elementy runbook.|  
|SequencedStartStop_Parent | Akcja: Zatrzymywania lub uruchamiania <br> WhatIf: Wartość PRAWDA lub FAŁSZ | Utwórz znaczniki o nazwie **SequenceStart** i **SequenceStop** na każdej maszynie Wirtualnej, dla którego chcesz działania uruchamiania/zatrzymywania sekwencji. Wartość tagu powinna być dodatnią liczbą całkowitą (1, 2, 3) umożliwiająca kolejność, w którym chcesz uruchomić lub zatrzymać. *WhatIf* działa tak samo jak inne elementy runbook. <br> **Uwaga**: maszyny wirtualne muszą się znajdować w zdefiniowany jako External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames i External_ExcludeVMNames w automatyzacji Azure zmiennych grupy zasobów. Muszą mieć odpowiednie znaczniki do wykonywania działań zaczęły obowiązywać.|

### <a name="variables"></a>Zmienne

W poniższej tabeli przedstawiono zmienne utworzone na Twoim koncie automatyzacji.  Należy modyfikować tylko zmienne prefiksem **zewnętrznych**. Modyfikowanie zmiennych prefiksem **wewnętrzne** powoduje, że niepożądane skutki.  

|**Zmienna** | **Opis**|
---------|------------|
|External_AutoStop_Condition | Operator warunkowy wymagane do skonfigurowania warunek przed wyzwolenie alertu. Dopuszczalne wartości to **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, i **LessThanOrEqual**.|  
|External_AutoStop_Description | Alert zatrzymania maszyny Wirtualnej, jeśli procent użycia procesora CPU przekracza wartość progową.|  
|External_AutoStop_MetricName | Nazwa metryki wydajności, dla którego ma zostać skonfigurowana Azure alertu.| 
|External_AutoStop_Threshold | Próg alertu Azure reguły określone w zmiennej *External_AutoStop_MetricName*. Wartości procentowe mogą należeć do zakresu od 1 do 100.|  
|External_AutoStop_TimeAggregationOperator | Operator agregacji czasu, która jest stosowana do rozmiaru wybranego okna Próba sprawdzenia warunku. Dopuszczalne wartości to **średni**, **Minimum**, **maksymalna**, **całkowita**, i **ostatniego**.|  
|External_AutoStop_TimeWindow | Rozmiar okna, w którym Azure analizuje wybranego metryki służącą do wyzwalania alertu. Ten parametr akceptuje dane wejściowe w formacie timespan. Możliwe wartości to od 5 minut do 6 godzin.|  
|External_EmailFromAddress | Określa nadawcy wiadomości e-mail.|  
|External_EmailSubject | Określa tekst dla wiersza tematu wiadomości e-mail.|  
|External_EmailToAddress | Określa adresatów wiadomości e-mail. Rozdziel nazwy za pomocą przecinka.|  
|External_ExcludeVMNames | Wprowadź nazw maszyn wirtualnych, które mają zostać wykluczone, oddzielając nazwy za pomocą przecinka nie może zawierać spacji.|  
|External_IsSendEmail | Określa opcje do wysyłania powiadomień e-mail po zakończeniu.  Określ **tak** lub **nr** nie wysłać wiadomości e-mail.  Ta opcja powinna być **nr** Jeśli powiadomienia e-mail nie została włączona podczas pierwszego wdrożenia.|  
|External_Start_ResourceGroupNames | Określa co najmniej jedną grupę zasobów, oddzielanie wartości przecinkami, przeznaczony dla działania uruchamiania.|  
|External_Stop_ResourceGroupNames | Określa co najmniej jedną grupę zasobów, oddzielanie wartości za pomocą przecinka, przeznaczony dla akcji zatrzymania.|  
|Internal_AutomationAccountName | Określa nazwę konta automatyzacji.|  
|Internal_AutoSnooze_WebhookUri | Określa, że identyfikator URI elementu Webhook wywołana dla tego scenariusza AutoStop.|  
|Internal_AzureSubscriptionId | Określa identyfikator subskrypcji platformy Azure.|  
|Internal_ResourceGroupName | Określa nazwę grupy zasobów konta automatyzacji.|  
|Internal_SendGridAccountName | Określa nazwę konta SendGrid.|  
|Internal_SendGridPassword | Określa hasło konta SendGrid.|  

<br>

We wszystkich scenariuszach **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, i **External_ExcludeVMNames** zmienne są wymagane przeznaczony do maszyn wirtualnych, z wyjątkiem podając rozdzielaną przecinkami listę maszyn wirtualnych dla **AutoStop_CreateAlert_Parent** elementu runbook. Oznacza to maszyn wirtualnych musi znajdować się w grup zasobów obiektu docelowego, aby rozpocząć i zatrzymać operacje. Logika działa nieco jak Azure zasad można grupy zasobów lub subskrypcji, i akcji dziedziczone przez nowo utworzony maszyn wirtualnych. Takie podejście pozwala uniknąć konieczności obsługi oddzielnych harmonogramu dla każdej maszyny Wirtualnej i Zarządzaj uruchamia i zatrzymuje w skali.

### <a name="schedules"></a>Harmonogramy

Poniższa tabela zawiera listę domyślnych planów utworzone na Twoim koncie automatyzacji.  Można je zmodyfikować lub utworzyć własne niestandardowe harmonogramy.  Domyślnie każdy z tych wyłączone z wyjątkiem **Scheduled_StartVM** i **Scheduled_StopVM**.

Nie należy włączać wszystkie harmonogramy, ponieważ może to powodować nakładające się akcje harmonogramu. Najlepiej określić optymalizacji, które chcesz wykonać i zmodyfikuj odpowiednio.  Zobacz przykładowe scenariusze, w sekcji Przegląd, aby uzyskać dokładniejsze objaśnienie.   

|**Nazwa harmonogramu** | **Częstotliwość** | **Opis**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Co 8 godzin | Uruchamia element runbook AutoStop_CreateAlert_Parent co 8 godzin, co z kolei uniemożliwia wartości na podstawie maszyny Wirtualnej w External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames i External_ExcludeVMNames w zmiennych automatyzacji Azure.  Alternatywnie można określić rozdzielaną przecinkami listę maszyn wirtualnych za pomocą parametru VMList.|  
|Scheduled_StopVM | Zdefiniowane przez codziennie użytkownika | Uruchamia element runbook Scheduled_Parent z parametrem *zatrzymać* każdego dnia o określonej godzinie.  Automatycznie zatrzymuje wszystkie maszyny wirtualne, które spełniają reguły zdefiniowane przez zmienne zasobów. Należy włączyć pokrewne harmonogram **StartVM zaplanowane**.|  
|Scheduled_StartVM | Zdefiniowane przez codziennie użytkownika | Uruchamia element runbook Scheduled_Parent z parametrem *Start* każdego dnia o określonej godzinie.  Wszystkie maszyny wirtualne, które spełniają reguły zdefiniowane przez odpowiednie zmienne jest uruchamiana automatycznie.  Należy włączyć pokrewne harmonogram **StopVM zaplanowane**.|
|StopVM sekwencjonowania | 1:00:00 (UTC), każdy piątek | Uruchamia element runbook Sequenced_Parent z parametrem *zatrzymać* każdy piątek o określonej godzinie.  Sekwencyjnie (rosnąco) zatrzymuje wszystkich maszyn wirtualnych przy użyciu tagu z **SequenceStop** wynika z odpowiednich zmiennych.  Zapoznaj się z sekcją elementów Runbook, aby uzyskać więcej informacji o wartości tagów i zmienne zasobów.  Należy włączyć pokrewne harmonogram **Sequenced StartVM**.|
|StartVM sekwencjonowania | 1:00 PM (UTC), każdy poniedziałek | Uruchamia element runbook Sequenced_Parent z parametrem *Start* w każdy poniedziałek o określonej godzinie. Sekwencyjnie (malejąco) uruchamiania wszystkich maszyn wirtualnych przy użyciu tagu z **SequenceStart** wynika z odpowiednich zmiennych.  Zapoznaj się z sekcją elementów Runbook, aby uzyskać więcej informacji o wartości tagów i zmienne zasobów.  Należy włączyć pokrewne harmonogram **Sequenced StopVM**.|

<br>

## <a name="configuration"></a>Konfigurowanie

Wykonaj poniższe kroki, aby dodać uruchamiania/zatrzymywania maszyn wirtualnych podczas rozwiązania poza godzinami szczytu na koncie automatyzacji, a następnie skonfiguruj zmienne, aby dostosować rozwiązania.

1. W portalu Azure kliknij **Utwórz zasób**.<br> ![Azure portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. W okienku Marketplace wpisz słowo kluczowe, taką jak **Start** lub **uruchamiania i zatrzymywania**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Alternatywnie można wpisz słowa kluczowe co najmniej jeden z pełną nazwę rozwiązania i naciśnij klawisz Enter.  Wybierz **uruchamiania/zatrzymywania maszyn wirtualnych w godzinach [Podgląd]** w wynikach wyszukiwania.  
3. W **uruchamiania/zatrzymywania maszyn wirtualnych w godzinach [Podgląd]** okienko dla wybranego rozwiązania, sprawdź informacje, a następnie kliknij przycisk **Utwórz**.  
4. **Dodaj rozwiązanie** pojawi się okienko. Monit o skonfigurowanie rozwiązania, przed zaimportowaniem go w ramach subskrypcji automatyzacji.<br><br> ![Zarządzanie maszynami wirtualnymi — blok Dodawanie rozwiązania](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Na **Dodaj rozwiązanie** okienku wybierz **obszaru roboczego**. Wybierz obszar roboczy OMS jest połączony z tej samej subskrypcji Azure, która jest konta automatyzacji. Jeśli nie masz obszaru roboczego wybierz **Utwórz nowy obszar roboczy**. Na **obszarem roboczym pakietu OMS** okienko, wykonaj następujące czynności: 
   - Określ nazwę dla nowego **Obszaru roboczego OMS**.
   - Wybierz **subskrypcji** się połączyć, wybierając z listy rozwijanej, jeśli wybrana domyślnie nie jest odpowiedni.
   - Aby uzyskać **grupy zasobów**, możesz utworzyć nową grupę zasobów lub wybierz istniejący.  
   - Wybierz **lokalizację**.  Obecnie jedynymi lokalizacjami, dostępne są **południowo-wschodnia Australia**, **Kanada centralnej**, **Indie środkowe**, **wschodnie stany USA**, **Japonia Wschodnia**, **Azja południowo-wschodnia**, **Wielka Brytania Południowa**, i **Europa**.
   - Wybierz **warstwę cenową**.  Rozwiązanie oferuje dwie warstwy: Zwolnij i płatnej OMS.  Warstwa bezpłatna ma ograniczenie ilości zbieranych danych codziennie, okres przechowywania i minut czasu wykonywania zadania elementu runbook.  W warstwie płatnej usługi OMS nie stosuje się dziennego limitu ilości zbieranych danych.  

        > [!NOTE]
        > Mimo że warstwy na GB (autonomiczna) płatnej jest wyświetlany jako opcja, nie ma zastosowania.  Jeśli zaznacz go i kontynuować tworzenie tego rozwiązania, w ramach subskrypcji, zwróci błąd.  Ten problem zostanie rozwiązany po oficjalnym wydaniu tego rozwiązania.<br>To rozwiązanie tylko używa automatyzacji zadań minut i wprowadzanie dziennika.  Dodatkowe węzły OMS nie dodaje do środowiska.  

6. Po podaniu wymaganych informacji w **obszarem roboczym pakietu OMS** okienku, kliknij przycisk **Utwórz**.  Można śledzić postęp w obszarze **powiadomienia** z menu, która zwraca do **Dodaj rozwiązanie** okienko po zakończeniu.  
7. Na **Dodaj rozwiązanie** okienku wybierz **konto automatyzacji**.  Jeśli tworzysz nowy obszar roboczy OMS, należy także utworzyć nowe konto automatyzacji ma zostać skojarzony z nim.  Wybierz **utworzyć konto usługi automatyzacja**i na **konto automatyzacji dodać** okienku zapewnia następujące korzyści: 
  - W polu **Nazwa** wprowadź nazwę konta usługi Automation.

    Inne opcje są automatycznie wypełniane oparte na obszar roboczy OMS wybrane. Nie można modyfikować tych opcji.  Konto Uruchom jako platformy Azure jest domyślną metodą uwierzytelniania dla elementów Runbook zawartych w tym rozwiązaniu.  Po kliknięciu **OK**, opcje konfiguracji są weryfikowane i utworzeniu konta automatyzacji.  Postęp możesz śledzić w sekcji **Powiadomienia** z poziomu menu. 

    W przeciwnym razie możesz wybrać istniejące konto Uruchom jako usługi Automation.  Należy pamiętać, że wybrane konto już nie można połączyć z innym obszarem roboczym pakietu OMS. Jeśli jest już połączony, komunikat o błędzie i wybierz inne konto Uruchom jako automatyzacji lub Utwórz nową.<br><br> ![Konto usługi Automation, które jest już połączone z obszarem roboczym OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Na koniec na **Dodaj rozwiązanie** okienku wybierz **konfiguracji**. **Parametry** pojawi się okienko.<br><br> ![Okienko parametrów dla rozwiązania](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  W tym miejscu zostanie wyświetlony monit o:  
   - Określ **docelowa grupa zasobów nazwy**. Są to nazwy grup zasobów, które zawierają maszyny wirtualne mają być zarządzane przy użyciu tego rozwiązania.  Można wprowadzić więcej niż jedną nazwę i oddzielić od siebie za pomocą przecinka (wartości nie jest uwzględniana wielkość liter).  Użycie symbolu wieloznacznego jest obsługiwane. Możesz skorzystać z tej opcji, jeśli chcesz uwzględnić maszyny wirtualne we wszystkich grupach zasobów w subskrypcji.
   - Określ **listę wykluczyć maszyny Wirtualnej (ciąg)**. Jest to nazwa co najmniej jednej maszyny wirtualnej, docelowa grupa zasobów.  Można wprowadzić więcej niż jedną nazwę i oddzielić od siebie za pomocą przecinka (wartości nie jest uwzględniana wielkość liter).  Przy użyciu symboli wieloznacznych jest obsługiwana.
   - Wybierz **harmonogram**. Jest to cykliczne datę i godzinę, uruchamianie i zatrzymywanie maszyn wirtualnych w docelowej grupy zasobów.  Domyślnie harmonogramu jest skonfigurowany zgodnie ze strefą czasową UTC. Wybrać inny region nie jest dostępna.  Aby skonfigurować harmonogram określonej strefy czasowej po skonfigurowaniu rozwiązania, zobacz [modyfikowanie harmonogramu uruchamiania i wyłączania](#modifying-the-startup-and-shutdown-schedule).
   - Aby otrzymywać **wiadomości E-mail z powiadomieniami** z SendGrid, zaakceptuj wartość domyślną **tak** i podaj prawidłowy adres e-mail. W przypadku wybrania **nr** , ale później zdecydować, czy chcesz otrzymywać powiadomienia pocztą e-mail, możesz zaktualizować **External_EmailToAddress** zmiennej z prawidłowych adresów e-mail oddzielonych przecinkami, a następnie Modyfikowanie zmiennej **External_IsSendEmail** z wartością **tak**.  

9. Po skonfigurowaniu początkowego ustawienia wymagane dla rozwiązania, wybierz **Utwórz**.  Po zweryfikowaniu wszystkich ustawień rozwiązanie jest wdrożone do subskrypcji.  Ten proces może potrwać kilka sekund, aby zakończyć, a można śledzić postęp w obszarze **powiadomienia** z menu. 

## <a name="collection-frequency"></a>Częstotliwość zbierania

Automatyzacja zadania dziennika i zadania strumienia danych jest pozyskanych w repozytorium analizy dzienników co 5 minut.  

## <a name="using-the-solution"></a>Użycie rozwiązania

Po dodaniu rozwiązania do zarządzania maszyny Wirtualnej, **StartStopVMView** kafelka zostanie dodany do pulpitu nawigacyjnego w swoim obszarze roboczym analizy dzienników z portalu Azure.  Tego kafelka Wyświetla graficzną reprezentację zadań elementu runbook dla rozwiązania, które zostały uruchomione i zakończyło się pomyślnie i liczba.<br><br> ![Zarządzanie maszynami wirtualnymi — kafelek Widok StartStopVM](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Na koncie automatyzacji można uzyskać dostęp i zarządzać nimi rozwiązania, wybierając **obszaru roboczego** opcji. Na stronie analizy dzienników wybierz **rozwiązań** w lewym okienku.  Na **rozwiązań** wybierz rozwiązanie **Start-Stop-VM [obszaru roboczego]** z listy.<br><br> ![Lista rozwiązań w analizy dzienników](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Wybranie rozwiązania powoduje wyświetlenie **Start-Stop-VM [obszaru roboczego]** okienko rozwiązania. W tym miejscu można przejrzeć ważne informacje takie jak **StartStopVM** kafelka. Jak obszaru roboczego analizy dzienników tego kafelka Wyświetla graficzną reprezentację zadań elementu runbook dla rozwiązania, które zostały uruchomione i zakończyły się pomyślnie i liczba.<br><br> ![Strona rozwiązania Automatyzacja zarządzania aktualizacjami](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

W tym miejscu można wykonywać dalszej analizy rekordów zadań, klikając Kafelek pierścień. Pulpit nawigacyjny rozwiązania zawiera historię zadania i wstępnie zdefiniowane zapytania wyszukiwania dziennika. Przełącz się do portalu zaawansowane analizy dziennika do wyszukiwania oparte na zapytania wyszukiwania.  

Obejmują wszystkie nadrzędne elementy runbook *WhatIf* parametru. Jeśli wartość **True**, ten obsługuje określające zachowanie dokładne wykonuje element runbook, gdy uruchamiane bez *WhatIf* parametr który sprawdza poprawność docelowej poprawne maszyn wirtualnych. Elementy Runbook tylko akcje ich zdefiniowane podczas *WhatIf* ustawiono parametr **False**.  


### <a name="scenario-1-daily-startstop-vms-across-a-subscription-or-target-resource-groups"></a>Scenariusz 1: Codziennie uruchamiania/zatrzymywania maszyn wirtualnych w ramach subskrypcji lub docelowych grup zasobów 

To jest konfiguracja domyślna najpierw wdrażania rozwiązania.  Na przykład można skonfigurować jego zatrzymanie wszystkich maszyn wirtualnych w ramach subskrypcji, po opuszczeniu pracy wieczorem i uruchom je w nocy, podczas pracy w biurze. Po skonfigurowaniu harmonogramy **zaplanowane-StartVM** i **StopVM zaplanowane** podczas wdrażania, uruchomić i zatrzymać docelowych maszyn wirtualnych.
>[!NOTE]
>Strefa czasowa jest bieżącej strefy czasowej po skonfigurowaniu harmonogramu parametru czasu. Jednak jest on przechowywany w formacie UTC automatyzacji Azure.  Nie trzeba wykonywać żadnych konwersji strefy czasowej odbywa się to podczas wdrażania.

Możesz kontrolować, które maszyny wirtualne są w zakresie przez skonfigurowanie następujących zmiennych: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, i **External_ ExcludeVMNames**.  

>[!NOTE]
> Wartość **nazw grupa zasobów docelowych** jest przechowywana jako wartość dla obu **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupNames**. Dla dalszego szczegółowości można zmodyfikować każdą z tych zmiennych pod kątem różnych grup zasobów.  Do rozpoczęcia działania, użyj **External_Start_ResourceGroupNames**i zatrzymania działania, użyj **External_Stop_ResourceGroupNames**. Maszyny wirtualne są automatycznie dodawane do uruchomienia i zatrzymania harmonogramów.

Aby przetestować i sprawdzić poprawności konfiguracji, należy ręcznie uruchomić **ScheduledStartStop_Parent** runbook i ustaw dla parametru akcji **start** lub **zatrzymać** i WHATIF Parametr **True**.<br><br> ![Konfiguruj parametry elementu runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)


 Wyświetl podgląd zaplanowanego działania i wprowadź niezbędne zmiany przed zaimplementowaniem względem maszyn wirtualnych w środowisku produkcyjnym.  Element runbook można wykonywać ręcznie za pomocą parametru ustawioną **False**, lub pozwolić programowi Harmonogram automatyzacji **harmonogram-StartVM** i **StopVM harmonogram** Uruchom automatycznie po określonego harmonogramu.

### <a name="scenario-2-sequence-the-startstop-vms-across-a-subscription-by-using-tags"></a>Scenariusz 2: Sekwencji uruchamiania/zatrzymywania maszyn wirtualnych w ramach subskrypcji przy użyciu tagów
W środowisku zawierającym co najmniej dwóch składników na wiele maszyn wirtualnych, obsługa obciążeń rozproszonych ważne jest Obsługa sekwencji, w którym uruchamiania i zatrzymywania elementów w kolejności.  Można to zrobić, wykonując następujące czynności:

1. Dodaj **SequenceStart** i **SequenceStop** tag z dodatnią liczbę całkowitą na maszynach wirtualnych, które są stosowane w **External_Start_ResourceGroupNames** i  **External_Stop_ResourceGroupNames** zmiennych.  Uruchamianie i zatrzymywanie działania są wykonywane w kolejności rosnącej.  Aby dowiedzieć się, jak znacznik maszyny Wirtualnej, zobacz [tagu maszyny wirtualnej systemu Windows na platformie Azure](../virtual-machines/windows/tag.md) i [tagu maszyny wirtualnej systemu Linux na platformie Azure](../virtual-machines/linux/tag.md).
2. Modyfikuj harmonogramy **Sequenced-StartVM** i **Sequenced StopVM** datę i godzinę, które spełniają wymagania i włączony harmonogram.  

Aby przetestować i sprawdzić poprawności konfiguracji, należy ręcznie uruchomić **SequencedStartStop_Parent** elementu runbook. Ustaw dla parametru akcji **start** lub **zatrzymać** i parametr WHATIF **True**.<br><br> ![Konfiguruj parametry elementu runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Przeglądanie akcji i wprowadź niezbędne zmiany przed zaimplementowaniem względem maszyn wirtualnych w środowisku produkcyjnym.  Podczas gotowe, ręcznie wykonywania elementu runbook za pomocą parametru ustawioną **False**, lub programowi Harmonogram automatyzacji **Sequenced-StartVM** i **Sequenced StopVM** Uruchom automatycznie po określonego harmonogramu.  

### <a name="scenario-3-automate-startstop-vms-across-a-subscription-based-on-cpu-utilization"></a>Scenariusz 3: Zautomatyzować uruchamiania/zatrzymywania maszyn wirtualnych między subskrypcjami, na podstawie wykorzystania procesora CPU
To rozwiązanie może ułatwić zarządzanie kosztów maszyn wirtualnych działających w Twojej subskrypcji, oceniając maszynach wirtualnych platformy Azure, które nie są używane w okresach, poza szczytem, takich jak po godzinach i automatycznego zamykania ich Jeśli wykorzystanie procesora jest mniej niż x %.  

Domyślnie rozwiązania jest wstępnie skonfigurowana do oceny Metryka procent procesora CPU czy średnie wykorzystanie jest 5 procent lub mniej.  To jest kontrolowany przez następujące zmienne i może być modyfikowany, jeśli wartości domyślne nie spełniają wymagań:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Można włączyć przeznaczonych dla akcji względem subskrypcji i grupy zasobów, lub przeznaczonych dla określonej listy maszyn wirtualnych, ale nie oba.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Akcja zatrzymania w odniesieniu do subskrypcji i zasobu grupy docelowe

1. Skonfiguruj **External_Stop_ResourceGroupNames** i **External_ExcludeVMNames** zmienne do określania docelowych maszyn wirtualnych.  
2. Włącz i zaktualizuj **Schedule_AutoStop_CreateAlert_Parent** harmonogramu.
3. Uruchom **AutoStop_CreateAlert_Parent** runbook przy użyciu parametru ACTION ustawioną **start** i ustawić parametr WHATIF **True** Aby przejrzeć wprowadzone zmiany.

#### <a name="target-the-stop-action-by-vm-list"></a>Docelowy Akcja zatrzymania przez listę maszyn wirtualnych

1. Uruchom **AutoStop_CreateAlert_Parent** runbook przy użyciu parametru ACTION ustawioną **start**, dodać rozdzielaną przecinkami listę maszyn wirtualnych w *VMList* parametr, a następnie ustawić Parametr WHATIF **True**. Podgląd zmian.  
2. Skonfiguruj **External_ExcludeVMNames** parametru rozdzielaną przecinkami listę maszyn wirtualnych (VM1, maszyny VM2, VM3).
3. W tym scenariuszu nie honoruje **External_Start_ResourceGroupNames** i **External_Stop_ResourceGroupnames** zmiennych.  W tym scenariuszu należy utworzyć własny harmonogram automatyzacji. Aby uzyskać więcej informacji, zobacz [Planowanie elementu runbook automatyzacji Azure](../automation/automation-schedules.md).

Teraz, gdy masz harmonogram zatrzymywanie maszyn wirtualnych na podstawie wykorzystania procesora CPU, musisz włączyć jedną z następujących harmonogramów, aby je uruchomić.

* Docelowy uruchomienie akcji przez subskrypcji i grupy zasobów.  Zobacz kroki w [scenariusz 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) do testowania i włączenie **StartVM zaplanowane** harmonogramów.
* Docelowy uruchomienie akcji przez subskrypcji, grupy zasobów i tagów.  Zobacz kroki w [scenariuszu 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) do testowania i włączenie **Sequenced StartVM** harmonogramów.


### <a name="configuring-email-notifications"></a>Konfigurowanie powiadomień e-mail

Aby skonfigurować powiadomienia e-mail, po wdrożeniu rozwiązania, zmodyfikuj trzy następujące zmienne:

* External_EmailFromAddress: Określ adres e-mail nadawcy.
* External_EmailToAddress: Określ listę adresów e-mail rozdzielana przecinkami (user@hotmail.com, user@outlook.com) do odbierania wiadomości e-mail z powiadomieniem.
* External_IsSendEmail: Ustaw **tak** do odbierania wiadomości e-mail. Aby wyłączyć powiadomienia e-mail, ustaw wartość na **nr**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Modyfikowanie uruchamiania i wyłączania harmonogramów

Zarządzanie uruchamiania i wyłączania harmonogramów w tym rozwiązaniu opisano te same kroki, zgodnie z opisem w [Planowanie elementu runbook automatyzacji Azure](automation-schedules.md).     

## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics

Automatyzacja tworzy dwa typy rekordów w repozytorium OMS: zadania dzienniki i strumieni zadania.

### <a name="job-logs"></a>Dzienniki zadań

Właściwość | Opis|
----------|----------|
Obiekt wywołujący |  Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań.|
Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobLogs.|
CorrelationId | Identyfikator GUID, który jest identyfikatorem korelacji zadania elementu Runbook.|
JobId | Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook.|
operationName | Określa typ operacji wykonywanej na platformie Azure.  Do automatyzacji wartość jest zadanie.|
resourceId | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
ResourceGroup | Określa nazwę grupy zasobów zadania elementu Runbook.|
ResourceProvider | Określa nazwę usługi platformy Azure, która zapewnia zasoby do wdrożenia i zarządzania.  W przypadku usługi Automation wartością jest Azure Automation.|
ResourceType | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
resultType | Stan zadania elementu Runbook.  Możliwe wartości:<br>— Uruchomione<br>— Zatrzymane<br>— Wstrzymane<br>— Nie powiodło się<br>— Powiodło się|
resultDescription | Opisuje stan wyniku zadania elementu Runbook.  Możliwe wartości:<br>— Zadanie jest uruchomione<br>— Zadanie nie powiodło się<br>— Zadanie zostało ukończone|
RunbookName | Określa nazwę elementu Runbook.|
SourceSystem | Określa system źródłowy dla przesłanych danych.  W przypadku automatyzacji wartość jest OpsManager|
StreamType | Określa typ zdarzenia. Możliwe wartości:<br>— Pełne<br>— Dane wyjściowe<br>— Błąd<br>— Ostrzeżenie|
SubscriptionId | Określa identyfikator subskrypcji zadania.
Time | Data i godzina dla wykonania zadania elementu Runbook.|


### <a name="job-streams"></a>Strumienie zadania

Właściwość | Opis|
----------|----------|
Obiekt wywołujący |  Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań.|
Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobStreams.|
JobId | Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook.|
operationName | Określa typ operacji wykonywanej na platformie Azure.  Do automatyzacji wartość jest zadanie.|
ResourceGroup | Określa nazwę grupy zasobów zadania elementu Runbook.|
resourceId | Określa identyfikator zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
ResourceProvider | Określa nazwę usługi platformy Azure, która zapewnia zasoby do wdrożenia i zarządzania.  W przypadku usługi Automation wartością jest Azure Automation.|
ResourceType | Określa typ zasobu na platformie Azure.  W przypadku usługi Automation wartością jest konto usługi Automation skojarzone z elementem Runbook.|
resultType | Wynik zadania elementu Runbook w czasie wygenerowania zdarzenia. Możliwa wartość to:<br>— W toku|
resultDescription | Obejmuje strumień wyjściowy z elementu Runbook.|
RunbookName | Nazwa elementu Runbook.|
SourceSystem | Określa system źródłowy dla przesłanych danych.  Do automatyzacji wartość jest OpsManager.|
StreamType | Typ strumienia zadania. Możliwe wartości:<br>-Postępu<br>— Dane wyjściowe<br>— Ostrzeżenie<br>— Błąd<br>— Debugowanie<br>— Pełne|
Time | Data i godzina dla wykonania zadania elementu Runbook.|

Po wykonaniu wyszukiwania dziennika zwracające rekordów kategorii **JobLogs** lub **JobStreams**, można wybrać **JobLogs** lub **JobStreams**widoku, który wyświetla zestaw kafelków podsumowania zwrócona przez wyszukiwanie aktualizacji.

## <a name="sample-log-searches"></a>Przykładowe wyszukiwania dzienników

Poniższa tabela zawiera przykładowe wyszukiwania dzienników dla rekordów dzienników zbieranych przez to rozwiązanie. 

Zapytanie | Opis|
----------|----------|
Znajdź zadania dla elementu runbook ScheduledStartStop_Parent, które zakończyły się pomyślnie | Wyszukiwanie kategorii == "JobLogs" &#124; gdzie (RunbookName_s == "ScheduledStartStop_Parent") &#124; gdzie (ResultType == "Ukończone") &#124; Podsumuj AggregatedValue = count() ResultType, bin (TimeGenerated, 1h) &#124; Sortuj według TimeGenerated desc|
Znajdź zadania dla elementu runbook SequencedStartStop_Parent, które zakończyły się pomyślnie | search Category == "JobLogs" &#124; where ( RunbookName_s == "SequencedStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc

## <a name="removing-the-solution"></a>Usuwanie rozwiązania

Jeśli zdecydujesz, że nie trzeba używać rozwiązania, należy ją usunąć z konta automatyzacji.  Usuwanie rozwiązania spowoduje usunięcie tylko elementy runbook. Nie powoduje usunięcia harmonogramy lub zmiennych, które zostały utworzone, gdy rozwiązanie zostało dodane.  Zasoby, które należy usunąć ręcznie, jeśli nie używasz je z innymi elementami runbook.  

Aby usunąć rozwiązanie, wykonaj następujące czynności:

1.  Twoje konto usługi Automatyzacja, zaznacz **obszaru roboczego** w lewym okienku.  
2.  Na **rozwiązań** wybierz rozwiązanie **Start-Stop-VM [obszaru roboczego]**.  Na **VMManagementSolution [obszaru roboczego]** strony z menu wybierz opcję **usunąć**.<br><br> ![Usuwanie maszyny Wirtualnej Mgmt rozwiązania](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  W **Usuń rozwiązanie** okna, upewnij się, że chcesz usunąć rozwiązania.
4.  Informacje została zweryfikowana i rozwiązanie zostanie usunięty, można śledzić postęp w obszarze **powiadomienia** z menu.  Nastąpi powrót do **rozwiązań** strony po rozpoczęciu procesu, aby usunąć rozwiązania.  

Konto usługi Automatyzacja i obszaru roboczego analizy dzienników nie są usuwane w ramach tego procesu.  Jeśli nie chcesz zachować obszaru roboczego analizy dzienników, musisz ręcznie je usunąć.  Można to zrobić w portalu Azure:

1.    Wybierz z portalu Azure ekranu głównego, **analizy dzienników**.
2. Na **analizy dzienników** okienku, wybierz obszar roboczy.
3. Wybierz **usunąć** z menu w okienku obszaru roboczego ustawienia.  
      
## <a name="next-steps"></a>Kolejne kroki

- Aby dowiedzieć się więcej na temat sposobu konstruowania różne zapytania i przejrzyj dzienniki zadania automatyzacji z analizy dzienników, zobacz [Zaloguj wyszukiwania analizy dzienników](../log-analytics/log-analytics-log-searches.md).
- Aby dowiedzieć się więcej o wykonywaniu elementów runbook, sposobie monitorowania zadań elementów runbook i innych szczegółach technicznych, zobacz [Track a runbook job](automation-runbook-execution.md) (Śledzenie zadania elementu runbook).
- Aby dowiedzieć się więcej na temat analizy dzienników i źródeł danych kolekcji, zobacz [Azure zbierania danych magazynu w omówieniu analizy dzienników](../log-analytics/log-analytics-azure-storage.md).






   

