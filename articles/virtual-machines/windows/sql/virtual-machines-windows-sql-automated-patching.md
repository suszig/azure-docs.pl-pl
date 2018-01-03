---
title: Automatyczne stosowanie poprawek dla programu SQL Server VMs (Resource Manager) | Dokumentacja firmy Microsoft
description: "Zawiera opis funkcji Automatyczne stosowanie poprawek dla programu SQL Server maszyn wirtualnych działających na platformie Azure za pomocą Menedżera zasobów."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 7d501ab45a85010a8dbfd6135d77f18f1743354e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatyczne stosowanie poprawek dla programu SQL Server w usłudze Azure Virtual Machines (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Wdrożenie klasyczne](../classic/sql-automated-patching.md)
> 
> 

Automatyczne Patching ustanawia okna obsługi dla maszyny wirtualnej platformy Azure działa program SQL Server. Automatyczne aktualizacje można zainstalować tylko w tym oknie obsługi. Dla programu SQL Server to rescriction gwarantuje, że aktualizacje systemu i ponowne uruchomienia, skojarzone są wykonywane w najlepiej czasie dla bazy danych. Automatyczne Patching jest zależna od [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Aby wyświetlić klasycznej wersji w tym artykule, zobacz [automatyczne stosowanie poprawek dla programu SQL Server w klasycznym maszyny wirtualne Azure](../classic/sql-automated-patching.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby użyć automatyczne stosowanie poprawek, należy wziąć pod uwagę następujące wymagania wstępne:

**System operacyjny**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**Wersja programu SQL Server**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Program Azure PowerShell**:

* [Zainstaluj najnowsze poleceń programu PowerShell Azure](/powershell/azure/overview) Jeśli zamierzasz skonfigurować automatyczne stosowanie poprawek przy użyciu programu PowerShell.

> [!NOTE]
> Automatyczne Patching zależy od rozszerzenie agenta programu SQL Server IaaS. Bieżący obrazów Galeria maszyny wirtualnej SQL dodać to rozszerzenie domyślnie. Aby uzyskać więcej informacji, zobacz [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Ustawienia
W poniższej tabeli opisano opcje, które można skonfigurować dla automatyczne stosowanie poprawek. Kroki konfiguracji rzeczywisty zależy od przy użyciu portalu Azure lub poleceń programu PowerShell systemu Windows Azure.

| Ustawienie | Możliwe wartości | Opis |
| --- | --- | --- |
| **Automatyczne stosowanie poprawek** |Włącza/wyłącza (wyłączone) |Włącza lub wyłącza automatyczne stosowanie poprawek dla maszyny wirtualnej platformy Azure. |
| **Harmonogram konserwacji** |Codziennie, poniedziałek, Wtorek, środę, czwartek, piątek, sobota, niedziela |Harmonogram pobieranie i instalowanie aktualizacji systemu Windows, programu SQL Server i Microsoft dla maszyny wirtualnej. |
| **Godzina rozpoczęcia konserwacji** |0-24 |Godzina rozpoczęcia lokalnego można zaktualizować maszyny wirtualnej. |
| **Czas trwania okna obsługi** |30-180 |Liczba minut uprawnienia do pobierania i instalacji aktualizacji. |
| **Poprawka kategorii** |Ważne |Kategoria aktualizacje do pobrania i zainstalowania. |

## <a name="configuration-in-the-portal"></a>Konfiguracja w portalu
Aby skonfigurować automatyczne stosowanie poprawek, podczas udostępniania lub dla istniejących maszyn wirtualnych, można użyć portalu Azure.

### <a name="new-vms"></a>Nowe maszyny wirtualne
Użyj portalu Azure, aby skonfigurować automatyczne stosowanie poprawek, podczas tworzenia nowej maszyny wirtualnej serwera SQL w modelu wdrażania usługi Resource Manager.

W **ustawienia programu SQL Server** bloku, wybierz opcję **automatyczne stosowanie poprawek**. Poniżej przedstawiono zrzut ekranu na portalu Azure **automatyczne stosowanie poprawek SQL** bloku.

![SQL automatyczne stosowanie poprawek w portalu Azure](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Dla kontekstu, zobacz temat pełną na [obsługi maszyny wirtualnej programu SQL Server na platformie Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Istniejące maszyny wirtualne
W przypadku istniejących maszyn wirtualnych programu SQL Server należy wybrać maszyny wirtualnej programu SQL Server. Następnie wybierz **konfigurację programu SQL Server** sekcji **ustawienia** bloku.

![SQL automatyczne stosowanie poprawek dla istniejących maszyn wirtualnych](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

W **konfigurację programu SQL Server** bloku, kliknij przycisk **Edytuj** przycisku na automatyczne stosowanie poprawek sekcji.

![Skonfiguruj automatyczne stosowanie poprawek SQL dla istniejących maszyn wirtualnych](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Gdy skończysz, kliknij przycisk **OK** przycisk w dolnej części **konfigurację programu SQL Server** bloku, aby zapisać zmiany.

Jeśli włączasz automatyczne stosowanie poprawek po raz pierwszy, Azure konfiguruje agenta programu SQL Server IaaS w tle. W tym czasie portalu Azure może nie informować, że skonfigurowano automatyczne stosowanie poprawek. Poczekaj kilka minut dla agenta, który ma zostać zainstalowany, skonfigurowany. Po utworzeniu tego portalu Azure odzwierciedla nowych ustawień.

> [!NOTE]
> Można również skonfigurować automatyczne stosowanie poprawek za pomocą szablonu. Aby uzyskać więcej informacji, zobacz [szablon Szybki Start Azure automatyczne stosowanie poprawek](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>Konfiguracja przy użyciu programu PowerShell
Po aprowizacji maszyny Wirtualnej SQL, skonfiguruj automatyczne stosowanie poprawek za pomocą programu PowerShell.

W poniższym przykładzie programu PowerShell służy do konfigurowania automatyczne stosowanie poprawek na istniejącej maszyny Wirtualnej programu SQL Server. **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** polecenie konfiguruje nowe okno obsługi, aktualizacje automatyczne.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Na podstawie tego przykładu, w poniższej tabeli opisano praktyczne wpływa na docelowej maszynie Wirtualnej platformy Azure:

| Parametr | Efekt |
| --- | --- |
| **DayOfWeek** |Każdy czwartek zainstalowanych poprawek. |
| **MaintenanceWindowStartingHour** |Aktualizacje BEGIN o 11:00. |
| **MaintenanceWindowsDuration** |Poprawki muszą być zainstalowane w ciągu 120 minut. Na podstawie czasu rozpoczęcia, użytkownicy muszą wykonać przez 1:00 pm. |
| **PatchCategory** |Jedyną możliwą ustawienie tego parametru to **ważne**. |

Go może potrwać kilka minut, aby zainstalować i skonfigurować agenta programu SQL Server IaaS.

Aby wyłączyć automatyczne stosowanie poprawek, uruchom ten sam skrypt bez **-Włącz** parametr **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Brak **-Włącz** parametru sygnały polecenie, aby wyłączyć funkcję.

## <a name="next-steps"></a>Kolejne kroki
Informacje o innych zadaniach automatyzacji dostępny, zobacz [rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Aby uzyskać więcej informacji na temat uruchamiania programu SQL Server na maszynach wirtualnych Azure, zobacz [programu SQL Server na maszynach wirtualnych platformy Azure — omówienie](virtual-machines-windows-sql-server-iaas-overview.md).

