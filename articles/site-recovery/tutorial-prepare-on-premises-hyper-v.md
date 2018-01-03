---
title: Przygotowanie serwera funkcji Hyper-V lokalnego odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V do platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak przygotować maszyn wirtualnych funkcji Hyper-V lokalnych nie są zarządzane przez program System Center VMM, odzyskiwania po awarii na platformie Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6e2837341e16f5077cfff18d4a34c097c165ef09
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Przygotowywanie serwerów funkcji Hyper-V lokalnych do odzyskiwania awaryjnego na platformie Azure

Ten samouczek pokazuje, jak przygotować o lokalnej infrastrukturze do funkcji Hyper-V umożliwia replikowanie maszyn wirtualnych funkcji Hyper-V do platformy Azure, na potrzeby odzyskiwania po awarii. Hosty funkcji Hyper-V mogą być zarządzane przez System Center Virtual Machine Manager (VMM), ale nie jest to wymagane.  Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przejrzyj wymagania dotyczące funkcji Hyper-V i wymagania dotyczące programu VMM, jeśli ma to zastosowanie.
> * Przygotowanie programu VMM, jeśli to konieczne
> * Sprawdź dostęp do Internetu dla lokalizacji platformy Azure
> * Przygotowywanie maszyn wirtualnych, dzięki czemu będą dostępne po przejściu w tryb failover Azure

Jest to drugi samouczek z tej serii. Upewnij się, że masz [Konfigurowanie składników Azure](tutorial-prepare-azure.md) zgodnie z opisem w poprzedniej samouczka.



## <a name="review-server-requirements"></a>Przejrzyj wymagania dotyczące serwera

Upewnij się, że hosty funkcji Hyper-V spełniają następujące wymagania. Jeśli zarządzasz hostami w chmurach programu System Center Virtual Machine Manager (VMM), sprawdź wymagania programu VMM.


**Składnik** | **Zarządzany przez Menedżera maszyny Wirtualnej funkcji Hyper-V** | **Funkcja Hyper-V bez programu VMM**
--- | --- | ---
**System operacyjny hosta funkcji Hyper-V** | Windows Server 2016, 2012 R2 | Nie dotyczy
**VMM** | PROGRAM VMM 2012, PROGRAM VMM 2012 R2 | Nie dotyczy


## <a name="review-hyper-v-vm-requirements"></a>Przejrzyj wymagania dotyczące maszyny Wirtualnej funkcji Hyper-V

Upewnij się, że maszyna wirtualna spełnia wymagania podsumowane w tabeli.

**Wymaganie maszyny Wirtualnej** | **Szczegóły**
--- | ---
**System operacyjny gościa** | Wszelkie system operacyjny gościa [obsługiwany przez platformę Azure](https://technet.microsoft.com/library/cc794868.aspx).
**Wymagania platformy Azure** | Lokalna Usługa maszyn wirtualnych funkcji Hyper-V muszą spełniać requirements(site-recovery-support-matrix-to-azure.md) maszyny Wirtualnej platformy Azure.

## <a name="prepare-vmm-optional"></a>Przygotowanie programu VMM (opcjonalnie)

Hosty funkcji Hyper-V są zarządzane przez program VMM, należy przygotować na lokalnym serwerze VMM. 

- Upewnij się, że serwer VMM ma co najmniej jedna chmura z co najmniej jedną grupę hostów. Host funkcji Hyper-V, na którym są uruchomione maszyny wirtualne powinien znajdować się w chmurze.
- Przygotuj serwer programu VMM do mapowania sieci.

### <a name="prepare-vmm-for-network-mapping"></a>Przygotowanie programu VMM do mapowania sieci

Jeśli używasz programu VMM, [mapowania sieci](site-recovery-network-mapping.md) mapowania między lokalnymi sieciami maszyny Wirtualnej VMM i sieci wirtualnych platformy Azure. Mapowanie gwarantuje, że maszynach wirtualnych platformy Azure są połączone siecią prawo podczas są tworzone po pracy awaryjnej.

Przygotowanie programu VMM do mapowania sieci w następujący sposób:

1. Upewnij się, że masz [sieć logiczna VMM](https://docs.microsoft.com/system-center/vmm/network-logical) która jest skojarzona z chmury, w którym znajdują się hosty funkcji Hyper-V.
2. Upewnij się, masz [sieci maszyny Wirtualnej](https://docs.microsoft.com/system-center/vmm/network-virtual) połączony z siecią logiczną.
3. W programie VMM należy połączyć z maszyn wirtualnych z siecią maszyny Wirtualnej.

## <a name="verify-internet-access"></a>Sprawdź dostęp do Internetu

1. Na potrzeby tego samouczka najprostsza konfiguracja jest dla hostów funkcji Hyper-V i serwera programu VMM, jeśli to konieczne, aby mieć bezpośredni dostęp do Internetu bez użycia serwera proxy. 
2. Upewnij się, że hosta funkcji Hyper-V i serwer programu VMM, jeśli jest to istotne, mogą uzyskiwać dostęp do tych adresów URL: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Upewnij się, że:
    - Wszystkie reguły zapory oparte na adresie IP powinna zezwalać na komunikację z platformą Azure.
    - Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.
    - Zezwalaj na zakresy adresów IP dla regionu Azure Twojej subskrypcji i zachodnie stany USA (używanych do zarządzania tożsamości i kontroli dostępu).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowanie do połączenia z maszynami wirtualnymi Azure po przejściu do trybu failover

W scenariuszu trybu failover można połączyć się z siecią zreplikowanych lokalnymi.

Aby połączyć się za pomocą protokołu RDP po pracy awaryjnej maszyn wirtualnych systemu Windows, wykonaj następujące czynności:

1. Aby uzyskać dostęp przez internet, należy włączyć protokół RDP na lokalnej maszynie Wirtualnej przed trybu failover. Upewnij się, że TCP i UDP reguły są dodawane do **publicznego** profilu oraz że RDP jest dozwolone w **zapory systemu Windows** > **dozwolone aplikacje** we wszystkich profilach.
2. Aby uzyskać dostęp za pośrednictwem połączenia VPN lokacja lokacja, należy włączyć RDP na maszynie lokalnej. RDP powinno być dozwolone w **zapory systemu Windows** -> **dozwolone aplikacje i funkcje** dla **domeny i prywatnej** sieci.
   Sprawdź, czy zasady sieci SAN systemu operacyjnego są ustawione na **OnlineAll**. [Dowiedz się więcej](https://support.microsoft.com/kb/3031135). Powinien istnieć nie oczekujące aktualizacje systemu Windows na maszynie Wirtualnej, gdy użytkownik zainicjuje tryb failover. Jeśli, nie będzie można logować się do maszyny wirtualnej do momentu ukończenia aktualizacji.
3. Na maszynie Wirtualnej Azure z systemem Windows po w tryb failover, sprawdź **diagnostyki rozruchu** Aby wyświetlić zrzut ekranu maszyny wirtualnej. Jeśli nie możesz połączyć, sprawdź, czy maszyna wirtualna działa i przejrzyj te [porady dotyczące rozwiązywania problemów](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych funkcji Hyper-V](tutorial-hyper-v-to-azure.md)
> [Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM](tutorial-hyper-v-vmm-to-azure.md)
