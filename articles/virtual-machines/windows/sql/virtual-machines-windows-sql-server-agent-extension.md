---
title: "Automatyzacji zadań zarządzania na maszynach wirtualnych SQL (Resource Manager) | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób zarządzania rozszerzenie agenta programu SQL Server, który automatyzuje określonych zadań administracyjnych programu SQL Server. Obejmują one automatyczna usługa Backup, automatyczne stosowanie poprawek i integracji magazynu kluczy Azure."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 414c985d21112d658b6e22473f67ed1c3afd00ef
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>Automatyzacji zadań zarządzania na maszynach wirtualnych Azure z rozszerzeniem agenta serwera SQL (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Wdrożenie klasyczne](../classic/sql-server-agent-extension.md)
> 
> 

Rozszerzenie agenta SQL Server IaaS (SQLIaaSExtension) działa na maszynach wirtualnych Azure w celu zautomatyzowania zadań administracyjnych. Ten artykuł zawiera omówienie usług obsługiwanych przez rozszerzenie, jak również instrukcje dotyczące instalacji, stanu i usuwania.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Aby wyświetlić klasycznej wersji w tym artykule, zobacz [rozszerzenie agenta serwera SQL dla programu SQL Server VMs klasycznego](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Obsługiwane usługi
Rozszerzenie agenta programu SQL Server IaaS obsługuje następujące zadania:

| Funkcja administracji | Opis |
| --- | --- |
| **Automatyczna usługa Backup SQL** |Automatyzuje Planowanie kopii zapasowych dla wszystkich baz danych dla domyślnego wystąpienia programu SQL Server w maszynie Wirtualnej. Aby uzyskać więcej informacji, zobacz [automatycznego tworzenia kopii zapasowej dla programu SQL Server w maszynach wirtualnych platformy Azure (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatyczne stosowanie poprawek SQL** |Konfiguruje okno obsługi, w którym do maszyny Wirtualnej można przeprowadzać aktualizacje, aby uniknąć aktualizacje w godzinach szczytu dla obciążenia. Aby uzyskać więcej informacji, zobacz [automatyczne stosowanie poprawek dla programu SQL Server w maszynach wirtualnych platformy Azure (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integracja z usługą Azure Key Vault** |Umożliwia automatyczne instalowanie i konfigurowanie usługi Azure Key Vault na maszyną Wirtualną programu SQL Server. Aby uzyskać więcej informacji, zobacz [Konfigurowanie integracji magazynu kluczy Azure dla programu SQL Server na maszynach wirtualnych Azure (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Po zainstalowaniu i uruchomione, rozszerzenie agenta programu SQL Server IaaS udostępnia te funkcje administracyjne na panelu programu SQL Server na maszynie wirtualnej w portalu Azure i przy użyciu programu Azure PowerShell dla obrazów w witrynie marketplace programu SQL Server i za pośrednictwem platformy Azure PowerShell w przypadku ręcznej instalacji rozszerzenia. 

## <a name="prerequisites"></a>Wymagania wstępne
Wymagania dotyczące używania rozszerzenie agenta IaaS serwera SQL na maszynie Wirtualnej:

**System operacyjny**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**Wersje programu SQL Server**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Program Azure PowerShell**:

* [Pobierz i skonfiguruj najnowszych poleceń programu Azure PowerShell](/powershell/azure/overview)

## <a name="installation"></a>Instalacja
Rozszerzenie agenta programu SQL Server IaaS jest automatycznie instalowany podczas udostępniania jednego z obrazów Galeria maszyny wirtualnej programu SQL Server. Jeśli musisz ręcznie zainstaluj rozszerzenie na jednym z tych maszyn wirtualnych serwera SQL, użyj następującego polecenia programu PowerShell:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

Istnieje również możliwość zainstalować rozszerzenie agenta IaaS serwera SQL na maszynie wirtualnej tylko do systemu operacyjnego Windows Server. To ustawienie jest obsługiwane tylko, jeśli ręcznie zainstalowano program SQL Server na tym komputerze. Następnie zainstaluj rozszerzenie ręcznie przy użyciu takie same **AzureVMSqlServerExtension zestaw** polecenia cmdlet programu PowerShell.

> [!NOTE]
> Jeśli rozszerzenie agenta programu SQL Server IaaS można ręcznie zainstalować na maszynie Wirtualnej tylko do systemu operacyjnego Windows Server, nie można zarządzać ustawienia konfiguracji programu SQL Server za pośrednictwem portalu Azure. W tym scenariuszu należy wszystkie zmiany przy użyciu programu PowerShell.

## <a name="status"></a>Stan
Jest jednym ze sposobów Sprawdź, czy rozszerzenie jest zainstalowane, aby wyświetlić stan agenta w portalu Azure. Wybierz **wszystkie ustawienia** w oknie maszyny wirtualnej, a następnie kliknij na **rozszerzenia**. Powinny pojawić się **SQLIaaSExtension** rozszerzenie na liście.

![Rozszerzenie agenta programu SQL Server IaaS w portalu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Można również użyć **Get-AzureVMSqlServerExtension** polecenia cmdlet programu Azure PowerShell.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Poprzednie polecenie potwierdza agent jest zainstalowany i zawiera ogólne informacje. Możesz również uzyskać informacje dotyczące automatycznego tworzenia kopii zapasowej i Patching stanu określonego za pomocą następujących poleceń.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Usunięcie
W portalu Azure, można odinstalować rozszerzenia, klikając wielokropek **rozszerzenia** okna właściwości maszyny wirtualnej. Następnie kliknij przycisk **usunąć**.

![Odinstaluj rozszerzenie agenta programu SQL Server IaaS, w portalu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Można również użyć **AzureRmVMSqlServerExtension Usuń** polecenia cmdlet programu PowerShell.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Następne kroki
Rozpocznij przy użyciu jednej z usług obsługiwanych przez rozszerzenie. Aby uzyskać więcej informacji, zobacz artykuły, do którego odwołuje się [obsługiwane usługi](#supported-services) sekcji tego artykułu.

Aby uzyskać więcej informacji na temat uruchamiania programu SQL Server na maszynach wirtualnych platformy Azure, zobacz [programu SQL Server na maszynach wirtualnych platformy Azure — omówienie](virtual-machines-windows-sql-server-iaas-overview.md).

