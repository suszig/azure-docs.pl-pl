<properties
    pageTitle="Mój pierwszy element Runbook programu PowerShell w usłudze Azure Automation | Microsoft Azure"
    description="W tym samouczku przedstawiono procesy tworzenia, testowania i publikowania prostego elementu Runbook programu PowerShell."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="azure powershell, samouczek skryptu programu powershell, automatyzacja programu powershell"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;sngun"/>

# Mój pierwszy element Runbook programu PowerShell

> [AZURE.SELECTOR] - [Element graficzny](automation-first-runbook-graphical.md) - [Program PowerShell](automation-first-runbook-textual-PowerShell.md) - [Przepływ pracy programu PowerShell](automation-first-runbook-textual.md)  

W tym samouczku przedstawiono proces tworzenia [elementu Runbook programu PowerShell](automation-runbook-types.md#powershell-runbooks) w usłudze Azure Automation. Rozpoczniemy pracę od prostego elementu Runbook, który przetestujemy i opublikujemy, objaśniając równocześnie, jak śledzić stan zadania elementu Runbook. Następnie zmodyfikujemy element Runbook, aby faktycznie zarządzać zasobami platformy Azure (w tym przypadku uruchomić maszynę wirtualną platformy Azure). Udoskonalimy również element Runbook, dodając parametry, aby działał on bardziej niezawodnie.

## Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

-   Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub <a href="/pricing/free-account/" target="_blank">[utworzyć bezpłatne konto](https://azure.microsoft.com/free/).
-   [Konto usługi Automation](automation-security-overview.md) do przechowywania elementu Runbook i uwierzytelniania w zasobach platformy Azure.  To konto musi mieć uprawnienia do uruchamiania i zatrzymywania maszyny wirtualnej.
-   Maszyna wirtualna platformy Azure. Będziemy uruchamiać i zatrzymywać tę maszynę, dlatego należy użyć maszyny innej niż produkcyjna.

## Krok 1. Tworzenie nowego elementu Runbook

Rozpoczniemy od utworzenia prostego elementu Runbook z wyświetlonym tekstem *Witaj, świecie*.

1.  W portalu Azure otwórz konto usługi Automation.  
    Strona konta usługi Automation umożliwia szybki przegląd zasobów na tym koncie. Konto powinno mieć już pewne elementy zawartości. Większość z nich to moduły, które są automatycznie uwzględniane na nowym koncie usługi Automation. Powinien istnieć również element zawartości Poświadczenie wymieniony w części dotyczącej [wymagań wstępnych](#prerequisites).
2.  Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.  
    ![Ekran elementów Runbooks](media/automation-first-runbook-textual-powershell/automation-runbooks-control.png)  
3.  Utwórz nowy element Runbook, klikając przycisk **Dodaj element Runbook**, a następnie pozycję **Utwórz nowy element Runbook**.
4.  Nadaj elementowi Runbook nazwę *MyFirstRunbook-PowerShell*.
5.  W tym przypadku będziemy tworzyć [element Runbook programu PowerShell](automation-runbook-types.md#powershell-runbooks), dlatego wybierz wartość **Powershell** dla parametru **Typ elementu Runbook**.  
    ![Typ elementu Runbook](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6.  Kliknij pozycję **Utwórz**, aby utworzyć element Runbook i otworzyć edytor tekstów.

## Krok 2. Dodawanie kodu do elementu Runbook

Możesz wpisać kod bezpośrednio w elemencie Runbook lub wybrać polecenia cmdlet, elementy Runbook i elementy zawartości, używając kontrolki Biblioteka, a następnie dodać je do elementu Runbook z powiązanymi parametrami. W przypadku tego przewodnika wpiszemy informacje bezpośrednio w elemencie Runbook.

1.  Nasz element Runbook jest obecnie pusty. Wpisz *Write-Output „Witaj, świecie”*.  
    ![Witaj, świecie](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2.  Zapisz element Runbook, klikając przycisk **Zapisz**.  
    ![Przycisk Zapisz](media/automation-first-runbook-textual-powershell/automation-save-button.png)  

## Krok 3. Testowanie elementu Runbook

Przed opublikowaniem elementu Runbook w celu udostępnienia go w środowisku produkcyjnym chcemy go przetestować, aby upewnić się, że działa prawidłowo. Testowanie elementu Runbook polega na uruchomieniu jego **wersji roboczej** i interaktywnym przejrzeniu danych wyjściowych.

1.  Kliknij pozycję **Okienko testowania**, aby otworzyć okienko testowania.  
    ![Okienko testowania](media/automation-first-runbook-textual-powershell/automation-testpane.png)  
2.  Kliknij opcję **Uruchom**, aby rozpocząć test. Powinna to być jedyna włączona opcja.
3.  Zostanie utworzone [zadanie elementu Runbook](automation-runbook-execution.md) i pojawi się jego stan.  
    Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
4.  Po zakończeniu zadania elementu Runbook zostaną wyświetlone jego dane wyjściowe. W naszym przypadku powinien być widoczny ciąg *Witaj, świecie*.  
    ![Dane wyjściowe w okienku testowania](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5.  Zamknij okienko testowania, aby wrócić do kanwy.

## Krok 4. Publikowanie i uruchamianie elementu Runbook

Nowo utworzony element Runbook nadal działa w trybie roboczym. Przed uruchomieniem elementu w środowisku produkcyjnym musimy go opublikować. Podczas publikowania elementu Runbook można zastąpić istniejącą wersję opublikowaną wersją roboczą. W naszym przypadku nie mamy jeszcze wersji opublikowanej, ponieważ element Runbook został dopiero utworzony.

1.  Kliknij przycisk **Opublikuj**, aby opublikować element Runbook, a następnie kliknij opcję **Tak** po wyświetleniu monitu.  
    ![Przycisk Opublikuj](media/automation-first-runbook-textual-powershell/automation-publish-button.png)  
2.  Jeśli teraz przewiniesz w lewo, aby wyświetlić element Runbook w okienku **Elementy Runbook**, element **Stan tworzenia** będzie mieć wartość **Opublikowano**.
3.  Przewiń w prawo, aby wyświetlić okienko elementu **MyFirstRunbook-PowerShell**.  
    Opcje w górnej części umożliwiają nam uruchamianie elementu Runbook, wyświetlanie elementu Runbook, planowanie jego uruchomienia w przyszłości lub utworzenie [elementu webhook](automation-webhooks.md) w celu umożliwienia uruchamiania za pośrednictwem wywołania HTTP.
4.  Chcemy tylko uruchomić element Runbook, dlatego w otwartym bloku uruchamiania elementu Runbook kliknij przycisk **Uruchom**, a następnie kliknij przycisk **OK**.  
    ![Przycisk Uruchom](media/automation-first-runbook-textual-powershell/automation-start-button.png)  
5.  Zostanie otwarte okienko zadania elementu Runbook, który właśnie utworzyliśmy. Możemy zamknąć to okienko, ale w tym przypadku pozostawimy je otwarte, aby obserwować postęp zadania.
6.  Stan zadania jest wyświetlany w oknie **Podsumowanie zadania** i odpowiada stanom obserwowanym podczas testowania elementu Runbook.  
    ![Podsumowanie zadania](media/automation-first-runbook-textual-powershell/automation-job-summary.png)  
7.  Gdy stan elementu Runbook zmieni się na *Ukończono*, kliknij pozycję **Dane wyjściowe**. Zostanie otwarte okienko danych wyjściowych z naszym ciągiem *Witaj, świecie*.  
    ![Dane wyjściowe zadania](media/automation-first-runbook-textual-powershell/automation-job-output.png)
8.  Zamknij okienko danych wyjściowych.
9.  Kliknij pozycję **Wszystkie dzienniki**, aby otworzyć okienko strumieni dla zadania elementu Runbook. W strumieniu danych wyjściowych powinien być widoczny tylko ciąg *Witaj, świecie*, ale mogą zostać wyświetlone inne strumienie zadania elementu Runbook, takie jak Pełne informacje i Błąd, jeśli element Runbook wykonuje w nich operacje zapisywania.  
    ![Wszystkie dzienniki](media/automation-first-runbook-textual-powershell/automation-alllogs.png)  
10. Zamknij okienko strumieni i okienko zadania, aby wrócić do okienka MyFirstRunbook-PowerShell.
11. Kliknij pozycję **Zadania**, aby otworzyć okienko zadań dla tego elementu Runbook. Zawiera ono listę wszystkich zadań utworzonych przez dany element Runbook. Ponieważ uruchomiliśmy zadanie tylko raz, powinniśmy zobaczyć tylko jedno zadanie.  
    ![Lista zadań](media/automation-first-runbook-textual-powershell/automation-job-list.png)  
12. Możesz kliknąć to zadanie, aby otworzyć okienko zadania wyświetlone przez nas wcześniej po uruchomieniu elementu Runbook. Dzięki temu możesz cofnąć się w czasie i wyświetlić szczegóły dowolnego zadania, które zostało utworzone dla określonego elementu Runbook.

## Krok 5. Dodawanie uwierzytelniania w celu zarządzania zasobami platformy Azure

Element Runbook został przetestowany i opublikowany, ale jak do tej pory nie wykonuje on żadnych użytecznych czynności. Chcemy, aby zarządzał zasobami platformy Azure. Będzie to jednak niemożliwe do momentu uwierzytelnienia go przy użyciu poświadczeń podanych w części dotyczącej [wymagań wstępnych](#prerequisites). Czynność tę możemy wykonać przy użyciu polecenia cmdlet **Add-AzureRmAccount**.

1.  Otwórz edytor tekstów, klikając pozycję **Edytuj** w okienku MyFirstRunbook-PowerShell.  
    ![Edytowanie elementu Runbook](media/automation-first-runbook-textual-powershell/automation-edit-runbook.png)  
2.  Nie potrzebujemy już wiersza **Write-Output**. Usuń go.
3.  Wpisz lub skopiuj i wklej poniższy kod, który będzie obsługiwać uwierzytelnianie za pomocą konta Uruchom jako w usłudze Automation:

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ``` 
<br>
4.  Kliknij pozycję **Okienko testowania**, aby umożliwić przetestowanie elementu Runbook.
5.  Kliknij opcję **Uruchom**, aby rozpocząć test. Po zakończeniu powinny pojawić się dane wyjściowe podobne do poniższych, zawierające podstawowe informacje powiązane z kontem. Będzie to potwierdzenie, że poświadczenie jest prawidłowe. <br> ![Uwierzytelnianie](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## Krok 6. Dodawanie kodu w celu uruchomienia maszyny wirtualnej

Teraz, gdy nasz element Runbook jest uwierzytelniany w subskrypcji platformy Azure, możemy zarządzać zasobami. Dodamy polecenie, aby uruchomić maszynę wirtualną. Możesz wybrać dowolną maszynę wirtualną w ramach subskrypcji Azure. Wybrana nazwa zostanie trwale zakodowana w poleceniu cmdlet.

1.  Po poleceniu *Add-AzureRmAccount* wpisz polecenie *Start-AzureRmVM -Name 'nazwa_maszyny_wirtualnej' -ResourceGroupName 'nazwa_grupy_zasobów'*, podając nazwę maszyny wirtualnej do uruchomienia i nazwę jej grupy zasobów.  
    
    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
     ```
<br>
2.  Zapisz element Runbook, a następnie kliknij pozycję **Okienko testowania**, aby umożliwić jego przetestowanie.
3.  Kliknij opcję **Uruchom**, aby rozpocząć test. Po zakończeniu sprawdź, czy maszyna wirtualna została uruchomiona.

## Krok 7. Dodawanie parametru wejściowego do elementu Runbook

Obecnie nasz element Runbook uruchamia maszynę wirtualną trwale w nim zakodowaną, ale bardziej użyteczna byłaby możliwość wybrania maszyny wirtualnej po uruchomieniu elementu Runbook. Aby to umożliwić, dodamy do elementu Runbook parametry wejściowe.

1.  Dodaj parametry *VMName* i *ResourceGroupName* do elementu Runbook i użyj tych zmiennych z poleceniem cmdlet **Start AzureRmVM**, tak jak w przykładzie poniżej.  
    
    ```
    Param(
       [string]$VMName,
       [string]$ResourceGroupName
    )
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
     ```
<br> 
2.  Zapisz element Runbook, a następnie otwórz okienko testowania. Teraz możesz podać wartości dwóch zmiennych wejściowych, które zostaną użyte w teście.
3.  Zamknij okienko testowania.
4.  Kliknij pozycję **Opublikuj**, aby opublikować nową wersję elementu Runbook.
5.  Zatrzymaj maszynę wirtualną uruchomioną w poprzednim kroku.
6.  Kliknij pozycję **Uruchom**, aby uruchomić element Runbook. Wpisz wartości parametrów **VMName** i **ResourceGroupName** dla maszyny wirtualnej do uruchomienia.  
    ![Przekazywanie parametru](media/automation-first-runbook-textual-powershell/automation-pass-params.png)  
7.  Po zakończeniu działania elementu Runbook sprawdź, czy maszyna wirtualna została uruchomiona.

## Różnice w stosunku do przepływu pracy programu PowerShell

Elementy Runbook programu PowerShell mają taki sam cykl życia, możliwości i procesy zarządzania jak elementy Runbook przepływu pracy programu PowerShell, ale istnieją pewne różnice i ograniczenia:

1.  Elementy Runbook programu PowerShell działają szybko w porównaniu do elementów Runbook przepływu pracy programu PowerShell, ponieważ nie obejmują kroku kompilacji.
2.  Elementy Runbook przepływu pracy programu PowerShell obsługują punkty kontrolne, a dzięki temu ich działanie może być wznawiane w dowolnym punkcie elementu Runbook. Elementy Runbook programu PowerShell mogą wznawiać działanie tylko od początku.
3.  Elementy Runbook przepływu pracy programu PowerShell obsługują wykonywanie równoległe i szeregowe, natomiast elementy Runbook programu PowerShell mogą wykonywać polecenia tylko szeregowo.
4.  W elemencie Runbook przepływu pracy programu PowerShell działanie, polecenie lub blok skryptu może mieć własny obszar działania, a w elemencie Runbook programu PowerShell wszystkie elementy skryptu są uruchamiane w jednym obszarze działania. Istnieją również pewne [różnice składniowe](https://technet.microsoft.com/magazine/dn151046.aspx) między natywnym elementem Runbook programu PowerShell i elementem Runbook przepływu pracy programu PowerShell.

## Następne kroki

-   Aby rozpocząć pracę z graficznymi elementami Runbook, zobacz artykuł [My first graphical runbook](automation-first-runbook-graphical.md) (Mój pierwszy graficzny element Runbook).
-   Aby rozpocząć pracę z elementami Runbook przepływu pracy programu PowerShell, zobacz artykuł [My first PowerShell workflow runbook](automation-first-runbook-textual.md) (Mój pierwszy element Runbook przepływu pracy programu PowerShell).
-   Aby dowiedzieć się więcej na temat typów elementów Runbook, ich zalet i ograniczeń, zobacz [Azure Automation runbook types](automation-runbook-types.md) (Typy elementów Runbook usługi Azure Automation).
-   Aby uzyskać więcej informacji o funkcji obsługi skryptów programu PowerShell, zobacz [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) (Obsługa natywnych skryptów programu PowerShell w usłudze Azure Automation).



<!--HONumber=sep16_HO1-->


