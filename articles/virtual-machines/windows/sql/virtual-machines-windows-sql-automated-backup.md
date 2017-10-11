---
title: "Automatyczna usługa Backup SQL Server 2014 maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: "Zawiera opis funkcji automatycznego tworzenia kopii zapasowej dla programu SQL Server 2014 maszyn wirtualnych działających na platformie Azure. Ten artykuł dotyczy maszyn wirtualnych za pomocą Menedżera zasobów."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 91aab896dd5f06c950ee0ed8f36cc6a953d91611
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatyczne kopie zapasowe maszyn wirtualnych programu SQL Server 2014 (Resource Manager)

> [!div class="op_single_selector"]
> * [Program SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatyczne kopie zapasowe automatycznie konfiguruje [zarządzanej kopii zapasowej Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) dla wszystkich istniejących i nowych baz danych na maszynie Wirtualnej platformy Azure, programem SQL Server 2014 Standard lub Enterprise. Dzięki temu można skonfigurować kopie zapasowe zwykłej bazy danych, które korzystać z magazynu trwałego obiektów blob platformy Azure. Automatyczne kopie zapasowe jest zależna od [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Wymagania wstępne
Aby korzystać z automatycznego tworzenia kopii zapasowej, należy wziąć pod uwagę następujące wymagania wstępne:

**System operacyjny**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**Wydanie wersji programu SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatyczne działa kopii zapasowej z programu SQL Server 2014. Jeśli używasz programu SQL Server 2016 służy v2 automatycznego tworzenia kopii zapasowej do tworzenia kopii zapasowych baz danych. Aby uzyskać więcej informacji, zobacz [v2 automatyczna usługa Backup SQL Server 2016 maszyn wirtualnych platformy Azure](virtual-machines-windows-sql-automated-backup-v2.md).

**Baza danych konfiguracji**:

- Docelowej bazy danych muszą używać modelu odzyskiwania pełnego. Aby uzyskać więcej informacji dotyczących wpływu modelu odzyskiwania pełnego na wykonywanie kopii zapasowych, zobacz [kopii zapasowej w obszarze pełny Model odzyskiwania](https://technet.microsoft.com/library/ms190217.aspx).
- Docelowymi bazami danych musi być w domyślnym wystąpieniu programu SQL Server. Rozszerzenie IaaS serwera SQL obsługuje tylko nazwane wystąpienia.

**Model wdrożenia usługi Azure**:

- Resource Manager

**Program Azure PowerShell**:

- [Zainstaluj najnowsze poleceń programu PowerShell Azure](/powershell/azure/overview) Jeśli planujesz Konfigurowanie automatycznego tworzenia kopii zapasowej przy użyciu programu PowerShell.

> [!NOTE]
> Automatyczne kopie zapasowe wykorzystuje rozszerzenie agenta programu SQL Server IaaS. Bieżący obrazów Galeria maszyny wirtualnej SQL dodać to rozszerzenie domyślnie. Aby uzyskać więcej informacji, zobacz [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ustawienia

W poniższej tabeli opisano opcje, które można skonfigurować do automatycznego tworzenia kopii zapasowej. Kroki konfiguracji rzeczywisty zależy od przy użyciu portalu Azure lub poleceń programu PowerShell systemu Windows Azure.

| Ustawienie | Zakres (ustawienie domyślne) | Opis |
| --- | --- | --- |
| **Automatyczne kopie zapasowe** | Włącza/wyłącza (wyłączone) | Włącza lub wyłącza funkcję automatycznego tworzenia kopii zapasowej dla maszyny Wirtualnej platformy Azure, programem SQL Server 2014 Standard lub Enterprise. |
| **Okres przechowywania** | 1 do 30 dni (30 dni) | Liczba dni przechowywania kopii zapasowej. |
| **Konto magazynu** | Konto magazynu Azure | Konto magazynu Azure do przechowywania plików automatycznego tworzenia kopii zapasowej w magazynie obiektów blob. Kontener jest tworzony w tej lokalizacji, aby zapisać wszystkie pliki kopii zapasowej. Konwencja nazewnictwa pliku kopii zapasowej obejmuje daty, godziny i nazwy komputera. |
| **Szyfrowanie** | Włącza/wyłącza (wyłączone) | Włącza lub wyłącza funkcję szyfrowania. Po włączeniu szyfrowania certyfikatów służących do przywrócenia kopii zapasowej znajdują się na koncie magazynu określonym w tym samym `automaticbackup` kontenera przy użyciu tej samej konwencji nazewnictwa. Zmiana hasła nowego certyfikatu jest generowana za pomocą tego hasła, ale pozostaje stary certyfikat do przywrócenia poprzedniego kopii zapasowych. |
| **Hasło** | Tekst hasła | Hasło dla kluczy szyfrowania. Jest to tylko wymagane, jeśli jest włączone szyfrowanie. Aby przywrócić zaszyfrowanej kopii zapasowej, musi mieć prawidłowe hasło, a powiązany certyfikat, który został użyty w momencie utworzenia kopii zapasowej. |

## <a name="configuration-in-the-portal"></a>Konfiguracja w portalu

Azure portal umożliwia konfigurowanie automatycznego tworzenia kopii zapasowej, podczas udostępniania lub dla maszyn wirtualnych istniejącego programu SQL Server 2014.

### <a name="new-vms"></a>Nowe maszyny wirtualne

Skonfiguruj automatyczne kopie zapasowe, podczas tworzenia nowej maszyny wirtualnej 2014 serwera SQL w modelu wdrażania usługi Resource Manager za pomocą portalu Azure.

W **ustawienia programu SQL Server** bloku, wybierz opcję **automatyczne kopie zapasowe**. Poniżej przedstawiono zrzut ekranu na portalu Azure **SQL automatyczna usługa Backup** bloku.

![Konfiguracja SQL automatyczna usługa Backup w portalu Azure](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Dla kontekstu, zobacz temat pełną na [obsługi maszyny wirtualnej programu SQL Server na platformie Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Istniejące maszyny wirtualne

W przypadku istniejących maszyn wirtualnych programu SQL Server należy wybrać maszyny wirtualnej programu SQL Server. Następnie wybierz **konfigurację programu SQL Server** sekcji **ustawienia** bloku.

![SQL automatyczna usługa Backup dla istniejących maszyn wirtualnych](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

W **konfigurację programu SQL Server** bloku, kliknij przycisk **Edytuj** przycisk w sekcji automatycznego tworzenia kopii zapasowej.

![Konfigurowanie kopii zapasowej SQL automatycznego dla istniejących maszyn wirtualnych](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Gdy skończysz, kliknij przycisk **OK** przycisk w dolnej części **konfigurację programu SQL Server** bloku, aby zapisać zmiany.

Jeśli po raz pierwszy włączasz automatyczna usługa Backup, Azure konfiguruje agenta programu SQL Server IaaS w tle. W tym czasie portalu Azure może nie informować, że skonfigurowano automatycznego tworzenia kopii zapasowej. Poczekaj kilka minut dla agenta, który ma zostać zainstalowany, skonfigurowany. Po utworzeniu tego portalu Azure będzie odzwierciedlać nowe ustawienia.

> [!NOTE]
> Istnieje również możliwość skonfigurowania automatycznego tworzenia kopii zapasowej przy użyciu szablonu. Aby uzyskać więcej informacji, zobacz [szablonu Azure szybkiego startu dla automatyczna usługa Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfiguracja przy użyciu programu PowerShell

Można skonfigurować automatyczne tworzenie kopii zapasowych za pomocą programu PowerShell. Przed rozpoczęciem należy:

- [Pobierz i zainstaluj najnowsze programu Azure PowerShell](http://aka.ms/webpi-azps).
- Otwórz program Windows PowerShell i skojarzyć go z Twoim kontem. Można to zrobić, wykonując kroki opisane w [skonfiguruj subskrypcję](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) tematu inicjowania obsługi administracyjnej.

### <a name="install-the-sql-iaas-extension"></a>Zainstaluj rozszerzenie IaaS SQL
Jeśli aprowizowanej maszyny wirtualnej programu SQL Server w portalu Azure, rozszerzenie IaaS serwera SQL powinno być już zainstalowane. Można określić, czy jest ona instalowana dla maszyny Wirtualnej przez wywołanie metody **Get-AzureRmVM** polecenia i sprawdzeniu **rozszerzenia** właściwości.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Jeśli zainstalowano rozszerzenia SQL Server IaaS Agent, powinien pojawić się wymienionym "SqlIaaSAgent" lub "SQLIaaSExtension". **ProvisioningState** dla rozszerzenia należy również wyświetlić "Powodzenie".

Jeśli nie jest zainstalowane lub nie można zainicjować obsługi administracyjnej, należy zainstalować go za pomocą następującego polecenia. Oprócz grupy nazwy i zasobów maszyny Wirtualnej, należy także określić region (**$region**) maszyny Wirtualnej znajdujący się w.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a>Sprawdź bieżące ustawienia

Jeśli włączono automatyczne kopie zapasowe podczas inicjowania obsługi, można sprawdzić bieżącej konfiguracji za pomocą programu PowerShell. Uruchom **Get-AzureRmVMSqlServerExtension** polecenia i sprawdź, czy **AutoBackupSettings** właściwości:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Należy pobrać dane wyjściowe podobne do następujących:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Jeśli dane wyjściowe wskazuje, że **włączyć** ma ustawioną wartość **False**, a następnie należy włączyć automatyczne kopie zapasowe. Dobre wieści jest włączone, a następnie skonfiguruj automatyczne kopie zapasowe w taki sam sposób. W następnej sekcji dla tych informacji.

> [!NOTE] 
> Jeśli wybierzesz ustawienia natychmiast po wprowadzeniu zmian jest możliwe, że wystąpi ponownie starych wartości konfiguracji. Poczekaj kilka minut, a następnie sprawdź ustawienia ponownie, aby upewnić się, że zmiany zostały zastosowane.

### <a name="configure-automated-backup"></a>Skonfiguruj automatyczne kopie zapasowe
Można włączyć automatycznego tworzenia kopii zapasowej oraz aby zmodyfikować jego konfigurację i zachowanie w dowolnej chwili za pomocą programu PowerShell.

Najpierw wybierz lub Utwórz konto magazynu w przypadku plików kopii zapasowych. Poniższy skrypt wybiera konta magazynu lub jeśli nie istnieje, tworzy go.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Automatyczne kopie zapasowe nie obsługuje przechowywania kopii zapasowych w magazynie premium, ale może potrwać kopii zapasowych z dysków maszyny Wirtualnej, które używają magazyn w warstwie Premium.

Następnie użyj **AzureRmVMSqlServerAutoBackupConfig nowy** polecenie, aby włączyć i skonfigurować ustawienia automatycznego tworzenia kopii zapasowej do przechowywania kopii zapasowych w ramach konta magazynu platformy Azure. W tym przykładzie ustawiono kopie zapasowe będą przechowywane w ciągu 10 dni. Drugie polecenie **AzureRmVMSqlServerExtension zestaw**, aktualizuje określonej maszyny Wirtualnej platformy Azure przy użyciu tych ustawień.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Go może potrwać kilka minut, aby zainstalować i skonfigurować agenta programu SQL Server IaaS.

> [!NOTE]
> Istnieją inne ustawienia **AzureRmVMSqlServerAutoBackupConfig nowy** dotyczą tylko programu SQL Server 2016 i automatyczna usługa Backup v2. SQL Server 2014 nie obsługuje następujące ustawienia: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, i **LogBackupFrequencyInMinutes**. Jeśli do konfigurowania tych ustawień na maszynie wirtualnej programu SQL Server 2014, nie było błędu, ale nie zostać zastosowane ustawienia. Jeśli chcesz użyć tych ustawień maszyny wirtualnej programu SQL Server 2016, zobacz [v2 automatyczna usługa Backup SQL Server 2016 maszyn wirtualnych platformy Azure](virtual-machines-windows-sql-automated-backup-v2.md).

Aby włączyć szyfrowanie, zmodyfikuj poprzedni skrypt do przekazania **EnableEncryption** parametru wraz z hasła (bezpieczny ciąg) **CertificatePassword** parametru. Poniższy skrypt umożliwia ustawienia automatycznego tworzenia kopii zapasowej w poprzednim przykładzie i dodaje szyfrowania.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Aby potwierdzić ustawienia są stosowane, [Sprawdź konfigurację automatycznego tworzenia kopii zapasowej](#verifysettings).

### <a name="disable-automated-backup"></a>Wyłącz automatyczne kopie zapasowe

Aby wyłączyć automatyczna usługa Backup, uruchom ten sam skrypt bez **— Włącz** parametr **AzureRmVMSqlServerAutoBackupConfig nowy** polecenia. Brak **-Włącz** parametru sygnały polecenie, aby wyłączyć funkcję. Podobnie jak w przypadku instalacji, może upłynąć kilka minut, aby wyłączyć automatyczne wykonywanie kopii zapasowych.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Przykładowy skrypt

Poniższy skrypt zawiera zestaw zmiennych, które można dostosować, aby włączyć i skonfigurować automatyczna usługa Backup dla maszyny Wirtualnej. W Twoim przypadku może być konieczne dostosowanie skryptu, w zależności od wymagań. Na przykład użytkownik musi wprowadzić zmiany, aby wyłączyć tworzenie kopii zapasowej bazy danych systemu lub włączyć szyfrowanie.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Następne kroki

Automatyczne kopie zapasowe konfiguruje zarządzanej kopii zapasowej na maszynach wirtualnych platformy Azure. Dlatego ważne jest, aby [zapoznaj się z dokumentacją dla zarządzanej kopii zapasowej](https://msdn.microsoft.com/library/dn449496.aspx) zrozumienie zachowania i skutki.

Można znaleźć dodatkowe kopii zapasowej i przywracanie wskazówki dotyczące programu SQL Server na maszynach wirtualnych Azure w następującym temacie: [kopii zapasowej i przywracania dla programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informacje o innych zadaniach automatyzacji dostępny, zobacz [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Aby uzyskać więcej informacji na temat uruchamiania programu SQL Server na maszynach wirtualnych Azure, zobacz [programu SQL Server na maszynach wirtualnych platformy Azure — omówienie](virtual-machines-windows-sql-server-iaas-overview.md).

