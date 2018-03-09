---
title: "SAP HANA dostępność w regionach platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano omówienie zagadnienia dotyczące dostępności podczas uruchamiania SPA HANA na maszynach wirtualnych Azure"
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
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 052394884f85d527caa88ff6c9464af669f47bb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>SAP HANA dostępność w regionach platformy Azure
W tym artykule scenariuszach SAP HANA dostępności w różnych regionach platformy Azure są opisane i opisem. Biorąc pod uwagę fakt, że oddzielnych regionach platformy Azure mają większy odległość między nimi, istnieją specjalne uwagi, które są wymienione w tym artykule.

## <a name="motivation-to-deploy-across-multiple-azure-regions"></a>Motywacją do wdrożenia w wielu regionach platformy Azure
Różnych regionach platformy Azure są oddzielone w większej odległości. Zależne od regionu geograficznymi, może to być setki milach lub nawet kilka mil tysięcy, podobnie jak w Stanach Zjednoczonych. Z powodu odległość między różnych regionach platformy Azure ruchem sieciowym między zasoby wdrożone w dwóch różnych regionach platformy Azure środowiska sieci znaczących obie strony opóźnienia. Tyle duże, aby wykluczyć synchroniczna wymiana danych między dwoma wystąpieniami SAP HANA w typowych zadań SAP. Z drugiej strony można często muszą stawiać czoła fakt, że jest zdefiniowane wymagania na odległość między centrum danych podstawowego i dodatkowego centrum danych w celu zapewnienia dostępności w przypadku klęski żywiołowej naciśnięcie większych obszarów. Przykład Huragany, które obszar Karaibów i Florida aktywny września i października 2017 r. Lub co najmniej minimalną odległość wymaganiem. W większości przypadków klienta, definicja ta minimalną odległość wymaga można projektować pod kątem dostępności w [regiony platformy Azure](https://azure.microsoft.com/regions/). Ponieważ odległość jest zbyt duży między dwóch regionach platformy Azure do korzystania z trybu Replikacja synchroniczna HANA, wymagania dotyczące RTO i cel punktu odzyskiwania może wymusić na wdrażanie konfiguracji dostępności w ramach jednego regionu, a następnie uzupełnić o dodatkowe wdrożenia na sekundę region.

Innym aspektem wziąć pod uwagę w tych konfiguracjach jest tryb failover i przekierowania klienta. Zakłada się, że tryb failover między wystąpieniami SAP HANA w dwóch różnych regionach platformy Azure zawsze jest ręcznego przełączania trybu failover. Ponieważ tryb replikacji replikacji systemu SAP HANA ustawiono asynchroniczne, istnieje możliwość, że danych w podstawowej wystąpienia HANA nie wprowadził on jeszcze do dodatkowej wystąpienia HANA. W związku z tym automatycznej pracy awaryjnej nie może zostać zaakceptowana konfiguracji replikacji w przypadku asynchronicznego. Nawet w przypadku ręcznego kontrolowane trybu failover, tak jak wykonywania pracy awaryjnej należy podjąć środki, aby upewnić się, że wszystkie dane przekazane na głównej stronie wprowadzone go do dodatkowej wystąpienia przed przeniesieniem ręcznie za pośrednictwem w regionie Azure.
 
Ponieważ możesz pracować z różnych zakresów adresów IP w sieci wirtualnych platformy Azure, które są wdrażane w drugim region platformy Azure, klienci SAP HANA albo muszą zostać zmienione w konfiguracji lub temu lepsze trzeba umieścić czynności w celu zmiany rozpoznawania nazw. Dlatego że klienci są pobierania przekierowanie do nowego dodatkowej na adres IP serwera. Artykuł SAP na [odzyskiwania połączenia klienta po przejęciu](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html) przechodzi w stan więcej szczegółów.   

## <a name="simple-availability-between-two-azure-regions"></a>Proste dostępności między dwóch regionach platformy Azure
W tym scenariuszu decyzję o wprowadzone żadnej konfiguracji dostępności w jednym regionie. Jednak żądanie ma obciążeń obsługiwanych w przypadku awarii. Typowe przypadki dla systemów, takich jak będących systemów nieprodukcyjnych. Chociaż może wytrzymać, aby system w dół do połowy lub nawet na dzień, nie może dopuścić systemu nie może być dostępna przez co najmniej 48 godzin. Aby Instalatora mniej kosztowne, można uruchomić inny system, który jest nawet mniej ważne zakres przechowywania maszyny Wirtualnej, która działa jako docelowa lokalizacja lub rozmiaru maszyny Wirtualnej w regionie pomocniczym mniejsze i wybierz nie wstępnie załadować dane. Ponieważ przejście w tryb failover będzie ręczne, a pociąga za sobą wielu czynności więcej pracy awaryjnej na stosie kompletna aplikacja, dopuszczalny jest dodatkowy czas doprowadzić do maszyny Wirtualnej, rozmiar i ponownie uruchom maszynę Wirtualną.

> [!NOTE]
> Wstępne ładowanie danych w celu replikacji systemu HANA, nawet jeśli nie potrzebujesz co najmniej 64 GB pamięci RAM i poza za mało pamięci, aby zachować dane magazynu wierszy w pamięci obiektu docelowego.

![Dwie maszyny wirtualne za pośrednictwem dwóch regionach](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> W tej konfiguracji nie może dostarczyć RPO = 0, ponieważ tryb replikacji systemu HANA jest asynchroniczne. Jeśli musisz podać RPO = 0, ta konfiguracja nie jest konfiguracji wybór.

Niewielkie zmiany w konfiguracji można skonfigurować wstępnego ładowania danych. Jednak charakter ręcznej pracy awaryjnej i fakt, że trzeba przenieść do drugiego region również go może nie mieć sensu wstępne ładowanie danych warstwy aplikacji. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Łączenie dostępności w ramach jednego regionu w regionach 
Kombinacja dostępności w obrębie regionów oraz mogą być określane przez:

- Wymagania dotyczące RPO = 0 w obrębie regionu platformy Azure.
- Nie chce lub nie mogą mieć globalne operacje firmy będą miały wpływ przez głównych katastrofy fizyczne mające wpływ większych regionu. Jak miało to miejsce przez niektóre huragany, które trafień Karaibów w ciągu kilku ostatnich lat.
- Zapewniają przepisy dotyczące żądanie odległości od lokacji głównej i dodatkowej, która wyraźnie wykraczają poza jakiego stref dostępności Azure.

 
W takiej sytuacji należy skonfigurować jakie wywołania SAP [konfiguracji SAP HANA Multitier systemu replikacji](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) z replikacji systemu HANA. Architektura wyglądałyby tak jak:

![trzech maszyn wirtualnych za pośrednictwem dwóch regionach](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Ta konfiguracja zapewnia RPO = 0 z małych RTO razy w regionie podstawowym, a ponadto zapewnia elementem podrzędnym elementu cel punktu odzyskiwania w przypadku ruchu za pośrednictwem drugi regionu. Razy RTO w regionie drugi są zależne od tego, czy skonfigurować wstępnego ładowania danych lub nie. W wielu przypadkach klient maszyny Wirtualnej w regionie pomocniczym służy do uruchamiania systemu testowego. W wyniku tego użycia danych nie można wstępnie załadowane.

> [!NOTE]
> Ponieważ logreplay tryb działania jest używana do replikacji systemu HANA przechodzenia z warstwy 1 do warstwy 2 (Replikacja synchroniczna w regionie podstawowym), replikację między warstwy 2 i 3 warstwy (replikacji do lokacji dodatkowej) nie może być w operacji delta_datashipping tryb. Szczegóły trybów pracy i pewne ograniczenia są udokumentowane przez SAP w [tryby działania dla replikacji systemu SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html). 

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać wskazówki krok po kroku dotyczące sposobu konfigurowania takiej konfiguracji, na platformie Azure, zapoznaj się z artykułami:

- [Instalator programu SAP HANA System replikacji na maszynach wirtualnych platformy Azure](sap-hana-high-availability.md)
- [Twoje SAP na wysoką dostępność SAP HANA za pomocą replikacji systemu Azure — część 4 —](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

 



 
  
