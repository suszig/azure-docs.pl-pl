---
title: "Wymagania wstępne dotyczące serwerów fizycznych do platformy Azure replikacji przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie wymagań wstępnych dotyczących replikacji obciążeń uruchomionych na serwerach fizycznych systemu Windows i Linux na platformie Azure, przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 5f5cb4b9b6fcee6b56ccdcb6223afddd9de90fec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-2-review-the-prerequisites-for-physical-server-to-azure-replication"></a>Krok 2: Sprawdź wymagania wstępne dotyczące serwerów fizycznych do platformy Azure replikacji

Przejrzyj wymagania wstępne podsumowane w tabeli.


**Wymagania wstępne** | **Szczegóły**
--- | ---
**Azure** | Dowiedz się więcej o [wymagania dotyczące usługi Azure](site-recovery-prereq.md#azure-requirements)
**Lokalny serwer konfiguracji** | Potrzebujesz serwera fizycznego (lub maszyny Wirtualnej VMware) systemem Windows Server 2012 R2 lub nowszym. Serwer jest skonfigurowany podczas wdrażania usługi Site Recovery.<br/><br/> Domyślnie serwer przetwarzania i główny serwer docelowy są również instalowane na tym komputerze. Skalowanie w górę, może być konieczne oddzielnego serwera przetwarzania. Jeśli to zrobisz, ma te same wymagania co serwer konfiguracji.<br/><br/> [Dowiedz się więcej](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Lokalne maszyny wirtualne** | Chcesz replikować maszyny powinny działać pod kontrolą [obsługiwanym systemie operacyjnym](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) i być zgodne z [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL** | Serwer konfiguracji musi mieć dostęp do tych adresów URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Jeśli masz reguły zapory oparte na adresie IP, zadbaj o to, aby reguły zezwalały na komunikację z platformą Azure.<br/></br> Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.<br/></br> Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA (służy do kontrolowania dostępu i zarządzania tożsamościami).<br/><br/> Zezwalaj na ten adres URL do pobrania MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Usługa mobilności** | Zainstalowane na każdym serwerze zreplikowane.




## <a name="limitations"></a>Ograniczenia

Należy pamiętać o ograniczeniach podsumowane w tabeli przed wdrożeniem.

**Ograniczenia** | **Szczegóły**
--- | ---
**Azure** | Konta magazynu i sieci muszą być w tym samym regionie co magazyn<br/><br/> Jeśli używasz konta magazynu w warstwie premium, należy również konta standardowe magazynu do przechowywania dzienników replikacji<br/><br/> Nie są replikowane do konta premium w środkowej i Południowa, Indie.
**Lokalny serwer konfiguracji** | Jeśli używasz maszyny Wirtualnej VMware jako z maszyną serwera konfiguracji typ karty maszyny Wirtualnej VMware powinien być VMXNET3. Jeśli nie, [Zainstaluj tę aktualizację](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> Dla maszyny Wirtualnej VMware vSphere PowerCLI 6.0 powinien być zainstalowany.<br/><br> Komputer nie powinien być kontrolerem domeny.<br/><br/> Komputer ma statyczny adres IP.<br/><br/> Nazwa hosta powinna być 15 znaków lub mniej, a system operacyjny będzie w języku angielskim.
**Replikowanie maszyn** | Sprawdź [ograniczenia maszyny Wirtualnej Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nie można replikować maszyny wirtualne z zaszyfrowanych dysków lub maszyn wirtualnych z rozruchem UEFI/EFI.<br/><br> Udostępniony dysk, który klastrów nie są obsługiwane. Jeśli źródło maszyny Wirtualnej ma wiele kart, zostanie przekonwertowane na jedną kartę Sieciową po pracy awaryjnej.<br/><br/> Jeśli maszyny wirtualne mają dysku iSCSI, Site Recovery po pracy awaryjnej konwertuje ją na plik VHD. Jeśli obiekt docelowy iSCSI może być osiągnięta poprzez maszyny Wirtualnej Azure, nawiązuje z nim połączenie, a widzi ona i wirtualnego dysku twardego. W takim przypadku można odłączyć obiektu docelowego iSCSI.<br/><br/> Jeśli chcesz włączyć spójność wielu maszyn wirtualnych, dzięki czemu maszyny wirtualne obsługujące te same obciążenia mają zostać odzyskane razem z punktem spójność danych, należy otworzyć port 20004 na maszynie Wirtualnej.<br/><br/> Na dysku C, musi zostać zainstalowany system Windows. Dysk systemu operacyjnego należy podstawowych i nie dynamicznych. Dysk danych może być dynamiczny.<br/><br/> Plik/etc/hosts systemu Linux na maszynach wirtualnych powinien zawierać wpisów, które mapują nazwę hosta lokalnego na adresy IP skojarzone z wszystkich kart sieciowych. Nazwa hosta, punkty instalacji, nazwa urządzenia, systemu ścieżki i nazwy pliku (/ etc; usr) powinny być tylko w języku angielskim.<br/><br/> Określone typy [magazynu Linux](site-recovery-support-matrix-to-azure.md#support-for-storage) są obsługiwane.<br/><br/>Utwórz lub ustawić **disk.enableUUID=true** w ustawieniach maszyny Wirtualnej. Zapewnia to spójny UUID do pliku VMDK, tak, aby poprawnie instaluje i zapewnia, że tylko zmiany różnicowe są przekazywane do lokalnego podczas powrotu po awarii bez pełnej replikacji.


## <a name="next-steps"></a>Następne kroki

- Jeśli pełne wdrożenie robimy, przejdź do [krok 3: Planowanie pojemności](physical-walkthrough-capacity.md)
- Jeśli podczas wykonywania testu proste wdrożenie, przejdź do [krok 4: Planowanie sieci](physical-walkthrough-network.md).
