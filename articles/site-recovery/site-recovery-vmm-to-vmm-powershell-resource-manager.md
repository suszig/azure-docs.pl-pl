---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V w programie VMM do lokacji dodatkowej przy użyciu programu PowerShell (usługi Azure Resource Manager) | Dokumentacja firmy Microsoft"
description: "Opisuje sposób wdrażania usługi Azure Site Recovery do organizowania replikacji, trybu failover i odzyskiwania maszyn wirtualnych funkcji Hyper-V w chmurach VMM do dodatkowej lokacji programu VMM przy użyciu programu PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: sutalasi
ms.openlocfilehash: c978c2e31e775f56824d765491f6d7b73648b8ae
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do dodatkowej lokacji programu VMM przy użyciu programu PowerShell (Resource Manager)

W tym artykule przedstawiono sposób automatyzacji typowych zadań, po skonfigurowaniu usługi Azure Site Recovery do replikowania maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center VMM do programu System Center VMM chmur w lokacji dodatkowej przy użyciu programu PowerShell.



## <a name="on-premises-prerequisites"></a>Lokalne wymagania wstępne
Oto, co musisz w lokacjach głównych i dodatkowych lokalnymi wdrożyć ten scenariusz:

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **VMM** |Zaleca się wdrożenie serwer VMM w lokacji głównej, a serwer programu VMM w lokacji dodatkowej.<br/><br/> Można także replikować między chmurami na jednym serwerze programu VMM. W tym celu należy co najmniej dwóch chmur skonfigurowane na serwerze programu VMM.<br/><br/> Serwery VMM powinna być uruchomiona co najmniej System Center 2012 SP1 z najnowszymi aktualizacjami.<br/><br/> Każdy serwer VMM musi mieć jedną lub więcej chmur skonfigurowane i wszystkich chmur musi mieć zestawu profilu pojemności funkcji Hyper-V. <br/><br/>Chmury muszą zawierać co najmniej jedną grupę hostów programu VMM. Program VMM serwery powinny mieć dostęp do Internetu. |
| **Funkcja Hyper-V** |Serwery funkcji Hyper-V musi działać co najmniej Windows Server 2012 z rolą funkcji Hyper-V i mieć zainstalowane najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V powinien zawierać co najmniej jedną maszynę wirtualną.<br/><br/>  Serwery hosta funkcji Hyper-V powinien znajdować się w grupach hostów w głównych i dodatkowych chmurach VMM.<br/><br/> Jeśli używasz funkcji Hyper-V w klastrze systemu Windows Server 2012 R2 należy zainstalować [zaktualizować 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Jeśli korzystasz z funkcji Hyper-V w klastrze na Uwaga systemu Windows Server 2012 broker klastra nie jest tworzony automatycznie w przypadku statycznej klastra oparte na adresie IP. Konieczne będzie ręczne skonfigurowanie brokera klastra. [Dowiedz się więcej](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Dostawca** |Podczas wdrażania usługi Site Recovery należy zainstalować dostawcę usługi Azure Site Recovery na serwerach VMM. Dostawca komunikuje się z usługą Site Recovery za pośrednictwem protokołu HTTPS 443 do organizowania replikacji. Dane replikacji między podstawowe i pomocnicze serwery funkcji Hyper-V za pośrednictwem sieci LAN lub połączenie sieci VPN.<br/><br/> Dostawca uruchomiony na serwerze VMM musi mieć dostęp do tych adresów URL: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Ponadto Zezwalaj na komunikację zapory z serwerów programu VMM do [zakresy IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i Zezwalaj na protokół HTTPS (port 443). |

### <a name="network-mapping-prerequisites"></a>Wymagania wstępne związane z mapowaniem sieci
Mapowanie sieci działa między sieciami maszyn wirtualnych VMM na serwerach VMM podstawowych i pomocniczych wykonać następujące czynności:

* Optymalnie umieścić po pracy awaryjnej maszyn wirtualnych repliki dodatkowej hosty funkcji Hyper-V.
* Połączenie maszyn wirtualnych repliki do odpowiedniej sieci maszyny Wirtualnej.
* Jeśli nie zostanie skonfigurowana sieć mapowania repliki maszyn wirtualnych nie będzie podłączona do żadnej sieci po pracy awaryjnej.
* Aby skonfigurować sieć mapowania podczas odzyskiwania lokacji w tym miejscu wdrożenia to co jest potrzebne:

  * Upewnij się, że maszyny wirtualne na źródłowym serwerze hosta funkcji Hyper-V są podłączone do sieci maszyny wirtualnej VMM. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
  * Sprawdź, czy dodatkowej chmury, które będzie używane do odzyskiwania ma skonfigurowane odpowiednie sieci maszyny Wirtualnej. Ta sieć maszyn wirtualnych powinna być połączona z siecią logiczną skojarzoną z chmurą dodatkowej.

Dowiedz się więcej na temat konfigurowania sieci w programie VMM w poniżej artykułów

* [Jak skonfigurować sieci logiczne w programie VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Konfigurowanie sieci Vmnetwork i bram w programie VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)


### <a name="powershell-prerequisites"></a>Wymagania wstępne programu PowerShell
Upewnij się, że masz przejść programu Azure PowerShell. Jeśli korzystasz już z programu PowerShell, należy uaktualnić do wersji 0.8.10 lub nowszej. Aby uzyskać informacje o konfigurowaniu programu PowerShell, zobacz [Przewodnik instalowania i konfigurowania programu Azure PowerShell](/powershell/azureps-cmdlets-docs). Po należy skonfigurować i skonfigurowaniu programu PowerShell można wyświetlić wszystkie dostępne polecenia cmdlet dla usługi [tutaj](/powershell/azure/overview).

Aby dowiedzieć się więcej o wskazówki, które mogą pomóc używać poleceń cmdlet, takich jak jak wartości parametru, dane wejściowe i dane wyjściowe są zazwyczaj obsługiwane w programie Azure PowerShell, zobacz [przewodnik Wprowadzenie do poleceń cmdlet Azure](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Krok 1: Ustaw subskrypcji
1. Z programu Azure powershell, zaloguj się do konta platformy Azure: za pomocą następujących poleceń cmdlet

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Pobierz listę subskrypcji. Będzie to również listę subscriptionIDs dla każdej subskrypcji. Zanotuj identyfikator subskrypcji subskrypcji, w którym chcesz utworzyć magazyn usług odzyskiwania    

        Get-AzureRmSubscription
3. Ustaw subskrypcji, w którym ma być tworzony, podając identyfikator subskrypcji magazynu usług odzyskiwania

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2. Tworzenie magazynu Usług odzyskiwania
1. Tworzenie grupy zasobów platformy Azure Resource Manager, jeśli nie masz już

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Utwórz nowy magazyn usług odzyskiwania, a następnie zapisz obiekt magazynu usług odzyskiwania w zmiennej (zostanie później użyty). Można również pobierać tworzenia post obiektu magazynu przy użyciu polecenia cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Ustaw kontekst magazynu usług odzyskiwania
1. Jeśli masz już utworzony magazyn, uruchom poniżej polecenie, aby uzyskać magazyn.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Ustaw kontekst magazynu, uruchamiając poniżej polecenia.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalowanie dostawcy usługi Azure Site Recovery
1. Na maszynie VMM należy utworzyć katalog, uruchamiając następujące polecenie:

       New-Item c:\ASR -type directory
2. Wyodrębnij pliki przy użyciu pobranego dostawcy, uruchamiając następujące polecenie

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Zainstaluj dostawcę przy użyciu następujących poleceń:

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

   Poczekaj na zakończenie instalacji.
4. Zarejestruj serwer w magazynie przy użyciu następującego polecenia:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a>Krok 5: Utwórz i Skojarz zasady replikacji
1. Utwórz zasady replikacji funkcji Hyper-V 2012 R2, uruchamiając następujące polecenie:

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
    > Chmury VMM może zawierać hosty funkcji Hyper-V z różnymi wersjami systemu Windows Server (jak wspomniano w wymagania wstępne funkcji Hyper-V), ale zasady replikacji jest właściwego dla wersji systemu operacyjnego. Jeśli masz różnych hostach z systemem w wersji systemów operacyjnych, następnie utwórz zasady replikacji osobne dla każdego typu wersji systemu operacyjnego. Na przykład jeśli masz pięć hostów z uruchomionym systemem Windows 2012 serwerów i trzech w systemie Windows Server 2012 R2, należy utworzyć dwie zasady replikacji — po jednej dla każdego typu wersji systemu operacyjnego.

1. Pobierz kontener ochronę podstawową (podstawowy chmury VMM) i odzyskiwania kontenera ochrony (odzyskiwania chmury VMM), uruchamiając następujące polecenia:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Pobrać zasady, który został utworzony w kroku 1 przy użyciu przyjaznej nazwy zasady

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Uruchom skojarzenie kontenera ochrony (w chmurze VMM) z zasadami replikacji:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Poczekaj na ukończenie zadania skojarzenia zasad. Można sprawdzić, czy ukończeniu zadania przy użyciu następującego fragmentu kodu programu PowerShell.

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

Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowanie aktywności](#monitor).

## <a name="step-6-configure-network-mapping"></a>Krok 6: Skonfigurowanie mapowania sieci
1. Pierwsze polecenie pobiera serwerów dla bieżącego magazynu Azure Site Recovery. Polecenie zapisuje serwerów Microsoft Azure Site Recovery w zmiennej tablicy $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Polecenie Pobierz sieci usługi Site Recovery dla serwera VMM źródłowym i docelowym serwerze VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Źródłowy serwer VMM może być pierwszy lub drugi w tablicy serwerów. Sprawdź nazwy serwerów programu VMM i odpowiednio uzyskać sieci


1. Końcowe polecenie cmdlet tworzy mapowanie między sieci podstawowej i odzyskiwania. Polecenie cmdlet określa sieci podstawowej jako pierwszy element $PrimaryNetworks i sieć odzyskiwania jako pierwszy element $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>Krok 7: Konfigurowanie mapowania magazynu
1. Poniżej polecenie pobiera listę klasyfikacji magazynu do zmiennej $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. Następujące polecenia Pobierz klasyfikacji źródła w zmiennej $SourceClassificaion i klasyfikacji docelowego w zmiennej $TargetClassification.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > Klasyfikacje źródłowym i docelowym może być dowolny element w tablicy. Zapoznaj się z danymi wyjściowymi poniżej polecenie, aby ustalić indeksu źródłowej i docelowej klasyfikacje w tablicy $storageclassifications.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - FriendlyName właściwości, identyfikator | Formatowanie tabeli


1. Poniżej polecenie cmdlet tworzy mapowanie między klasyfikacji źródłowego i docelowego klasyfikacji.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8. Włączanie ochrony dla maszyn wirtualnych
Po serwerów, chmur i sieci są skonfigurowane poprawnie, można włączyć ochrony dla maszyn wirtualnych w chmurze.

1. Aby włączyć ochronę, uruchom następujące polecenie, aby pobrać kontenera ochrony:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Pobierz jednostki ochrony (VM), uruchamiając następujące polecenie:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Włącz replikację dla maszyny Wirtualnej, uruchamiając następujące polecenie:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Testowanie wdrożenia
Aby przetestować wdrożenie można uruchomić test trybu failover dla jednej maszyny wirtualnej, lub utworzyć plan odzyskiwania uwzględniający wiele maszyn wirtualnych i testować tryb failover planu. Próba przejścia w tryb failover symuluje mechanizm pracy awaryjnej i odzyskiwania w sieci izolowanej.

> [!NOTE]
> Plan odzyskiwania można utworzyć aplikacji w portalu Azure.
>
>

Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowanie aktywności](#monitor).

### <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover
1. Uruchom poniżej polecenia cmdlet, aby pobrać maszyn wirtualnych do sieci maszyny Wirtualnej, z którą chcesz przetestować tryb failover.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Wykonaj test trybu failover maszyny wirtualnej, wykonując następujące czynności:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Wykonaj testowy tryb failover planu odzyskiwania, wykonując następujące czynności:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Realizacja planowanego trybu failover
1. Wykonaj planowany tryb failover maszyny wirtualnej, wykonując następujące czynności:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Wykonaj planowany tryb failover planu odzyskiwania, wykonując następujące czynności:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Uruchom nieplanowanego trybu failover
1. Wykonaj nieplanowany tryb failover maszyny wirtualnej, wykonując następujące czynności:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Wykonaj nieplanowany tryb failover planu odzyskiwania, wykonując następujące czynności:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name=monitor></a>Monitorowanie aktywności
Użyj następujących poleceń, aby monitorować aktywność. Należy pamiętać, że należy poczekać Between zadania do przetwarzania zakończyć.

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



## <a name="next-steps"></a>Następne kroki
[Dowiedz się więcej](/powershell/module/azurerm.recoveryservices.backup/#recovery) dotyczące usługi Azure Site Recovery za pomocą poleceń cmdlet programu PowerShell usługi Azure Resource Manager.
