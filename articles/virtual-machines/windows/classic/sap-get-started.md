---
title: "W przypadku maszyn wirtualnych z systemem Windows przy użyciu programu SAP | Dokumentacja firmy Microsoft"
description: "Wyczyść o korzystaniu z programu SAP na maszynach wirtualnych systemu Windows (VM) w systemie Microsoft Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 1d6326d5f79ea4e975abd1aa90b0d4a635f4c31a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>W przypadku maszyn wirtualnych systemu Windows na platformie Azure przy użyciu SAP
Chmura obliczeniowa jest szeroko używanych terminem, który zyskuje coraz większe znaczenie w branży IT, od małych firm po duże i międzynarodowe korporacje. Microsoft Azure to platforma usług w chmurze firmy Microsoft oferująca szerokie spektrum nowych możliwości. Obecnie użytkownicy mogą szybko aprowizować zasoby jako usługi w chmurze i anulować ich aprowizację, dzięki czemu nie muszą przejmować się ograniczeniami technicznymi czy budżetowymi. Zamiast inwestować czas i budżet w infrastrukturę sprzętową firmy mogą skoncentrować się na aplikacjach, procesie biznesowym oraz korzyściach dla swoich klientów i użytkowników.

Maszynom wirtualnym Microsoft Azure firma Microsoft oferuje kompleksowe infrastruktura jako usługa (IaaS) platformy. Aplikacje oparte na oprogramowaniu SAP NetWeaver są obsługiwane w usłudze Azure Virtual Machines (IaaS). Oficjalne dokumenty poniżej opisano, jak zaplanować i wdrożyć aplikacje SAP NetWeaver oparte na maszynach wirtualnych systemu Windows na platformie Azure. Można też wdrożyć aplikacje SAP NetWeaver oparte na [maszyn wirtualnych systemu Linux](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver na platformie Azure - HA
Nazwa: SAP NetWeaver na platformie Azure - SAP ASCS/SCS wystąpień klastrowania przy użyciu klastra trybu Failover systemu Windows Server na platformie Azure za pomocą SIOS DataKeeper

Podsumowanie: "w tym dokumencie opisano ustawienia konfiguracji o wysokiej dostępności SAP ASCS/SCS na platformie Azure przy użyciu SIOS DataKeeper. SAP chroni ich pojedynczego punktu awarii składników, takich jak SAP ASCS/SCS lub umieścić w kolejce replikacji usługi z konfiguracji klastra trybu Failover systemu Windows Server, które wymagają udostępnione dyski. Te składniki SAP są istotne dla funkcji systemu SAP. W związku z tym funkcje wysokiej dostępności musi znajdować się w miejscu, aby upewnić się, że te składniki mogła obsłużyć awarię serwera lub maszyny Wirtualnej, jak odbywa się za pomocą konfiguracji klastra systemu Windows bez systemu operacyjnego i w środowiskach funkcji Hyper-V. Począwszy od sierpnia 2015 Azure na sama nie udostępnia udostępnione dyski, które będą wymagane dla systemu Windows na podstawie konfiguracji wysokiej dostępności, wymagane dla tych krytycznych składników SAP. Jednak za pomocą produktu DataKeeper przez SIOS konfiguracje klastra pracy awaryjnej systemu Windows Server, w razie potrzeby dla SAP ASCS/SCS mogą być tworzone na platformie Azure IaaS. W tym dokumencie opisano podejście krok do kroku instalowania konfiguracji klastra trybu Failover systemu Windows Server z udostępnionego dysku dostarczonych przez Datakeeper SIOS na platformie Azure. Papieru objaśnia szczegółów w konfiguracji po stronie Azure, Windows i SAP, która działa w sposób optymalny konfigurację wysokiej dostępności. Papieru uzupełnia SAP instalacji dokumentacji i uwagi SAP, reprezentujące głównej zasobów dla instalacji i wdrożenia oprogramowania SAP na podany platform.

Zaktualizowano: Sierpień 2015

[Pobierz teraz ten przewodnik](http://go.microsoft.com/fwlink/?LinkId=613056)

