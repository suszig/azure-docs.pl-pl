---
title: "SAP HANA dostępności na maszynach wirtualnych Azure — omówienie | Dokumentacja firmy Microsoft"
description: Operacje SAP HANA na maszynach wirtualnych Azure natywnego
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b22f89750234a4715d2b7fd2df6ad8740b1d085
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-high-availability-guide-for-azure-virtual-machines"></a>Przewodnik SAP HANA wysokiej dostępności dla maszyn wirtualnych platformy Azure
Platforma Azure oferuje wiele funkcji, które umożliwia wdrażanie misji krytyczne baz danych, takie jak SAP HANA w maszynach wirtualnych platformy Azure. Ten dokument zawiera wskazówki dotyczące sposobu uzyskania dostępności SAP HANA wystąpień, które są obsługiwane w maszynach wirtualnych platformy Azure. W tym dokumencie opisano kilka scenariuszy, które można zaimplementować w infrastrukturę platformy Azure, aby zwiększyć dostępność SAP HANA na platformie Azure. 

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku założono, że czytelnik zna takie infrastruktura jako usługa (IaaS) podstawowe informacje o na platformie Azure jako: 

- Jak wdrożyć maszyny wirtualne lub sieci wirtualnych za pośrednictwem portalu Azure lub programu PowerShell.
- Azure i platform interfejsu wiersza polecenia (CLI), w tym możliwość użycia szablonów JavaScript Object Notation (JSON).

W tym przewodniku założono również, że czytelnik zna instalowanie wystąpieniami SAP HANA i administrowania i działających wystąpień SAP HANA. Szczególnie ustawiania i operacje wokół HANA System replikacji lub zadań, takich jak Kopia zapasowa i przywracanie bazy danych SAP HANA.

Inne artykuły, które zapewniają omówienie na tematy SAP HANA na platformie Azure są:

- [Instalacja ręczna pojedynczym wystąpieniem SAP HANA na maszynach wirtualnych Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Instalator programu SAP HANA System replikacji na maszynach wirtualnych platformy Azure](sap-hana-high-availability.md)
- [Podręcznik tworzenia kopii zapasowej programu SAP HANA na maszynach wirtualnych platformy Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Artykuł i zawartości, należy się zapoznać o SAP HANA może być wymieniona takich jak:

- [Wysoka dostępność dla programu SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [Często zadawane pytania: Wysoka dostępność dla programu SAP HANA](https://archive.sap.com/documents/docs/DOC-66702)
- [Sposób przeprowadzenia replikacji systemu SAP HANA](https://archive.sap.com/documents/docs/DOC-47702)
- [SAP HANA 2.0 SPS 01 nowości: wysokiej dostępności](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [Zalecenia dotyczące sieci dla replikacji systemu SAP HANA](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [Replikacji systemu SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [SAP HANA usługi automatycznego ponownego uruchamiania](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [Konfigurowanie replikacji systemu SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)


Poza stanowi zapoznać się z wdrożeniem maszyn wirtualnych na platformie Azure, zalecamy również przeczytaniu artykułu [Zarządzaj dostępnością maszyn wirtualnych systemu Windows na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) przed kontynuowaniem definiowanie architektury dostępności na platformie Azure.

## <a name="service-level-agreements-for-different-azure-components"></a>Umowy dotyczące poziomu usług dla różnych składników platformy Azure
Platforma Azure ma inną dostępności umowy SLA dla różnych składników, takich jak sieci, magazynu i maszyn wirtualnych. Wszystkie te SLA są udokumentowane i można go znaleźć, począwszy od [umową dotyczącą poziomu usług Microsoft Azure](https://azure.microsoft.com/support/legal/sla/) strony. Po wyewidencjonowaniu [umowy SLA dla maszyn wirtualnych](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), należy pamiętać, że platforma Azure oferuje dwa różne SLA z dwóch różnych konfiguracji. SLA zdefiniowano takich jak:

- Używając jednej maszyny Wirtualnej [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) dla systemu operacyjnego, dysku i wszystkich dysków danych udostępnia miesięczne procent czasu z wartością 99,9%
- Wiele (co najmniej dwa) maszyn wirtualnych, które są zorganizowane w [zestawu dostępności Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) Podaj miesięczne procent 99,95% czasu

Miara wymagań dostępności względem składników umowy SLA platformy Azure może udostępniać i zdecydujesz się na różnych scenariuszy, należy wdrożyć SAP HANA w celu uzyskania dostępności wymagają zapewnienie.

## <a name="next-steps"></a>Kolejne kroki
Nadal dokumentów:

- [SAP HANA dostępności w ramach jednego regionu Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region)
- [SAP HANA dostępność w regionach platformy Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 















  


