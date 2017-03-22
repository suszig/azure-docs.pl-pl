---
title: "Najlepsze rozwiązania dotyczące usługi Azure Site Recovery | Microsoft Docs"
description: "W tym artykule opisano najlepsze rozwiązania dotyczące wdrażania usługi Azure Site Recovery"
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a53e2a09fb2aabe96dd5347ea7509815d92bfdb8
ms.lasthandoff: 03/15/2017

---

# <a name="get-ready-to-deploy-azure-site-recovery"></a>Przygotowanie do wdrażania usługi Azure Site Recovery

W tym artykule opisano sposób przygotowania do wdrażania usługi Azure Site Recovery.

## <a name="protecting-hyper-v-virtual-machines"></a>Ochrona maszyn wirtualnych funkcji Hyper-V

Aby chronić maszyny wirtualne funkcji Hyper-V, masz kilka opcji wdrażania do wyboru. Możesz replikować lokalne maszyny wirtualne funkcji Hyper-V do platformy Azure lub dodatkowego centrum danych. Każde wdrożenie ma inne wymagania.

**Wymaganie** | **Replikacja do platformy Azure (z programem VMM)** | **Replikacja maszyn wirtualnych funkcji Hyper-V do platformy Azure (bez programu VMM)** | **Replikacja maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej (z programem VMM)** | **Szczegóły**
---|---|---|---|---
**VMM** | Program VMM działający w programie System Center 2012 R2 <br/><br/>Co najmniej jedna chmura programu VMM, która zawiera co najmniej jedną grupę hostów programu VMM. | Nie dotyczy | Serwery programu VMM w lokacjach głównych i dodatkowych działające co najmniej w programie System Center 2012 z dodatkiem SP1 z najnowszymi aktualizacjami. <br/><br/> Co najmniej jedna chmura na każdym serwerze programu VMM. Dla chmur należy ustawić profil możliwości funkcji Hyper-V.<br/><br/> Dla chmury źródłowej należy ustawić co najmniej jedną grupę hostów programu VMM. | Opcjonalny. Do replikowania maszyn wirtualnych funkcji Hyper-V do platformy Azure nie trzeba mieć wdrożonego programu System Center VMM, ale jeśli go masz, upewnij się, że serwer programu VMM został skonfigurowany prawidłowo. Obejmuje to sprawdzenie, czy uruchomiono właściwą wersję programu VMM i czy skonfigurowano chmury.
**Funkcja Hyper-V** | Co najmniej jeden serwer hosta funkcji Hyper-V w lokacji lokalnej z systemem Windows Server 2012 R2 lub nowszym | Co najmniej jeden serwer funkcji Hyper-V w lokacji źródłowej i docelowej z co najmniej systemem Windows Server 2012 z zainstalowanymi najnowszymi aktualizacjami i połączeniem z Internetem.<br/><br/> Serwery funkcji Hyper-V muszą należeć do grupy hostów w chmurze programu VMM. | Co najmniej jeden serwer funkcji Hyper-V w lokacji źródłowej i docelowej z co najmniej systemem Windows Server 2012 z zainstalowanymi najnowszymi aktualizacjami i połączeniem z Internetem.<br/><br/> Serwery funkcji Hyper-V muszą znajdować się w grupie hostów w chmurze programu VMM. |
**Maszyny wirtualne** | Co najmniej jedna maszyna wirtualna na źródłowym serwerze hosta funkcji Hyper-V | Co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V w źródłowej chmurze programu VMM | Co najmniej jedna maszyna wirtualna na serwerze hosta funkcji Hyper-V w źródłowej chmurze programu VMM. |  Replikacja maszyn wirtualnych do platformy Azure musi przebiegać zgodnie z warunkami wstępnymi dotyczącymi maszyn wirtualnych platformy Azure
**Konto platformy Azure** | Musisz mieć konto platformy Azure i subskrypcję usługi Site Recovery. | Musisz mieć konto platformy Azure i subskrypcję usługi Site Recovery. | Nie dotyczy | Jeśli nie masz konta, rozpocznij pracę z bezpłatną wersją próbną.
**Magazyn platformy Azure** | Musisz mieć subskrypcję konta magazynu platformy Azure z włączoną replikacją geograficzną. | Musisz mieć subskrypcję konta magazynu platformy Azure z włączoną replikacją geograficzną. | Nie dotyczy | Konto powinno znajdować się w tym samym regionie co magazyn usługi Azure Site Recovery i musi być skojarzone z tą samą subskrypcją.
**Sieć** | Skonfiguruj mapowanie sieci, aby upewnić się, że wszystkie maszyny, które przechodzą w tryb failover w tej samej sieci platformy Azure, mogą się ze sobą łączyć niezależnie od tego, jakiego planu odzyskiwania używają. Ponadto jeśli brama sieci została skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne mogą łączyć się z innymi lokalnymi maszynami wirtualnymi. Jeśli nie skonfigurowano mapowania sieci, mogą się łączyć tylko maszyny, które przechodzą w tryb failover w ramach tego samego planu odzyskiwania. | Nie dotyczy |  <br/><br/>Skonfiguruj mapowanie sieci, aby upewnić się, że maszyny wirtualne zostały podłączone do odpowiednich sieci po przejściu w tryb failover oraz że maszyny wirtualne repliki zostały optymalnie rozmieszczone na serwerach hostów funkcji Hyper-V. Jeśli nie skonfigurowano mapowania sieci, replikowane maszyny nie będą się łączyć z żadną siecią maszyn wirtualnych po przejściu w tryb failover. |  Aby skonfigurować mapowanie sieci w programie VMM, musisz upewnić się, że sieci logiczne i maszyn wirtualnych programu VMM zostały skonfigurowane prawidłowo.
**Dostawcy i agenci** | Podczas wdrożenia na serwerach programu VMM zostanie zainstalowany dostawca usługi Azure Site Recovery. Na serwerach funkcji Hyper-V w chmurach programu VMM zostanie zainstalowany agent usług Azure Recovery Services. | Podczas wdrażania w klastrze lub na serwerze hosta funkcji Hyper-V zostanie zainstalowany dostawca usługi Azure Site Recovery oraz agent usług Azure Recovery Services.| Podczas wdrożenia na serwerach programu VMM zostanie zainstalowany dostawca usługi Azure Site Recovery. Na serwerach funkcji Hyper-V w chmurach programu VMM zostanie zainstalowany agent usług Azure Recovery Services. | Dostawcy i agenci łączą się z usługą Site Recovery przez Internet przy użyciu szyfrowanego połączenia HTTPS. Nie musisz dodawać wyjątków zapory ani tworzyć serwera proxy przeznaczonego dla wyjątku dostawcy.
**Łączność z Internetem** | Tylko serwery programu VMM muszą mieć połączenie z Internetem | Tylko serwery hostów funkcji  Hyper-V muszą mieć połączenie z Internetem | Tylko serwery programu VMM muszą mieć połączenie z Internetem | Nie trzeba instalować niczego na maszynach wirtualnych ani łączyć ich z Internetem.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Ochrona serwerów fizycznych lub maszyn wirtualnych VMware

W przypadku obejmowania ochroną maszyn wirtualnych VMware lub serwerów fizycznych z systemem Windows/Linux. Możesz replikować je do platformy Azure lub dodatkowego centrum danych. Każde wdrożenie ma inne wymagania.

**Wymaganie** | **Replikacja maszyn wirtualnych VMware/serwerów fizycznych do platformy Azure** | * **Replikacja maszyn wirtualnych VMware/serwerów fizycznych do lokacji dodatkowej**  
---|---|---
**Lokacja główna** | **Serwer przetwarzania**: dedykowany serwer systemu Windows (fizyczny lub wirtualny) | **Serwer przetwarzania**: dedykowany serwer systemu Windows (fizyczny lub maszyny wirtualnej VMware)<br/><br/>  
**Dodatkowa lokacja lokalna** | Nie dotyczy | **Serwer konfiguracji**: dedykowany serwer systemu Windows (fizyczny lub wirtualny) <br/><br/> **Główny serwer docelowy**: dedykowany serwer systemu Windows (fizyczny lub wirtualny) Skonfiguruj przy użyciu systemu Windows, aby chronić maszyny wirtualne z systemem Windows, lub przy użyciu systemu Linux, aby chronić maszyny wirtualne z systemem Linux.
**Azure** | **Subskrypcja**: musisz mieć subskrypcję usługi Site Recovery. <br/><br/> **Konto magazynu**: musisz mieć konto magazynu z włączoną replikacją geograficzną. Konto powinno znajdować się w tym samym regionie co magazyn usługi Site Recovery i musi być skojarzone z tą samą subskrypcją. <br/><br/> **Serwer konfiguracji**: musisz skonfigurować serwer konfiguracji jako maszynę wirtualną platformy Azure <br/><br/> **Główny serwer docelowy**: musisz skonfigurować główny serwer docelowy jako maszynę wirtualną platformy Azure <br/><br/> Skonfiguruj przy użyciu systemu Windows, aby chronić maszyny wirtualne z systemem Windows, lub przy użyciu systemu Linux, aby chronić maszyny wirtualne z systemem Linux.<br/><br/> **Sieć wirtualna platformy Azure**: musisz mieć sieć wirtualną platformy Azure, w której zostanie zainstalowany serwer konfiguracji i główny serwer docelowy. Powinna ona znajdować się w tej samej subskrypcji i regionie co magazyn usługi Azure Site Recovery | Nie dotyczy  
**Maszyny wirtualne/serwery fizyczne** | Co najmniej jedna maszyna wirtualna VMware lub serwer fizyczny z systemem Windows/Linux.<br/><br/>Podczas wdrażania usługa Mobility zostanie zainstalowana na każdej maszynie| Co najmniej jedna maszyna wirtualna VMware lub serwer fizyczny z systemem Windows/Linux.<br/><br/> Podczas wdrażania program Unified Agent zostanie zainstalowany na każdej maszynie




## <a name="azure-virtual-machine-requirements"></a>Wymagania maszyny wirtualnej platformy Azure

Możesz wdrożyć usługę Site Recovery, aby replikować maszyny wirtualne i serwery fizyczne z dowolnym systemem operacyjnym obsługiwanym przez platformę Azure. Jest to większość wersji systemów Windows i Linux. Musisz upewnić się, że lokalne maszyny wirtualne do objęcia ochroną działają zgodnie z wymaganiami platformy Azure.


## <a name="optimizing-your-deployment"></a>Optymalizacja wdrożenia

Poniższe porady będą pomocne w przypadku optymalizacji i skalowania wdrożenia.

- **Rozmiar woluminu systemu operacyjnego**: w przypadku replikacji maszyny wirtualnej do platformy Azure wolumin systemu operacyjnego musi być mniejszy niż 1 TB. Jeśli masz więcej woluminów, możesz przenieść je ręcznie na inny dysk przed rozpoczęciem wdrażania.
- **Rozmiar dysku danych**: w przypadku replikowania do platformy Azure możesz mieć maksymalnie 32 dyski danych na maszynie wirtualnej, każdy o maksymalnym rozmiarze wynoszącym 1 TB. Maszynę wirtualną o rozmiarze około&32; TB można skutecznie replikować i przenosić w tryb failover.
- **Limity planu odzyskiwania**: usługa Site Recovery może skalować do tysięcy maszyn wirtualnych. Plany odzyskiwania są projektowane jako model dla aplikacji, które powinny razem przechodzić w tryb failover, dlatego ograniczamy liczbę maszyn w planie odzyskiwania do 50.
- **Limity usługi platformy Azure**: każda subskrypcja platformy Azure udostępnia zestaw domyślnych limitów dotyczących rdzeni, usług w chmurze itd. Zalecamy uruchomienie testu pracy w trybie failover w celu zweryfikowania dostępności zasobów w ramach subskrypcji. Aby zmodyfikować te limity, należy skontaktować się z działem pomocy technicznej platformy Azure.
- **Planowanie pojemności**: planowanie wydajności i skanowania.
- **Przepustowość replikacji**: w przypadku niewielkiej przepustowości replikacji pamiętaj, że:
    - **ExpressRoute**: usługa Site Recovery współpracuje z optymalizatorami usług Azure ExpressRoute i WAN, takimi jak Riverbed.
    - **Ruch związany z replikacją**: usługa Site Recovery wykonuje inteligentną replikację początkową tylko przy użyciu tylko bloków danych, a nie całego dysku VHD. Podczas trwającej replikacji są replikowane tylko zmiany.
    - **Ruch sieciowy**: możesz kontrolować ruch sieciowy używany podczas replikacji, konfigurując jakość usług QoS systemu Windows za pomocą zasad opartych na docelowym adresie IP i porcie.  Dodatkowo jeśli replikujesz do usługi Azure Site Recovery przy użyciu agenta usługi Azure Backup. Możesz skonfigurować ograniczanie przepływności dla tego agenta.
- **Cel czasu odzyskiwania**: jeśli chcesz mierzyć cel czasu odzyskiwania (RTO) oczekiwany w przypadku korzystania z usługi Site Recovery, proponujemy uruchomienie testu pracy w trybie failover i przejrzenie zadań usługi Site Recovery w celu przeanalizowania czasu wymaganego do ukończenia operacji. W przypadku przechodzenia w tryb failover na platformie Azure zalecamy, aby w celu uzyskania najlepszej wartości RTO zautomatyzować wszystkie akcje wykonywane ręcznie, integrując usługę Azure Automation i plany odzyskiwania.
- **Cel punktu odzyskiwania**: usługa Site Recovery obsługuje prawie synchroniczny cel punktu odzyskiwania (RPO) podczas replikacji do platformy Azure. W takiej sytuacji zakładamy, że przepustowość między centrum danych i platformą Azure jest wystarczająca.

