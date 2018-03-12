---
title: "Architektura replikacji serwera VMware/fizyczne w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera omówienie składników i architektury używany podczas replikowania lokalnych maszyn wirtualnych VMware lub serwerach fizycznych systemu Windows i Linux do dodatkowej lokacji oprogramowania VMware z usługą Azure Site Recovery"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 97a990aa3ed9043280888900d8fc7b604b6c22b5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="vmware-vmphysical-server-to-vmware-replication-architecture"></a>Serwer VMware maszyny Wirtualnej/fizycznej do architektury replikacji VMware

W tym artykule opisano architektura i procesy stosowane podczas replikacji, pracy awaryjnej i odzyskać maszynach wirtualnych VMware lokalnymi (VM) lub serwerach fizycznych systemu Windows i Linux do dodatkowej lokacji VMware, za pomocą [usługi Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Składniki architektury

**Obszar** | **Składnik** | **Szczegóły**
--- | --- | ---
**Azure** | Ten scenariusz jest wdrażany przy użyciu programu InMage Scout. | Aby uzyskać program InMage Scout, konieczna jest subskrypcja platformy Azure.<br/><br/> Po utworzeniu magazynu usługi Recovery Services należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.
**Serwer przetwarzania** | Znajduje się w lokacji głównej | Serwer przetwarzania jest wdrażany w celu obsługi pamięci podręcznej, kompresji i optymalizacji danych.<br/><br/> Umożliwia on również obsługę instalacji wypychanej programu Unified Agent na maszynach, które chcesz chronić.
**Serwer konfiguracji** | Znajduje się w lokacji dodatkowej | Serwer konfiguracji umożliwia konfigurowanie i monitorowanie wdrożenia oraz zarządzanie nim za pomocą witryny sieci Web zarządzania lub konsoli vContinuum.
**Serwer vContinuum** | Opcjonalny. Instalowany w tej samej lokalizacji co serwer konfiguracji. | Zapewnia on konsolę do monitorowania chronionego środowiska i zarządzania nim.
**Główny serwer docelowy** | Znajduje się w lokacji dodatkowej | Na głównym serwerze docelowym przechowywane są zreplikowane dane. Odbiera on dane z serwera przetwarzania i tworzy maszynę repliki w lokacji dodatkowej, a także znajdują się na nim punkty przechowywania danych.<br/><br/> Liczba potrzebnych głównych serwerów docelowych zależy od liczby chronionych maszyn.<br/><br/> Aby powrócić po awarii do lokacji głównej, konieczne jest również posiadanie w tej sieci głównego serwera docelowego. Na tym serwerze jest instalowany program Unified Agent.
**Program VMware ESX/ESXi i serwer vCenter** |  Maszyny wirtualne są hostowane na hostach ESX/ESXi. Hosty są zarządzane za pomocą serwera vCenter | Aby móc replikować maszyny wirtualne VMware, konieczne jest posiadanie infrastruktury VMware.
**Maszyny wirtualne/serwery fizyczne** |  Program Unified Agent zainstalowany na przeznaczonych do replikowania maszynach wirtualnych programu VMware i serwerach fizycznych. | Agent działa jako dostawca komunikacji między wszystkimi składnikami.

### <a name="replication-process"></a>Proces replikacji

1. Następnie należy skonfigurować serwery składników w każdej lokacji (konfiguracja, przetwarzanie, główny docelowy) oraz zainstalować program Unified Agent na maszynach, które mają zostać zreplikowane.
2. Po początkowej replikacji agent na każdej maszynie wysyła zmiany replikacji przyrostowej na serwer przetwarzania.
3. Serwer przetwarzania optymalizuje dane i transferuje je na główny serwer docelowy w lokacji dodatkowej. Serwer konfiguracji zarządza procesem replikacji.

**Rysunek 6: Replikacja z programu VMware do programu VMware**

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Kolejne kroki

[Konfigurowanie](vmware-physical-secondary-disaster-recovery.md) odzyskiwania po awarii maszyn wirtualnych VMware i serwerów fizycznych do lokacji dodatkowej.
