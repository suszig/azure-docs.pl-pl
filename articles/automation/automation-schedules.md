---
title: "Harmonogramów w usłudze Automatyzacja Azure | Dokumentacja firmy Microsoft"
description: "Harmonogramy automatyzacji służą do planowania elementów runbook automatyzacji Azure do automatycznego uruchamiania. Opisuje sposób tworzenia i zarządzania nimi harmonogramu w, dzięki czemu może automatycznie uruchomić element runbook o określonej godzinie lub zgodnie z harmonogramem cyklicznym."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 1c2da639-ad20-4848-920b-88e471b2e1d9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: c651ab70977367d0e41364120c89561a04a45cf4
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Planowanie elementu Runbook w usłudze Azure Automation
Aby zaplanować elementu runbook automatyzacji Azure można uruchomić w określonym czasie, łącze do co najmniej jeden harmonogram. Harmonogram można skonfigurować do uruchamiania raz lub pojawiał co godzinę lub codziennie harmonogramu dla elementów runbook w klasycznym portalu Azure i elementów runbook w portalu Azure, można również zaplanować je na co tydzień, co miesiąc, określone dni tygodnia lub dni miesiąca lub określony dzień miesiąca.  Element runbook może odnosić się do wielu harmonogramów i harmonogram może mieć wielu elementów runbook z nim połączone.

> [!NOTE]
> Harmonogramy aktualnie nie obsługuje konfiguracji DSC automatyzacji Azure.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Polecenia cmdlet programu Windows PowerShell
Polecenia cmdlet w poniższej tabeli służą do tworzenia i zarządzania nimi harmonogramów z programu Windows PowerShell w automatyzacji Azure. One dostarczane jako część [modułu Azure PowerShell](/powershell/azure/overview).

| Polecenia cmdlet | Opis |
|:--- |:--- |
| **Polecenia cmdlet Menedżera zasobów systemu Azure** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Pobiera harmonogram. |
| [Nowe AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Tworzy nowy harmonogram. |
| [Usuń AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Usuwa harmonogram. |
| [Zestaw AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Ustawia właściwości istniejącego harmonogramu. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Pobiera zaplanowanego elementach runbook. |
| [Rejestr AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Kojarzy elementu runbook z harmonogramem. |
| [Wyrejestruj AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Dissociates elementu runbook z harmonogramem. |
| **Polecenia cmdlet do zarządzania usługi Azure** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Pobiera harmonogram. |
| [Nowe AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Tworzy nowy harmonogram. |
| [Usuń AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Usuwa harmonogram. |
| [Zestaw AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Ustawia właściwości istniejącego harmonogramu. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Pobiera zaplanowanego elementach runbook. |
| [Rejestr AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Kojarzy elementu runbook z harmonogramem. |
| [Wyrejestruj AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Dissociates elementu runbook z harmonogramem. |

## <a name="creating-a-schedule"></a>Tworzenie harmonogramu
Możesz utworzyć nowy harmonogram dla elementów runbook w portalu Azure w klasycznym portalu lub przy użyciu programu Windows PowerShell. Istnieje również możliwość utworzenia nowego harmonogramu w przypadku powiązania elementu runbook z harmonogramem przy użyciu portalu Azure classic lub Azure.

> [!NOTE]
> Po uruchomieniu nowe zadanie zaplanowane usługi Automatyzacja Azure korzysta z modułów najnowszego na Twoim koncie automatyzacji.  Aby uniknąć wpływu na elementy runbook i ich automatyzacji procesów, należy najpierw przetestować wszystkie elementy runbook, które zostały połączone harmonogramy przy użyciu konta automatyzacji przeznaczonego do testowania.  To sprawdzenie poprawności zaplanowane elementy runbook w dalszym ciągu działać poprawnie, a jeśli nie, dalsze rozwiązywanie problemów z i zastosować zmiany wymagane do przeprowadzenia migracji do środowiska produkcyjnego runbook zaktualizowanej wersji.  
>  Konta automatyzacji nie automatycznie pobrać wszystkich nowych wersji modułów, chyba że zaktualizowano je ręcznie, wybierając [modułów Azure aktualizacji](automation-update-azure-modules.md) opcję **modułów** . 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Aby utworzyć nowy harmonogram w portalu Azure
1. W portalu Azure, z Twojego konta automatyzacji wybierz **harmonogramy** w sekcji **udostępnione zasoby** po lewej stronie. 
2. Kliknij przycisk **Dodaj harmonogram** w górnej części strony.
4. Na **nowy harmonogram** okienku wpisz **nazwa** i opcjonalnie **opis** nowego harmonogramu.
5. Wybierz harmonogram jest uruchamiane jeden raz, czy reoccurring zgodnie z harmonogramem, wybierając **raz** lub **cyklu**.  W przypadku wybrania **raz** określ **godzina rozpoczęcia** , a następnie kliknij przycisk **Utwórz**.  W przypadku wybrania **cyklu**, określ **godzina rozpoczęcia** i częstotliwość Częstotliwość elementu runbook przez powtarzanie - **godzina**, **dzień**, **tygodnia**, lub **miesiąca**.  W przypadku wybrania **tygodnia** lub **miesiąca** z listy rozwijanej **opcji cyklu** zostanie wyświetlona w okienku i na wybór, **opcji cyklu** okienku są prezentowane i można wybrać dzień tygodnia, w przypadku wybrania **tygodnia**.  W przypadku wybrania **miesiąca**, aby wybrać **dni tygodnia** lub określone dni miesiąca w kalendarzu i na koniec ma Uruchom ostatniego dnia miesiąca lub nie, a następnie kliknij przycisk **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Aby utworzyć nowy harmonogram w klasycznym portalu Azure
1. W klasycznym portalu Azure wybierz automatyzacji, a następnie wybierz nazwę konta automatyzacji.
2. Wybierz **zasoby** kartę.
3. W dolnej części okna kliknij **Dodaj ustawienie**.
4. Kliknij przycisk **Dodaj harmonogram**.
5. Wpisz **nazwa** i opcjonalnie **opis** nowego harmonogramu. Harmonogram można uruchomić **jeden raz**, **co godzinę**, **codzienne**, **co tydzień**, lub **miesięczne**.
6. Określ **czas rozpoczęcia** i inne opcje, w zależności od typu wybranego harmonogramu.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Aby utworzyć nowy harmonogram za pomocą środowiska Windows PowerShell
Można użyć [AzureAutomationSchedule nowy](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) , aby utworzyć nowy harmonogram automatyzacji Azure classic elementów runbook, lub [AzureRmAutomationSchedule nowy](/powershell/module/azurerm.automation/new-azurermautomationschedule) polecenia cmdlet dla elementów runbook w portalu Azure. Należy określić godzinę rozpoczęcia harmonogramu i częstotliwości, który ma być uruchamiany.

W następujących przykładowych poleceniach pokazano, jak utworzyć harmonogram 15 i 30 co miesiąc, za pomocą polecenia cmdlet usługi Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

W następujących przykładowych poleceniach pokazano, jak utworzyć nowy harmonogram wykonywany codziennie o 3:30 będzie 20 stycznia 2015 począwszy od polecenia cmdlet usługi Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Powiązanie harmonogramu z elementem runbook
Element runbook może odnosić się do wielu harmonogramów i harmonogram może mieć wielu elementów runbook z nim połączone. Jeśli element runbook ma parametry, można podać wartości dla nich. Należy podać wartości parametrów obowiązkowych i może podać wartości parametrów opcjonalnych.  Te wartości są używane przy każdym uruchomieniu elementu runbook przez ten harmonogram.  Możesz dołączyć ten sam element runbook do innego harmonogramu i wartości różnych parametrów.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Aby połączyć harmonogramu do elementu runbook w portalu Azure
1. W portalu Azure, z Twojego konta automatyzacji wybierz **Runbook** w sekcji **automatyzacji procesu** po lewej stronie.
2. Kliknij nazwę elementu runbook do zaplanowania.
3. Jeśli element runbook nie jest obecnie połączony z harmonogramem, są oferowane opcję, aby utworzyć nowy harmonogram lub link do istniejącego harmonogramu.  
4. Jeśli element runbook ma parametry, można wybrać opcję **zmodyfikuj parametry uruchomieniowe (domyślne: Azure)** i **parametry** okienku są prezentowane, gdzie można wprowadzić informacje w związku z tym.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Aby połączyć harmonogramu do elementu runbook z klasycznego portalu Azure
1. W klasycznym portalu Azure, wybierz **automatyzacji** , a następnie kliknij nazwę konta automatyzacji.
2. Wybierz **elementów Runbook** kartę.
3. Kliknij nazwę elementu runbook do zaplanowania.
4. Kliknij przycisk **harmonogram** kartę.
5. Jeśli element runbook nie jest obecnie połączony z harmonogramem, a następnie opcję są oferowane **łącze do nowego harmonogramu** lub **Połącz z istniejącym harmonogramem**.  Jeśli element runbook jest aktualnie połączony z harmonogramem, kliknij przycisk **łącze** w dolnej części okna, aby uzyskać dostęp do tych opcji.
6. Jeśli element runbook ma parametry, zostanie wyświetlony monit o ich wartości.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Aby połączyć harmonogramu do elementu runbook przy użyciu programu Windows PowerShell
Można użyć [AzureAutomationScheduledRunbook rejestru](http://msdn.microsoft.com/library/azure/dn690265.aspx) celu powiązania harmonogramu z elementem runbook klasycznego lub [AzureRmAutomationScheduledRunbook rejestru](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) polecenia cmdlet dla elementów runbook w portalu Azure.  Można określić wartości parametrów elementu runbook przy użyciu parametru parametrów. Zobacz [uruchamianie elementu Runbook automatyzacji Azure](automation-starting-a-runbook.md) Aby uzyskać więcej informacji na temat określania wartości parametrów.

Następujące przykładowe polecenia pokazują, jak połączyć harmonogramu do elementu runbook za pomocą polecenia cmdlet usługi Azure Resource Manager z parametrami.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Następujące przykładowe polecenia pokazują, jak połączyć z harmonogramem przy użyciu polecenia cmdlet usługi Azure Service Management z parametrami.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Wyłączanie harmonogramu
Po wyłączeniu harmonogramu, wszystkie elementy runbook nie jest już powiązany z działa zgodnie z tym harmonogramem. Można ręcznie wyłączyć harmonogram lub ustaw czas wygaśnięcia harmonogramów z częstotliwością po ich utworzeniu. Po osiągnięciu czas wygaśnięcia harmonogramie jest wyłączona.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Aby wyłączyć harmonogram z portalu Azure
1. W portalu Azure, z Twojego konta automatyzacji wybierz **harmonogramy** w sekcji **udostępnione zasoby** po lewej stronie.
2. Kliknij nazwę harmonogramu, aby otworzyć okienko szczegółów.
3. Zmień **włączone** do **nr**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Aby wyłączyć harmonogram z klasycznego portalu Azure
Można wyłączyć harmonogram w klasycznym portalu Azure na stronie Szczegóły harmonogramu dla harmonogramu.

1. W klasycznym portalu Azure wybierz automatyzacji, a następnie kliknij nazwę konta automatyzacji.
2. Wybierz kartę zasoby.
3. Kliknij nazwę harmonogramu, otwórz jego stronę szczegółów.
4. Zmień **włączone** do **nr**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Aby wyłączyć harmonogram przy użyciu programu Windows PowerShell
Można użyć [AzureAutomationSchedule zestaw](http://msdn.microsoft.com/library/azure/dn690270.aspx) polecenia cmdlet, aby zmienić właściwości istniejącego harmonogramu klasycznego elementu runbook lub [AzureRmAutomationSchedule zestaw](/powershell/module/azurerm.automation/set-azurermautomationschedule) polecenia cmdlet dla elementów runbook w portalu Azure. Aby wyłączyć harmonogram, określ **false** dla **IsEnabled** parametru.

W następujących przykładowych poleceniach pokazano, jak wyłączyć harmonogram dla elementu runbook za pomocą polecenia cmdlet usługi Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Następujące przykładowe polecenia pokazują, jak wyłączyć harmonogram za pomocą polecenia cmdlet usługi Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Następne kroki
* Aby rozpocząć pracę z elementami runbook w automatyzacji Azure, zobacz [uruchamianie elementu Runbook automatyzacji Azure](automation-starting-a-runbook.md) 

