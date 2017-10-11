---
title: "Replikowanie maszyn wirtualnych funkcji Hyper-V do platformy Azure w klasycznym portalu przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Automatyzowanie replikacji maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM przy użyciu usługi Site Recovery i programu PowerShell w klasycznym portalu"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 581daaaa5cc0cf8be782f834c6bdb3f27ee413fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-vms-to-azure-with-powershell-in-the-classic-portal"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V do platformy Azure przy użyciu programu PowerShell w klasycznym portalu
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasyczny portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell — model klasyczny](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Omówienie
Usługa Azure Site Recovery przyczynia się do strategii biznesowej ciągłości i odzyskiwaniem po awarii odzyskiwania (BCDR) poprzez organizowanie replikacji, trybu failover i odzyskiwania maszyn wirtualnych w różnych scenariuszy wdrażania. Pełną listę wdrażania scenariuszy dla [Omówienie usługi Azure Site Recovery](site-recovery-overview.md).

W tym artykule przedstawiono sposób automatyzacji typowych zadań, które należy wykonać podczas konfigurowania usługi Azure Site Recovery do replikowania maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center VMM do magazynu platformy Azure przy użyciu programu PowerShell.

Artykuł zawiera wymagania wstępne dla scenariusza i przedstawiono sposób konfigurowania magazynu usługi Site Recovery, zainstaluj dostawcę usługi Azure Site Recovery na źródłowym serwerze programu VMM, Zarejestruj serwer w magazynie, dodawania konta magazynu platformy Azure, zainstaluj agenta usług odzyskiwania Azure na serwerach hostów funkcji Hyper-V, skonfigurować ustawienia ochrony dla chmur programu VMM, które będą stosowane do wszystkich chronionych maszyn wirtualnych , a następnie włącz ochronę tych maszyn wirtualnych. Aby zakończyć, przetestuj tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.

Jeśli napotkasz problem ze skonfigurowaniem w tym scenariuszu Opublikuj swoje pytania na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia.
>
>

## <a name="before-you-start"></a>Przed rozpoczęciem
Upewnij się, że te wymagania wstępne zostały spełnione:

### <a name="azure-prerequisites"></a>Wymagania wstępne platformy Azure
* Będziesz potrzebować konta platformy [Microsoft Azure](https://azure.microsoft.com/). Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* Będziesz potrzebować konta magazynu Azure do przechowywania replikowanych danych. Konto musi włączyć replikację geograficzną. Powinien znajdować się w tym samym regionie co magazyn usługi Azure Site Recovery i być skojarzone z tą samą subskrypcją. [Dowiedz się więcej na temat usługi Azure storage](../storage/common/storage-introduction.md).
* Należy się upewnić, że maszyny wirtualne, które mają być chronione są zgodne z [wymagania wstępne maszyny wirtualnej platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>Wymagania wstępne programu VMM
* Należy na serwerze programu VMM działa na System Center 2012 R2.
* Będziesz potrzebować co najmniej jednej chmury na serwerze programu VMM, który chcesz chronić. Chmura powinna zawierać:
  * Co najmniej jedną grupę hostów programu VMM.
  * Serwery hosta funkcji Hyper-V lub klastry w każdej grupie hostów.
  * Co najmniej jednej maszyny wirtualnej na serwerze źródłowym funkcji Hyper-V.

### <a name="hyper-v-prerequisites"></a>Wymagania wstępne funkcji Hyper-V
* Serwery hosta funkcji Hyper-V musi działać co najmniej **systemu Windows Server 2012** z rolą funkcji Hyper-V lub **Microsoft Hyper-V Server 2012** i mieć zainstalowane najnowsze aktualizacje.
* Jeśli używasz funkcji Hyper-V w klastrze, broker klastra nie jest tworzony automatycznie, jeśli masz klaster oparty na statycznym adresie IP. Konieczne będzie ręczne skonfigurowanie brokera klastra. W tym celu w Menedżerze serwera > Menedżera klastra trybu Failover, kliknij pozycję Połącz się z klastrem **Konfiguruj rolę** i wybierz **brokera funkcji Hyper-V Replica** w **wybierz rolę** ekran Kreatora wysokiej dostępności.
* Dowolnego serwera hosta funkcji Hyper-V lub klastra, dla którego chcesz zarządzać ochrony musi być uwzględniona w chmurze VMM.

### <a name="network-mapping-prerequisites"></a>Wymagania wstępne związane z mapowaniem sieci
Gdy chronisz maszyny wirtualne na platformie Azure, mapowanie sieci działa między źródłowymi sieciami maszyny wirtualnej na źródłowym serwerze programu VMM a docelowymi sieciami platformy Azure. Dzięki temu dostępne są następujące korzyści:

* Wszystkie komputery, które nie przejdą pomyślnie za pośrednictwem tej sieci można połączyć ze sobą, niezależnie od tego, jakiego planu odzyskiwania znajdują się w.
* Jeśli brama sieci została skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne mogą łączyć się z innymi lokalnymi maszynami wirtualnymi.
* Jeśli nie skonfigurujesz mapowania sieci, tylko maszyny wirtualne, które przechodzą w tryb failover w tym samym planie odzyskiwania, będą mogły się ze sobą łączyć po przejściu w tryb failover na platformie Azure.

Jeśli chcesz wdrożyć mapowanie sieci, musisz upewnić się, że spełnione są następujące warunki:

* Maszyny wirtualne, które chcesz chronić na źródłowym serwerze programu VMM, powinny być połączone z siecią maszyn wirtualnych. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
* Sieć platformy Azure, z którą replikowane maszyny wirtualne mogą łączyć się po przejściu w tryb failover na platformie Azure. Tę sieć wybierzesz podczas przejścia w tryb failover. Sieć powinna znajdować się w tym samym regionie co subskrypcja usługi Azure Site Recovery.

### <a name="powershell-prerequisites"></a>Wymagania wstępne programu PowerShell
Upewnij się, że masz przejść programu Azure PowerShell. Jeśli korzystasz już z programu PowerShell, należy uaktualnić do wersji 0.8.10 lub nowszej. Aby uzyskać informacje o konfigurowaniu programu PowerShell, zobacz [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs). Po należy skonfigurować i skonfigurowaniu programu PowerShell można wyświetlić wszystkie dostępne polecenia cmdlet dla usługi [tutaj](/powershell/azure/overview).

Aby dowiedzieć się więcej o wskazówki, które mogą pomóc używać poleceń cmdlet, takich jak jak wartości parametru, dane wejściowe i dane wyjściowe są zazwyczaj obsługiwane w programie Azure PowerShell, zobacz [wprowadzenie do poleceń cmdlet Azure](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Krok 1: Ustaw subskrypcji
W programie PowerShell Uruchom te polecenia cmdlet:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Zastąp elementy wewnątrz "< >" konkretnych informacji.

## <a name="step-2-create-a-site-recovery-vault"></a>Krok 2: Tworzenie magazynu usługi Site Recovery
W programie PowerShell Zastąp elementy wewnątrz "< >" konkretnych informacji i uruchom następujące polecenia:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Krok 3: Generowanie klucza rejestracji magazynu
Wygeneruj klucz rejestracji magazynu. Po pobraniu dostawcy usługi Azure Site Recovery i zainstalowaniu go na serwerze programu VMM użyjesz tego klucza do zarejestrowania serwera programu VMM w magazynie.

1. Pobierz plik ustawienia magazynu i Ustaw kontekst:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Ustaw kontekst magazynu, uruchamiając następujące polecenia:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalowanie dostawcy usługi Azure Site Recovery
1. Na maszynie VMM należy utworzyć katalog, uruchamiając następujące polecenie:

   ```

   pushd C:\ASR\

   ```
2. Wyodrębnij pliki przy użyciu pobranego dostawcy, uruchamiając następujące polecenie

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Zainstaluj dostawcę przy użyciu następujących poleceń:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Poczekaj na zakończenie instalacji.
4. Zarejestruj serwer w magazynie przy użyciu następującego polecenia:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Tworzenie konta magazynu platformy Azure
Jeśli nie masz konta magazynu platformy Azure, Utwórz konto włączona replikacja geograficzna, uruchamiając następujące polecenie:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Należy pamiętać, że konto magazynu musi znajdować się w tym samym regionie co usługa Azure Site Recovery i musi być skojarzony z tą samą subskrypcją.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Zainstaluj agenta usług odzyskiwania Azure
W portalu Azure Zainstaluj agenta usług odzyskiwania Azure na każdym serwerze hosta funkcji Hyper-V znajdujących się w chmurach programu VMM, który chcesz chronić.

Uruchom następujące polecenie na wszystkich hostach VMM:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurowanie chmury ustawienia ochrony
1. Utwórz profil ochrony chmury Azure, uruchamiając następujące polecenie:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Pobierz kontenera ochrony, uruchamiając następujące polecenia:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Uruchom skojarzenie kontenera ochrony z chmurą:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. Po zakończeniu zadania, uruchom następujące polecenie:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. Po zakończeniu przetwarzania zadania, uruchom następujące polecenie:

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



Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowanie aktywności](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Konfigurowanie mapowania sieci
Przed rozpoczęciem mapowania sieci należy się upewnić, że maszyny wirtualne na źródłowym serwerze programu VMM są połączone z siecią maszyny wirtualnej. Ponadto należy utworzyć jeden lub więcej sieci wirtualnych platformy Azure. Wiele sieci maszyn wirtualnych można mapować do pojedynczej sieci platformy Azure.

Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej zostanie podłączona do tej docelowej podsieci po przejściu w tryb failover. Jeśli nie istnieje docelowa podsieć o pasującej nazwie, maszyny wirtualnej zostaną podłączone do pierwszej podsieci w sieci.

Pierwsze polecenie pobiera serwerów dla bieżącego magazynu Azure Site Recovery. Polecenie zapisuje serwerów Microsoft Azure Site Recovery w zmiennej tablicy $Servers.

    $Servers = Get-AzureSiteRecoveryServer


Drugie polecenie pobiera sieci odzyskiwania lokacji dla pierwszego serwera w tablicy $Servers. Polecenie zapisuje sieci w zmiennej $Networks.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Trzecie polecenie pobiera subskrypcji platformy Azure za pomocą polecenia cmdlet Get-AzureSubscription, a następnie zapisanie tej wartości w zmiennej $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Polecenie czwarty pobiera sieci wirtualnych platformy Azure przy użyciu polecenia cmdlet Get-AzureVNetSite, a następnie tę wartość w zmiennej $AzureVmNetworks.

    $AzureVmNetworks = Get-AzureVNetSite



Końcowe polecenie cmdlet tworzy mapowanie między sieci podstawowej i sieci maszyny wirtualnej platformy Azure. Polecenie cmdlet określa sieci podstawowej jako pierwszy element $Networks. Polecenie cmdlet określa sieć maszyny wirtualnej jako pierwszy element $AzureVmNetworks za pomocą jego identyfikatora. Polecenie zawiera swój identyfikator subskrypcji platformy Azure.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Włączanie ochrony dla maszyn wirtualnych
Po poprawnym skonfigurowaniu serwerów, chmur i sieci można włączyć ochronę dla maszyn wirtualnych w chmurze. Pamiętaj o następujących kwestiach:

Maszyny wirtualne muszą spełniać [wymagania wstępne maszyny wirtualnej platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Aby włączyć ochronę, należy ustawić właściwości systemu operacyjnego i dysku systemu operacyjnego dla maszyny wirtualnej. Podczas tworzenia maszyny wirtualnej w programie VMM przy użyciu szablonu maszyny wirtualnej można ustawić właściwości. Można również ustawić te właściwości dla istniejących maszyn wirtualnych na kartach **Ogólne** i **Konfiguracja sprzętu** właściwości maszyny wirtualnej. Jeśli nie ustawisz tych właściwości w programie VMM, możesz skonfigurować je w portalu Azure Site Recovery.

1. Aby włączyć ochronę, uruchom następujące polecenie, aby pobrać kontenera ochrony:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-Name $CloudName
2. Pobierz jednostki ochrony (VM), uruchamiając następujące polecenie:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. Włącz DR dla maszyny Wirtualnej, uruchamiając następujące polecenie:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Testowanie wdrożenia
Aby przetestować wdrożenie można uruchomić test trybu failover dla jednej maszyny wirtualnej, lub utworzyć plan odzyskiwania uwzględniający wiele maszyn wirtualnych i testować tryb failover planu. Próba przejścia w tryb failover symuluje mechanizm pracy awaryjnej i odzyskiwania w sieci izolowanej. Należy pamiętać, że:

* Jeśli chcesz nawiązać połączenie z maszyną wirtualną na platformie Azure przy użyciu Pulpitu zdalnego po przejściu w tryb failover, włącz funkcję Podłączanie pulpitu zdalnego na maszynie wirtualnej przed rozpoczęciem próby przejścia w tryb failover.
* Po przejściu w tryb failover użyjesz publicznego adresu IP nawiązywania połączenia z maszyną wirtualną na platformie Azure przy użyciu pulpitu zdalnego. Jeśli chcesz to zrobić, upewnij się, że nie ma żadnych zasad domeny, które uniemożliwiają połączenie z maszyną wirtualną przy użyciu adresu publicznego.

Aby sprawdzić zakończenie operacji, postępuj zgodnie z instrukcjami [monitorowanie aktywności](#monitor).

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania
1. Utwórz plik .xml jako szablon dla planu odzyskiwania przy użyciu danych poniżej, a następnie zapisz go jako "C:\RPTemplatePath.xml".
2. Zmień węzeł RecoveryPlan identyfikator, nazwę, PrimaryServerId i SecondaryServerId.
3. Zmień węzeł ProtectionEntity PrimaryProtectionEntityId (vmid z programu VMM).
4. Możesz dodać więcej maszyn wirtualnych, dodając więcej węzłów ProtectionEntity.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Wypełnij danych w szablonie:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. Utwórz RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover
1. Pobierz obiekt RecoveryPlan, uruchamiając następujące polecenie:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-Name $RPName;
2. Uruchom test trybu failover, uruchamiając następujące polecenie:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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
[Dowiedz się więcej](/powershell/azure/overview) o poleceniach cmdlet programu PowerShell systemu Azure lokacji odzyskiwania. </a>.
