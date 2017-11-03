---
title: "Przekazywanie danych zadanie usługi Automatyzacja Azure z analizą dzienników OMS | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono sposób wysyłania stan zadania i runbook strumieni zadania do zarządzania i analizy dzienników pakiet zarządzania Operations Microsoft dostarczać szczegółowe informacje o dodatkowych."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: 21923adaa8f8118995799319c1fd496a6e449faa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Przekazuj strumienie zadania i stan zadania z automatyzacji analizy dzienników (OMS)
Automatyzacja może wysyłać runbook strumieni zadania stanu i zadania do swojego obszaru roboczego analizy dzienników programu Microsoft Operations Management Suite (OMS).  Rejestruje zadania i strumieni zadania są widoczne w portalu Azure lub przy użyciu programu PowerShell, dla poszczególnych zadań i to umożliwia wykonywanie prostych dochodzenia. Teraz przy użyciu analizy dzienników można:

* Uzyskiwanie wglądu w zadaniach automatyzacji
* Wyzwalacz poczty e-mail lub alertu oparte na stan zadania elementu runbook (na przykład nie powiodło się lub wstrzymane)
* Zapis zaawansowanych zapytań na strumienie zadania
* Korelowanie zadań na konta automatyzacji
* Wizualizuj historię zadania wraz z upływem czasu     

## <a name="prerequisites-and-deployment-considerations"></a>Wymagania wstępne i zagadnienia dotyczące wdrażania
Aby rozpocząć wysyłanie dzienników automatyzacji do analizy dzienników, potrzebne są:

1. Wydanie listopada 2016 lub nowszy [programu Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Obszar roboczy analizy dzienników. Aby uzyskać więcej informacji, zobacz [wprowadzenie do analizy dzienników](../log-analytics/log-analytics-get-started.md). 
3. Element ResourceId dla Twojego konta usługi Automatyzacja Azure

Aby znaleźć element ResourceId Twoje konto usługi Automatyzacja Azure i obszaru roboczego analizy dzienników, uruchom następujące środowiska PowerShell:

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Jeśli masz wiele kont automatyzacji lub znaleźć obszary robocze, w wyniku poprzedniego polecenia, *nazwa* należy skonfigurować i skopiuj wartość *ResourceId*.

Jeśli trzeba znaleźć *nazwa* Twojego konta automatyzacji w portalu Azure wybierz konto automatyzacji z **konta automatyzacji** bloku, a następnie wybierz **wszystkie ustawienia** .  W bloku **Wszystkie ustawienia** w obszarze **Ustawienia konta** wybierz pozycję **Właściwości**.  W bloku **Właściwości** możesz zauważyć poniższe wartości.<br> ![Właściwości konta automatyzacji](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Konfigurowanie integracji z analizy dzienników
1. Na komputerze, należy uruchomić **programu Windows PowerShell** z **Start** ekranu.  
2. Skopiuj i wklej następujący środowiska PowerShell i edytować wartość `$workspaceId` i `$automationAccountId`.  Dla `-Environment` są prawidłowe wartości parametru, *AzureCloud* lub *AzureUSGovernment* w zależności od pracy w środowisku chmury.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Po uruchomieniu tego skryptu, zostanie wyświetlony w ciągu 10 minut nowe JobLogs lub JobStreams zapisywana rekordów w analizy dzienników.

Aby wyświetlić dzienniki, uruchom następujące zapytanie w wyszukiwania dziennika analizy dzienników:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Weryfikowanie konfiguracji
Aby upewnić się, że Twoje konto usługi Automatyzacja jest wysyłania dzienników do swojego obszaru roboczego analizy dzienników, sprawdź, czy diagnostyki są poprawnie ustawione na koncie automatyzacji za pomocą następującego środowiska PowerShell:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

W danych wyjściowych upewnij się, że:
+ W obszarze *dzienniki*, wartość *włączone* jest *wartość True*
+ Wartość *WorkspaceId* ustawiono ResourceId obszaru roboczego analizy dzienników


## <a name="log-analytics-records"></a>Rekordy usługi Log Analytics
Diagnostyka usługi Automatyzacja Azure tworzy dwa typy rekordów w analizy dzienników i są oznaczone jako **typu = AzureDiagnostics**.

### <a name="job-logs"></a>Rejestruje zadania
| Właściwość | Opis |
| --- | --- |
| TimeGenerated |Data i godzina dla wykonania zadania elementu Runbook. |
| RunbookName_s |Nazwa elementu Runbook. |
| Caller_s |Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań. |
| Tenant_g | Identyfikator GUID, który identyfikuje dzierżawy do obiektu wywołującego. |
| JobId_g |Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook. |
| Typ ResultType |Stan zadania elementu Runbook.  Możliwe wartości:<br>— Nowy<br>— Uruchomione<br>— Zatrzymane<br>— Wstrzymane<br>— Nie powiodło się<br>-Ukończone |
| Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobLogs. |
| OperationName | Określa typ operacji wykonywanej na platformie Azure.  Do automatyzacji wartość jest zadanie. |
| Zasób | Nazwa konta automatyzacji |
| SourceSystem | Jak analizy dzienników zbierane dane. Zawsze *Azure* diagnostyki Azure. |
| ResultDescription |Opisuje stan wyniku zadania elementu Runbook.  Możliwe wartości:<br>— Zadanie jest uruchomione<br>— Zadanie nie powiodło się<br>— Zadanie zostało ukończone |
| CorrelationId |Identyfikator GUID, który jest identyfikatorem korelacji zadania elementu Runbook. |
| Identyfikator zasobu |Określa identyfikator zasobu konto usługi Automatyzacja Azure elementu runbook. |
| SubscriptionId | Subskrypcja platformy Azure identyfikatora (GUID) dla konta automatyzacji. |
| ResourceGroup | Nazwa grupy zasobów dla konta automatyzacji. |
| ResourceProvider | FIRMY MICROSOFT. AUTOMATYZACJA |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Strumienie zadania
| Właściwość | Opis |
| --- | --- |
| TimeGenerated |Data i godzina dla wykonania zadania elementu Runbook. |
| RunbookName_s |Nazwa elementu Runbook. |
| Caller_s |Użytkownik, który zainicjował operację.  Możliwe wartości to adres e-mail lub system w przypadku zaplanowanych zadań. |
| StreamType_s |Typ strumienia zadania. Możliwe wartości:<br>— Postęp<br>— Dane wyjściowe<br>— Ostrzeżenie<br>— Błąd<br>— Debugowanie<br>— Pełne |
| Tenant_g | Identyfikator GUID, który identyfikuje dzierżawy do obiektu wywołującego. |
| JobId_g |Identyfikator GUID, który jest identyfikatorem zadania elementu Runbook. |
| Typ ResultType |Stan zadania elementu Runbook.  Możliwe wartości:<br>— W toku |
| Kategoria | Klasyfikacja typu danych.  W przypadku usługi Automation wartością jest JobStreams. |
| OperationName | Określa typ operacji wykonywanej na platformie Azure.  Do automatyzacji wartość jest zadanie. |
| Zasób | Nazwa konta automatyzacji |
| SourceSystem | Jak analizy dzienników zbierane dane. Zawsze *Azure* diagnostyki Azure. |
| ResultDescription |Obejmuje strumień wyjściowy z elementu Runbook. |
| CorrelationId |Identyfikator GUID, który jest identyfikatorem korelacji zadania elementu Runbook. |
| Identyfikator zasobu |Określa identyfikator zasobu konto usługi Automatyzacja Azure elementu runbook. |
| SubscriptionId | Subskrypcja platformy Azure identyfikatora (GUID) dla konta automatyzacji. |
| ResourceGroup | Nazwa grupy zasobów dla konta automatyzacji. |
| ResourceProvider | FIRMY MICROSOFT. AUTOMATYZACJA |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Wyświetlanie automatyzacji loguje analizy dzienników
Teraz, gdy została uruchomiona wysyłania dzienników zadania automatyzacji do analizy dzienników, zobacz, co można zrobić za pomocą tych dzienników wewnątrz analizy dzienników.

Aby wyświetlić dzienniki, uruchom następujące zapytanie:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Wyślij wiadomość e-mail, gdy zadanie elementu runbook nie powiodło się lub wstrzymuje
Jeden z naszych najwyższym odbiorcy zapyta, jest możliwość wysyłania wiadomości e-mail lub tekstu w przypadku wystąpienia problemów z zadanie elementu runbook.   

Aby utworzyć regułę alertu, rozpoczyna się od utworzenia dziennika wyszukiwanie rekordów zadań elementu runbook, które powinny wywoływać alert.  Kliknij przycisk **Alert** przycisk, aby utworzyć i skonfigurować regułę alertu.

1. Na stronie Przegląd analizy dziennika kliknij **wyszukiwania dziennika**.
2. Utwórz kwerendę wyszukiwania dziennika dla alertu przez wpisanie następującego wyszukiwania w polu zapytania: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` można także grupować według RunbookName przy użyciu:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Jeśli zdefiniowano dzienniki z więcej niż jednego konta automatyzacji lub subskrypcji do swojego obszaru roboczego, można grupować alerty subskrypcji i konto automatyzacji.  Nazwa konta automatyzacji mogą pochodzić z pola zasobów do wyszukiwania JobLogs.  
3. Aby otworzyć **Dodaj regułę alertu** kliknij **alertu** w górnej części strony. Aby uzyskać szczegółowe informacje na temat opcji, aby skonfigurować alert, zobacz [alertów w analizy dzienników](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Znajdź wszystkie zadania, które zostały ukończone z błędami
Oprócz alertów przy błędach, można znaleźć błąd niepowodujący po zadanie elementu runbook. W takich przypadkach PowerShell tworzy strumień błędów, ale błędy niepowodujący nie powodują zadania wstrzymać lub zakończyć się niepowodzeniem.    

1. W obszarze roboczym analizy dzienników, kliknij przycisk **wyszukiwania dziennika**.
2. W polu kwerendy wpisz `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` , a następnie kliknij przycisk **wyszukiwania**.

### <a name="view-job-streams-for-a-job"></a>Wyświetl zadania strumieni zadania
Podczas debugowania zadania, można również przyglądać strumieni zadania.  Następujące zapytanie Wyświetla wszystkich strumieni dla jednego zadania z identyfikatorem GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Widok stanu zadania historycznych
Na koniec można zwizualizować historię zadania wraz z upływem czasu.  Skorzystaj z tej kwerendy, aby wyszukać stan zadań wraz z upływem czasu.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![Wykres stanu zadania historycznych OMS](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Podsumowanie
Wysyłając automatyzacji zadań stan strumienia danych i do analizy dzienników, można uzyskać lepszy wgląd w stan zadań automatyzacji przez:
+ Konfigurowanie alertów powiadomienie, gdy występuje problem
+ Przy użyciu niestandardowych widoków i zapytania wyszukiwania, aby wizualizować wyniki elementu runbook, stan zadania elementu runbook i inne powiązane kluczowych wskaźników i metryki.  

Analiza dzienników zapewnia lepszą widoczność operacyjnej do automatyzacji zadań i może ułatwić adres zdarzenia szybciej.  

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej o sposobie tworzenia różnych zapytań wyszukiwania i sprawdzaniu dzienników zadań usługi Automation przy użyciu usługi Log Analytics, zobacz [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md) (Wyszukiwanie dzienników w usłudze Log Analytics)
* Aby poznać sposób tworzenia i pobrać dane wyjściowe i komunikaty o błędach z elementów runbook, zobacz [Runbook dane wyjściowe i komunikaty](automation-runbook-output-and-messages.md)
* Aby dowiedzieć się więcej o wykonywaniu elementów Runbook, sposobie monitorowania zadań elementów Runbook i innych szczegółach technicznych, zobacz [Track a runbook job](automation-runbook-execution.md) (Śledzenie zadania elementu Runbook)
* Aby dowiedzieć się więcej o usłudze OMS Log Analytics i źródłach zbierania danych, zobacz [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md) (Zbieranie danych magazynu platformy Azure w usłudze Log Analytics — omówienie)
