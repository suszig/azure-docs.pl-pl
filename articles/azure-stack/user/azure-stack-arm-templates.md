---
title: "Użyj szablonów usługi Azure Resource Manager w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać szablonów usługi Azure Resource Manager w stosie Azure do udostępniania zasobów."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 6d4ef16881ef8dc249116aec706f760b163a2972
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Użyj szablonów usługi Azure Resource Manager w stosie Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Szablony usługi Azure Resource Manager wdrażania i obsługi administracyjnej wszystkie zasoby aplikacji w jednej, skoordynowanej operacji. Można także wdrożyć ponownie szablony, aby wprowadzić zmiany dotyczące zasobów w grupie zasobów.

Szablony te można wdrażać w portalu Microsoft Azure stosu, programu PowerShell, wiersz polecenia i Visual Studio.

Następujące szablony szybkiego startu są dostępne na [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Wdrażanie programu SharePoint (z systemem innym niż wysokiej dostępności)
Użyj rozszerzenia DSC środowiska PowerShell do utworzenia farmy programu SharePoint 2013, która obejmuje następujące zasoby:

* Sieć wirtualna
* Trzy konta magazynu
* Dwa zewnętrzne moduły równoważenia obciążenia
* Jedna maszyna wirtualna, skonfigurowany jako kontroler domeny dla nowego lasu z jedną domeną
* Jedna maszyna wirtualna skonfigurowana jako autonomiczny serwer programu SQL Server 2014
* Jedna maszyna wirtualna skonfigurowana w farmie programu SharePoint 2013 na jednym komputerze

## <a name="deploy-ad-non-high-availability"></a>Wdrażanie AD (z systemem innym niż wysokiej dostępności)
Użyj rozszerzenia DSC środowiska PowerShell, aby utworzyć serwer kontrolera domeny usługi AD, która obejmuje następujące zasoby:

* Sieć wirtualna
* Jedno konto magazynu
* Jeden zewnętrznej usługi równoważenia obciążenia
* Jedna maszyna wirtualna, skonfigurowany jako kontroler domeny dla nowego lasu z jedną domeną

## <a name="deploy-adsql-non-high-availability"></a>Wdrażanie AD/SQL (z systemem innym niż wysokiej dostępności)
Użyj rozszerzenia DSC środowiska PowerShell do utworzenia autonomicznego serwera SQL Server 2014, który obejmuje następujące zasoby:

* Sieć wirtualna
* Dwa konta magazynu
* Jeden zewnętrznej usługi równoważenia obciążenia
* Jedna maszyna wirtualna, skonfigurowany jako kontroler domeny dla nowego lasu z jedną domeną
* Jedna maszyna wirtualna skonfigurowana jako autonomiczny serwer programu SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server
Użyj rozszerzenia DSC środowiska PowerShell do konfigurowania istniejącej maszyny wirtualnej lokalnego Configuration Manager (LCM) i zarejestruj go z serwerem ściągania usługi Konfiguracja DSC konta automatyzacji Azure.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Utwórz maszynę wirtualną z obrazu użytkownika
Utwórz maszynę wirtualną z obrazu użytkownika niestandardowego. Ten szablon wdraża również sieć wirtualną (z systemem DNS), publiczny adres IP i interfejsu sieciowego.

## <a name="simple-vm"></a>Proste maszyny Wirtualnej
Wdróż maszynę Wirtualną systemu Windows, która zawiera sieć wirtualną (z systemem DNS), publiczny adres IP i interfejsu sieciowego.

## <a name="cancel-a-running-template-deployment"></a>Anuluj uruchomione wdrażanie szablonu
Aby anulować uruchomionego wdrożenia szablonu, należy użyć `Stop-AzureRmResourceGroupDeployment` polecenia cmdlet programu PowerShell.

## <a name="next-steps"></a>Kolejne kroki
[Wdrażanie szablonów za pomocą portalu](azure-stack-deploy-template-portal.md)

[Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)

