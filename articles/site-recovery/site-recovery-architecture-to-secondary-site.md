---
title: "Jak działa replikacja maszyn lokalnych do dodatkowej lokacji lokalnej w usłudze Azure Site Recovery? | Microsoft Docs"
description: "Ten artykuł zawiera omówienie składników i architektury używanych podczas replikowania lokalnych maszyn wirtualnych i serwerów fizycznych do lokacji dodatkowej za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: fca95c63964b955db7ddfbe53250702cc8af122e
ms.contentlocale: pl-pl
ms.lasthandoff: 06/15/2017

---
# <a name="how-does-on-premises-machine-replication-to-a-secondary-site-work-in-site-recovery"></a>Jak działa replikacja maszyn lokalnych do lokacji dodatkowej w usłudze Site Recovery?

Ten artykuł zawiera opis składników i procesów związanych z replikacją lokalnych maszyn wirtualnych i serwerów fizycznych do platformy Azure za pomocą usługi [Azure Site Recovery](site-recovery-overview.md).

Oto co możesz replikować do dodatkowej lokacji lokalnej:
- Lokalne maszyny wirtualne funkcji Hyper-V i maszyny wirtualne funkcji Hyper-V na hostach autonomicznych i w klastrach funkcji Hyper-V zarządzanych w chmurach programu System Center Virtual Machine Manager (VMM).
- Lokalne maszyny wirtualne VMware oraz serwery fizyczne z systemami Windows i Linux. W tym scenariuszu replikacja jest zarządzana przez program Scout.

Zamieść wszelkie komentarze pod tym artykułem lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="replicate-hyper-v-vms-to-a-secondary-on-premises-site"></a>Replikacja maszyn wirtualnych funkcji Hyper-V do dodatkowej lokacji lokalnej


### <a name="architectural-components"></a>Składniki architektury

Poniżej przedstawiono wymagania dotyczące replikowania maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej.

**Składnik** | **Lokalizacja** | **Szczegóły**
--- | --- | ---
**Azure** | Będziesz potrzebować konta w usługach firmy Microsoft. |
**Serwer VMM** | Zalecamy, aby zarówno w lokacji głównej, jak i dodatkowej znajdował się jeden serwer VMM | Oba serwery VMM powinny być połączone z Internetem.<br/><br/> Każdy serwer powinien mieć co najmniej jedną chmurę prywatną programu VMM z ustawionym profilem możliwości funkcji Hyper-V.<br/><br/> Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM. Dostawca koordynuje i organizuje replikację za pomocą usługi Site Recovery przez Internet. Komunikacja między dostawcą i platformą Azure jest bezpieczna i szyfrowana.
**Serwer funkcji Hyper-V** |  Co najmniej jeden serwer hosta funkcji Hyper-V w głównych i dodatkowych chmurach programu VMM.<br/><br/> Serwery powinny być połączone z Internetem.<br/><br/> Dane są replikowane między głównymi i dodatkowymi serwerami hosta funkcji Hyper-V za pośrednictwem sieci LAN albo sieci VPN korzystającej z protokołu Kerberos lub uwierzytelniania certyfikatu.  
**Maszyny wirtualne funkcji Hyper-V** | Znajduje się na źródłowym serwerze hosta funkcji Hyper-V. | Źródłowy serwer hosta powinien mieć co najmniej jedną maszynę wirtualną, która ma być replikowana.

### <a name="replication-process"></a>Proces replikacji

1. Należy skonfigurować konto platformy Azure.
2. Należy utworzyć magazyn usługi Replication Services na potrzeby usługi Site Recovery i skonfigurować ustawienia magazynu, w tym następujące elementy:

    - Źródło i cel replikacji (lokacja główna i dodatkowa).
    - Instalacja dostawcy usługi Azure Site Recovery i agenta usługi Microsoft Azure Recovery Services. Dostawca jest instalowany na serwerach VMM, a agent na każdym hoście funkcji Hyper-V.
    - Należy utworzyć zasady replikacji dla źródłowej chmury programu VMM. Te zasady są stosowane do wszystkich maszyn wirtualnych znajdujących się na hostach w chmurze.
    - Należy włączyć replikację maszyn wirtualnych funkcji Hyper-V. Początkowa replikacja jest wykonywana zgodnie z ustawieniami zasad replikacji.
4. Zmiany danych są śledzone i po zakończeniu początkowej replikacji rozpoczyna się replikowanie zmian różnicowych. Śledzone zmiany elementu są przechowywane w pliku hrl.
5. Należy uruchomić testowanie trybu failover, aby upewnić się, że wszystkie funkcje działają.

**Rysunek 1: Replikacja z programu VMM do programu VMM**

![Ze środowiska lokalnego do środowiska lokalnego](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

1. Planowane lub nieplanowane przejście w tryb [failover](site-recovery-failover.md) można uruchomić między lokacjami lokalnymi. Jeśli zostanie uruchomione planowane przejście w tryb failover, źródłowe maszyny wirtualne zostaną wyłączone w celu zapewnienia, że nie będzie miała miejsca utrata danych.
2. W tryb failover można przełączyć pojedynczą maszynę lub można utworzyć [plany odzyskiwania](site-recovery-create-recovery-plans.md), aby zarządzać trybem failover na wielu maszynach.
4. Jeśli wykonywane jest nieplanowane przejście w tryb failover do lokacji dodatkowej, po przejściu w tryb failover na maszynach znajdujących się w lokalizacji dodatkowej nie jest włączona ochrona ani replikacja. Jeśli dokonano planowanego przejścia w tryb failover, po przejściu w tryb failover maszyny znajdujące się w lokalizacji dodatkowej są chronione.
5. Następnie należy zatwierdzić tryb failover, aby można było rozpocząć uzyskiwanie dostępu do obciążenia z poziomu repliki maszyny wirtualnej.
6. Gdy lokacja główna będzie znowu dostępna, należy zainicjować replikację odwrotną w celu przeprowadzenia replikacji z lokacji dodatkowej do lokacji głównej. Replikacja odwrotna powoduje przełączenie maszyn wirtualnych do stanu chronionego, ale aktywną lokalizacją jest nadal dodatkowe centrum danych.
7. Aby lokacja główna stała się z powrotem lokalizacją aktywną, należy zainicjować planowane przejście w tryb failover z lokacji dodatkowej do głównej, a następnie przeprowadzić kolejną replikację odwrotną.




## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Replikacja maszyn wirtualnych VMware/serwerów fizycznych do lokacji dodatkowej

Serwery fizyczne lub maszyny wirtualne VMware są replikowane do lokacji dodatkowej przy użyciu programu InMage Scout za pomocą następujących składników infrastruktury:


### <a name="architectural-components"></a>Składniki architektury

**Składnik** | **Lokalizacja** | **Szczegóły**
--- | --- | ---
**Azure** | InMage Scout. | Aby uzyskać program InMage Scout, konieczna jest subskrypcja platformy Azure.<br/><br/> Po utworzeniu magazynu usługi Recovery Services należy pobrać program InMage Scout i zainstalować najnowsze aktualizacje w celu skonfigurowania wdrożenia.
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

**Rysunek 2: Replikacja z programu VMware do programu VMware**

![Z programu VMware do programu VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [macierzą obsługi](site-recovery-support-matrix-to-sec-site.md)

