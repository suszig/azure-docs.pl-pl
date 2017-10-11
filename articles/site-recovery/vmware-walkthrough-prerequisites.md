---
title: "Wymagania wstępne dotyczące VMware do platformy Azure replikacji przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie wymagań wstępnych dotyczących replikacji obciążeń uruchomionych na maszynach wirtualnych VMware do platformy Azure, przy użyciu usługi Azure Site Recovery."
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
ms.openlocfilehash: 7a82e58d9ff9208130c43fcd11d03dcc3238696a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-review-the-prerequisites-for-vmware-to-azure-replication"></a>Krok 2: Sprawdź wymagania wstępne dotyczące VMware do platformy Azure replikacji

Przeczytaj te warunki wstępne podsumowane w poniższej tabeli.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Azure** | Dowiedz się więcej o [wymagania dotyczące usługi Azure](site-recovery-prereq.md#azure-requirements)
**Lokalny serwer konfiguracji** | Należy maszyny Wirtualnej VMware systemem Windows Server 2012 R2 lub nowszym. Serwer jest skonfigurowany podczas wdrażania usługi Site Recovery.<br/><br/> Domyślnie serwer przetwarzania i główny serwer docelowy są również instalowane na tej maszynie Wirtualnej. Skalowanie w górę, może być konieczne oddzielnego serwera przetwarzania i ma te same wymagania co serwer konfiguracji.<br/><br/> Dowiedz się więcej o tych składników [tutaj](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Serwery lokalne, VMware** | Co najmniej jeden VMware vSphere serwery, 6.5, 6.0, 5.5, 5.1 z uruchomionym najnowsze aktualizacje. Serwery powinny znajdować się w tej samej sieci co serwer konfiguracji (lub oddzielnego serwera przetwarzania).<br/><br/> Firma Microsoft zaleca serwera vCenter do zarządzania hostami program uruchomiony 6.5, 6.0 lub 5.5 z najnowszymi aktualizacjami.
**Lokalne maszyny wirtualne** | Maszyny wirtualne, które mają być replikowane, powinny mieć [obsługiwane systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) i spełniać [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Maszyna wirtualna powinna mieć uruchamiania narzędzi VMware.
**Adresy URL** | Serwer konfiguracji musi mieć dostęp do tych adresów URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Jeśli masz reguły zapory oparte na adresie IP, zadbaj o to, aby reguły zezwalały na komunikację z platformą Azure.<br/></br> Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.<br/></br> Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA (służy do kontrolowania dostępu i zarządzania tożsamościami).<br/><br/> Zezwalaj na ten adres URL do pobrania MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Usługa mobilności** | Zainstalowany na każdym zreplikowanej maszyny Wirtualnej.




## <a name="limitations"></a>Ograniczenia

Upewnij się, że rozumiesz ograniczenia podsumowane w tabeli przed wdrożeniem.

**Ograniczenia** | **Szczegóły**
--- | ---
**Azure** | Konta magazynu i sieci muszą być w tym samym regionie co magazyn<br/><br/> Jeśli używasz konta magazynu w warstwie premium, należy również konta standardowe magazynu do przechowywania dzienników replikacji<br/><br/> Nie są replikowane do konta premium w środkowej i Południowa, Indie.
**Lokalny serwer konfiguracji** | Typ karty maszyny Wirtualnej VMware powinna być VMXNET3. Jeśli nie, [Zainstaluj tę aktualizację](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> należy zainstalować vSphere PowerCLI 6.0.<br/><br> Komputer nie powinien być kontrolerem domeny. Komputer ma statyczny adres IP.<br/><br/> Nazwa hosta powinna być 15 znaków lub mniej, a system operacyjny będzie w języku angielskim.
**VMware** | Usługa Site Recovery nie obsługuje nowe vCenter i vSphere 6.5 i 6.0 funkcje takie jak cross vCenter vMotion, woluminy i magazynu usługi rejestracji urządzeń.
**Maszyny wirtualne** | Sprawdź [ograniczenia maszyny Wirtualnej Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nie można replikować maszyny wirtualne z zaszyfrowanych dysków lub maszyn wirtualnych z rozruchem UEFI/EFI.<br/><br> Udostępniony dysk, który klastrów nie są obsługiwane. Jeśli źródło maszyny Wirtualnej ma wiele kart, zostanie przekonwertowane na jedną kartę Sieciową po pracy awaryjnej.<br/><br/> Jeśli maszyny wirtualne mają dysku iSCSI, Site Recovery po pracy awaryjnej konwertuje ją na plik VHD. Jeśli obiekt docelowy iSCSI może być osiągnięta poprzez maszyny Wirtualnej Azure, nawiązuje z nim połączenie, a widzi ona i wirtualnego dysku twardego. W takim przypadku można odłączyć obiektu docelowego iSCSI.<br/><br/> Jeśli chcesz włączyć spójność wielu maszyn wirtualnych, dzięki czemu maszyny wirtualne obsługujące te same obciążenia mają zostać odzyskane razem z punktem spójność danych, należy otworzyć port 20004 na maszynie Wirtualnej.<br/><br/> Na dysku C, musi zostać zainstalowany system Windows. Dysk systemu operacyjnego należy podstawowych i nie dynamicznych. Dysk danych może być dynamiczny.<br/><br/> Plik/etc/hosts systemu Linux na maszynach wirtualnych powinien zawierać wpisów, które mapują nazwę hosta lokalnego na adresy IP skojarzone z wszystkich kart sieciowych. Nazwa hosta, punkty instalacji, nazwa urządzenia, systemu ścieżki i nazwy pliku (/ etc; usr) powinny być tylko w języku angielskim.<br/><br/> Określone typy [magazynu Linux](site-recovery-support-matrix-to-azure.md#support-for-storage) są obsługiwane.<br/><br/>Utwórz lub ustawić **disk.enableUUID=true** w ustawieniach maszyny Wirtualnej. Zapewnia to spójny UUID do pliku VMDK, tak, aby poprawnie instaluje i zapewnia, że tylko zmiany różnicowe są przekazywane do lokalnego podczas powrotu po awarii bez pełnej replikacji.


## <a name="next-steps"></a>Następne kroki

- Jeśli pełne wdrożenie robimy, przejdź do [krok 3: Planowanie pojemności](vmware-walkthrough-capacity.md)
- Jeśli podczas wykonywania testu proste wdrożenie, przejdź do [krok 4: Planowanie sieci](vmware-walkthrough-network.md).
