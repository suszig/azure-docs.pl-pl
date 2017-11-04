---
title: "Przygotowanie infrastruktury platformy Azure dla programu SAP wysokiej dostępności przy użyciu klastra trybu Failover systemu Windows i udostępnionego dysku dla SAP () wystąpienia SCS | Dokumentacja firmy Microsoft"
description: "Azure przygotowanie infrastruktury do programu SAP HA przy użyciu klastra trybu Failover systemu Windows i udostępnionego dysku dla wystąpienia SCS SAP (A)"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: ec976257-396b-42a0-8ea1-01c97f820fa6
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 16c5a2ccfb27b87ba76aa0390ca6c57d2885e43d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="azure-infrastructure-preparation-for-sap-ha-using-windows-failover-cluster-and-shared-disk-for-sap-ascs-instance"></a>Przygotowanie infrastruktury platformy Azure dla programu SAP HA przy użyciu klastra trybu Failover systemu Windows i udostępniony dysk dla wystąpienia SCS SAP (A)

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides
[tuning-failover-cluster-network-thresholds]:https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f


[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md


> ![Windows][Logo_Windows] Windows
>

Ten dokument jest opisujące kroki przygotowania infrastruktury platformy Azure, niezbędne do instalacji i konfiguracji wysokiej dostępności systemu SAP na **Windows Failover Cluster (WSFC)**za pomocą **klastra udostępnionego dysku** jako Opcja w przypadku klastrowania wystąpienia SCS SAP (A).

## <a name="prerequisites"></a>Wymagania wstępne

Upewnij się przejrzeć te dokumenty przed rozpoczęciem instalacji:

* [Przewodnik dotyczący architektury — klastrowanie wystąpienia SCS SAP (A) w **klastra pracy awaryjnej systemu Windows** przy użyciu **dysku udostępnionego klastra**][sap-high-availability-guide-wsfc-shared-disk]

* [Azure przygotowanie infrastruktury do programu SAP HA przy użyciu **klastra pracy awaryjnej systemu Windows** i **udostępnionego dysku** dla **SAP (A) SCS** wystąpienia][sap-high-availability-infrastructure-wsfc-shared-disk]

## <a name="prepare-the-infrastructure-for-architectural-template-1"></a>Przygotowanie infrastruktury dla architektury szablonu 1
Szablony usługi Azure Resource Manager dla programu SAP uprościć wdrażanie wymaganych zasobów.

Trójwarstwowa szablonów usługi Azure Resource Manager obsługuje również scenariuszy wysokiej dostępności, takich jak w architektury 1 szablon, który ma dwa klastry. Każdy klaster jest SAP pojedynczego punktu awarii dla programu SAP ASCS/SCS i bazami danych.

Oto, gdzie można uzyskać szablonów usługi Azure Resource Manager przykładowy scenariusz, który opisano w tym artykule:

* [Obraz Azure Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Za pomocą zarządzanych dysków Azure obrazu z witryny Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-md)  
* [Obraz niestandardowy](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)
* [Niestandardowy obraz za pomocą dysków zarządzanych](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-md)

Aby przygotować infrastrukturę do architektury szablon 1:

- W portalu Azure na **parametry** bloku, w **SYSTEMAVAILABILITY** wybierz opcję **HA**.

  ![Rysunek 1: Ustaw parametry usługi Azure Resource Manager wysokiej dostępności SAP][sap-ha-guide-figure-3000]

_**Rysunek 1.** ustawić parametry usługi Azure Resource Manager wysokiej dostępności SAP_


  Tworzenie szablonów:

  * **Maszyny wirtualne**:
    * Maszyny wirtualne serwera aplikacji SAP: <*SAPSystemSID*> - podpisane — <*numer*>
    * Maszyny wirtualne klastra ASCS/SCS: <*SAPSystemSID*> - ascs - <*numer*>
    * Klaster systemu DBMS: <*SAPSystemSID*> - db - <*numer*>

  * **Sieci karty dla wszystkich maszyn wirtualnych adresów IP skojarzonych**:
    * <*SAPSystemSID*> - nic - podpisane — <*numer*>
    * <*SAPSystemSID*> - nic - ascs - <*numer*>
    * <*SAPSystemSID*> - nic - db - <*numer*>

  * **Konta magazynu platformy Azure (tylko w przypadku dysków niezarządzany)**

  * **Grupy dostępności** dla:
    * Maszyny wirtualne serwera aplikacji SAP: <*SAPSystemSID*> - avset - podpisane
    * Maszyny wirtualne klastra SAP ASCS/SCS: <*SAPSystemSID*> - avset - ascs
    * System DBMS klastrowanie maszyn wirtualnych: <*SAPSystemSID*> - avset - db

  * **Azure wewnętrznego modułu równoważenia obciążenia**:
    * Z wszystkimi portami dla wystąpienia ASCS/SCS i adresu IP <*SAPSystemSID*> - lb - ascs
    * Z wszystkimi portami dla adresu IP i bazami danych programu SQL Server <*SAPSystemSID*> - lb - db

  * **Grupy zabezpieczeń sieci**: <*SAPSystemSID*> - nsg - ascs-0  
    * Z otwartego portu zewnętrznego protokołu RDP (Remote Desktop) do <*SAPSystemSID*> - ascs - 0 maszyny wirtualnej

> [!NOTE]
> Wszystkie adresy IP Azure wewnętrzne moduły równoważenia obciążenia i karty sieciowe są **dynamiczne** domyślnie. Zmień je, aby **statycznych** adresów IP. Firma Microsoft opisano, jak to zrobić w dalszej części tego artykułu.
>
>

## <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Wdrażanie maszyn wirtualnych z połączeniem w sieci firmowej (między lokalizacjami) do użycia w środowisku produkcyjnym
Dla systemów SAP produkcyjnych, wdrażanie maszyn wirtualnych platformy Azure z [łączności z siecią firmową (między lokalizacjami)] [ planning-guide-2.2] za pomocą usługi Azure Site to Site VPN lub rozwiązania Azure ExpressRoute.

> [!NOTE]
> Można użyć wystąpienia sieci wirtualnej platformy Azure. Sieć wirtualna i podsieć już zostały utworzone i przygotowane.
>
>

1.  W portalu Azure na **parametry** bloku, w **NEWOREXISTINGSUBNET** wybierz opcję **istniejących**.
2.  W **SUBNETID** Dodaj pełne ciąg przygotowanego sieci Azure SubnetID, w którym zamierzasz wdrożyć maszyny wirtualne Azure.
3.  Aby uzyskać listę wszystkich podsieci w sieci Azure, uruchom następujące polecenie programu PowerShell:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  **Identyfikator** pola pokazuje **SUBNETID**.
4. Aby uzyskać listę wszystkich **SUBNETID** wartości, uruchom to polecenie programu PowerShell:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  **SUBNETID** wygląda podobnie do następującej:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

## <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Wdrażanie wystąpień SAP tylko w chmurze dla testu i pokaz
Można wdrożyć systemu SAP wysokiej dostępności w modelu wdrażania tylko w chmurze. Tego rodzaju wdrożenia przede wszystkim jest przydatna do demo i test przypadki użycia. Nie nadaje się do przypadków użycia w środowisku produkcyjnym.

- W portalu Azure na **parametry** bloku, w **NEWOREXISTINGSUBNET** wybierz opcję **nowe**. Pozostaw **SUBNETID** pole puste.

  Szablon SAP usługi Azure Resource Manager automatycznie tworzy sieci wirtualnej platformy Azure i podsieć.

> [!NOTE]
> Należy również wdrożyć co najmniej jednej maszyny wirtualnej dedykowanego dla usługi Active Directory i DNS w tym samym wystąpieniu sieci wirtualnej platformy Azure. Szablon nie tworzy tych maszyn wirtualnych.
>
>


## <a name="prepare-the-infrastructure-for-architectural-template-2"></a>Przygotowanie infrastruktury dla architektury szablonu 2

Ten szablon Menedżera zasobów Azure dla programu SAP służy do uproszczenia wdrażania zasobów wymaganej infrastruktury dla programu SAP architektury 2 szablonu.

Oto, gdzie można uzyskać szablonów usługi Azure Resource Manager dla tego scenariusza wdrażania:

* [Obraz Azure Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Za pomocą zarządzanych dysków Azure obrazu z witryny Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged-md)  
* [Obraz niestandardowy](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)
* [Niestandardowy obraz za pomocą dysków zarządzanych](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged-md)


## <a name="prepare-the-infrastructure-for-architectural-template-3"></a>Przygotowanie infrastruktury dla architektury 3 szablonu

Można przygotować infrastrukturę i skonfigurować SAP dla **multi-SID**. Na przykład można dodać dodatkowe wystąpienia SAP ASCS/SCS do *istniejących* konfiguracji klastra. Aby uzyskać więcej informacji, zobacz [skonfigurować dodatkowe wystąpienia SAP ASCS/SCS do istniejącej konfiguracji klastra do utworzenia konfiguracji wielu SID SAP w usłudze Azure Resource Manager][sap-ha-multi-sid-guide].

Jeśli chcesz utworzyć nowy klaster multi-identyfikator SID, można użyć identyfikatora SID multi [szablony szybkiego startu w serwisie GitHub](https://github.com/Azure/azure-quickstart-templates).

Aby utworzyć nowy klaster multi-identyfikator SID, należy wdrożyć trzy następujące szablony:

* [ASCS/SCS szablonu](#ASCS-SCS-template)
* [Szablon bazy danych](#database-template)
* [Szablon serwerów aplikacji](#application-servers-template)

Poniższe sekcje mają więcej szczegółów na temat szablonów oraz parametry, które należy podać w szablonach.

### <a name="ASCS-SCS-template"></a>ASCS/SCS szablonu

Szablon ASCS/SCS wdraża dwie maszyny wirtualne, których można utworzyć klaster trybu failover systemu Windows Server, który obsługuje wiele wystąpień ASCS/SCS.

Aby skonfigurować szablon identyfikatora SID multi ASCS/SCS, w [szablon identyfikatora SID multi ASCS/SCS] [ sap-templates-3-tier-multisid-xscs-marketplace-image] lub [ASCS/SCS identyfikatora SID multi szablonu za pomocą dysków zarządzanych] [ sap-templates-3-tier-multisid-xscs-marketplace-image-md], wprowadź wartości dla następujących parametrów:

  - **Prefiks zasobów**.  Ustaw prefiks zasobu, który jest używany do prefiksu wszystkie zasoby, które są tworzone podczas wdrażania. Ponieważ zasoby nie należą do tylko jednej systemu SAP, prefiks zasobu nie jest częścią identyfikatora SID jednego systemu SAP.  Prefiks musi należeć do zakresu od **trzech do sześciu znaków**.
  - **Na stosie typu**. Wybierz typ stosu systemu SAP. W zależności od typu stosu modułu równoważenia obciążenia Azure ma jeden (ABAP lub tylko Java) lub dwóch (ABAP + Java) prywatnych adresów IP na systemu SAP.
  -  **Typ systemu operacyjnego**. Wybierz system operacyjny maszyn wirtualnych.
  -  **Liczba systemu SAP**. Wybierz liczbę systemów SAP, który ma zostać zainstalowany w tym klastrze.
  -  **Dostępność systemu**. Wybierz **HA**.
  -  **Nazwa użytkownika i hasło administratora**. Utwórz nowego użytkownika, który może służyć do logowania się na tym komputerze.
  -  **Nowy lub istniejący podsieci**. Określ, czy należy utworzyć nową sieć wirtualną i podsieć lub istniejącą podsieć powinna być używana. Jeśli masz już sieć wirtualną, która jest połączona z siecią lokalną, wybierz **istniejących**.
  -  **Identyfikator podsieci**. Ustawienia identyfikatorów podsieci, do którego powinny być połączone maszyn wirtualnych. Wybierz podsieć sieci wirtualnej sieci prywatnej (VPN) lub usługi sieci wirtualnej sieci lokalnej nawiązać połączenia z maszyną wirtualną. Identyfikator zwykle wygląda następująco:

   /Subscriptions/ <*identyfikator subskrypcji*> /resourceGroups/ <*Nazwa grupy zasobów*> /providers/Microsoft.Network/virtualNetworks/ <*wirtualnej nazwy sieciowej*> /subnets/ <*nazwy podsieci*>

Szablon wdraża jedno wystąpienie usługi równoważenia obciążenia Azure, która obsługuje wiele systemów SAP.

- Wystąpienia ASCS powinny być skonfigurowane dla liczby wystąpień 00, 10, 20...
- Wystąpienia SCS powinny być skonfigurowane dla liczby wystąpień 01 11, 21...
- Wystąpienia ASCS umieścić w kolejce replikacji serwera (Wywołujących) (tylko w systemie Linux) są skonfigurowane dla liczby wystąpień 02, 12, 22...
- Wystąpienia Wywołujących SCS (tylko w systemie Linux) są skonfigurowane dla liczby wystąpień 03, 13, 23...

Moduł równoważenia obciążenia zawiera 1 (2 dla systemu Linux) VIP(s), 1 x dla ASCS/SCS i 1 x adres VIP dla Wywołujących (tylko w systemie Linux).

#### <a name="0f3ee255-b31e-4b8a-a95a-d9ed6200468b"></a>SAP ASCS / SCS portów
Poniższa lista zawiera wszystkie reguły, o których (gdzie x jest numerem systemu SAP, na przykład 1, 2, 3...) równoważenia obciążenia:
- Porty właściwe dla systemu Windows dla każdego systemu SAP: 445, 5985
- Porty ASCS (liczby wystąpień x0): 32 x 0, 36 x 0, 39 x 0, 81 x 0, 5 x 013, 5 x 014, 5 x 016
- Porty SCS (liczby wystąpień x1): 32 x 1, 33 x 1, 39 x 1, 81 x 1, 5 x 113, 5 x 114, 5 x 116
- Wywołujących ASCS porty w systemie Linux (liczby wystąpień x2): 33 x 2, 5 x 213, 5 x 214, 5 x 216
- Wywołujących SCS porty w systemie Linux (liczby wystąpień x3): 33 x 3, 5 x 313, 5 x 314, 5 x 316

Moduł równoważenia obciążenia jest skonfigurowana do używania następujące porty sondowania (gdzie x jest numerem systemu SAP, na przykład 1, 2, 3...):
- Port sondy modułu równoważenia obciążenia wewnętrznego ASCS/SCS: 620 x 0
- Wewnętrzny Wywołujących załadować port sondy modułu równoważenia (tylko w systemie Linux): 621 x 2

### <a name="database-template"></a>Szablon bazy danych

Szablon bazy danych wdraża co najmniej dwóch maszyn wirtualnych, które służy do instalowania systemu zarządzania relacyjnej bazy danych (RDBMS) dla jednego systemu SAP. Na przykład jeśli wdrożono szablon ASCS/SCS pięć systemów SAP, należy wdrożyć tego szablonu pięć razy.

Aby skonfigurować szablon identyfikatora SID multi bazy danych, w [bazy danych wielu SID szablonu] [ sap-templates-3-tier-multisid-db-marketplace-image] lub [szablon identyfikatora SID multi bazy danych przy użyciu dysków zarządzanych] [ sap-templates-3-tier-multisid-db-marketplace-image-md], wprowadź wartości dla następujących parametrów:

  -  **Identyfikator systemu SAP**. Wprowadź identyfikator systemu SAP systemu SAP, które chcesz zainstalować. Identyfikator jest służyć jako prefiks dla zasobów, które zostały wdrożone.
  -  **Typ systemu operacyjnego**. Wybierz system operacyjny maszyn wirtualnych.
  -  **Wartość DbType**. Wybierz typ bazy danych, którą chcesz zainstalować w klastrze. Wybierz **SQL** Jeśli chcesz zainstalować program Microsoft SQL Server. Wybierz **HANA** Jeśli zamierzasz zainstalować SAP HANA na maszynach wirtualnych. Upewnij się, że wybrano prawidłowy typ systemu operacyjnego: Wybierz **Windows** SQL i wybierz opcję dystrybucji systemu Linux HANA. Równoważenia obciążenia Azure, który jest podłączony do maszyny wirtualnej jest skonfigurowany do obsługi typu wybranej bazy danych:
    * **SQL**. Obciążenia równoważenia Równoważenie obciążenia port 1433. Upewnij się użyć tego portu dla ustawień programu SQL Server AlwaysOn.
    * **HANA**. Obciążenia równoważenia portów Równoważenie obciążenia 35015 i 35017. Upewnij się zainstalować SAP HANA z liczby wystąpień **50**.
    Moduł równoważenia obciążenia używa port sondy 62550.
  -  **Rozmiar systemu SAP**. Ustaw liczbę protokoły SAP zapewnia nowy system. Jeśli nie wiadomo jak wiele protokoły SAP wymaga systemu, skontaktuj się z partnerem technologii SAP lub Integrator systemu.
  -  **Dostępność systemu**. Wybierz **HA**.
  -  **Nazwa użytkownika i hasło administratora**. Utwórz nowego użytkownika, który może służyć do logowania się na tym komputerze.
  -  **Identyfikator podsieci**. Wprowadź identyfikator podsieci, który został użyty podczas wdrażania szablonu ASCS/SCS lub identyfikator podsieci, który został utworzony jako część wdrożenia szablonu ASCS/SCS.

### <a name="application-servers-template"></a>Szablon serwerów aplikacji

Szablon aplikacji serwerów wdraża dwóch lub więcej maszyn wirtualnych, które mogą służyć jako wystąpień serwera aplikacji SAP jednego systemu SAP. Na przykład jeśli wdrożono szablon ASCS/SCS pięć systemów SAP, należy wdrożyć tego szablonu pięć razy.

Aby skonfigurować szablon identyfikatora SID wielu serwerów aplikacji, w [szablon identyfikatora SID wielu serwerów aplikacji] [ sap-templates-3-tier-multisid-apps-marketplace-image] lub [szablon identyfikatora SID wielu serwerów aplikacji za pomocą zarządzania dyskami] [ sap-templates-3-tier-multisid-apps-marketplace-image-md], wprowadź wartości dla następujących parametrów:

  -  **Identyfikator systemu SAP**. Wprowadź identyfikator systemu SAP systemu SAP, które chcesz zainstalować. Identyfikator jest służyć jako prefiks dla zasobów, które zostały wdrożone.
  -  **Typ systemu operacyjnego**. Wybierz system operacyjny maszyn wirtualnych.
  -  **Rozmiar systemu SAP**. Liczba protokoły SAP zapewnia nowy system. Jeśli nie wiadomo jak wiele protokoły SAP wymaga systemu, skontaktuj się z partnerem technologii SAP lub Integrator systemu.
  -  **Dostępność systemu**. Wybierz **HA**.
  -  **Nazwa użytkownika i hasło administratora**. Utwórz nowego użytkownika, który może służyć do logowania się na tym komputerze.
  -  **Identyfikator podsieci**. Wprowadź identyfikator podsieci, który został użyty podczas wdrażania szablonu ASCS/SCS lub identyfikator podsieci, który został utworzony jako część wdrożenia szablonu ASCS/SCS.


## <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Sieć wirtualna platformy Azure
W naszym przykładzie przestrzeni adresowej sieci wirtualnej platformy Azure jest 10.0.0.0/16. Brak jednej podsieci o nazwie **podsieci**, z zakresu adresów 10.0.0.0/24. Wszystkie maszyny wirtualne i wewnętrzne moduły równoważenia obciążenia są wdrażane w tej sieci wirtualnej.

> [!IMPORTANT]
> Nie wprowadzono żadnych zmian w ustawieniach sieci w systemie operacyjnym gościa. W tym adresy IP, serwery DNS i podsieci. Skonfiguruj ustawienia sieci na platformie Azure. Usługa protokołu dynamicznej konfiguracji hosta (DHCP) propaguje ustawienia.
>
>

## <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>Adresy IP serwera DNS

Aby ustawić wymagany adres IP DNS adresów, wykonaj następujące czynności:

1.  W portalu Azure na **serwerów DNS** bloku, upewnij się, że sieci wirtualnej **serwerów DNS** ustawiono opcję **niestandardowe DNS**.
2.  Wybierz ustawienia, na podstawie typu sieci, z którą masz. Więcej informacji zawierają następujące zasoby:
    * [Łączność sieci firmowej (między lokalizacjami)][planning-guide-2.2]: Dodaj adresy IP lokalnych serwerów DNS.  
    Można rozszerzyć lokalnych serwerów DNS do maszyn wirtualnych, które są uruchomione na platformie Azure. W tym scenariuszu można dodać adresy IP maszyn wirtualnych platformy Azure, na których uruchomiono usługi DNS.
    * [Tylko w chmurze wdrożenia][planning-guide-2.1]: wdrożyć dodatkowe maszyny wirtualnej, w tym samym wystąpieniu sieci wirtualnej, która służy jako serwer DNS. Dodaj adresy IP maszyn wirtualnych platformy Azure, które zostały skonfigurowane do uruchamiania usługi DNS.

    ![Rysunek 2: Konfigurowanie serwerów DNS dla sieci wirtualnej platformy Azure][sap-ha-guide-figure-3001]

    _**Rysunek 2.** DNS Konfigurowanie serwerów na potrzeby sieci wirtualnej platformy Azure_

  > [!NOTE]
  > W przypadku zmiany adresów IP serwerów DNS, musisz ponownie uruchomić maszyn wirtualnych platformy Azure, aby zastosować zmiany oraz propagację nowych serwerów DNS.
  >
  >

W tym przykładzie usługa DNS jest zainstalowany i skonfigurowany na tych maszyn wirtualnych systemu Windows:

| Roli maszyny wirtualnej | Nazwa hosta maszyny wirtualnej | Nazwa karty sieciowej | Statyczny adres IP |
| --- | --- | --- | --- |
| Pierwszy serwer DNS |domcontr 0 |PR1-nic-domcontr-0 |10.0.0.10 |
| Drugi serwer DNS |domcontr-1 |PR1-nic-domcontr-1 |10.0.0.11 |

## <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Nazwy hostów i adresów statycznego adresu IP dla SAP ASCS/SCS klastrowanego wystąpienia i DBMS klastrowanego wystąpienia

Dla wdrożenia lokalnego należy te hosta zarezerwowanych nazw i adresów IP:

| Rola nazwy hostów wirtualnych | Nazwy hostów wirtualnych | Wirtualne statycznego adresu IP |
| --- | --- | --- |
| Nazwa SAP ASCS/SCS do hostów wirtualnych pierwszej klastra (dla klastra zarządzania) |PR1-ascs-vir |10.0.0.42 |
| Nazwa hosta wirtualnego wystąpienia programu SAP ASCS/SCS |PR1 ascs sap |10.0.0.43 |
| Nazwa systemu DBMS SAP do hostów wirtualnych drugi klastra (klastra zarządzania) |PR1-dbms-vir |10.0.0.32 |

Podczas tworzenia klastra, można utworzyć nazwy hostów wirtualnych **pr1-ascs-vir** i **pr1-dbms-vir** i skojarzony adresy IP, które zarządzania klastrem. Aby dowiedzieć się, jak to zrobić, zobacz [zbieranie węzłów klastra w konfiguracji klastra][sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config].

Inne dwie nazwy hostów wirtualnych, można ręcznie utworzyć **pr1 ascs sap** i **pr1 dbms sap**i skojarzony adresy IP na serwerze DNS. Wystąpienia SAP ASCS/SCS klastra i klastrowanego wystąpienia systemu DBMS używać tych zasobów. Aby dowiedzieć się, jak to zrobić, zobacz [tworzenie nazwę hosta wirtualnego dla klastrowanego wystąpienia programu SAP ASCS/SCS] [sap-ha przewodnik-9.1.1].

## <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Ustaw adres statyczny adres IP dla maszyn wirtualnych SAP
Po wdrożeniu maszyny wirtualnej do użycia w klastrze, należy określić statycznych adresów IP dla wszystkich maszyn wirtualnych. W tym w konfiguracji sieci wirtualnej platformy Azure, a nie w systemie operacyjnym gościa.

1.  W portalu Azure wybierz **grupy zasobów** > **karta sieciowa** > **ustawienia** > **adres IP**.
2.  Na **adresów IP** bloku, w obszarze **przypisania**, wybierz pozycję **statycznych**. W **adres IP** wprowadź adres IP, który ma być używany.

  > [!NOTE]
  > W przypadku zmiany adresu IP karty sieciowej, musisz ponownie uruchomić maszyn wirtualnych platformy Azure, aby zastosować zmiany.  
  >
  >

  ![Rysunek 3: Ustaw statycznych adresów IP dla każdej maszyny wirtualnej karty sieciowej][sap-ha-guide-figure-3002]

  _**Rysunek 3.** ustawić statyczny adres IP dla każdej maszyny wirtualnej karty sieciowej_

  Powtórz ten krok dla wszystkich interfejsów sieciowych, dla wszystkich maszyn wirtualnych, włącznie ze maszyn wirtualnych, które ma być używany dla usługi Active Directory/serwera DNS.

W naszym przykładzie mamy tych maszyn wirtualnych i statycznymi adresami IP:

| Roli maszyny wirtualnej | Nazwa hosta maszyny wirtualnej | Nazwa karty sieciowej | Statyczny adres IP |
| --- | --- | --- | --- |
| Pierwsze wystąpienie serwera aplikacji SAP |PR1 podpisane 0 |PR1-nic podpisane-0 |10.0.0.50 |
| Drugie wystąpienie serwera aplikacji SAP |PR1 podpisane 1 |PR1-nic podpisane-1 |10.0.0.51 |
| Przyciski ... |Przyciski ... |Przyciski ... |Przyciski ... |
| Ostatnie wystąpienie serwera aplikacji SAP |PR1-podpisane-5 |PR1-nic podpisane-5 |10.0.0.55 |
| Pierwszym węźle klastra dla wystąpienia ASCS/SCS |PR1-ascs-0 |PR1-nic-ascs-0 |10.0.0.40 |
| Drugi węzeł klastra dla wystąpienia ASCS/SCS |PR1-ascs-1 |PR1-nic-ascs-1 |10.0.0.41 |
| Pierwszym węźle klastra dla systemu DBMS wystąpienia |PR1-db-0 |PR1-nic-db-0 |10.0.0.30 |
| Drugi węzeł klastra dla systemu DBMS wystąpienia |PR1-db-1 |PR1-nic-db-1 |10.0.0.31 |

## <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Ustaw adres statyczny adres IP usługi równoważenia obciążenia wewnętrznego Azure

Szablon SAP usługi Azure Resource Manager tworzy Azure wewnętrznego modułu równoważenia obciążenia używanego SAP ASCS/SCS wystąpienia klastra i klastrów systemu DBMS.

> [!IMPORTANT]
> Adres IP, nazwy hostów wirtualnych SAP ASCS/SCS jest taki sam jak adres IP usługi równoważenia obciążenia wewnętrznego SAP ASCS/SCS: **pr1-lb-ascs**.
> Adres IP, nazwy wirtualnego systemu DBMS jest taki sam jak adres IP usługi równoważenia obciążenia wewnętrznego systemu DBMS: **pr1-lb-dbms**.
>
>

Aby ustawić statyczny adres IP usługi równoważenia obciążenia wewnętrznego Azure:

1.  Początkowe wdrożenie ustawia adres IP usługi równoważenia obciążenia wewnętrznego **dynamiczne**. W portalu Azure na **adresów IP** bloku, w obszarze **przypisania**, wybierz pozycję **statycznych**.
2.  Ustawienie adresu IP usługi równoważenia obciążenia wewnętrznego **pr1-lb-ascs** adres IP nazwę hosta wirtualnego wystąpienia programu SAP ASCS/SCS.
3.  Ustawienie adresu IP usługi równoważenia obciążenia wewnętrznego **pr1-lb-dbms** adres IP nazwę hosta wirtualnego wystąpienia systemu DBMS.

  ![Rysunek 4: Ustaw statycznych adresów IP dla wewnętrznego modułu równoważenia obciążenia dla tego wystąpienia programu SAP ASCS/SCS][sap-ha-guide-figure-3003]

  _**Rysunek 4:** ustawić statycznych adresów IP dla wewnętrznego modułu równoważenia obciążenia dla tego wystąpienia programu SAP ASCS/SCS_

W naszym przykładzie mamy dwa Azure wewnętrzne moduły równoważenia obciążenia mających te statycznych adresów IP:

| Rola usługi równoważenia obciążenia wewnętrznego Azure | Nazwa modułu równoważenia obciążenia wewnętrznego platformy Azure | Statyczny adres IP |
| --- | --- | --- |
| SAP ASCS/SCS wystąpienia wewnętrznego modułu równoważenia obciążenia |PR1-lb-ascs |10.0.0.43 |
| System DBMS SAP wewnętrznego modułu równoważenia obciążenia |PR1-lb-dbms |10.0.0.33 |


## <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Domyślne ASCS/SCS reguły równoważenia obciążenia dla platformy Azure wewnętrznego modułu równoważenia obciążenia

Szablon SAP usługi Azure Resource Manager tworzy portów, które są potrzebne:
* Wystąpienie ABAP ASCS z domyślnej liczby wystąpień **00**
* Wystąpienie Java SCS, numer wystąpienia domyślnego **01**

Po zainstalowaniu wystąpieniem SAP ASCS/SCS, należy użyć domyślnej liczby wystąpień **00** dla wystąpienia programu ABAP ASCS i numer wystąpienia domyślnego **01** dla swojego wystąpienia Java SCS.

Następnie należy utworzyć wymagane równoważenia punktów końcowych dla portów SAP NetWeaver obciążenia wewnętrznego.

Aby utworzyć wymagane obciążenia wewnętrznego równoważenia punktów końcowych, najpierw należy utworzyć te równoważenia punktów końcowych dla portów SAP NetWeaver ABAP ASCS obciążenia:

| Nazwa reguły równoważenia obciążenia/usługi | Domyślne numery portów | Konkretnych portów (ASCS wystąpienia z liczby wystąpień 00) (Wywołujących z 10) |
| --- | --- | --- |
| Umieścić serwer / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| Serwer komunikatów ABAP / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Komunikat wewnętrzny ABAP / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| Serwer HTTP / *Lbrule8100* |81 <*InstanceNumber*> |8100 |
| SAP uruchamiania usługi ASCS HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP uruchamiania usługi ASCS HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Umieścić w kolejce replikacji / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP uruchamiania usługi Wywołujących HTTP *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP uruchamiania usługi Wywołujących HTTP *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Win RM *Lbrule5985* | |5985 |
| Udział plików *Lbrule445* | |445 |

**Tabela 1:** portu liczby wystąpień programu SAP NetWeaver ABAP ASCS

Następnie utwórz te równoważenia punktów końcowych dla portów SAP NetWeaver Java SCS obciążenia:

| Nazwa reguły równoważenia obciążenia/usługi | Domyślne numery portów | Konkretnych portów (SCS wystąpienia z liczby wystąpień 01) (Wywołujących z 11) |
| --- | --- | --- |
| Umieścić serwer / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Serwer bramy / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Serwer komunikatów Java / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| Serwer HTTP / *Lbrule8101* |81 <*InstanceNumber*> |8101 |
| SAP uruchamiania usługi SCS HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP uruchamiania usługi SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Umieścić w kolejce replikacji / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP uruchamiania usługi Wywołujących HTTP *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP uruchamiania usługi Wywołujących HTTP *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Win RM *Lbrule5985* | |5985 |
| Udział plików *Lbrule445* | |445 |

**Tabela 2:** portu liczby wystąpień programu SAP NetWeaver Java SCS

![Rysunek 5: Reguły dla usługi równoważenia obciążenia Azure wewnętrzny równoważenia obciążenia domyślne ASCS/SCS][sap-ha-guide-figure-3004]

_**Rysunek 5.** reguły dla usługi równoważenia obciążenia Azure wewnętrzny równoważenia obciążenia domyślne ASCS/SCS_

Ustawienie adresu IP usługi równoważenia obciążenia **pr1-lb-dbms** adres IP nazwę hosta wirtualnego wystąpienia systemu DBMS.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Zmień domyślny ASCS/SCS Równoważenie obciążenia reguły dla platformy Azure wewnętrznego modułu równoważenia obciążenia

Jeśli chcesz użyć innej liczby wystąpień SAP ASCS lub SCS, należy zmienić nazwy i wartości ich porty wartości domyślnych.

1.  W portalu Azure wybierz  **<* SID*> - lb - ascs załadować równoważenia ** > **reguły równoważenia obciążenia**.
2.  Dla wszystkich reguł, które należą do wystąpienia SAP ASCS lub SCS równoważenia obciążenia Zmień wartości tych:

  * Nazwa
  * Port
  * Port zaplecza

  Na przykład jeśli chcesz zmienić domyślną liczbę wystąpień ASCS od 00 do 31, musisz wprowadzić zmiany dla wszystkich portów wymienionych w tabeli 1.

  Oto przykład aktualizacji dla portu *lbrule3200*.

  ![Rysunek 6: Zmień obciążenia domyślne ASCS/SCS równoważenia zasady Azure wewnętrznego modułu równoważenia obciążenia][sap-ha-guide-figure-3005]

  _**Rysunek 6.** Zmień zasady dla usługi równoważenia obciążenia Azure wewnętrzny równoważenia obciążenia domyślne ASCS/SCS_

## <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Dodawanie maszyn wirtualnych systemu Windows do domeny

Po przypisaniu statycznego adresu IP do maszyn wirtualnych, Dodaj maszyny wirtualne do domeny.

![Rysunek 7: Dodaj maszynę wirtualną do domeny][sap-ha-guide-figure-3006]

_**Rysunek 7.** Dodaj maszynę wirtualną do domeny_

## <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Dodawanie wpisów rejestru na obu węzłach wystąpienia ASCS/SCS SAP

Moduł równoważenia obciążenia Azure zawiera wewnętrzny moduł równoważenia obciążenia zamknięciu połączeń w przypadku połączenia są w stanie bezczynności pewien okres czasu (limit czasu bezczynności). Procesy robocze SAP w oknie dialogowym wystąpień otwarte połączenia można umieścić w kolejce SAP przetworzyć zaraz po pierwszym umieścić w kolejce/usuwania z kolejki żądania musi być wysyłane. Te połączenia zazwyczaj pozostają ustalonych dopóki proces pracy lub ponownego uruchomienia procesu umieścić w kolejce. Jednak jeśli połączenie jest bezczynne na wybrany okres czasu, Azure wewnętrznego modułu równoważenia obciążenia zamyka połączenia. Nie stanowi problemu, ponieważ proces roboczy SAP przywraca połączenie do procesu umieścić w kolejce, jeśli już nie istnieje. Te działania są udokumentowane w ślady developer procesów SAP, ale ich tworzyć dużą ilością zawartości dodatkowe w tych danych śledzenia. Zaleca się zmienić TCP/IP `KeepAliveTime` i `KeepAliveInterval` na obu węzłów klastra. Połącz te zmiany w parametrach TCP/IP z parametrami profilu SAP, opisane w dalszej części tego artykułu.

Aby dodać wpisów rejestru na obu węzłach klastra z wystąpieniem SAP ASCS/SCS, najpierw dodaj te wpisu rejestru systemu Windows na obu węzłów klastra systemu Windows dla programu SAP ASCS/SCS:

| Ścieżka | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Nazwa zmiennej |`KeepAliveTime` |
| Typ zmiennej |REG_DWORD (dziesiętna) |
| Wartość |120000 |
| Połącz się z dokumentacją |[https://technet.microsoft.com/library/cc957549.aspx](https://technet.microsoft.com/library/cc957549.aspx) |

**Tabela 3:** zmienić pierwszy parametr TCP/IP

Następnie należy dodać ten wpisów rejestru systemu Windows na obu węzłów klastra systemu Windows dla programu SAP ASCS/SCS:

| Ścieżka | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Nazwa zmiennej |`KeepAliveInterval` |
| Typ zmiennej |REG_DWORD (dziesiętna) |
| Wartość |120000 |
| Połącz się z dokumentacją |[https://technet.microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

**Tabela 4:** zmienić drugi parametr TCP/IP

**Aby zastosować zmiany, należy ponownie uruchomić oba węzły klastra**.

## <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Konfigurowanie systemu Windows Server klastra trybu Failover Clustering dla wystąpienia ASCS/SCS SAP

Konfigurowanie klastra systemu Windows Server Failover Clustering dla wystąpienia programu SAP ASCS/SCS obejmuje następujące zadania:

- Zbieranie węzłów klastra w konfiguracji klastra
- Konfigurowanie monitora udziału plików klastra

### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Zbieraj węzły klastra w konfiguracji klastra

1.  Dodawanie roli i funkcji — Kreator Dodaj klaster dla obu węzłów klastra trybu failover.
2.  Konfigurowanie klastra trybu failover za pomocą Menedżera klastra trybu Failover. W Menedżerze klastra trybu Failover wybierz **tworzenia klastrów**, a następnie dodaj tylko nazwę klastra pierwszy węzeł A. Nie dodawaj drugiego węzła jeszcze; należy dodać drugi węzeł w kolejnym kroku.

  ![Rysunek 8: Dodaj nazwę serwera lub maszyny wirtualnej na pierwszym węźle klastra][sap-ha-guide-figure-3007]

  _**Rysunek 8.** Dodaj nazwę serwera lub maszyny wirtualnej w pierwszym węźle klastra_

3.  Wprowadź nazwę sieci (nazwa hosta wirtualnego) klastra.

  ![Rysunek 9: Wprowadź nazwę klastra][sap-ha-guide-figure-3008]

  _**Rysunek 9:** wprowadź nazwę klastra_

4.  Po utworzeniu klastra, uruchom test sprawdzania poprawności klastra.

  ![Rysunek 10: Uruchom sprawdzenie poprawności klastra][sap-ha-guide-figure-3009]

  _**Rysunek 10:** uruchom sprawdzenie poprawności klastra_

  Możesz zignorować ostrzeżenia dotyczące dysków na tym etapie procesu. Zostanie dodana monitora udostępniania plików i SIOS udostępnionych dysków później. Na tym etapie nie trzeba martwić o kworum.

  ![Rysunek 11: Odnaleziono żadnego dysku kworum][sap-ha-guide-figure-3010]

  _**Rysunek 11:** odnaleziono żadnego dysku kworum_

  ![Rysunek 12: Zasobu klastra Core wymaga nowego adresu IP][sap-ha-guide-figure-3011]

  _**Rysunek 12:** zasobu klastra Core wymaga nowego adresu IP_

5.  Zmienianie adresu IP usługi klastrowania core. Klastra nie można uruchomić, dopóki nie zmieni adres IP klastra usługi podstawowej, ponieważ adres IP serwera wskazuje na jednym z węzłów maszyny wirtualnej. To zrobić na **właściwości** strony podstawowej usługi klastrowania IP zasobu.

  Na przykład, należy przypisać adres IP (w naszym przykładzie **10.0.0.42**) dla nazwy hostów wirtualnych klastra **pr1-ascs-vir**.

  ![Rysunek 13: W oknie dialogowym właściwości Zmień adres IP][sap-ha-guide-figure-3012]

  _**Rysunek 13:** w **właściwości** okno dialogowe Zmień adres IP_

  ![Rysunek 14: Przypisz adres IP, które jest zastrzeżone dla klastra][sap-ha-guide-figure-3013]

  _**Rysunek 14:** przypisać adres IP, które jest zastrzeżone dla klastra_

6.  Przełącz tryb online nazwę hosta wirtualnego klastra.

  ![Rysunek 15: Usługi podstawowej klastra jest uruchomiony i działa i o poprawny adres IP adresów][sap-ha-guide-figure-3014]

  _**Rysunek 15:** usługi podstawowej klastra jest uruchomiony i działa prawidłowo i z poprawny adres IP adresów_

7.  Dodawanie drugiego węzła klastra.

  Teraz, podstawowa usługa klastrowania jest uruchomiona, można dodać drugiego węzła klastra.

  ![Rysunek 16 Dodaj drugi węzeł klastra][sap-ha-guide-figure-3015]

  _**Rysunek 16:** dodania drugiego węzła klastra_

8.  Wprowadź nazwę hosta drugiego węzła klastra.

  ![Rysunek 17: Wprowadź nazwę hosta drugiego węzła klastra][sap-ha-guide-figure-3016]

  _**Rysunek 17:** wprowadź nazwę hosta drugiego węzła klastra_

  > [!IMPORTANT]
  > Upewnij się, że **Dodaj wszystkie odpowiednie magazyny do klastra** pole wyboru jest **nie** wybrane.  
  >
  >

  ![Rysunek 18: Nie zaznaczaj pola wyboru][sap-ha-guide-figure-3017]

  _**Rysunek 18:** czy **nie** zaznacz pole wyboru_

  Możesz zignorować ostrzeżenia dotyczące kworum i dysków. Należy ustawić kworum i udostępnić dysk później, zgodnie z opisem w [instalowanie SIOS DataKeeper Cluster Edition dla programu SAP ASCS/SCS dysku klastra, udziału][sap-high-availability-infrastructure-wsfc-shared-disk-install-sios].

  ![Rysunek 19: Ignoruj ostrzeżenia dotyczące dysku kworum][sap-ha-guide-figure-3018]

  _**Rysunek 19:** Ignoruj ostrzeżenia dotyczące dysku kworum_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Skonfiguruj Monitor udziału plików klastra

Konfigurowanie monitora udziału plików klastra obejmuje następujące zadania:

- Tworzenie udziału plików
- Ustawienia kworum monitora udziału plików w Menedżerze klastra trybu Failover

#### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Tworzenie udziału plików

1.  Wybierz monitor udziału plików, zamiast dysku kworum. SIOS DataKeeper obsługuje tę opcję.

  W przykładach w niniejszym artykule monitora udziału plików jest na serwerze/DNS usługi Active Directory, który działa na platformie Azure. Monitor udziału plików jest nazywany **domcontr 0**. Czy skonfigurować połączenia sieci VPN platformy Azure (za pośrednictwem sieci VPN typu lokacja-lokacja lub usługa Azure ExpressRoute), monitor udostępniania sieci/DNS usługi Active Directory usługi lokalnej i nie jest odpowiedni do uruchomienia pliku.

  > [!NOTE]
  > Jeśli usługa/DNS usługi Active Directory działa tylko na lokalne, nie skonfigurować Twoje monitora udziału plików na Active Directory/serwera DNS systemu operacyjnego działającej lokalnie. Opóźnienie sieci między węzłami klastra z systemem Azure i/DNS usługi Active Directory w lokalnym programie może być zbyt duży i spowodować problemy z połączeniem. Należy skonfigurować monitor udostępniania plików na maszynie wirtualnej platformy Azure, uruchomionym bliski węzeł klastra.  
  >
  >

  Stacja kworum wymaga co najmniej 1024 MB wolnego miejsca. Zaleca się 2048 MB wolnego miejsca na dysku kworum.

2.  Dodaj obiekt nazwy klastra.

  ![Rysunek 20: Przypisz uprawnienia udziału dla obiekt nazwy klastra][sap-ha-guide-figure-3019]

  _**Rysunek 20:** przypisać uprawnienia do udziału dla obiekt nazwy klastra_

  Pamiętaj, że uprawnienia obejmuje uprawnienia do zmiany danych w udziale dla obiekt nazwy klastra (w naszym przykładzie **pr1-ascs-vir$**).

3.  Aby dodać obiekt nazwy klastra do listy, wybierz opcję **Dodaj**. Zmień filtr, aby Sprawdź, czy obiekty komputerów, oprócz tych górny rysunku.

  ![Rysunek 21: Zmień typy obiektów, aby obejmują komputery][sap-ha-guide-figure-3020]

  _**Rysunek 21:** zmienić typ obiektu do obejmują komputery_

  ![Rysunek 22: Zaznacz pole wyboru komputerów][sap-ha-guide-figure-3021]

  _**Rysunek 22:** wybierz **komputery** pole wyboru_

4.  Wprowadź obiektem nazwy klastra, jak pokazano w **rysunek: Zmień typy obiektów, aby obejmują komputery**. Ponieważ rekord został już utworzony, można zmienić uprawnienia, jak pokazano w **rysunek: przypisz uprawnienia udziału dla obiekt nazwy klastra**.

5.  Wybierz **zabezpieczeń** karcie udziału, a następnie ustawić bardziej szczegółowe uprawnienia dla obiekt nazwy klastra.

  ![Rysunek 23: Ustawić atrybutów zabezpieczeń dla obiekt nazwy klastra kworum udziału plików][sap-ha-guide-figure-3022]

  _**Rysunek 23:** ustawić atrybutów zabezpieczeń dla obiekt nazwy klastra kworum udziału plików_

#### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Ustaw kworum monitora udziału plików w Menedżerze klastra trybu Failover

1.  Otwórz kworum ustawienia kreatora konfiguracji.

  ![Rysunek 24: Uruchom Kreator ustawienia kworum klastra Konfiguruj][sap-ha-guide-figure-3023]

  _**Rysunek 24:** Uruchamianie Kreatora ustawienie Konfigurowanie kworum klastra_

2.  Na **wybrać konfigurację kworum** wybierz **Wybieranie monitora kworum**.

  ![Rysunek 25: Konfiguracji kworum, możesz wybrać z][sap-ha-guide-figure-3024]

  _**Rysunek 25:** konfiguracji kworum, możesz wybrać spośród_

3.  Na **Wybieranie monitora kworum** wybierz **Konfigurowanie monitora udziału plików**.

  ![Rysunek 26: Wybierz monitor udziału plików][sap-ha-guide-figure-3025]

  _**Rysunek 26:** Wybierz monitor udziału plików_

4.  Wprowadź ścieżkę UNC do udziału plików (w naszym przykładzie \\domcontr 0\FSW). Aby wyświetlić listę wprowadzania zmian, zaznacz **dalej**.

  ![Rysunek 27: Zdefiniuj lokalizację udziału plików monitora udziału][sap-ha-guide-figure-3026]

  _**Rysunek 27:** określić lokalizację udziału plików monitora udziału_

5.  Wybierz zmiany, a następnie wybierz **dalej**. Musisz pomyślnie skonfigurować konfigurację klastra, jak pokazano:  

  ![Rysunek 28: Potwierdzenie, że po zmianie konfiguracji klastra][sap-ha-guide-figure-3027]

  _**Rysunek 28:** potwierdzenie, że po zmianie konfiguracji klastra_

Po pomyślnym zainstalowaniu klastra pracy awaryjnej systemu Windows, należy dokonać niektórych progi w celu dostosowania wykrywania trybu failover do warunków na platformie Azure zmiany. Można zmienić parametry są opisane w blogu [dostrajanie progi sieci klastra pracy awaryjnej][tuning-failover-cluster-network-thresholds]. Przy założeniu, że Twoje dwóch maszyn wirtualnych, które kompilacji konfiguracji klastra systemu Windows dla ASCS/SCS znajdują się w tej samej podsieci, wymaga zmiany tych wartości następujących parametrów:

- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Te ustawienia zostały przetestowane z klientami i dostępne dobrej naruszenia pozwala uzyskać odporność na jednej stronie. Z drugiej strony te ustawienia zostały udostępnianie fast wystarczająco dużo pracy awaryjnej w warunkach rzeczywistych błędu w przypadku awarii oprogramowania lub węzeł/wirtualna SAP.

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Zainstaluj dla dysku udziału klastra ASCS/SCS SAP SIOS DataKeeper Cluster Edition

Masz teraz pracy konfiguracji klastra trybu Failover systemu Windows Server na platformie Azure. Jednak, aby zainstalować wystąpienie programu SAP ASCS/SCS, zasób udostępniony dysk. Nie można utworzyć o współużytkowane zasoby dyskowe potrzebne na platformie Azure. SIOS DataKeeper Cluster Edition jest rozwiązań innych firm, których można używać do tworzenia współużytkowane zasoby dyskowe.

Instalowanie SIOS DataKeeper Cluster Edition dla udziału dysk klastrowy SAP ASCS/SCS obejmuje następujące zadania:

- Dodawanie programu .NET Framework 3.5
- Instalowanie SIOS DataKeeper
- Konfigurowanie SIOS DataKeeper

### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Dodaj programu .NET Framework 3.5
Microsoft .NET Framework 3.5 nie jest automatycznie aktywowane lub zainstalowany w systemie Windows Server 2012 R2. Ponieważ SIOS DataKeeper wymaga programu .NET Framework jako we wszystkich węzłach, które należy zainstalować na DataKeeper, należy zainstalować program .NET Framework 3.5 w systemie operacyjnym gościa wszystkich maszyn wirtualnych w klastrze.

Istnieją dwa sposoby dodawania programu .NET Framework 3.5:

- Użyj funkcji Kreatora dodawania ról i w systemie Windows, jak pokazano:

  ![Rysunek 29: Zainstaluj program .NET Framework 3.5 przy użyciu funkcji Kreatora dodawania ról i][sap-ha-guide-figure-3028]

  _**Rysunek 29:** zainstalować program .NET Framework 3.5 przy użyciu dodawania ról i funkcji — Kreator_

  ![30 rysunek: Postęp instalacji pasek po zainstalowaniu programu .NET Framework 3.5 przy użyciu dodawania ról i funkcji — Kreator][sap-ha-guide-figure-3029]

  _**Rysunek 30:** pasek po zainstalowaniu programu .NET Framework 3.5 przy użyciu funkcji Kreatora dodawania ról i postępu instalacji_

- Użyj narzędzia wiersza polecenia dism.exe. Ten typ instalacji należy uzyskać dostępu do katalogu SxS na nośniku instalacyjnym systemu Windows. W wierszu polecenia z podwyższonym poziomem uprawnień wpisz polecenie:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>Zainstaluj SIOS DataKeeper

Zainstaluj SIOS DataKeeper Cluster Edition w każdym węźle w klastrze. Aby utworzyć wirtualny magazynu udostępnionego z SIOS DataKeeper, Utwórz zsynchronizowanej dublowania i następnie symulować magazyn udostępniony klastra.

Przed zainstalowaniem oprogramowania SIOS, Utwórz użytkownika domeny **DataKeeperSvc**.

> [!NOTE]
> Dodaj **DataKeeperSvc** użytkownikowi **administratora lokalnego** na obu węzłów klastra.
>
>

Aby zainstalować SIOS DataKeeper:

1.  Zainstaluj oprogramowanie SIOS na obu węzłów klastra.

  ![Instalator SIOS][sap-ha-guide-figure-3030]

  ![31 rysunku: Pierwsza strona instalacji SIOS DataKeeper][sap-ha-guide-figure-3031]

  _**Rysunek 31:** pierwszej strony instalacji SIOS DataKeeper_

2.  W oknie dialogowym wybierz **tak**.

  ![Rysunek 32: DataKeeper informuje, czy usługa zostanie wyłączona][sap-ha-guide-figure-3032]

  _**Rysunek 32:** DataKeeper informuje, że usługa zostanie wyłączona_

3.  W oknie dialogowym zaleca się, że wybrano **konta domeny lub tego serwera**.

  ![Rysunek nr 33: Wybór użytkownika dla SIOS DataKeeper][sap-ha-guide-figure-3033]

  _**Rysunek nr 33:** SIOS DataKeeper określonego użytkownika_

4.  Wprowadź nazwę użytkownika konta domeny i hasła, które zostały utworzone dla SIOS DataKeeper.

  ![Rysunek 34: Wprowadź nazwę użytkownika domeny i hasło dla instalacji SIOS DataKeeper][sap-ha-guide-figure-3034]

  _**Rysunek 34:** wprowadź nazwę użytkownika domeny i hasło dla instalacji SIOS DataKeeper_

5.  Zainstaluj klucz licencji dla swojego wystąpienia SIOS DataKeeper, jak pokazano na rysunku 45.

  ![Rysunek 35: Wprowadź klucz licencji SIOS DataKeeper][sap-ha-guide-figure-3035]

  _**Rysunek 35:** wprowadź klucz licencji SIOS DataKeeper_

6.  Po wyświetleniu monitu uruchom ponownie maszynę wirtualną.

### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>Konfigurowanie SIOS DataKeeper

Po zainstalowaniu SIOS DataKeeper na obu węzłach, należy uruchomić konfigurację. Cel konfiguracji ma synchroniczne dane replikacji między dodatkowych dysków dołączonych do poszczególnych maszyn wirtualnych.

1.  Uruchom narzędzie DataKeeper zarządzania i konfiguracji, a następnie wybierz **połączenia serwera**. (Ta opcja jest zaznaczona kółkiem na czerwono).

  ![Rysunek 36: SIOS DataKeeper zarządzanie i narzędzia do konfiguracji][sap-ha-guide-figure-3036]

  _**Rysunek 36:** narzędzie SIOS DataKeeper zarządzania i konfiguracji_

2.  Wprowadź nazwę lub adres TCP/IP pierwszy węzeł, który narzędzia zarządzania i konfiguracji należy połączenia oraz, w drugim kroku drugiego węzła.

  ![Rysunek 37: Wstaw nazwę lub adres TCP/IP pierwszego węzła Zarządzanie i narzędzia do konfiguracji należy łączyć, a w drugim etapie drugiego węzła][sap-ha-guide-figure-3037]

  _**Rysunek 37:** Wstaw nazwę lub adres TCP/IP pierwszego węzła narzędzie zarządzania i konfiguracji należy łączyć, a w drugim etapie drugiego węzła_

3.  Utwórz zadanie replikacji między dwoma węzłami.

  ![Rysunek 38: Tworzenie zadania replikacji][sap-ha-guide-figure-3038]

  _**Rysunek 38:** Utwórz zadanie replikacji_

  Kreator prowadzi przez proces tworzenia zadania replikacji.
4.  Zdefiniuj nazwę, adres TCP/IP i wolumin dysku węzła źródłowego.

  ![Rysunek 39: Zdefiniuj nazwę zadania replikacji][sap-ha-guide-figure-3039]

  _**Rysunek 39:** Zdefiniuj nazwę zadania replikacji_

  ![Rysunek 40: Definiowanie danych podstawowych dla tego węzła, który powinien być bieżącego węzła źródłowego][sap-ha-guide-figure-3040]

  _**Rysunek 40:** zdefiniować podstawowe dane dla tego węzła, który powinien być bieżącego węzła źródłowego_

5.  Zdefiniuj nazwę, adres TCP/IP i wolumin dysku węzła docelowego.

  ![Rysunek 41: Definiowanie danych podstawowych dla tego węzła, który powinien być bieżący węzeł docelowy][sap-ha-guide-figure-3041]

  _**Rysunek 41:** zdefiniować podstawowe dane dla tego węzła, który powinien być bieżący węzeł docelowy_

6.  Zdefiniuj algorytmy kompresji. W naszym przykładzie zaleca się, że kompresji strumienia replikacji. Szczególnie w sytuacjach, ponowna synchronizacja kompresji, replikacji strumienia znacznie skraca czas ponownej synchronizacji. Kompresja używa zasobów Procesora i pamięci RAM maszyny wirtualnej. Jak zwiększa się częstotliwość kompresji, co powoduje ilości zasobów procesora CPU używanych. Można również dostosować to ustawienie później.

7.  Inne ustawienie, które należy sprawdzić to, czy replikacja odbywa się asynchronicznie lub synchronicznie. *W przypadku ochrony SAP ASCS/SCS konfiguracji, należy użyć Replikacja synchroniczna*.  

  ![Rysunek 42: Zdefiniuj szczegóły replikacji][sap-ha-guide-figure-3042]

  _**Rysunek 42:** Zdefiniuj szczegóły replikacji_

8.  Należy określić, czy wolumin, który jest replikowany za zadanie replikacji powinny być reprezentowane w konfiguracji klastra z systemem Windows Server Failover Clustering jako udostępniony dysk. SAP ASCS/SCS konfiguracji, wybierz **tak** tak, aby klaster systemu Windows widzi replikowanego woluminu jako udostępnionego dysku, której można użyć jako woluminu klastra.

  ![Rysunek 43: Wybierz tak, aby ustawić replikowanego woluminu jako woluminu klastra][sap-ha-guide-figure-3043]

  _**Rysunek 43:** wybierz **tak** można ustawić replikowanego woluminu jako woluminu klastra_

  Po utworzeniu woluminu narzędzie DataKeeper zarządzania i konfiguracji pokazuje, że zadanie replikacji jest aktywne.

  ![Rysunek 44: DataKeeper synchronicznego duplikowania dysku udziału SAP ASCS/SCS jest aktywny][sap-ha-guide-figure-3044]

  _**Rysunek 44:** DataKeeper duplikowanie synchroniczne dla ASCS/SCS programu SAP współużytkować dysku jest aktywny_

  Menedżer klastra trybu failover zawiera obecnie dysku jako dysku DataKeeper, jak pokazano:

  ![Rysunek 45: Menedżer klastra trybu Failover zawiera dysk, który jest replikowany DataKeeper][sap-ha-guide-figure-3045]

  _**Rysunek 45:** Menedżera klastra trybu Failover zawiera dysk tego DataKeeper replikowane_

## <a name="next-steps"></a>Następne kroki

* [SAP NetWeaver HA instalacji, używając **klastra pracy awaryjnej systemu Windows** i **udostępnionego dysku** dla wystąpienia SCS SAP (A)][sap-high-availability-installation-wsfc-shared-disk]
