---
title: Plan sieci dla VMware do platformy Azure replikacji | Dokumentacja firmy Microsoft
description: W tym artykule opisano planowanie wymagane sieci podczas replikowania maszyn wirtualnych VMware do platformy Azure
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5578a0b4-0352-41c3-9cce-56beb17a4d97
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f164ac68ba6ec650bb3996b4aa870e1b98533a23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-4-plan-networking-for-vmware-to-azure-replication"></a>Krok 4: Planowanie sieci dla VMware do platformy Azure replikacji

W tym artykule przedstawiono zagadnienia związane z planowaniem podczas replikowania lokalnych maszyn wirtualnych VMware do platformy Azure przy użyciu sieci [usługi Azure Site Recovery](site-recovery-overview.md) usługi.

Wszelkie komentarze umieszczaj pod tym artykułem lub zadawaj pytania na [Forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="connect-to-replica-vms"></a>Połącz się z repliką maszyny wirtualne

Podczas planowania strategii trybu failover i replikacji, na których, jedno z pytań klucza jest sposób nawiązywania połączenia z maszyną wirtualną Azure po pracy awaryjnej. Istnieje kilka opcji do wyboru podczas projektowania strategii sieci dla repliki maszyny wirtualne platformy Azure:

- **Użyj innego adresu IP**: można określić, aby użyć różnych zakresów adresów IP dla sieci maszyny Wirtualnej Azure zreplikowane. W tym scenariuszu maszyny Wirtualnej pobiera nowego adresu IP po w tryb failover i aktualizowania DNS jest wymagany.
- **Zachowaj tego samego adresu IP**: może chcesz użyć tego samego zakresu adresów IP jak w witrynie głównej lokalnymi dla sieci platformy Azure po pracy awaryjnej. Utrzymywanie tego samego adresu IP adresów upraszcza odzyskiwanie dzięki zmniejszeniu problemów dotyczących sieci po pracy awaryjnej. Jednak jeśli przeprowadzasz replikację do platformy Azure, należy zaktualizować tras z nowej lokalizacji adresy IP po pracy awaryjnej. 


## <a name="retain-ip-addresses"></a>Zachowaj adresów IP

Usługa Site Recovery zapewnia adresy możliwości, aby zachować stałego adresu IP podczas przechodzenie w tryb failover na platformie Azure, z trybu failover podsieci.

Tryb failover podsieci określonej podsieci jest obecny w lokacji 1 lub 2 lokacji, ale nigdy nie w obu lokacjach jednocześnie. Aby zachować przestrzeń adresów IP w przypadku trybu failover, można programowo Rozmieść infrastruktury router przenieść podsieci z jednej lokacji. Podczas pracy w trybie failover podsieci są przenoszone wraz z skojarzone chronionych maszyn wirtualnych. Główną wadą jest, że w przypadku awarii, należy przenieść całej podsieci.


### <a name="failover-example"></a>Przykład trybu failover

Oto przykład dla trybu failover na platformie Azure.

- Firmy ficticious, banku Woodgrove ma infrastruktury lokalnej hosting aplikacji swoich biznesowych. Ich aplikacji dla urządzeń przenośnych znajdują się na platformie Azure.
- Łączność między maszynami wirtualnymi banku Woodgrove na serwerach Azure i lokalnymi są udostępniane przez połączenie lokacja lokacja (VPN) między siecią lokalną krawędzi i sieci wirtualnej platformy Azure.
- Tej sieci VPN oznacza, że sieci wirtualnej firmy w usłudze Azure jest wyświetlany jako rozszerzenie sieci lokalnej.
- Woodgrove chce Użyj usługi Site Recovery, aby replikować obciążenia lokalnego do platformy Azure.
 - Woodgrove ma radzenia sobie z aplikacje i konfiguracje, które są zależne od stałe adresy IP, a w związku z tym konieczne zachowanie adresów IP dla swoich aplikacji po w tryb failover na platformie Azure.
 - Woodgrove ma przypisanych adresów IP z zakresu 172.16.1.0/24, 172.16.2.0/24 do jej zasobów, które działają na platformie Azure.


Dla Woodgrove można było do replikowania jego maszyn wirtualnych na platformie Azure przy zachowaniu adresy IP, tutaj firmy, co firma musi wykonać:

1. Tworzenie sieci wirtualnej platformy Azure. Rozszerzenie sieci lokalnej, należy tak, aby aplikacje działają w trybie Failover bezproblemowo.
2. Azure umożliwia dodanie połączenie sieci VPN typu lokacja lokacja, oprócz połączenie punkt lokacja sieci wirtualnych utworzonych na platformie Azure.
3. Podczas konfigurowania połączenia lokacja lokacja, w sieci platformy Azure może kierować ruchem do lokalizacji lokalnego (sieci lokalne) tylko wtedy, gdy zakres adresów IP jest inny niż zakres adresów lokalnych.
    - Jest to spowodowane Azure nie obsługuje rozciągnięty podsieci. Jeśli masz podsieci 192.168.1.0/24 lokalnymi, nie można dodać 192.168.1.0/24 sieci lokalnej w sieci platformy Azure.
    - Oczekiwany jest Azure nie może ustalić, czy brak aktywnych maszyn wirtualnych w podsieci i że podsieć jest tworzony tylko odzyskiwania po awarii.
    - Aby można było poprawnie kierować ruchem sieciowym spoza sieci platformy Azure podsieci w sieci i sieci lokalnej nie może powodować konflikt.

![Przed podsieci trybu failover](./media/site-recovery-network-design/network-design7.png)

### <a name="before-failover"></a>Przed trybu failover

1. Utwórz dodatkowe sieci (na przykład odzyskiwania). To jest sieć w którego przejścia w tryb failover maszyny wirtualne są tworzone.
2. Aby upewnić się, że adres IP dla maszyny Wirtualnej jest zachowywany po przejściu w tryb failover, we właściwościach maszyny Wirtualnej > **Konfiguruj**, należy określić ten sam adres IP, że maszyna wirtualna ma lokalnego i kliknij **zapisać**.
3. Gdy maszyna wirtualna przeszła w tryb failover, usługi Azure Site Recovery przypisze podanego adresu IP do niego.

    ![Właściwości sieci](./media/site-recovery-network-design/network-design8.png)

4. Po wyzwoleniu wyzwalacza i maszyn wirtualnych są tworzone na platformie Azure przy użyciu wymaganych adresu IP trybu failover możesz nawiązać połączenie przy użyciu sieci [sieci wirtualnej do sieci wirtualnej połączenia](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Ta akcja umożliwia pisanie skryptów.
5. Trasy należy odpowiednio można zmodyfikować, aby odzwierciedlał tego 192.168.1.0/24 teraz zostały przeniesione do platformy Azure.

    ![Po podsieci w tryb failover](./media/site-recovery-network-design/network-design9.png)

### <a name="after-failover"></a>Po pracy awaryjnej

Jeśli nie ma sieci platformy Azure, jak pokazano powyżej, można utworzyć połączenie VPN lokacja lokacja między lokacją główną a Azure, po pracy awaryjnej.

## <a name="change-ip-addresses"></a>Zmiana adresów IP

To [wpis w blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wyjaśniono, jak skonfigurować infrastrukturę sieci Azure, gdy nie trzeba zachować adresy IP po pracy awaryjnej. Go rozpoczyna się od opisu aplikacji wygląda jak skonfigurować sieci lokalnej na platformie Azure i zawiera informacje o uruchamianiu przechodzenia w tryb failover.  

## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 5: przygotowanie Azure](vmware-walkthrough-prepare-azure.md)
