---
title: Konfigurowanie konta Uruchom jako platformy Azure | Microsoft Docs
description: "Samouczek przeprowadzający przez proces tworzenia, testowania i przykładowego użycia uwierzytelniania podmiotu zabezpieczeń w usłudze Automatyzacji Azure."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "nazwa główna usługi, setspn, uwierzytelnianie azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1e7529de2968b2745b42001cc16b54d70b2a5b86


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Uwierzytelnianie elementów Runbook przy użyciu konta Uruchom jako platformy Azure
W tym temacie opisano, w jaki sposób można skonfigurować konto automatyzacji w portalu Azure za pomocą funkcji konta Uruchom jako, aby uwierzytelniać elementy Runbook zarządzające zasobami w usłudze Azure Resource Manager lub Azure Service Management.

Po utworzeniu nowego konta usługi Automation portal Azure automatycznie tworzy:

* Konto Uruchom jako, które tworzy nową nazwę główną usługi w usłudze Azure Active Directory i certyfikat oraz przypisuje rolę Współautor kontroli dostępu opartej na rolach (RBAC), która będzie używana do zarządzania zasobami usługi Resource Manager za pomocą elementów Runbook.   
* Klasyczne konto Uruchom jako przez przekazanie certyfikatu zarządzania, który będzie używany do zarządzania usługą Azure Service Management lub klasycznymi zasobami przy użyciu elementów Runbook.  

Upraszcza to proces i pomaga szybko rozpocząć tworzenie i wdrażanie elementów Runbook dla różnych potrzeb automatyzacji.      

Używając konta Uruchom jako i klasycznego konta Uruchom jako, możesz:

* Zapewnić standardowy sposób uwierzytelniania w systemie Azure podczas zarządzania zasobami usługi Azure Resource Manager lub Azure Service Management z poziomu elementów Runbook w portalu Azure.  
* Zautomatyzować użycie globalnych elementów Runbook skonfigurowanych w alertach Azure.

> [!NOTE]
> [Funkcja integracji alertu](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) platformy Azure z globalnymi elementami Runbook automatyzacji wymaga konta usługi Automation ze skonfigurowanym kontem Uruchom jako lub klasycznym kontem Uruchom jako. Można wybrać konto usługi Automation, które ma już zdefiniowane konto Uruchom jako i klasyczne konto Uruchom jako, lub wybrać opcję utworzenia nowego.
> 
> 

Pokażemy, jak utworzyć konto usługi Automation z portalu Azure, zaktualizować konto usługi Automation przy użyciu programu PowerShell oraz uwierzytelnić się w elementach Runbook.

Jednak wcześniej musisz zrozumieć i wziąć pod uwagę kilka rzeczy.

1. Nie wpływa to na istniejące konta usługi Automation, które zostały już utworzone w modelu klasycznym lub modelu wdrażania usługi Resource Manager.  
2. Ma to zastosowanie tylko w przypadku kont usługi Automation utworzonych za pośrednictwem portalu Azure.  Próba utworzenia konta za pomocą portalu klasycznego nie spowoduje replikacji konfiguracji konta Uruchom jako.
3. Jeśli masz już elementy Runbook i zasoby (tj. harmonogramy, zmienne itp.) utworzone wcześniej w celu zarządzania zasobami klasycznymi i chcesz, aby były one uwierzytelniane przy użyciu nowego konta klasycznego Uruchom jako, musisz przeprowadzić ich migrację do nowego konta usługi Automation lub zaktualizować istniejące konto za pomocą poniższego skryptu programu PowerShell.  
4. Aby uwierzytelniać się przy użyciu nowego konta Uruchom jako i klasycznego konta Uruchom jako usługi Automation, musisz zmodyfikować istniejące elementy Runbook za pomocą poniższego przykładowego kodu.  **Pamiętaj**, że konto Uruchom jako służy do uwierzytelniania względem zasobów usługi Resource Manager za pomocą nazwy głównej usługi opartej na certyfikatach, natomiast klasyczne konto Uruchom jako służy do uwierzytelniania w odniesieniu do zasobów usługi Service Management za pomocą certyfikatu zarządzania.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Tworzenie nowego konta automatyzacji w portalu Azure
W tej części wykonasz poniższe kroki, aby utworzyć nowe konto usługi Azure Automation z portalu Azure.  Spowoduje to utworzenie zarówno konta Uruchom jako, jak i klasycznego konta Uruchom jako.  

> [!NOTE]
> Użytkownik wykonujący te czynności *musi* być członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji, która udziela dostępu do subskrypcji użytkownikowi.  Użytkownik musi być również dodany jako użytkownik do subskrypcji domyślnych usługi Active Directory; konto nie musi być przypisane do roli uprzywilejowanej.
> 
> 

1. Zaloguj się do portalu Azure przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
2. Wybierz opcję **Konta automatyzacji**.
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br>![Dodawanie konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Jeśli w bloku **Dodaj konto automatyzacji** widzisz poniższe ostrzeżenie, jest to spowodowane tym, że Twoje konto nie jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.<br>![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, określ jedną z nich dla nowego konta, podaj także nową lub istniejącą **grupę zasobów** i **lokalizację** centrum danych Azure.
6. Sprawdź, czy została wybrana wartość **Tak** dla opcji **Utwórz konto Azure Uruchom jako**, a następnie kliknij przycisk **Utwórz**.  
   
   > [!NOTE]
   > Jeśli wybrano opcję **Nie**, aby nie tworzyć konta Uruchom jako, w bloku **Dodaj konto automatyzacji** zostanie wyświetlony komunikat ostrzegawczy.  Gdy konto zostanie utworzone w portalu Azure, nie będzie ono miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji klasycznej lub subskrypcji usługi Resource Manager i dlatego nie będzie miało dostępu do zasobów w ramach subskrypcji.  Pozwoli to zapobiec sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów w tych modelach wdrożenia.
   > 
   > ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Gdy nie została utworzona nazwa główna usługi, nie można przypisać roli Współautor.
   > 
   > 
7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="resources-included"></a>Zasoby dołączone
Po pomyślnym utworzeniu konta usługi Automation automatycznie zostanie utworzonych kilka zasobów.  Poniższa tabela zawiera podsumowanie zasobów dla konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureAutomationTutorial Runbook |Przykładowy element Runbook programu PowerShell, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureAutomationTutorialScript Runbook |Przykładowy element Runbook programu PowerShell, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureRunAsCertificate |Zasób Certyfikat utworzony automatycznie podczas tworzenia konta automatyzacji lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta.  Umożliwia uwierzytelnianie za pomocą platformy Azure, aby można było zarządzać zasobami usługi Azure Resource Manager z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureRunAsConnection |Zasób Połączenie utworzony automatycznie podczas tworzenia konta automatyzacji lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta. |

Poniższa tabela zawiera podsumowanie zasobów dla klasycznego konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Przykładowy element Runbook, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicAutomationTutorial Script Runbook |Przykładowy element Runbook, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicRunAsCertificate |Automatycznie utworzony zasób Certyfikat, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureClassicRunAsConnection |Automatycznie utworzony zasób Połączenie, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook. |

## <a name="verify-run-as-authentication"></a>Sprawdzanie uwierzytelniania Uruchom jako
W następnej kolejności wykonamy mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego konta Uruchom jako.     

1. Otwórz utworzone wcześniej konto automatyzacji w portalu Azure.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich zasobów dostępnych w tej grupie zasobów.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu Runbook **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Sprawdzanie klasycznego uwierzytelniania Uruchom jako
W następnej kolejności wykonamy mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego klasycznego konta Uruchom jako.     

1. Otwórz utworzone wcześniej konto automatyzacji w portalu Azure.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureClassicAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich klasycznych maszyn wirtualnych w subskrypcji.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu Runbook **AzureClassicAutomationTutorialScript**.

## <a name="update-an-automation-account-using-powershell"></a>Aktualizacja konta automatyzacji przy użyciu programu PowerShell
W tym miejscu udostępniamy opcję aktualizacji istniejącego konta usługi Automation przy użyciu programu PowerShell, gdy:

1. Użytkownik utworzył konto usługi Automation, ale zrezygnował z tworzenia konta Uruchom jako
2. Użytkownik już ma konto usługi Automation do zarządzania zasobami usługi Resource Manager i chce zaktualizować je, aby umożliwiało uwierzytelnianie elementu Runbook za pomocą konta Uruchom jako
3. Użytkownik już ma konto usługi Automation do zarządzania zasobami klasycznymi i chce zaktualizować je, aby używać klasycznego konta Uruchom jako, zamiast tworzenia nowego konta i migrowania do niego elementów Runbook i zasobów   

Przed kontynuowaniem sprawdź, czy następujące czynności zostały wykonane:

1. Został pobrany i zainstalowany program [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855), jeśli korzystasz z systemu Windows 7.   
    Jeśli używasz systemu Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 lub Windows 7 z dodatkiem SP1, program [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) jest dostępny do zainstalowania.
2. Azure PowerShell 1.0. Informacje o tej wersji i sposobie jej instalowania można znaleźć w temacie [Sposób instalowania i konfigurowania programu PowerShell Azure](../powershell-install-configure.md).
3. Utworzono konto automatyzacji.  To konto zostanie użyte jako wartości parametrów -AutomationAccountName i -ApplicationDisplayName w obu poniższych skryptach.

Aby uzyskać wartości dla parametrów *SubscriptionID*, *ResourceGroup* i *AutomationAccountName*, które są wymagane w skryptach, w portalu Azure wybierz konto usługi Automation z bloku **Konto usługi Automation**, a następnie wybierz pozycję **Wszystkie ustawienia**.  W bloku **Wszystkie ustawienia** w obszarze **Ustawienia konta** wybierz pozycję **Właściwości**.  W bloku **Właściwości** możesz zauważyć poniższe wartości.<br> ![Konto usługi Automation — właściwości](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Tworzenie skryptu programu PowerShell konta Uruchom jako
Poniższy skrypt programu PowerShell skonfiguruje następujące elementy:

* Aplikacja Azure AD, która będzie uwierzytelniana przy użyciu certyfikatu z podpisem własnym, utworzy konto dla nazwy głównej usługi dla tej aplikacji w usłudze Azure AD i przypisze to konto w bieżącej subskrypcji do roli Współautor (można ją zmienić na rolę Właściciel lub inną).  Więcej informacji można znaleźć w artykule [Kontrola dostępu oparta na rolach w automatyzacji Azure](automation-role-based-access-control.md).
* Zasób certyfikatu usługi Automatyzacja w ramach określonego konta automatyzacji o nazwie **AzureRunAsCertificate**, który posiada certyfikat używany przez nazwę główną usługi.
* Zasób połączenia usługi Automatyzacja w ramach określonego konta automatyzacji o nazwie **AzureRunAsConnection**, który zawiera identyfikator aplikacji, identyfikator dzierżawy, identyfikator subskrypcji i odcisk palca certyfikatu.    

Poniższa procedura przeprowadzi Cię przez proces wykonywania skryptu.

1. Zapisz na komputerze poniższy skrypt.  W tym przykładzie zapisz plik pod nazwą **Nowy-NazwaGlownaUslugi.ps1**.  
   
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
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
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
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
2. Na komputerze uruchom program **Windows PowerShell** z ekranu **Start** z podwyższonym poziomem uprawnień użytkownika.
3. Z poziomu powłoki wiersza polecenia programu PowerShell z podwyższonym poziomem uprawnień przejdź do folderu, który zawiera skrypt utworzony w kroku 1 i uruchom skrypt zmieniający wartości parametrów *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* i *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Po wykonaniu skryptu otrzymasz monit do uwierzytelnienia w usłudze Azure. Musisz zalogować się przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
   > 
   > 
   
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
   <br>

Po pomyślnym ukończeniu działania skryptu skorzystać z [przykładowego kodu](#sample-code-to-authenticate-with-resource-manager-resources) poniżej, aby uwierzytelnić się za pomocą zasobów usługi Resource Manager i sprawdzić poprawność konfiguracji poświadczeń.

### <a name="create-classic-run-as-account-powershell-script"></a>Tworzenie skryptu programu PowerShell klasycznego konta Uruchom jako
Poniższy skrypt programu PowerShell skonfiguruje następujące elementy:

* Zasób certyfikatu usługi Automation w ramach określonego konta automatyzacji o nazwie **AzureClassicRunAsCertificate**, który posiada certyfikat używany do uwierzytelniania elementów Runbook.
* Zasób połączenia usługi Automation w ramach określonego konta automatyzacji o nazwie **AzureClassicRunAsConnection**, który zawiera nazwę subskrypcji, identyfikator subskrypcji i nazwę zasobu certyfikatu.

Skrypt utworzy certyfikat zarządzania z podpisem własnym i zapisze go w folderze plików tymczasowych na komputerze w ramach profilu użytkownika służącego do wykonania sesji programu PowerShell — *%USERPROFILE%\AppData\Local\Temp*.  Po wykonaniu skryptu musisz przekazać certyfikat zarządzania Azure do magazynu zarządzania dla subskrypcji, w ramach której zostało utworzone konto usługi Automation.  Poniższa procedura przeprowadzi Cię przez proces wykonywania skryptu i przekazywania certyfikatu.  

1. Zapisz na komputerze poniższy skrypt.  W tym przykładzie zapisz plik pod nazwą **New-AzureClassicRunAsAccount.ps1**.
   
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
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose
   
        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues
   
        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"
2. Na komputerze uruchom program **Windows PowerShell** z ekranu **Start** z podwyższonym poziomem uprawnień użytkownika.  
3. Z poziomu powłoki wiersza polecenia programu PowerShell z podwyższonym poziomem uprawnień przejdź do folderu, który zawiera skrypt utworzony w kroku 1 i uruchom skrypt zmieniający wartości parametrów *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* i *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Po wykonaniu skryptu otrzymasz monit do uwierzytelnienia w usłudze Azure. Musisz zalogować się przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
   > 
   > 
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"

Po pomyślnym ukończeniu działania skryptu musisz skopiować certyfikat utworzony w folderze **Temp** Twojego profilu użytkownika.  Postępuj zgodnie z instrukcjami [przekazywania certyfikatu interfejsu API zarządzania](../azure-api-management-certs.md) do klasycznego portalu Azure, a następnie skorzystaj z [przykładowego kodu](#sample-code-to-authenticate-with-service-management-resources), aby sprawdzić poprawność konfiguracji poświadczeń za pomocą zasobów usługi Service Management.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Przykładowy kod służący do uwierzytelniania przy użyciu Menedżera zasobów
Możesz skorzystać z podanego poniżej przykładowego kodu, pobranego z przykładowego elementu Runbook **AzureAutomationTutorialScript**, do uwierzytelniania za pomocą konta Uruchom jako do zarządzania zasobami Menedżera zasobów przy użyciu Twoich elementów Runbook.   

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


Skrypt zawiera dwa dodatkowe wiersze kodu odwołujące się do kontekstu subskrypcji, dzięki czemu można łatwo pracować z wieloma subskrypcjami. Zasób zmiennej o nazwie SubscriptionId zawiera identyfikator subskrypcji i po wykonaniu polecenia cmdlet Add-AzureRmAccount, [polecenie cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) jest określane za pomocą parametru *-SubscriptionId*. Jeśli nazwa zmiennej jest zbyt ogólna, można ją skorygować w taki sposób, aby dołączyć prefiks lub zastosować inną konwencję nazewnictwa w celu ułatwienia identyfikacji do własnych celów. Alternatywnie można zastosować parametr -SubscriptionName zamiast -SubscriptionId z odpowiednim zasobem zmiennej.  

Zwróć uwagę, że polecenie cmdlet służące do uwierzytelniania w elemencie Runbook — **Add-AzureRmAccount** — używa zestawu parametrów *ServicePrincipalCertificate*.  Uwierzytelnia się ono za pomocą certyfikatu nazwy głównej usługi, a nie poświadczeń.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Przykładowy kod służący do uwierzytelniania przy użyciu zasobów usługi Service Management
Możesz skorzystać z podanego poniżej zaktualizowanego przykładowego kodu, pobranego z przykładowego elementu Runbook **AzureClassicAutomationTutorialScript**, do uwierzytelniania za pomocą klasycznego konta Uruchom jako do zarządzania klasycznymi zasobami przy użyciu Twoich elementów Runbook.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat nazw głównych usługi można znaleźć w temacie [Obiekty aplikacji i obiekty nazwę głównej usługi](../active-directory/active-directory-application-objects.md).
* Więcej informacji na temat kontroli dostępu opartej na rolach w usłudze Automatyzacja platformy Azure można znaleźć w temacie [Kontrola dostępu oparta na rolach w usłudze Automatyzacja platformy Azure](automation-role-based-access-control.md).
* Aby uzyskać więcej informacji na temat certyfikatów i usług Azure, zobacz [Omówienie certyfikatów usług w chmurze platformy Azure](../cloud-services/cloud-services-certs-create.md)




<!--HONumber=Nov16_HO2-->


