---
title: "O mapowania sieci dla replikacji maszyny Wirtualnej funkcji Hyper-V za pomocą usługi Site Recovery | Dokumentacja firmy Microsoft"
description: "Ustaw mapowanie sieci dla funkcji Hyper-V replikację maszyny wirtualnej z lokalnego centrum danych do platformy Azure lub lokacji dodatkowej."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: d56f8f5bfb40c1c43090f43e119bf9b98918d6e5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="about-network-mapping-for-hyper-v-vm-replication"></a>Mapowanie sieci do replikacji maszyny Wirtualnej funkcji Hyper-V — informacje


Ten artykuł pomaga zrozumieć i Planowanie sieci mapowania podczas replikacji maszyn wirtualnych funkcji Hyper-V do platformy Azure lub lokacji dodatkowej, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md).

Po zapoznaniu się z tym artykule post wszelkie komentarze u dołu w tym artykule i zadawaj pytania techniczne na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>Mapowanie sieci dla replikacji do platformy Azure

Mapowanie sieci jest używany podczas replikowania maszyn wirtualnych funkcji Hyper-V (zarządzane w programie VMM) na platformie Azure. Sieci mapy mapowanie między sieciami maszyn wirtualnych na źródłowym serwerze programu VMM i docelowymi sieciami platformy Azure. Mapowanie wykonuje następujące czynności:

- **Połączenie sieciowe**— zapewnia czy replikowane maszyny wirtualne Azure są podłączone do mapowanej sieci. Wszystkie komputery, które w tryb failover w tej samej sieci może się ze sobą łączyć, nawet jeśli ich przejścia w tryb failover w planie odzyskiwania inny.
- **Brama sieci**— Jeśli brama sieci jest skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne podłączyć do innych maszyn wirtualnych lokalnie.

Należy pamiętać, że:

- Źródłowej sieci maszyny Wirtualnej VMM są mapowane na sieć wirtualną platformy Azure.
- Po przejściu w tryb failover maszyn wirtualnych platformy Azure w lokalizacji źródłowej sieci zostaną podłączone do sieci wirtualnej zamapowanego docelowego.
- Nowe maszyny wirtualne dodawane do źródłowej sieci maszyny Wirtualnej są podłączone do mapowanej sieci platformy Azure, podczas replikacji.
- Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę, jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej jest łączona z tą docelową podsiecią po przejściu do trybu failover.
- Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna jest łączona z pierwszą podsiecią w sieci.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Mapowanie sieci dla replikacji do dodatkowego centrum danych

Mapowanie sieci jest używany podczas replikowania maszyn wirtualnych funkcji Hyper-V (zarządzane w System Center Virtual Machine Manager (VMM)) do dodatkowego centrum danych. Mapowanie sieci działa między sieciami maszyn wirtualnych na źródłowym serwerze programu VMM i sieci maszyn wirtualnych na docelowym serwerze VMM. Mapowanie wykonuje następujące czynności:

- **Połączenie sieciowe**— łączy maszyn wirtualnych do odpowiedniej sieci po pracy awaryjnej. Maszyna wirtualna repliki zostaną podłączone do sieci docelowej, który jest zamapowany na Sieć źródłowa.
- **Oferującą**— optymalnie umieszcza repliki maszyny wirtualne na serwerach hostów funkcji Hyper-V. Maszyny wirtualne repliki są umieszczane na hostach, które mogą uzyskiwać dostęp do mapowanej sieci maszyny Wirtualnej.
- **Brak mapowania sieci**— Jeśli nie skonfigurujesz mapowania sieci, repliki maszyn wirtualnych nie będzie podłączona do sieci maszyn wirtualnych po pracy awaryjnej.

Należy pamiętać, że:

- Mapowanie sieci można skonfigurować między sieciami maszyn wirtualnych na dwóch serwerach VMM lub na jednym serwerze programu VMM, gdy dwie lokacje są zarządzane przez ten sam serwer.
- Podczas mapowania jest poprawnie skonfigurowany i replikacja jest włączona, w lokalizacji głównej maszyny Wirtualnej zostaną podłączone do sieci i jej replika w lokalizacji docelowej zostaną podłączone do mapowanej sieci.
-
- Jeśli sieci są skonfigurowane poprawnie w programie VMM po wybraniu opcji sieć maszyny Wirtualnej docelową podczas mapowania sieci, chmur programu VMM źródła, które używają źródłowej sieci maszyny Wirtualnej zostanie wyświetlony, wraz z dostępnych sieci maszyn wirtualnych w chmurach docelowych, które są używane do ochrony.
- Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę jak podsieć, na którym znajduje się źródłowa maszyna wirtualna, następnie repliki maszyny wirtualnej zostanie podłączona do tej docelowej podsieci po pracy awaryjnej. Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna zostanie podłączona do pierwszej podsieci w sieci.



### <a name="example"></a>Przykład

Oto przykład ilustrujący ten mechanizm. Spójrzmy organizacji z dwóch lokalizacji w Nowym Jorku i Chicago.

**Lokalizacja** | **Serwer VMM** | **Sieci maszyn wirtualnych** | **Mapowane na**
---|---|---|---
Nowy Jork | VMM-NewYork| VMNetwork1-NewYork | Mapowany do VMNetwork1 Chicago
 |  | VMNetwork2-NewYork | Nie zostały zamapowane
Chicago | VMM-Chicago| VMNetwork1-Chicago | Mapowany do NowyJork VMNetwork1
 | | VMNetwork1-Chicago | Nie zostały zamapowane

W tym przykładzie:

- Po utworzeniu maszyny wirtualnej repliki dla żadnej maszyny wirtualnej, która jest połączona z VMNetwork1 NowyJork zostaną podłączone do VMNetwork1 Chicago.
- Po utworzeniu maszyny wirtualnej repliki dla NowyJork VMNetwork2 lub VMNetwork2 Chicago nie zostanie połączona z żadną siecią.

Oto, jak chmur programu VMM są zainstalowane w naszym przykładzie organizacji i sieci logiczne skojarzone z chmury.

#### <a name="cloud-protection-settings"></a>Ustawienia ochrony chmury

**Chronionej chmurze** | **Ochrona chmury** | **Sieć logiczna (Nowy Jork)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>Nie dotyczy</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>Nie dotyczy</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Ustawienia sieci logicznych, jak i maszyny Wirtualnej

**Lokalizacja** | **Sieć logiczna** | **Skojarzone sieci maszyny Wirtualnej**
---|---|---
Nowy Jork | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

#### <a name="target-network-settings"></a>Ustawienia sieci docelowej

Na podstawie tych ustawień, gdy wybrana sieć docelowa maszyna wirtualna, w poniższej tabeli przedstawiono opcje, które będą dostępne.

**Wybierz** | **Chronionej chmurze** | **Ochrona chmury** | **Sieć docelowa jest dostępna**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Dostępna
 | GoldCloud1 | GoldCloud2 | Dostępna
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Niedostępne
 | GoldCloud1 | GoldCloud2 | Dostępna


Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę jak podsieć, na którym znajduje się źródłowa maszyna wirtualna, następnie repliki maszyny wirtualnej zostanie podłączona do tej docelowej podsieci po pracy awaryjnej. Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna zostanie podłączona do pierwszej podsieci w sieci.


#### <a name="failback-behavior"></a>Zachowanie w przypadku powrotu po awarii

Aby zobaczyć, co się stanie w przypadku powrotu po awarii (replikacja odwrotna), załóżmy, że czy NowyJork VMNetwork1 jest zamapowana do VMNetwork1 Chicago, z następującymi ustawieniami.


**Maszyny wirtualne** | **Połączone z siecią maszyny Wirtualnej**
---|---
Maszyna wirtualna 1 | VMNetwork1 sieci
Maszyny VM2 (repliki VM1) | VMNetwork1-Chicago

Przy użyciu tych ustawień umożliwia przeglądanie, co dzieje się w kilku możliwych scenariuszach.

**Scenariusz** | **Wynik**
---|---
Brak zmian właściwości sieci maszyny Wirtualnej 2 po pracy awaryjnej. | 1 maszyna wirtualna pozostaje połączony z siecią źródła.
Właściwości sieci maszyny Wirtualnej 2 są zmieniane po pracy awaryjnej i jest odłączony. | 1 maszyna wirtualna jest odłączony.
Właściwości sieci maszyny Wirtualnej 2 są zmieniane po pracy awaryjnej i jest połączona z VMNetwork2 Chicago. | Jeśli nie jest zamapowana VMNetwork2 Chicago, 1 maszyna wirtualna zostanie odłączony.
Mapowanie sieci VMNetwork1 Chicago zostanie zmieniona. | 1 maszyna wirtualna zostanie podłączona do sieci, w obecnie mapowane do VMNetwork1 Chicago.



## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej o [planowania infrastruktury sieciowej](site-recovery-network-design.md).
