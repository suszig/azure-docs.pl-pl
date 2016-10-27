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

Wielu klientów tworzy więcej niż jedną grupę zasobów. Niektóre grupy mogą służyć do zarządzania aplikacjami produkcyjnymi, a inne mogą być używane jako środowiska rozwojowe, testowe lub przejściowe. Automatyzacja wdrażania tych zasobów to jedno, ale wymagane jest również zapewnienie możliwości łatwej likwidacji grupy zasobów. To typowe zadanie zarządzania można uprościć przy użyciu usługi Azure Automation. Przydaje się to w przypadku korzystania z subskrypcji platformy Azure z limitem wydatków w ramach takich ofert jak MSDN lub program Microsoft Partner Network Cloud Essentials.

Ten scenariusz jest oparty na elemencie Runbook programu PowerShell i jest przeznaczony do usuwania określonych grup zasobów z subskrypcji. Domyślne ustawienie elementu Runbook powoduje testowanie przed wykonaniem. Daje to gwarancję, że nie można przypadkowo usunąć grupy zasobów przed przygotowaniem wszystkiego do wykonania tej procedury.   

## Uzyskiwanie scenariusza

Ten scenariusz składa się z elementu Runbook programu PowerShell, który można pobrać z [Galerii programu PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Można również zaimportować go bezpośrednio z [Galerii elementów Runbook](automation-runbook-gallery.md) w witrynie Azure Portal.<br><br>

Element Runbook | Opis|
----------|------------|
Remove-ResourceGroup | Umożliwia usunięcie z subskrypcji grup zasobów (jednej lub wielu) platformy Azure wraz z powiązanymi zasobami.  
<br>
Dla tego elementu Runbook zdefiniowano następujące parametry wejściowe:

Parametr | Opis|
----------|------------|
NameFilter (Filtr nazw) (wymagany) | Określa filtr nazw w celu ograniczenia grup zasobów, które mają zostać usunięte. Można przekazać wiele wartości za pomocą listy rozdzielanej przecinkami.<br>Filtr nie uwzględnia wielkości liter i pokaże wszystkie grupy zasobów zawierające dany ciąg.|
PreviewMode (Tryb podglądu) (opcjonalny) | Umożliwia uruchomienie elementu Runbook w celu sprawdzenia, które grupy zasobów zostaną usunięte, ale bez podejmowania żadnych działań.<br>Wartość domyślna równa **true** (prawda) pozwala zapobiec przypadkowemu usunięciu jednej lub większej liczby grup zasobów przekazanych do elementu Runbook.  

## Instalowanie i konfigurowanie tego scenariusza

### Wymagania wstępne

Uwierzytelnianie elementu Runbook odbywa się przy użyciu [konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).    

### Instalowanie i publikowanie elementów Runbook

Po pobraniu elementu Runbook można go zaimportować za pomocą [procedury importowania elementów Runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation). Po pomyślnym zaimportowaniu elementu Runbook do konta usługi Automation należy go opublikować.


## Używanie elementu Runbook

Poniższe instrukcje umożliwiają uruchomienie tego elementu Runbook i zapoznanie się z jego działaniem. Ten przykład obejmuje tylko testowanie elementu Runbook bez faktycznego usuwania grupy zasobów.  

1. W witrynie Azure Portal otwórz konto usługi Automation i kliknij kafelek **Elementy Runbook**.
2. Wybierz element Runbook **Remove-ResourceGroup** i kliknij pozycję **Uruchom**.
3. Uruchomienie elementu Runbook powoduje otwarcie bloku **Uruchom element Runbook**, w którym można skonfigurować parametry. Wprowadź nazwy grup zasobów w subskrypcji, której możesz użyć do testów bez powodowania żadnych szkód w razie przypadkowego usunięcia.<br> ![Parametry elementu Runbook Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

    >[AZURE.NOTE] Aby uniknąć usunięcia wybranych grup zasobów, upewnij się, że parametr **Previewmode** (Tryb podglądu) ma wartość **true** (prawda).  **Pamiętaj**, że element Runbook nie usunie grupy zasobów zawierającej konto usługi Automation, na którym jest uruchomiony ten element Runbook.  

4. Po skonfigurowaniu wszystkich wartości parametrów kliknij przycisk **OK**. Element Runbook zostanie umieszczony w kolejce do wykonania.  

Aby wyświetlić szczegóły zadania elementu Runbook **Remove-ResourceGroup** w witrynie Azure Portal, kliknij kafelek **Zadania** w elemencie Runbook. W podsumowaniu zadania są wyświetlane parametry wejściowe i dane ze strumienia wyjściowego oraz ogólne informacje o zadaniu wraz z wygenerowanymi wyjątkami.<br> ![Stan zadania elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

W obszarze **Podsumowanie zadania** są widoczne ostrzeżenia i komunikaty o błędach oraz dane ze strumienia wyjściowego. Kliknij kafelek **Dane wyjściowe**, aby wyświetlić szczegółowe wyniki wykonania elementu Runbook.<br> ![Dane wyjściowe elementu Runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## Następne kroki

- Aby rozpocząć tworzenie własnego elementu Runbook, zobacz artykuł [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md) (Tworzenie lub importowanie elementu Runbook w usłudze Azure Automation).
- Aby rozpocząć pracę z elementami Runbook przepływu pracy programu PowerShell, zobacz artykuł [Mój pierwszy element Runbook przepływu pracy programu PowerShell](automation-first-runbook-textual.md).



<!--HONumber=Oct16_HO3-->


