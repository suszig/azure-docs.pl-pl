---
title: "Wdrażanie i zarządzanie nimi kopii zapasowej dla maszyn wirtualnych platformy Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie wdrażania i zarządzania nimi przy użyciu programu PowerShell usługi Kopia zapasowa Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/9/2017
ms.author: cwatson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 379686f70518b3c7773f1e0e0461eda2a4934d64
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a>Tworzenie kopii zapasowych maszyn wirtualnych przy użyciu poleceń cmdlet AzureRM.Backup
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Wdrożenie klasyczne](backup-azure-vms-classic-automation.md)
>
>

W tym artykule przedstawiono sposób użycia programu Azure PowerShell kopii zapasowych i odzyskiwania maszyn wirtualnych platformy Azure. Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: model wdrażania przy użyciu usługi Resource Manager i model klasyczny. W tym artykule omówiono, aby utworzyć kopię zapasową danych do magazynu kopii zapasowej przy użyciu klasycznego modelu wdrożenia. Jeśli nie utworzono magazyn kopii zapasowych w ramach subskrypcji, zobacz wersję usługi Resource Manager w tym artykule [AzureRM.RecoveryServices.Backup użyj poleceń cmdlet narzędzia do tworzenia kopii zapasowych maszyn wirtualnych](backup-azure-vms-automation.md). Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

> [!IMPORTANT]
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 30 listopada 2017 nie można utworzyć magazyny kopii zapasowych przy użyciu programu PowerShell.<br/> **W dniu 30 listopada 2017**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

## <a name="concepts"></a>Pojęcia
Ten artykuł zawiera informacje specyficzne dla poleceń cmdlet programu PowerShell, używany do tworzenia kopii zapasowych maszyn wirtualnych. Aby uzyskać informacje wprowadzające dotyczące ochrony maszyn wirtualnych platformy Azure, zobacz [Zaplanuj infrastrukturę kopii zapasowej maszyny Wirtualnej na platformie Azure](backup-azure-vms-introduction.md).

> [!NOTE]
> Przed rozpoczęciem przeczytaj [wymagania wstępne](backup-azure-vms-prepare.md) wymagane do pracy z usługą Azure Backup i [ograniczenia](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) bieżącej maszyny wirtualnej należy utworzyć kopię zapasową rozwiązania.
>
>

Efektywne przy użyciu programu PowerShell, Poświęć chwilę, aby zrozumieć hierarchii obiektów i z których ma rozpoczynać się.

![Hierarchia obiektów](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

Dwie najważniejsze przepływów są włączania ochrony dla maszyny Wirtualnej i przywrócić dane z punktu odzyskiwania. Ten artykuł koncentruje się ułatwiające stają się doświadczenie w pracy z poleceń cmdlet programu PowerShell na potrzeby tych dwóch scenariuszy.

## <a name="setup-and-registration"></a>Instalację i rejestrację
Aby rozpocząć:

1. [Pobierz najnowsze PowerShell](https://github.com/Azure/azure-powershell/releases) (minimalna wersja wymagana jest: 1.0.0)
2. Znajdowanie dostępnych poleceń cmdlet programu PowerShell usługi Azure Backup, wpisując następujące polecenie:

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

Następujące zadania instalację i rejestrację można zautomatyzować przy użyciu programu PowerShell:

* Tworzenie magazynu kopii zapasowych
* Rejestrowanie maszyn wirtualnych w usłudze Kopia zapasowa Azure

### <a name="create-a-backup-vault"></a>Tworzenie magazynu kopii zapasowych
> [!WARNING]
> Dla klientów korzystających z usługi Kopia zapasowa Azure po raz pierwszy należy zarejestrować dostawcę usługi Kopia zapasowa Azure do użycia w ramach subskrypcji. Można to zrobić, uruchamiając następujące polecenie: Register AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"
>
>

Można utworzyć nowego magazynu kopii zapasowych przy użyciu **AzureRmBackupVault nowy** polecenia cmdlet. Magazyn kopii zapasowych jest zasobem ARM, więc należy umieścić w grupie zasobów. W konsoli programu Azure PowerShell z podwyższonym poziomem uprawnień uruchom następujące polecenia:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Można uzyskać listę wszystkich magazynów kopii zapasowych w danej subskrypcji przy użyciu **Get-AzureRmBackupVault** polecenia cmdlet.

> [!NOTE]
> Rozwiązaniem jest przechowywanie obiektu magazynu kopii zapasowych do zmiennej. Wymagany jest obiekt magazynu jako dane wejściowe dla wielu poleceń cmdlet narzędzia Kopia zapasowa Azure.
>
>

### <a name="registering-the-vms"></a>Rejestrowanie maszyn wirtualnych
Pierwszy krok w kierunku Konfigurowanie tworzenia kopii zapasowej za pomocą usługi Kopia zapasowa Azure jest można zarejestrować Twojego komputera lub maszyny Wirtualnej w magazynie usługi Kopia zapasowa Azure. **AzureRmBackupContainer rejestru** polecenia cmdlet przyjmuje informacje wejściowe maszyny wirtualne Azure IaaS i rejestruje go z określonym magazynem. Operacja rejestracji kojarzy maszyny wirtualnej platformy Azure w magazynie kopii zapasowych i śledzi maszyny Wirtualnej za pomocą kopii zapasowej cyklu życia.

Rejestrowanie maszyny Wirtualnej w usłudze Azure Backup tworzy obiekt kontenerem najwyższego poziomu. Kontener zwykle zawiera wiele elementów, które można utworzyć kopię zapasową, ale w przypadku maszyn wirtualnych będzie tylko jeden element kopii zapasowej dla kontenera.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Tworzenie kopii zapasowych maszyn wirtualnych platformy Azure
### <a name="create-a-protection-policy"></a>Tworzenie zasad ochrony
Nie jest to konieczne, aby utworzyć nowe zasady ochrony, aby rozpocząć tworzenie kopii zapasowych maszyn wirtualnych. Magazyn jest dostarczany z "Domyślne zasady" można szybko włączania ochrony, a później edytować, z prawej szczegóły. Listę dostępnych w magazynie zasad można uzyskać za pomocą **Get-AzureRmBackupProtectionPolicy** polecenia cmdlet:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> Strefa czasowa BackupTime pola w programie PowerShell jest czasem UTC. Podczas wykonywania kopii zapasowej jest wyświetlana w portalu Azure, strefa czasowa jest wyrównany do systemu lokalnego, wraz z przesunięciem UTC.
>
>

Zasady tworzenia kopii zapasowej jest skojarzony z co najmniej jedne zasady przechowywania. Zasady przechowywania określają, jak długo punkt odzyskiwania jest przechowywany z usługi Kopia zapasowa Azure. **AzureRmBackupRetentionPolicy nowy** polecenie cmdlet tworzy obiekty programu PowerShell, zawierających informacje o zasadach przechowywania. Te obiekty zasad przechowywania są używane jako dane wejściowe *AzureRmBackupProtectionPolicy nowy* polecenia cmdlet, lub bezpośrednio z *AzureRmBackupProtection Włącz* polecenia cmdlet.

Zasady tworzenia kopii zapasowej definiuje kiedy i jak często jest wykonywane kopii zapasowej elementu. **AzureRmBackupProtectionPolicy nowy** polecenie cmdlet tworzy obiekt programu PowerShell, która przechowuje informacje o zasadach tworzenia kopii zapasowej. Zasady tworzenia kopii zapasowej są używane jako dane wejściowe *AzureRmBackupProtection Włącz* polecenia cmdlet.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Włączanie ochrony
Włączanie ochrony obejmuje dwa obiekty — element i zasady, a oba muszą należeć do tego samego magazynu. Gdy zasady został skojarzony z elementem, przepływ pracy tworzenia kopii zapasowej będzie zaczną działać zgodnie ze zdefiniowanym harmonogramem.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Początkowa kopia zapasowa
Wykonanie kolejnych kopii zapasowych wstępny pełny kopii dla elementu i przyrostowych kopii zajmie się harmonogram tworzenia kopii zapasowych. Jednak jeśli chcesz wymusić początkowa kopia zapasowa wystąpić w określonym czasie, lub nawet natychmiast następnie użyć **AzureRmBackupItem kopii zapasowej** polecenia cmdlet:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> Strefa czasowa pól wartości StartTime i EndTime wyświetlane w programie PowerShell jest czasem UTC. Gdy podobne informacje są wyświetlane w portalu Azure, strefa czasowa jest wyrównany zegara systemu lokalnego.
>
>

### <a name="monitoring-a-backup-job"></a>Monitorowanie zadania tworzenia kopii zapasowej
Większość długotrwałej operacji w usłudze Kopia zapasowa Azure są sporządzony według wzoru jako zadanie. To ułatwia śledzić postęp bez konieczności zachowania portalu Azure Otwórz przez cały czas.

Aby pobrać najnowszy stan zadania w toku, należy użyć **Get-AzureRmBackupJob** polecenia cmdlet.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Zamiast sondowania te zadania na zakończenie — czyli kod niepotrzebne, dodatkowe — jest łatwiejsze do użycia **AzureRmBackupJob oczekiwania** polecenia cmdlet. Używanego w skrypcie, dopóki zadanie zostało ukończone albo osiągnięto wartość limitu czasu określonego polecenia cmdlet wstrzyma wykonywania.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Przywracanie maszyny Wirtualnej platformy Azure
Aby przywrócić dane kopii zapasowej, trzeba będzie zidentyfikować element kopii zapasowej i punktu odzyskiwania, która przechowuje dane w momencie. Informacje te są dostarczane do polecenia cmdlet AzureRmBackupItem przywracania do inicjowania przywracania danych z magazynu do konta klienta.

### <a name="select-the-vm"></a>Wybierz maszynę Wirtualną
Aby uzyskać obiekt programu PowerShell, który identyfikuje element prawo kopii zapasowej, należy uruchomić z kontenera w magazynie i przechodzić w dół hierarchii obiektów. Aby wybrać kontener, który reprezentuje maszyny Wirtualnej, użyj **Get-AzureRmBackupContainer** polecenia cmdlet i przekazać do **Get-AzureRmBackupItem** polecenia cmdlet.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Wybierz punkt odzyskiwania
Można teraz wyświetlić listę wszystkich punktów odzyskiwania dla elementu kopii zapasowej za pomocą **Get-AzureRmBackupRecoveryPoint** polecenia cmdlet i wybierz punkt odzyskiwania, aby przywrócić. Zwykle ostatniego pobrania użytkowników *AppConsistent* punktu na liście.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

Zmienna ```$rp``` jest tablicą punkty odzyskiwania dla wybranej kopii zapasowej elementu, posortowane w kolejności odwrotnej czas — najnowszy punkt odzyskiwania znajduje się pod indeksem 0. Użyj standardowych indeksowanie tablicy środowiska PowerShell i wybierz punkt odzyskiwania. Na przykład: ```$rp[0]``` wybierze najnowszego punktu odzyskiwania.

### <a name="restoring-disks"></a>Przywracanie dysków
Ma klucza różnicy między operacje przywracania wykonywane za pośrednictwem portalu Azure i przy użyciu programu Azure PowerShell. Przy użyciu programu PowerShell operacja przywracania zatrzymuje przy przywracaniu dysków i informacje o konfiguracji z punktu odzyskiwania. Nie tworzy maszynę wirtualną.

> [!WARNING]
> AzureRmBackupItem przywracania nie powoduje utworzenia maszyny Wirtualnej. Przywraca tylko dyski do określonego konta magazynu. To nie jest takie samo zachowanie, które wystąpią w portalu Azure.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Można uzyskać szczegółowe informacje o użyciu operacji przywracania **Get-AzureRmBackupJobDetails** polecenia cmdlet, po zakończeniu zadania przywracania. *Szczegóły błędu* właściwość będzie mieć informacje potrzebne do odbudowywania maszyny Wirtualnej.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a>Tworzenie maszyny Wirtualnej
Tworzenie maszyny Wirtualnej poza przywróconej dysków może odbywać się przy użyciu starszej Azure usługi zarządzania cmdlet programu PowerShell, nowych szablonów usługi Azure Resource Manager lub nawet przy użyciu portalu Azure. W prosty przykład firma Microsoft będzie pokazują, jak przejść za pomocą poleceń cmdlet usługi Azure Service Management.

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

Aby uzyskać więcej informacji na temat tworzenia maszyny Wirtualnej z przywróconą dysków przeczytaj o następujących poleceń cmdlet:

* [Dodaj AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Nowe AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Nowe AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Przykłady kodu
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a>1. Pobierz stan ukończenia zadania podrzędne zadania
Aby śledzić stan ukończenia poszczególnych zadań podrzędnych, można użyć **Get-AzureRmBackupJobDetails** polecenia cmdlet:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Tworzenie raportu codziennie/cotygodniowych zadań tworzenia kopii zapasowej
Administratorzy zwykle zapoznać się zadania tworzenia kopii zapasowej uruchomione w ostatnich 24 godzin, stan tych zadań tworzenia kopii zapasowej. Ponadto ilości przetransferowanych danych pozwala administratorom sposób, aby oszacować ich miesięcznego użycia danych. Poniższy skrypt pobiera nieprzetworzone dane z usługi Kopia zapasowa Azure i wyświetla informacje w konsoli programu PowerShell.

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

Jeśli chcesz dodać rozbudowane możliwości do danych wyjściowych tego raportu, Dowiedz się od wpis w blogu TechNet [wykresów przy użyciu programu PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Następne kroki
Jeśli wolisz, skontaktuj się z zasobami platformy Azure przy użyciu programu PowerShell, zapoznaj się artykuł programu PowerShell do ochrony systemu Windows Server, [wdrażanie i zarządzanie kopia zapasowa systemu Windows Server](backup-client-automation-classic.md). Jest również artykuł programu PowerShell do zarządzania kopii zapasowych programu DPM, [wdrażanie i zarządzanie kopii zapasowej programu DPM](backup-dpm-automation-classic.md). Mieć obu tych artykułach wersji dla wdrożenia usługi Resource Manager oraz wdrożenia klasycznego.
