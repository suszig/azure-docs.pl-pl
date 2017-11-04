---
title: "Architektura wysoką dostępność maszyn wirtualnych platformy Azure i scenariusze dla programu SAP NetWeaver | Dokumentacja firmy Microsoft"
description: "Architektura wysokiej dostępności (HA) i scenariusze dla programu SAP NetWeaver na maszynach wirtualnych Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 867fe2835418a48e4e616d8137ba9fa4182c8fb7
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="high-availability-architecture-and-scenarios-for-sap-netweaver"></a>Architektura wysokiej dostępności i scenariusze dla programu SAP NetWeaver

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[sap-installation-guides]:http://service.sap.com/instguides

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
[sap-ascs-ha-multi-sid-wsfc-file-share]:sap-ascs-ha-multi-sid-wsfc-file-share.md
[sap-ascs-ha-multi-sid-wsfc-shared-disk]:sap-ascs-ha-multi-sid-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md
[sap-higher-availability]:sap-higher-availability-architecture-scenarios.md

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md

[sap-ha-bc-virtual-env-hyperv-vmware-white-paper]:http://scn.sap.com/docs/DOC-44415
[sap-ha-partner-information]:http://scn.sap.com/docs/DOC-8541
[azure-sla]:https://azure.microsoft.com/support/legal/sla/
[azure-virtual-machines-manage-availability]:http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability
[azure-storage-redundancy]:http://azure.microsoft.com/documentation/articles/storage-redundancy/
[azure-storage-managed-disks-overview]:https://docs.microsoft.com/azure/storage/storage-managed-disks-overview

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


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


## <a name="definition-of-terminologies"></a>Definicja terminologii

Termin **wysokiej dostępności (HA)** powiązany jest to zestaw technologii minimalizujące zakłóceń IT, zapewniając ciągłość prowadzenia działalności biznesowej usługi IT za pośrednictwem nadmiarowe, odpornej na uszkodzenia, lub pracy awaryjnej chronione składniki wewnątrz **tego samego** centrum danych. W tym przypadku w ramach jednego regionu Azure.

**Odzyskiwania awaryjnego (DR)** również jest docelowo zminimalizować zakłócenia usług IT i ich odzyskiwania ale wielu **różnych** centrów danych, które zlokalizować setki kilometrów optymalizacji. W tym przypadku zwykle od różnych regionach platformy Azure, w tym samym regionie geograficznymi lub jako ustalonych przez użytkownika jako klient.


## <a name="overview-of-high-availability"></a>Przegląd informacji o wysokiej dostępności
Można oddzielić omówienie SAP wysokiej dostępności na platformie Azure na trzy części:

* **Wysoka dostępność infrastruktury platformy Azure**, na przykład wysokiej dostępności mocy obliczeniowej (maszyn wirtualnych), sieci, magazynu itp. oraz korzyści zwiększenia dostępności aplikacji SAP.

* **Przy użyciu ponownego uruchomienia maszyny Wirtualnej infrastruktury platformy Azure w celu osiągnięcia "Wyższej dostępności" aplikacje SAP**

  Jeśli nie chcesz używać funkcji takich jak Windows Server Failover Clustering (WSFC) lub rozrusznik w systemie Linux, uruchom ponownie maszyny Wirtualnej Azure jest wykorzystywany do ochrony systemu SAP przed planowanych lub nieplanowanych przestojów infrastruktury Azure serwera fizycznego i ogólnej podstawowej platformy Azure.


* **Wysoka dostępność aplikacji SAP**

  Aby osiągnąć pełną SAP systemu wysokiej dostępności, musimy ochrony wszystkich krytycznych składników systemu SAP, na przykład:
  * Nadmiarowe **serwerów aplikacji SAP**, i
  * Unikatowych składników (na przykład **pojedynczego punktu awarii (SPOF)**) jak
    * **SAP () wystąpienia SCS** i
    *  **SYSTEM DBMS**.


SAP wysokiej dostępności na platformie Azure ma pewne różnice w porównaniu do programu SAP wysokiej dostępności w lokalnym środowisku fizycznym lub wirtualnym. Następujący dokument [SAP NetWeaver wysoką dostępność i ciągłość prowadzenia działalności biznesowej w środowiskach wirtualnych VMware i funkcji Hyper-V w systemie Microsoft Windows] [ sap-ha-bc-virtual-env-hyperv-vmware-white-paper] opisuje standardowe SAP wysokiej dostępności. konfiguracje w zwirtualizowanych środowiskach, w systemie Windows.

Nie istnieje zintegrowane sapinst SAP-HA konfiguracja dla systemu Linux, takich jak istnieje dla systemu Windows. Dotyczące SAP HA lokalnymi Linux znaleźć więcej informacji w [informacje o partnerze wysokiej dostępności][sap-ha-partner-information].

## <a name="azure-infrastructure-high-availability"></a>Wysoka dostępność infrastruktury platformy Azure

### <a name="single-instance-virtual-machine-sla"></a>Pojedyncze wystąpienie maszyny wirtualnej umowy SLA

Obecnie jest umowy SLA dla maszyn wirtualnych na jednym z wartością 99,9% z magazyn w warstwie premium. Aby poznać jak może wyglądać dostępności jednej maszyny wirtualnej, można tworzyć produktu różnych dostępnych [umowy dotyczące poziomu usług Azure][azure-sla].

Podstawa obliczania to 30 dni w miesiącu lub — 43 200 minut. W związku z tym przestoju 0,05% odpowiada 21,6 minut. Dostępność usług różnych należy pomnożyć w zwykły sposób, w następujący sposób:

(Usługa dostępności #1/100) * (dostępności usługi #2/100) * (dostępności usługi #3/100) \*...

Na przykład:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 lub ogólnej dostępności 99.75%.

### <a name="multiple-instances-of-virtual-machines-in-the-same-availability-set"></a>Wiele wystąpień z maszyn wirtualnych w tym samym zestawie dostępności
Dla wszystkich maszyn wirtualnych, które mają co najmniej dwa wystąpienia wdrażane w taki sam **zestawu dostępności**, gwarantujemy, że masz łączność maszyny wirtualnej do co najmniej jedno wystąpienie 99,95% czasu.

Jeśli co najmniej dwie maszyny wirtualne są częścią tego samego zestawu dostępności, przypisaną każdej maszyny wirtualnej w zestawie dostępności **domeny aktualizacji** i **domeny błędów** przez podstawowej platformy Azure.

**Odporność domen** gwarantuje, że maszyny wirtualne są wdrażane na inny sprzęt, który nie współużytkują wspólne źródło i sieci przełącznik. Po migracji ma wpływ nieplanowane przestoje serwerów, przełącznik sieci lub źródła zasilania, tylko jedna maszyna wirtualna.

**Aktualizowanie domeny** gwarancji, które różnych maszyn wirtualnych nie są ponownie uruchamiane w tym samym czasie podczas zaplanowanej konserwacji infrastruktury platformy Azure, ale tylko jedna maszyna wirtualna jest ponowny rozruch w czasie.

Aby uzyskać więcej informacji, zobacz [Zarządzaj dostępnością maszyn wirtualnych systemu Windows na platformie Azure][azure-virtual-machines-manage-availability].

Zestaw dostępności jest używany do uzyskania wysokiej dostępności:

* Serwery aplikacji SAP geograficznie nadmiarowy  

* Klastry z co najmniej dwa węzły (np. maszyny wirtualne), które zapewniają ochronę SPOFs, takie jak wystąpienia SCS SAP (A) i bazami danych

### <a name="virtual-machine-vm-planned-and-unplanned-maintenance"></a>Maszyna wirtualna (VM), planowane i nieplanowana konserwacja

Istnieją dwa typy zdarzeń platformy Azure, które mogą wpłynąć na dostępność maszyn wirtualnych: planowanych konserwacji i nieplanowanych konserwacji.

* **Zaplanowanej konserwacji** okresowych aktualizacji utworzone przez firmę Microsoft podstawowej platformy Azure, aby zwiększyć ogólną niezawodność, wydajność i bezpieczeństwo infrastruktury platformy, które są uruchamiane maszyny wirtualne są zdarzenia.

* **Nieplanowana konserwacja** zdarzenia występują po sprzętu lub infrastruktury fizycznej podstawowej maszyny wirtualnej wystąpił błąd w określony sposób. Mogą być to awarie sieci lokalnej, błędy na dysku lokalnym lub inne awarie na poziomie regału. W przypadku wykrycia takiej awarii platformy Azure automatycznie wykonuje migrację maszyny wirtualnej z złej kondycji serwera fizycznego obsługującego maszyny wirtualnej do dobrej kondycji serwera fizycznego. Te zdarzenia występują rzadko, ale mogą również spowodować ponowne uruchomienie maszyny wirtualnej.

Aby uzyskać więcej informacji, zobacz [Zarządzaj dostępnością maszyn wirtualnych systemu Windows na platformie Azure][azure-virtual-machines-manage-availability].

### <a name="azure-storage-redundancy"></a>Nadmiarowość magazynu Azure
Dane na koncie magazynu Microsoft Azure jest zawsze replikowane w celu zapewnienia trwałości i wysokiej dostępności, spotkania SLA magazynu platformy Azure, nawet w wypadku przejściowych awarii sprzętu.

Ponieważ Magazyn Azure może być utrzymywanie trzy obrazy danych domyślnie, RAID5 lub RAID1 na wielu dyskach platformy Azure nie jest konieczne.

Aby uzyskać więcej informacji, zobacz [replikacja usługi Azure Storage][azure-storage-redundancy].

### <a name="azure-managed-disks"></a>Azure Managed Disks
Dyski zarządzane są nowego typu zasobu w usłudze Azure Resource Manager używany zamiast wirtualne dyski twarde, które są przechowywane na kontach magazynu Azure. Dyski zarządzane automatycznie Dopasuj z zestawu dostępności maszyny wirtualnej, które są dołączone i w związku z tym zwiększenia dostępności maszyny wirtualnej i usług, które są uruchomione na maszynie wirtualnej.
Aby uzyskać więcej informacji, zobacz [omówienie dysków zarządzanych Azure][azure-storage-managed-disks-overview].

Firma Microsoft zaleca, aby użyć dysku zarządzanego, ponieważ ich uprościć wdrażanie i zarządzanie maszyn wirtualnych.
**SAP aktualnie obsługuje tylko dysków zarządzanych w warstwie Premium**. Aby uzyskać więcej informacji, przeczytaj Uwaga SAP [1928533].

## <a name="utilizing-azure-infrastructure-ha-to-achieve-sap-application-higher-availability"></a>Przy użyciu infrastruktury platformy Azure wysokiej dostępności, aby zapewnić dostępność "Wyżej" aplikacji SAP

Jeśli nie chcesz używać funkcji takich jak Windows Server Failover Clustering (WSFC) lub rozrusznik w systemie Linux (obecnie obsługiwana tylko dla SLES 12 lub nowszym), ponowne uruchomienie maszyny Wirtualnej Azure jest wykorzystywany do ochrony systemu SAP przed planowanych lub nieplanowanych przestojów infrastruktury Azure serwera fizycznego i ogólnej podstawowej platformy Azure.

Takie podejście jest bardziej opisany w następującym dokumencie [przy użyciu usługi Azure infrastruktury maszyny Wirtualnej po ponownym uruchomieniu Achieve "Wyższej dostępności" systemu SAP][sap-higher-availability].

## <a name="baed0eb3-c662-4405-b114-24c10a62954e"></a>SAP wysoką dostępność aplikacji na IaaS platformy Azure

Aby osiągnąć pełną SAP systemu wysokiej dostępności, musimy ochrony wszystkich krytycznych składników systemu SAP, na przykład:

* Nadmiarowe **serwerów aplikacji SAP**, i

* Unikatowych składników (na przykład **pojedynczego punktu awarii (SPOF)**) jak
  * **SAP () wystąpienia SCS** i
  *  **SYSTEM DBMS**.

Stosuje omówiono szczegółowo poniżej sposobu zapewniania wysokiej dostępności dla wszystkich składników systemu SAP krytyczne trzy.

### <a name="high-availability-for-sap-application-servers"></a>Wysoka dostępność dla serwerów aplikacji SAP

> W tym rozdziale dotyczy zarówno dla:
>
> ![Windows][Logo_Windows] System Windows i ![Linux][Logo_Linux] Linux
>

Zwykle nie trzeba konkretnego rozwiązania wysokiej dostępności dla wystąpień serwera aplikacji SAP i okna dialogowe. Zapewniania wysokiej dostępności przez nadmiarowości i skonfiguruj wiele wystąpień w oknie dialogowym w różnych wystąpień maszyn wirtualnych Azure. Powinien mieć co najmniej dwóch wystąpień aplikacji SAP zainstalowane w dwóch wystąpień maszyn wirtualnych Azure.

![Rysunek 1: Serwer aplikacji SAP wysokiej dostępności][sap-ha-guide-figure-2000]

_**Rysunek 1.** SAP wysokiej dostępności serwera aplikacji_

Należy umieścić wszystkich maszyn wirtualnych, które ustawić wystąpień serwera aplikacji SAP hosta w tym samym dostępności Azure. Zestaw dostępności Azure upewnia się, że:

* Wszystkie maszyny wirtualne są częścią tego samego **domeny uaktualnienia**. Domeny uaktualnienia, na przykład upewnia się, że maszyny wirtualne nie są aktualizowane w tym samym czasie podczas zaplanowanej konserwacji przestoju.
Podstawowe funkcje, bazując na różnych uaktualnienia i domen błędów w ramach jednostki skalowania Azure już została wprowadzona w rozdziale [domen uaktualnienia][planning-guide-3.2.2].

* Wszystkie maszyny wirtualne są częścią tego samego **domeny błędów**. Domeny błędów, na przykład upewnia się, że maszyny wirtualne są wdrażane tak, aby nie pojedynczego punktu awarii wpływa na dostępność maszyn wirtualnych.

Brak nie nieograniczoną liczbę usterek i domen uaktualnienia, używanego przez Azure zestawu dostępności w ramach jednostki skalowania Azure. Oznacza to, że wprowadzenia liczby maszyn wirtualnych do jednego zestawu dostępności, wcześniej lub później więcej niż jednej maszyny Wirtualnej kończy się w tej samej domeny uaktualnienia lub błędów.

Wdrażanie kilka wystąpień serwera aplikacji SAP w ich dedykowanych maszyn wirtualnych i przy założeniu, że dotarliśmy pięć uaktualnienia domen, na końcu pojawia się poniżej. Rzeczywiste maksymalna liczba domen błędów i aktualizacji w ramach zestawu dostępności może zmieniać się w przyszłości:

![Rysunek 2: HA SAP serwerów aplikacji w zestawie dostępności Azure][planning-guide-figure-3000]
_**na rysunku 2:** HA programu SAP serwerów aplikacji w zestawie dostępności Azure_ więcej szczegółów można znaleźć w tej dokumentacji: [Zarządzaj dostępnością maszyn wirtualnych][virtual-machines-manage-availability].


Zestawy dostępności Azure były widoczne w rozdziale [zestawami dostępności Azure] [ planning-guide-3.2.3] maszyny wirtualne Azure planowania i wdrażania dla programu SAP NetWeaver dokumentu.


**Niezarządzane tylko na dysku:** ponieważ konto magazynu Azure jest potencjalnych pojedynczy punkt awarii, jest należy dysponować co najmniej dwa konta magazynu platformy Azure, w których dystrybuowane są przynajmniej dwie maszyny wirtualne. W Instalatorze idealne dyski każdej maszyny wirtualnej, w którym jest uruchomione wystąpienie okna dialogowego SAP będzie można wdrożyć w innego konta magazynu.

> [!IMPORTANT]
>
> Zdecydowanie zaleca się użyć dysków zarządzanych Azure dla instalacji SAP HA, ponieważ automatycznie Dopasuj, z zestawu dostępności maszyny wirtualnej są dołączane do i z tego powodu zwiększenia dostępności maszyny wirtualnej i usługi uruchomione na maszynie wirtualnej.  
>


### <a name="high-availability-architecture-for-the-sap-ascs-instance"></a>Architektura wysokiej dostępności dla wystąpienia SCS SAP (A)

### <a name="high-availability-architecture-for-the-sap-ascs-instance-on-windows"></a>Architektura wysokiej dostępności dla wystąpienia SCS SAP (A) w systemie Windows


> ![Windows][Logo_Windows] Windows
>

Można użyć **Windows Server Failover Clustering** (**WSFC**) rozwiązanie, aby chronić wystąpienie SCS SAP (A). Istnieją dwa rodzaje rozwiązania:

* Klastrowanie przy użyciu wystąpienia SCS SAP (A) **udostępnionych dysków w klastrze**

   Więcej informacji na temat architektury wysokiej dostępności z klastra dysków można znaleźć w tym dokumencie: [wystąpienia SCS klastrowanie SAP (A) na Windows trybu Failover przy użyciu klastra udostępnionego dysku klastrowego][sap-high-availability-guide-wsfc-shared-disk].   

* Klastrowanie przy użyciu wystąpienia SCS SAP (A) **udziału plików**

  Więcej informacji na temat architektury wysokiej dostępności z udziału plików można znaleźć w tym dokumencie: [wystąpienia SCS klastrowanie SAP (A) w tryb Failover klastra przy użyciu udostępnianie plików systemu Windows][sap-high-availability-guide-wsfc-file-share].

### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Wysoka dostępność dla wystąpienia SCS SAP (A) w systemie Linux


> ![Linux][Logo_Linux] Linux
>

Można znaleźć więcej informacji na temat klastrów wystąpienie SCS SAP (A), używając strukturę SUSE Linux Enterprise Server klastra w tym dokumencie: [wysoką dostępność dla programu SAP NetWeaver na maszynach wirtualnych Azure w systemie SUSE Linux Enterprise Server aplikacji SAP][sap-suse-ascs-ha].

### <a name="sap-netweaver-multi-sid-configuration-for-clustered-sap-ascs-instance"></a>SAP NetWeaver identyfikatora SID wielu konfiguracji dla instancji SCS klastrowane SAP (A)

> ![Windows][Logo_Windows] Windows
>
>Obecnie Multi-identyfikator SID jest obsługiwana tylko z **Windows Server Failover Cluster (WSFC)**. Identyfikator SID Multi jest obsługiwana za pomocą **udziału plików** i **udostępnionego dysku**.
>

Więcej informacji na temat architektury HA Multi-SID można znaleźć w tych dokumentach architektura:

* [SCS SAP (A) wystąpienia identyfikatora SID Multi wysokiej dostępności dla z klastrem trybu Failover serwera systemu Windows i udziału plików][sap-ascs-ha-multi-sid-wsfc-file-share]

* [SCS SAP (A) wystąpienia identyfikatora SID Multi wysokiej dostępności dla z klastrem trybu Failover serwera systemu Windows i udostępniony dysk][sap-ascs-ha-multi-sid-wsfc-shared-disk]

### <a name="high-availability-dbms-instance"></a>Wystąpienie DBMS wysokiej dostępności

Systemu DBMS jest również pojedynczy punkt kontaktu w systemie SAP. Należy chronić go przy użyciu rozwiązania wysokiej dostępności. Poniższa ilustracja przedstawia rozwiązania wysokiej dostępności programu SQL Server zawsze na platformie Azure, Windows Server Failover Clustering i Azure wewnętrzne usługi równoważenia obciążenia. SQL Server AlwaysOn replikuje DBMS danych i plików dziennika przy użyciu własnego systemu DBMS replikacji. W takim przypadku nie trzeba udostępnionego dysku klastra, który upraszcza całą konfigurację.

![Rysunek 3: Przykład DBMS SAP wysokiej dostępności, z programu SQL Server AlwaysOn][sap-ha-guide-figure-2003]

_**Rysunek 3.** przykład DBMS SAP wysokiej dostępności, z programu SQL Server AlwaysOn_

Aby uzyskać więcej informacji o klastrach **bazami danych programu SQL Server** na platformie Azure przy użyciu modelu wdrażania usługi Azure Resource Manager, zobacz następujące artykuły:

* [Zawsze włączone grupy dostępności w maszynach wirtualnych platformy Azure ręcznie skonfigurować za pomocą Menedżera zasobów][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]

* [Konfigurowanie usługi Azure wewnętrznego modułu równoważenia obciążenia dla grupy dostępności AlwaysOn na platformie Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]

Aby uzyskać więcej informacji o klastrach **SAP HANA DBMS** na platformie Azure przy użyciu modelu wdrażania usługi Azure Resource Manager znajduje się w artykule:

* [Wysoka dostępność SAP HANA na maszynach wirtualnych platformy Azure (VM)][sap-hana-ha]
