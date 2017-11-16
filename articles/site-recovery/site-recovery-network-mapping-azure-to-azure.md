---
title: "Mapowanie sieci między dwóch regionach platformy Azure w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Usługa Azure Site Recovery koordynuje replikacji, trybu failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych. Więcej informacji na temat trybu failover do platformy Azure lub dodatkowego centrum danych."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 1df7e04fc9cec5722747bafce7e2e5519b76315d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Mapowanie sieci między dwóch regionach platformy Azure


W tym artykule opisano sposób mapowania sieci wirtualnych platformy Azure z dwóch regionach platformy Azure ze sobą. Mapowanie sieci zapewnia, że po utworzeniu maszyny wirtualnej zreplikowanej w docelowym regionie Azure jest tworzona na sieć wirtualną, która jest mapowana na sieć wirtualną dla źródłowej maszyny wirtualnej.  

## <a name="prerequisites"></a>Wymagania wstępne
Przed zamapowaniem sieci upewnij się, utworzono [sieci wirtualnych platformy Azure](../virtual-network/virtual-networks-overview.md) zarówno źródłowych i docelowych regiony platformy Azure.

## <a name="map-networks"></a>Mapowanie sieci

Do innej sieci wirtualnej w innym regionie mapowania sieci wirtualnej platformy Azure, w jednym regionie Azure, przejdź do infrastruktura usługi Site Recovery -> Mapowanie sieci (maszyn wirtualnych platformy Azure) i utworzyć mapowania sieci.

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


W poniższym przykładzie Moja maszyna wirtualna jest uruchomiona w regionie Azja Wschodnia i jest replikowana na Azja południowo-wschodnia.

Wybierz sieć źródłowa i docelowa, a następnie kliknij przycisk OK, aby utworzyć mapowanie sieci z Azja Wschodnia, Azja południowo-wschodnia.

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Tak samo postąpić, aby utworzyć mapowanie sieci z Azja południowo-wschodnia, Azja Wschodnia.  
![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Mapowanie sieci, podczas włączania replikacji

Jeśli mapowanie sieci nie odbywa się podczas replikowania maszyny wirtualnej po raz pierwszy z jednego regionu Azure, a następnie wybierz pozycję sieci docelowej w ramach tego samego procesu. Usługa Site Recovery tworzy mapowania sieci z regionu źródła na region docelowy i region docelowy do regionu źródła na podstawie tego zaznaczenia.   

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Domyślnie usługa Site Recovery tworzy sieć w region docelowy, który jest identyczny z siecią źródła i dodając "— Funkcja automatycznego odzyskiwania systemu" jako sufiks nazwy źródłowej sieci. Możesz wybrać utworzone sieci, klikając przycisk Dostosuj.

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Jeśli mapowanie sieci zostało już przeprowadzone, nie można zmienić wirtualnej sieci docelowej podczas włączania replikacji. Aby to zmienić, zmodyfikuj istniejące mapowanie sieci.  

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Mapowanie sieci](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Jeśli zmodyfikujesz mapowanie sieci regionu-1 do 2 regionu, upewnij się, że zmodyfikować mapowania sieci z regionu 2 region-1, jak również.
>
>


## <a name="subnet-selection"></a>Wybór podsieci
Podsieci docelowej maszyny wirtualnej jest zaznaczone, na podstawie nazwy podsieci źródłowej maszyny wirtualnej. Jeśli jest dostępny w sieci docelowej podsieci o takiej samej nazwie jak źródłowej maszyny wirtualnej, następnie który jest wybrany dla docelowej maszyny wirtualnej. Jeśli nie ma żadnych podsieci o takiej samej nazwie w sieci docelowej alfabetycznie pierwszej podsieci jest wybrany jako podsieci docelowej. Można zmodyfikować tej podsieci, przechodząc do obliczenia i sieć ustawienia maszyny wirtualnej.

![Modyfikowanie podsieci](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>Adres IP

Adres IP dla każdego interfejsu sieci docelowej maszyny wirtualnej jest wybierany w następujący sposób:

### <a name="dhcp"></a>DHCP
Jeśli interfejsu sieciowego dla źródłowej maszyny wirtualnej jest używany serwer DHCP, interfejsu sieci docelowej maszyny wirtualnej jest także ustawić jako DHCP.

### <a name="static-ip"></a>Statyczny adres IP
Jeśli interfejs sieciowy źródłowej maszyny wirtualnej używa statycznego adresu IP, interfejsu sieci docelowej maszyny wirtualnej jest także ustawić do użycia statycznego adresu IP. Statyczny adres IP jest wybierany w następujący sposób:

#### <a name="same-address-space"></a>Tą samą przestrzenią adresów

Jeśli podsieć źródłowej i docelowej podsieci z tą samą przestrzenią adresów, docelowy adres IP jest ustawić taki sam jak adres IP interfejsu sieciowego dla źródłowej maszyny wirtualnej. Jeśli nie ma tego samego adresu IP, dostępne IP jest ustawiony jako docelowy adres IP.

#### <a name="different-address-space"></a>Przestrzeń adresowa różnych

Jeśli na podsieci źródłowej i docelowej podsieci ma inny adres miejsca, docelowy adres IP jest ustawiony jako dostępny adres IP w podsieci docelowej.

Docelowy adres IP dla każdego interfejsu sieciowego można zmodyfikować, przechodząc do ustawień obliczania i sieci maszyny wirtualnej.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure networking](site-recovery-azure-to-azure-networking-guidance.md).
