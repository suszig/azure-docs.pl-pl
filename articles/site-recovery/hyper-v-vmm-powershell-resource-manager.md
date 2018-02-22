---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji dodatkowej przy użyciu programu PowerShell (usługi Azure Resource Manager) | Dokumentacja firmy Microsoft"
description: "Opisuje sposób replikacja maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacja dodatkowa programu VMM przy użyciu programu PowerShell (Resource Manager)"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej przy użyciu programu PowerShell (Resource Manager)

W tym artykule przedstawiono aby automatyzować czynności w przypadku replikacji maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center Virtual Machine Manager (VMM) do chmury programu VMM w dodatkowej lokacji lokalnej, za pomocą [usługi Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

- Przegląd [architektura scenariusza i składniki](hyper-v-vmm-architecture.md).
- Przegląd [spełnić wymagania dotyczące](site-recovery-support-matrix-to-sec-site.md) dla wszystkich składników.
- Upewnij się, że serwery VMM i hosty funkcji Hyper-V są zgodne z [spełnić wymagania dotyczące](site-recovery-support-matrix-to-sec-site.md).
- Sprawdź, czy mają być replikowane maszyny wirtualne są zgodne z [replikowane maszyny pomocy technicznej](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)


## <a name="prepare-for-network-mapping"></a>Przygotowanie do mapowania sieci

[Mapowanie sieci](hyper-v-vmm-network-mapping.md) mapowania między lokalnej sieci maszyny Wirtualnej programu VMM w źródłowej i docelowej chmury. Mapowanie wykonuje następujące czynności:

- Maszyny wirtualne łączy się z odpowiednich docelowych sieci maszyn wirtualnych po pracy awaryjnej. 
- Optymalnie umieszcza maszyny wirtualne repliki na serwerach hostów funkcji Hyper-V docelowych. 
- Jeśli nie skonfigurujesz mapowania sieci, repliki maszyn wirtualnych nie będzie podłączona do sieci maszyn wirtualnych po pracy awaryjnej.

Przygotowanie programu VMM w następujący sposób:

1. Upewnij się, że masz [sieci logicznych program VMM](https://docs.microsoft.com/system-center/vmm/network-logical) na serwerach VMM źródłowych i docelowych.
    - Sieci logicznej na serwerze źródłowym powinna być skojarzona z chmurą źródła, w którym znajdują się hosty funkcji Hyper-V.
    - Sieci logicznej na serwerze docelowym powinna być skojarzona z chmurą docelowej.
1. Upewnij się, że masz [sieci maszyn wirtualnych](https://docs.microsoft.com/system-center/vmm/network-virtual) na serwerach VMM źródłowych i docelowych. Sieci maszyn wirtualnych powinna być połączona z siecią logiczną, w każdej lokalizacji.
2. Połączenie maszyn wirtualnych na hostach funkcji Hyper-V źródła do źródłowej sieci maszyny Wirtualnej. 

## <a name="prepare-for-powershell"></a>Przygotowanie środowiska PowerShell

Upewnij się, że masz przejść programu Azure PowerShell.

- Jeśli korzystasz już z programu PowerShell, należy uaktualnić do wersji 0.8.10 lub nowszej.  [Dowiedz się więcej](/powershell/azureps-cmdlets-docs) o konfigurowaniu programu PowerShell.
- Po utworzeniu Konfigurowanie i skonfigurować programu PowerShell, przejrzyj [poleceń cmdlet usługi](/powershell/azure/overview).
- Aby dowiedzieć się więcej o korzystaniu z programu Azure PowerShell wartości parametrów, danych wejściowych i wyjściowych, przeczytaj [wprowadzenie](/powershell/azure/get-started-azureps) przewodnik.

## <a name="set-up-a-subscription"></a>Konfigurowanie subskrypcji
1. Zaloguj się do konta platformy Azure z programu Azure PowerShell:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Pobierz listę subskrypcji z subskrypcją identyfikatorów. Zanotuj identyfikator subskrypcji, w którym chcesz utworzyć magazyn usług odzyskiwania.   

        Get-AzureRmSubscription
3. Ustaw subskrypcji dla magazynu:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania
1. Utwórz grupę zasobów usługi Azure Resource Manager, jeśli nie masz.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Utwórz nowy magazyn usług odzyskiwania, a następnie zapisz obiekt magazynu w zmiennej, która będzie używana później: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Po jego utworzeniu, za pomocą polecenia cmdlet Get-AzureRMRecoveryServicesVault można pobrać obiektu magazynu.

## <a name="set-the-vault-context"></a>Ustaw kontekst magazynu
1. Pobrać istniejącego magazynu:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Ustaw kontekst magazynu:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Zainstaluj dostawcę usługi Azure Site Recovery
1. Na maszynie VMM należy utworzyć katalog, uruchamiając następujące polecenie:

       New-Item c:\ASR -type directory
2. Wyodrębnij pliki przy użyciu pobranego pliku instalacyjnego dostawcy: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Zainstaluj dostawcę i poczekaj na zakończenie instalacji:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Zarejestruj serwer w magazynie:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Utwórz i Skojarz zasady replikacji
1. Utwórz zasady replikacji (w tym przypadku dla funkcji Hyper-V 2012 R2) w następujący sposób:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > Chmury VMM może zawierać hosty funkcji Hyper-V z różnymi wersjami systemu Windows Server, ale zasady replikacji jest dla określonej wersji systemu operacyjnego. Jeśli masz inne hosty w różnych systemach operacyjnych, następnie utwórz zasady replikacji osobne dla każdego systemu. Na przykład jeśli masz pięć hostów z uruchomionym systemem Windows 2012 serwerów i trzech w systemie Windows Server 2012 R2, należy utworzyć dwie zasady replikacji — po jednej dla każdego typu systemu operacyjnego.

2. Pobierz kontener ochronę podstawową (podstawowy c VMM) i odzyskiwania kontenera ochrony (odzyskiwania chmury VMM):

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Pobieranie zasad replikacji, który został utworzony przy użyciu przyjaznej nazwy:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Uruchom skojarzenie kontenera ochrony (w chmurze VMM) z zasadami replikacji:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Poczekaj na ukończenie zadania skojarzenia zasad. Możesz sprawdzić, czy ukończeniu zadania przy użyciu następującego fragmentu kodu programu PowerShell:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Po zakończeniu przetwarzania zadania, uruchom następujące polecenie:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowania aktywności](#monitor-activity).

##  <a name="configure-network-mapping"></a>Konfiguracja mapowania sieci
1. Użyj tego polecenia, można pobrać serwerów dla bieżącego magazynu. Polecenie zapisuje serwerów usługi Azure Site Recovery w zmiennej tablicy $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Uruchom to polecenie, aby retrievet sieci na serwerze VMM źródłowym i docelowym serwerze VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Źródłowy serwer VMM może być pierwszym lub drugim w tablicy serwerów. Sprawdź nazwy serwera programu VMM i odpowiednio pobierania sieci.


3. To polecenie cmdlet tworzy mapowanie między sieci podstawowej i odzyskiwania. Określa sieci podstawowej jako pierwszy element $PrimaryNetworks oraz sieć odzyskiwania jako pierwszy element $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Włączyć ochronę maszyn wirtualnych
Po serwerów, chmur i sieci są skonfigurowane poprawnie, możesz włączyć ochronę maszyn wirtualnych w chmurze.

1. Aby włączyć ochronę, uruchom następujące polecenie, aby pobrać kontenera ochrony:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Pobierz jednostki ochrony (VM) w następujący sposób:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Włącz replikację dla maszyny Wirtualnej:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

Aby przetestować wdrożenie, możesz uruchomić test trybu failover dla jednej maszyny wirtualnej, lub utworzyć plan odzyskiwania, który zawiera wiele maszyn wirtualnych, a i testować tryb failover planu. Próba przejścia w tryb failover symuluje mechanizm pracy awaryjnej i odzyskiwania w sieci izolowanej.

1. Pobieranie maszyny Wirtualnej, w której maszyny wirtualne zostaną przełączone awaryjnie:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Wykonaj test trybu failover w następujący sposób:
    - Dla jednej maszyny Wirtualnej

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Plan odzyskiwania:

        $recoveryplanname = "test odzyskiwania plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowania aktywności](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Uruchomić tryb failover planowanych lub nieplanowanych

1. Planowany tryb failover należy wykonywać w następujący sposób:
    -  Dla jednej maszyny Wirtualnej:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
    - Dla planu odzyskiwania

        $recoveryplanname = "test odzyskiwania plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan
2. Nieplanowany tryb failover należy wykonywać w następujący sposób:
    - Dla jednej maszyny Wirtualnej
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

    - Plan odzyskiwania:

        $recoveryplanname = "test odzyskiwania plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Monitorowanie aktywności
Użyj następujących poleceń, aby monitorować aktywność failver. Należy pamiętać, że należy poczekać Between zadania do przetwarzania zakończyć.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Kolejne kroki

[Dowiedz się więcej](/powershell/module/azurerm.recoveryservices.backup/#recovery) temat odzyskiwania lokacji za pomocą poleceń cmdlet programu PowerShell usługi Azure Resource Manager.
