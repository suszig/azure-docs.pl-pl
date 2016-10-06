<properties
    pageTitle="Automatyczne usuwanie grup zasobów | Microsoft Azure"
    description="Wersja scenariusza użycia usługi Azure Automation obejmująca wykorzystanie przepływu pracy programu PowerShell i elementów Runbook umożliwiających usunięcie wszystkich grupy zasobów w subskrypcji."
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
    ms.date="09/26/2016"
    ms.author="magoedte"/>


# Scenariusz użycia usługi Azure Automation — automatyczne usuwanie grup zasobów

Wielu klientów tworzy dodatkowe grupy zasobów przeznaczone do zarządzania aplikacjami produkcyjnymi oraz na potrzeby środowiska projektowego, testowego i przejściowego. Automatyzacja wdrażania tych zasobów to jedno, ale wymagane jest również zapewnienie możliwości łatwej likwidacji grupy zasobów.  Doskonałym narzędziem, które to umożliwia, jest usługa Automation. Jej użycie stanowi okazję do usprawnienia tego typowego zadania zarządzania, a także jest przydatne, jeśli korzysta się z subskrypcji platformy Azure z limitem wydatków w ramach takich programów, jak na przykład MSDN lub Microsoft Partner Network Cloud Essentials. 

Ten scenariusz jest oparty na elemencie Runbook programu PowerShell i jest przeznaczony do usuwania określonych grup zasobów z subskrypcji.  Domyślne zachowanie elementu Runbook polega na przeprowadzeniu testów przed kontynuowaniem operacji.  Pozwala to zapobiec jego przypadkowemu usunięciu, gdy nie ma absolutnej pewności, że można ukończyć tę procedurę.   

## Uzyskiwanie scenariusza

Ten scenariusz składa się z elementu Runbook programu PowerShell, który można pobrać z [galerii programu PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript) lub zaimportować bezpośrednio z [galerii elementów Runbook](automation-runbook-gallery.md) dostępnej w witrynie Azure Portal.<br><br> 

Element Runbook | Opis|
----------|------------|
Remove-ResourceGroup | Umożliwia usunięcie z subskrypcji jednej lub większej liczby grup zasobów platformy Azure wraz z zasobami znajdującymi się w tych grupach.  
<br>
Dla tego elementu Runbook zdefiniowano następujące parametry wejściowe:

Parametr | Opis|
----------|------------|
NameFilter (Filtr nazw) (wymagany) | Umożliwia określenie filtru nazw w celu ograniczenia grup zasobów, które mają zostać usunięte. Można przekazać wiele wartości za pomocą listy rozdzielanej przecinkami.<br>W ramach tego filtru nie jest uwzględniana wielkość liter — zgodne są wszystkie grupy zasobów zawierające dany ciąg.|
PreviewMode (Tryb podglądu) (opcjonalny) | Umożliwia uruchomienie elementu Runbook w celu sprawdzenia, które grupy zasobów zostaną usunięte, ale bez podejmowania żadnych działań.<br>Wartość domyślna równa **true** (prawda) pozwala zapobiec przypadkowemu usunięciu jednej lub większej liczby grup zasobów przekazanych do elementu Runbook.  

## Instalowanie i konfigurowanie scenariusza

### Wymagania wstępne

Uwierzytelnianie elementu Runbook odbywa się przy użyciu [konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).    

### Instalowanie i publikowanie elementów Runbook

Po pobraniu elementu Runbook można go zaimportować za pomocą procedury zawartej w artykule [Importing runbook procedures](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation) (Procedury dotyczące importowania elementów Runbook).  Po pomyślnym zaimportowaniu elementu Runbook do konta usługi Automation należy go opublikować.


## Używanie elementu Runbook

Poniższe instrukcje umożliwiają uruchomienie tego elementu Runbook i zapoznanie się z jego działaniem.  Ten przykład obejmuje tylko testowanie elementu Runbook bez faktycznego usuwania grupy zasobów.  

1. W witrynie Azure Portal otwórz konto usługi Automation i kliknij kafelek **Elementy Runbook**.
2. Wybierz element Runbook **Remove-ResourceGroup** i kliknij pozycję **Uruchom**.
3. Uruchomienie elementu Runbook powoduje otwarcie bloku **Uruchamianie elementu Runbook**, w którym można skonfigurować następujące wartości parametrów.  Wprowadź nazwę co najmniej jednej grupy zasobów w subskrypcji, którą chcesz przetestować bez powodowania żadnych szkód w razie jej przypadkowego usunięcia.<br> ![Parametry elementu Runbook Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)
    
    >[AZURE.NOTE] Aby uniemożliwić usunięcie wybranych grup zasobów, upewnij się, że opcja **Previewmode** (Tryb podglądu) ma wartość **true** (prawda).  **Należy pamiętać** o tym, że element Runbook nie usunie grupy zasobów zawierającej konto usługi Automation, na którym jest uruchomiony ten element Runbook.  

4. Po skonfigurowaniu wszystkich wartości parametrów kliknij przycisk **OK**. Element Runbook zostanie umieszczony w kolejce do wykonania.  

Aby wyświetlić szczegóły zadania elementu Runbook **Remove-ResourceGroup** w witrynie Azure Portal, wybierz kafelek **Zadania** elementu Runbook. W obszarze podsumowania zadania są wyświetlane parametry wejściowe i dane ze strumienia wyjściowego oraz ogólne informacje o zadaniu wraz z wygenerowanymi wyjątkami.<br> ![Stan zadania elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

W obszarze **Podsumowanie zadania** są widoczne ostrzeżenia i komunikaty o błędach oraz dane ze strumienia wyjściowego. Wybierz kafelek **Dane wyjściowe**, aby wyświetlić szczegółowe wyniki wykonania elementu Runbook.<br> ![Dane wyjściowe elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png) 

## Następne kroki

- Aby rozpocząć tworzenie własnego elementu Runbook, zobacz artykuł [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md) (Tworzenie lub importowanie elementu Runbook w usłudze Azure Automation).
- Aby rozpocząć pracę z elementami Runbook przepływu pracy programu PowerShell, zobacz artykuł [Mój pierwszy element Runbook przepływu pracy programu PowerShell](automation-first-runbook-textual.md).


<!--HONumber=Sep16_HO4-->


