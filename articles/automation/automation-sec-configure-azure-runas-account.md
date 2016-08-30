<properties
    pageTitle="Konfigurowanie konta Uruchom jako platformy Azure | Microsoft Azure"
    description="Samouczek przeprowadzający przez proces tworzenia, testowania i przykładowego użycia uwierzytelniania podmiotu zabezpieczeń w usłudze Automatyzacji Azure."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="service principal name, setspn, azure authentication"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/07/2016"
    ms.author="magoedte"/>

# Uwierzytelnianie elementów Runbook przy użyciu konta Uruchom jako platformy Azure
W tym temacie opisano, w jaki sposób można skonfigurować konto automatyzacji w portalu Azure za pomocą nowej funkcji konta Uruchom jako (określanej także jako nazwa główna usługi) na dostęp do zasobów usługi Azure Resource Manager w ramach subskrypcji za pomocą elementów Runbook automatyzacji.  Po utworzeniu nowego konta automatyzacji w portalu Azure automatycznie powoduje ono utworzenie nowej nazwy głównej usługi i domyślne przypisanie do roli Współautor kontroli dostępu opartej na rolach (RBAC) w subskrypcji.  Upraszcza to proces i pomaga szybko rozpocząć tworzenie i wdrażanie elementów Runbook dla różnych potrzeb automatyzacji.      

Za pomocą nazwy głównej usługi można:

* Zapewnić standardowy sposób uwierzytelniania w systemie Azure podczas zarządzania zasobami usługi Azure Resource Manager za pomocą elementów Runbook
* Zautomatyzować użycie globalnych elementów Runbook skonfigurowanych w alertach Azure


>[AZURE.NOTE] [Funkcja integracji alertu](../azure-portal/insights-receive-alert-notifications.md) platformy Azure z globalnymi elementami Runbook automatyzacji wymaga konta automatyzacji ze skonfigurowaną nazwą główną usługi. Można wybrać konto automatyzacji, które ma już zdefiniowaną przez użytkownika nazwę główną usługi lub wybrać opcję utworzenia nowego.



W tekście objaśniono sposób tworzenia konta automatyzacji z portalu Azure i aktualizowania konta za pomocą konta Uruchom jako przy użyciu programu PowerShell Azure oraz sposób uwierzytelniania za pomocą tej nazwy głównej usługi w elementach Runbook.  

## Tworzenie nowego konta automatyzacji w portalu Azure
W tej części należy wykonać poniższe kroki, aby utworzyć nowe konto usługi Automatyzacja Azure i nazwę główną usługi z portalu Azure.

>[AZURE.NOTE] Użytkownik wykonujący te czynności *musi* być członkiem roli Administratorzy subskrypcji.

1. Zaloguj się do portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Konta automatyzacji**.
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br>![Dodaj konto automatyzacji](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, jedną z nich określ dla nowego konta, podaj także nową lub istniejącą **grupę zasobów** i **lokalizację** centrum danych Azure.
6. Sprawdź, czy została wybrana wartość **Tak** dla opcji **Utwórz konto Azure Uruchom jako**, a następnie kliknij przycisk **Utwórz**.  

    ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/add-account-decline-create-runas-msg.png)

    >[AZURE.NOTE] Jeśli nie chcesz utworzyć nowego konta Uruchom jako, wybierając opcję **Nie**, w bloku **Dodaj konto automatyzacji** zobaczysz komunikat ostrzegawczy.  Gdy konto zostanie utworzone i przypisane do roli **Współautor** w subskrypcji, nie będzie ono miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji i dlatego nie będzie miało dostępu do zasobów w ramach subskrypcji.  Pozwoli to zapobiec sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów usługi Azure Resource Manager.

    ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

    >[AZURE.NOTE] Jeśli zostanie wyświetlony komunikat o błędzie odmowy uprawnień po kliknięciu przycisku **Utwórz**, będzie to spowodowane tym, że konto nie jest członkiem roli Administratorzy subskrypcji.  

7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### Zasoby dołączone
Po zakończeniu tworzenia konta automatyzacji automatycznie zostanie utworzonych kilka zasobów. Ich podsumowanie znajduje się w poniższej tabeli.

Zasób|Opis 
----|----
AzureAutomationTutorial Runbook|Przykładowy element Runbook, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, wyświetlając 10 pierwszych maszyn wirtualnych platformy Azure w ramach subskrypcji.
AzureRunAsCertificate|Zasób Certyfikat utworzony, gdy wybrano utworzenie konta Uruchom jako podczas tworzenia konta automatyzacji lub skorzystanie z poniższego skryptu programu PowerShell dla istniejącego konta.  Ten certyfikat ma roczny okres obowiązywania. 
AzureRunAsConnection|Zasób Połączenie utworzony, gdy wybrano utworzenie konta Uruchom jako podczas tworzenia konta automatyzacji lub skorzystanie z poniższego skryptu programu PowerShell dla istniejącego konta.
Moduły|15 modułów z poleceniami cmdlet dotyczących platformy Azure, programu PowerShell i automatyzacji, pozwalających na rozpoczęcie korzystania z elementów Runbook natychmiast.  

## Aktualizacja konta automatyzacji przy użyciu programu PowerShell
Poniższa procedura aktualizuje istniejące konto automatyzacji i tworzy nazwę główną usługi za pomocą programu PowerShell.  Ta procedura jest niezbędna, jeśli użytkownik utworzył konto, ale zrezygnował z tworzenia konta Uruchom jako.

Przed kontynuowaniem sprawdź, czy następujące czynności zostały wykonane:

1. Został pobrany i zainstalowany program [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855), jeśli korzystasz z systemu Windows 7.   
    Jeśli używasz systemu Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 lub Windows 7 z dodatkiem SP1, program [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) jest dostępny do zainstalowania.
2. Azure PowerShell 1.0. Informacje o tej wersji i sposobie jej instalowania można znaleźć w temacie [Sposób instalowania i konfigurowania programu PowerShell Azure](../powershell-install-configure.md). 
3. Utworzono konto automatyzacji.  To konto zostanie użyte jako wartości parametrów -AutomationAccountName i -ApplicationDisplayName w poniższym skrypcie.


Skrypt programu PowerShell skonfiguruje następujące elementy:

* Aplikacja Azure AD, która będzie uwierzytelniana przy użyciu certyfikatu z podpisem własnym, utworzy konto dla nazwy głównej usługi dla tej aplikacji w usłudze Azure AD i przypisze to konto w bieżącej subskrypcji do roli Współautor (można ją zmienić na rolę Właściciel lub inną).  Więcej informacji można znaleźć w artykule [Kontrola dostępu oparta na rolach w automatyzacji Azure](../automation/automation-role-based-access-control.md).  
* Zasób certyfikatu usługi Automatyzacja w ramach określonego konta automatyzacji o nazwie **AzureRunAsCertificate**, który posiada certyfikat używany w nazwie głównej usługi.
* Zasób połączenia usługi Automatyzacja w ramach określonego konta automatyzacji o nazwie **AzureRunAsConnection**, który zawiera identyfikator aplikacji, identyfikator dzierżawy, identyfikator subskrypcji i odcisk palca certyfikatu.  


### Uruchom skrypt programu PowerShell

1. Zapisz na komputerze poniższy skrypt.  W tym przykładzie zapisz plik pod nazwą **Nowy-NazwaGlownaUslugi.ps1**.  

    ```
    #Requires -RunAsAdministrator
    Param (
    [Parameter(Mandatory=$true)]
    [String] $ResourceGroup,

    [Parameter(Mandatory=$true)]
    [String] $AutomationAccountName,

    [Parameter(Mandatory=$true)]
    [String] $ApplicationDisplayName,

    [Parameter(Mandatory=$true)]
    [String] $SubscriptionId,

    [Parameter(Mandatory=$true)]
    [String] $CertPlainPassword,

    [Parameter(Mandatory=$false)]
    [int] $NoOfMonthsUntilExpired = 12
    )

    Login-AzureRmAccount
    Import-Module AzureRM.Resources
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    $CurrentDate = Get-Date
    $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
    $KeyId = (New-Guid).Guid
    $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")

    $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

    $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
    Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose

    $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
    $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

    $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
    $KeyCredential.StartDate = $CurrentDate
    $KeyCredential.EndDate= $EndDate
    $KeyCredential.KeyId = $KeyId
    $KeyCredential.Type = "AsymmetricX509Cert"
    $KeyCredential.Usage = "Verify"
    $KeyCredential.Value = $KeyValue

    # Use Key credentials
    $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential

    New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
    Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose

    $NewRole = $null
    $Retries = 0;
    While ($NewRole -eq $null -and $Retries -le 2)
    {
      # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
      Sleep 5
      New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
      Sleep 5
      $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
      $Retries++;
    }

    # Get the tenant id for this subscription
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1

    # Create the automation resources
    New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose

    # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    $ConnectionAssetName = "AzureRunAsConnection"
    Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId.SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
    ```
<br>
2. Na komputerze uruchom program **Windows PowerShell** z ekranu **Start** z podwyższonym poziomem uprawnień użytkownika.
3. Z poziomu powłoki wiersza polecenia programu PowerShell z podwyższonym poziomem uprawnień przejdź do folderu, który zawiera skrypt utworzony w kroku 1 i uruchom skrypt zmieniający wartości parametrów *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* i *-CertPlainPassword*.<br>

    ```
    .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
     -AutomationAccountName <NameofAutomationAccount> `
     -ApplicationDisplayName <DisplayNameofAutomationAccount> `
     -SubscriptionId <SubscriptionId> `
     -CertPlainPassword "<StrongPassword>"
    ```   
<br>

    >[AZURE.NOTE] Po wykonaniu skryptu otrzymasz monit do uwierzytelnienia w usłudze Azure.  *Musisz* zalogować się za pomocą konta administratora usługi w subskrypcji.  
<br>
4. Po pomyślnym ukończeniu działania skryptu, przejdź do następnej sekcji, aby przetestować i sprawdzić nową konfigurację poświadczeń.

### Sprawdź uwierzytelnianie
W następnej kolejności wykonaj mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nową nazwy głównej usługi. Jeśli nie możesz pomyślnie wykonać uwierzytelnienia, wróć do kroku 1 i sprawdź ponownie każdy z poprzednich kroków.    

1. Otwórz utworzone wcześniej konto automatyzacji w portalu Azure.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Utwórz nowy element Runbook, klikając przycisk **Dodaj element Runbook**, a następnie w bloku **Dodaj Runbook** wybierz opcję **Utwórz nowy element Runbook**.
4. Nadaj elementowi Runbook nazwę *Test-SecPrin-Runbook* i w polu **Typ elementu Runbook** wybierz opcję PowerShell.  Kliknij przycisk **Utwórz**, aby utworzyć element Runbook.
5. W bloku **Edytuj element Runbook programu PowerShell**, wklej w kanwie następujący kod:<br>

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```  
<br>
6. Zapisz element Runbook, klikając przycisk **Zapisz**.
7. Kliknij pozycję **Okienko testowania**, aby otworzyć blok **Test**.
8. Kliknij opcję **Uruchom**, aby rozpocząć test.
9. Zostanie utworzone [zadanie elementu Runbook](automation-runbook-execution.md) i w okienku zostanie wyświetlony jego stan.  
10. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a kolejnym *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
11. Po zakończeniu zadania elementu Runbook zostaną wyświetlone jego dane wyjściowe. W naszym przypadku możemy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/runbook-test-results.png)<br>
12. Zamknij blok **Test**, aby wrócić do kanwy.
13. Zamknij blok **Edytuj element Runbook programu PowerShell**.
14. Zamknij blok **Test-SecPrin-Runbook**.

## Przykładowy kod służący do uwierzytelniania przy użyciu Menedżera zasobów

Możesz skorzystać z podanego poniżej przykładowego kodu, pobranego z przykładowego elementu Runbook AzureAutomationTutorial, do uwierzytelniania za pomocą konta Uruchom jako do zarządzania zasobami Menedżera zasobów przy użyciu Twoich elementów Runbook. 

   ```
   $connectionName = "AzureRunAsConnection"
   $SubId = Get-AutomationVariable -Name 'SubscriptionId'
   try
   {
      # Get the connection "AzureRunAsConnection "
      $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

      "Logging in to Azure..."
      Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
      "Setting context to a specific subscription"   
      Set-AzureRmContext -SubscriptionId $SubId          
   }
   catch {
       if (!$servicePrincipalConnection)
       {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
       } else{
           Write-Error -Message $_.Exception
           throw $_.Exception
       }
   } 
   ```

Skrypt zawiera dwa dodatkowe wiersze kodu odwołujące się do kontekstu subskrypcji, dzięki czemu można łatwo pracować z wieloma subskrypcjami. Zasób zmiennej o nazwie SubscriptionId zawiera identyfikator subskrypcji i po wykonaniu polecenia cmdlet Add-AzureRmAccount, [polecenie cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) jest określane za pomocą parametru *-SubscriptionId*. Jeśli nazwa zmiennej jest zbyt ogólna, można ją skorygować w taki sposób, aby dołączyć prefiks lub zastosować inną konwencję nazewnictwa w celu ułatwienia identyfikacji do własnych celów. Alternatywnie można zastosować parametr -SubscriptionName zamiast -SubscriptionId z odpowiednim zasobem zmiennej.  

## Następne kroki
- Więcej informacji na temat nazw głównych usługi można znaleźć w temacie [Obiekty aplikacji i obiekty nazwę głównej usługi](../active-directory/active-directory-application-objects.md).
- Więcej informacji na temat kontroli dostępu opartej na rolach w usłudze Automatyzacja platformy Azure można znaleźć w temacie [Kontrola dostępu oparta na rolach w usłudze Automatyzacja platformy Azure](../automation/automation-role-based-access-control.md).



<!--HONumber=jun16_HO2-->


