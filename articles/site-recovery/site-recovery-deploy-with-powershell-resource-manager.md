---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V z programu PowerShell i usługa Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Automatyzowanie replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure z usługą Azure Site Recovery przy użyciu programu PowerShell i Menedżera zasobów Azure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: dbd562b73b0caecd0feb993bd6fb796dddb0438c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikowanie między maszynami wirtualnymi funkcji Hyper-V lokalnymi i Azure przy użyciu programu PowerShell i usługi Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klasyczny portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Omówienie
Usługa Azure Site Recovery przyczynia się do strategii biznesowej ciągłości i odzyskiwaniem po awarii odzyskiwania poprzez organizowanie replikacji, trybu failover i odzyskiwania maszyn wirtualnych w różnych scenariuszy wdrażania. Aby uzyskać pełną listę scenariuszy wdrażania, zobacz [Omówienie usługi Azure Site Recovery](site-recovery-overview.md).

Program Azure PowerShell jest moduł, który udostępnia polecenia cmdlet do zarządzania za pomocą środowiska Windows PowerShell. Może współpracować z dwóch typów modułów: moduł profilu Azure lub w module usługi Azure Resource Manager.

Lokacji odzyskiwania poleceń cmdlet programu PowerShell, dostępna przy użyciu programu Azure PowerShell dla usługi Azure Resource Manager pomóc chronić i odzyskiwać serwerów na platformie Azure.

W tym artykule opisano sposób wdrażania usługi Site Recovery do konfigurowania i organizowania ochrony serwera na platformie Azure za pomocą programu Windows PowerShell, wraz z usługą Azure Resource Manager. W przykładzie używane w tym artykule przedstawiono do ochrony, tryb failover i odzyskiwania maszyn wirtualnych na hoście funkcji Hyper-V do platformy Azure, przy użyciu programu Azure PowerShell z usługą Azure Resource Manager.

> [!NOTE]
> Polecenia cmdlet programu PowerShell odzyskiwania lokacji pozwalają obecnie skonfiguruj następujące opcje: jedna lokacja programu Virtual Machine Manager do innej lokacji programu Virtual Machine Manager na platformie Azure i lokacji funkcji Hyper-V do platformy Azure.
>
>

Nie trzeba być ekspertów PowerShell, aby użyć tego artykułu, ale należy zrozumieć podstawowe pojęcia, takie jak moduły, polecenia cmdlet i sesje. Aby uzyskać więcej informacji na temat programu Windows PowerShell, zobacz [Wprowadzenie do programu Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Można również uzyskać więcej informacji [przy użyciu programu Azure PowerShell z usługą Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Partnerzy firmy Microsoft, które są częścią programu Cloud Solution Provider (CSP) można konfigurować i zarządzać ochrony serwerów swoich klientów do ich klientom odpowiednich subskrypcji dostawcy usług Kryptograficznych (subskrypcje dzierżawy).
>
>

## <a name="before-you-start"></a>Przed rozpoczęciem
Upewnij się, że te wymagania wstępne zostały spełnione:

* A [Microsoft Azure](https://azure.microsoft.com/) konta. Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). Ponadto możesz przeczytać temat [cennik usługi Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Informacje o tej wersji i sposobu jego instalacji, zobacz [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) i [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modułów. Możesz uzyskać najnowsze wersje tych modułów [galerii programu PowerShell](https://www.powershellgallery.com/)

W tym artykule przedstawiono sposób korzystania z usługi Azure Resource Manager programu Azure Powershell do konfigurowania i ochrony serwerów zarządzania. Przykład używane w tym artykule przedstawiono sposób ochrony maszyny wirtualnej, uruchomiona na hoście funkcji Hyper-V do platformy Azure. Następujące wymagania wstępne są specyficzne dla tego przykładu. Dla bardziej złożonego zestawu wymagania dla różnych scenariuszy odzyskiwania lokacji zapoznaj się z dokumentacją, dotyczące tego scenariusza.

* Hosta funkcji Hyper-V z systemem Windows Server 2012 R2 lub Microsoft Hyper-V Server 2012 R2 zawierającego co najmniej jednej maszyny wirtualnej.
* Serwery funkcji Hyper-V jest połączony z Internetem bezpośrednio lub za pośrednictwem serwera proxy.
* Maszyny wirtualne, które chcesz chronić powinna być zgodna z [wymagania wstępne dotyczące maszyny wirtualnej](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-to-your-azure-account"></a>Krok 1: Zaloguj się do konta platformy Azure
1. Otwórz konsolę programu PowerShell i uruchom to polecenie, aby zalogować się do konta platformy Azure. Polecenie cmdlet powoduje wyświetlenie strony sieci web, która spowoduje wyświetlenie monitu o podanie poświadczeń konta.

        Login-AzureRmAccount

    Alternatywnie możesz także dołączyć poświadczeń konta jako parametr `Login-AzureRmAccount` polecenia cmdlet, za pomocą `-Credential` parametru.

    W przypadku dostawcy usług Kryptograficznych partnera pracy imieniu dzierżawcy określenia klienta dzierżawcy, przy użyciu nazwy domeny głównej dla identyfikatora dzierżawcy lub dzierżawcy.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Konto może zawierać kilka subskrypcji, więc należy skojarzyć subskrypcję, której chcesz użyć przy użyciu konta.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Sprawdź, czy subskrypcja jest zarejestrowane do używania Azure dostawców usług odzyskiwania i lokacji odzyskiwania, za pomocą następujących poleceń:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   W danych wyjściowych polecenia Jeśli **RegistrationState** ustawiono **zarejestrowanej**, możesz przejść do kroku 2. Jeśli nie, należy zarejestrować dostawcę Brak w ramach subskrypcji.

   Aby zarejestrować dostawcę usługi Azure Site Recovery i usług odzyskiwania, uruchom następujące polecenia:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Sprawdź, czy dostawców pomyślnie zarejestrowane za pomocą następujących poleceń: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` i `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-recovery-services-vault"></a>Krok 2: Konfigurowanie magazyn usług odzyskiwania
1. Utwórz grupę zasobów usługi Azure Resource Manager, w którym można będzie utworzyć magazyn, lub użyj istniejącej grupy zasobów. Można utworzyć nową grupę zasobów za pomocą następującego polecenia:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    gdzie zmienna $ResourceGroupName zawiera nazwę grupy zasobów, które chcesz utworzyć, a zmienna $Geo region platformy Azure, w którym można utworzyć grupy zasobów (na przykład "Brazylia Południowa").

    Można uzyskać listy grup zasobów w ramach subskrypcji, za pomocą `Get-AzureRmResourceGroup` polecenia cmdlet.
2. Utwórz nowy magazyn usług odzyskiwania Azure w następujący sposób:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Można pobrać listy istniejących magazynów za pomocą `Get-AzureRmRecoveryServicesVault` polecenia cmdlet.

> [!NOTE]
> Jeśli chcesz wykonywać operacje na magazynów usługi Site Recovery utworzony przy użyciu klasycznego portalu lub moduł programu PowerShell do zarządzania usługi Azure, można pobrać listę takich magazynów za pomocą `Get-AzureRmSiteRecoveryVault` polecenia cmdlet. Należy utworzyć nowy magazyn usług odzyskiwania dla wszystkich nowych operacji. Magazynów usługi Site Recovery, utworzony wcześniej są obsługiwane, ale nie ma najnowszych funkcji.
>
>

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Ustaw kontekst magazynu usług odzyskiwania
1. Ustaw kontekst magazynu, uruchamiając następujące polecenie:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Krok 4: Tworzenie lokacji funkcji Hyper-V i wygenerowanie nowego klucza rejestracji magazynu dla tej lokacji.
1. Utwórz nową witrynę funkcji Hyper-V w następujący sposób:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    To polecenie cmdlet uruchomi zadanie odzyskiwania lokacji, aby utworzyć lokację i zwraca obiekt zadania usługi Site Recovery. Poczekaj na zakończenie i sprawdź, czy zadania zadanie zostało ukończone pomyślnie.

    Można pobrać obiektu zadania, a tym samym sprawdzić bieżący stan zadania, za pomocą polecenia cmdlet Get-AzureRmSiteRecoveryJob.
2. Generowanie i Pobierz klucz rejestracji dla lokacji, w następujący sposób:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Skopiuj pobrany klucz do hosta funkcji Hyper-V. Należy klawisz, aby zarejestrować hosta funkcji Hyper-V do lokacji.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Krok 5: Instalowanie dostawcy usługi Azure Site Recovery i Agent usług odzyskiwania Azure na hoście funkcji Hyper-V
1. Pobierz Instalatora, aby uzyskać najnowszą wersję dostawcy z [Microsoft](https://aka.ms/downloaddra).
2. Uruchom Instalator na hoście funkcji Hyper-V, a na końcu instalacji przejdź do kroku rejestracji.
3. Po wyświetleniu monitu podaj klucza rejestracji pobranego lokacji i zakończyć rejestrację hosta funkcji Hyper-V do lokacji.
4. Sprawdź, czy host funkcji Hyper-V jest zarejestrowana do witryny za pomocą następującego polecenia:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Krok 6: Tworzenie zasad replikacji i powiązać ją z kontenera ochrony
1. Utwórz zasady replikacji w następujący sposób:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Sprawdź zadanie zwrócone, aby upewnić się, że tworzenie zasad replikacji powiedzie się.

   > [!IMPORTANT]
   > Określone konto magazynu musi należeć do tego samego regionu Azure co magazyn usług odzyskiwania i ma włączoną replikację geograficzną.
   >
   > * Jeśli określone konto magazynu odzyskiwania jest typu usługi Azure Storage (klasyczny), trybu failover chronionych maszyn odzyskać maszyny do IaaS platformy Azure (klasyczne).
   > * Jeśli określone konto magazynu odzyskiwania jest typu magazynu Azure (Azure Resource Manager), trybu failover chronionych maszyn odzyskać maszyny do IaaS platformy Azure (Azure Resource Manager).
   >
   >
2. Pobierz kontener ochrony odpowiadający danej lokacji, w następujący sposób:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Uruchom skojarzenie kontenera ochrony z zasadami replikacji w następujący sposób:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-$Policy zasad - PrimaryProtectionContainer $protectionContainer

   Poczekaj na ukończenie zadania skojarzenie i upewnij się, czy zakończyła się pomyślnie.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Włączanie ochrony dla maszyn wirtualnych
1. Pobierz jednostki ochrony odpowiadający maszyny Wirtualnej, który chcesz chronić w następujący sposób:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Rozpocznij ochronę maszyny wirtualnej, w następujący sposób:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > Określone konto magazynu musi należeć do tego samego regionu Azure co magazyn usług odzyskiwania i ma włączoną replikację geograficzną.
   >
   > * Jeśli określone konto magazynu odzyskiwania jest typu usługi Azure Storage (klasyczny), trybu failover chronionych maszyn odzyskać maszyny do IaaS platformy Azure (klasyczne).
   > * Jeśli określone konto magazynu odzyskiwania jest typu magazynu Azure (Azure Resource Manager), trybu failover chronionych maszyn odzyskać maszyny do IaaS platformy Azure (Azure Resource Manager).
   >
   > Jeśli chronisz maszyny Wirtualnej ma więcej niż jeden dysk dołączony do niej, określ dysk systemu operacyjnego za pomocą *OSDiskName* parametru.
   >
   >
3. Poczekaj, aż do przejścia chronionych po replikacji początkowej maszyn wirtualnych. Może to potrwać kilka minut, w zależności od czynników, takich jak ilość danych, które powinny być replikowane i dostępnej przepustowości nadrzędnego na platformie Azure. Stan zadania i StateDescription są aktualizowane, po osiągnięciu chronionych stan maszyny Wirtualnej.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Zaktualizuj właściwości odzyskiwania, na przykład rozmiaru roli maszyny Wirtualnej i sieci platformy Azure można dołączyć maszyny wirtualnej kart interfejsu sieciowego do podczas pracy awaryjnej.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Uruchom test trybu failover
1. Uruchom zadania testowego trybu failover, w następujący sposób:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Sprawdź, czy test zostanie utworzona maszyna wirtualna na platformie Azure. (Test trybu failover zadanie zostanie zawieszone, po utworzeniu testowej maszyny Wirtualnej na platformie Azure. Zadanie zostało ukończone przez czyszczenie utworzony artefaktów po wznowieniu zadania, jak pokazano w następnym kroku.)
3. Testowy tryb failover, należy wykonać w następujący sposób:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Następne kroki
[Dowiedz się więcej](https://msdn.microsoft.com/library/azure/mt637930.aspx) dotyczące usługi Azure Site Recovery za pomocą poleceń cmdlet programu PowerShell usługi Azure Resource Manager.
