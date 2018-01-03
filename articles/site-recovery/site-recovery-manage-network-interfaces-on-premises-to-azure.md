---
title: "Zarządzanie interfejsów sieciowych w usłudze Azure Site Recovery dla lokalnego do platformy Azure scenariuszy | Dokumentacja firmy Microsoft"
description: "Opisuje sposób zarządzania interfejsów sieciowych do środowiska lokalnego do scenariuszy Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: manayar
ms.openlocfilehash: b60c746ae6c243d2b448056f8768df3baaed4cc1
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/23/2017
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Interfejsy sieciowe maszyny wirtualnej na potrzeby lokalnego do platformy Azure scenariuszy zarządzania

Maszynę wirtualną na platformie Azure, musi mieć co najmniej jeden interfejs sieciowy do niego dołączony i może mieć dowolną liczbę interfejsów sieciowych do niego dołączony jako obsługuje rozmiar maszyny Wirtualnej. Domyślnie pierwszy interfejs sieciowy dołączony do maszyny wirtualnej platformy Azure jest zdefiniowany jako podstawowy interfejs sieciowy. Wszystkie inne interfejsy sieciowe w maszynie wirtualnej są dodatkowych interfejsów sieciowych. Domyślnie ruch wychodzący z maszyny wirtualnej jest wysłane z adresu IP przypisanego do podstawowej konfiguracji IP interfejsu sieci podstawowej.

W środowisku lokalnym maszyny wirtualne/serwery może mieć wiele interfejsów sieciowych do różnych sieci w środowisku. Różnych sieciach są zwykle używane do wykonywania określonej operacji, takich jak uaktualnień, obsługi, dostęp do Internetu,... itd. Podczas migracji lub przechodzenie w tryb failover na platformie Azure ze środowiska lokalnego, należy pamiętać, że interfejsy sieciowe w tej samej maszyny wirtualnej musi połączone z tej samej sieci wirtualnej.

Domyślnie usługa Site Recovery tworzy jako wiele sieci interfejsy na maszynie wirtualnej platformy Azure, jak są podłączone do serwera lokalnego. Można uniknąć, tworzenie interfejsów sieciowych nadmiarowe podczas migracji lub pracy awaryjnej, edytując ustawienia interfejsu sieciowego w obszarze Ustawienia dla zreplikowanej maszyny wirtualnej.

## <a name="select-the-target-network"></a>Wybierz sieć docelowa

VMware i maszyn fizycznych i maszyn wirtualnych funkcji Hyper-V (bez VMM) można określić docelowych sieci wirtualnej dla poszczególnych maszyn wirtualnych. W przypadku maszyn wirtualnych funkcji Hyper-V zarządzanych za pomocą programu VMM [mapowania sieci](site-recovery-network-mapping.md) służy do mapowania sieci maszyny Wirtualnej na źródłowym serwerze programu VMM i docelowymi sieciami platformy Azure.

1. W obszarze "Zreplikowane elementy" w magazynie usług odzyskiwania kliknij dowolny element replikowane, aby uzyskać dostęp do ustawień dla tego elementu replikowane.

2. Kliknij kartę "Obliczeń i sieci", aby uzyskać dostęp do ustawień sieci dla replikowanych elementu.

3. W obszarze właściwości sieci wybierz sieć wirtualną z listy dostępnych interfejsów sieciowych.

    ![Obliczenia i sieć](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Modyfikowanie Sieć docelowa ma wpływ na wszystkie interfejsy sieciowe dla określonej maszyny wirtualnej.

Dla chmur programu VMM modyfikowanie mapowania sieci ma wpływ na wszystkie maszyny wirtualne i ich interfejsów sieciowych.

## <a name="select-the-target-interface-type"></a>Wybierz docelowy typ interfejsu

W sekcji "Interfejsów sieciowych" w bloku "Obliczeń i sieci" można wyświetlić i edytować ustawienia interfejsu sieciowego i określić typ interfejsu sieci docelowej.

- A **głównej** interfejsu sieciowego jest wymagany w trybie failover.
- Interfejsy innych wybranej sieci, jeśli istnieje, **dodatkowej** interfejsów sieciowych.
- Wybierz **nie używaj** wykluczyć interfejsu sieciowego z tworzenia w trybie failover.

Domyślnie podczas włączania replikacji, Usługa Site Recovery wybiera wszystkie wykryte interfejsów sieciowych na serwerze lokalnym oznaczenie jako "Podstawowy", a pozostałe jako "Dodatkowej" interfejsów sieciowych. Interfejsami kolejnych dodane na lokalnym serwerze oznaczonych jako "Nie należy używać" domyślnie. Dodając kilka interfejsów sieciowych, upewnij się, czy wybrano rozmiar docelowy poprawne maszyny wirtualnej platformy Azure, aby uwzględnić wszystkie interfejsy sieciowe wymagane.

## <a name="modifying-network-interface-settings"></a>Modyfikacja ustawienia interfejsu sieciowego

Można zmodyfikować podsieci i adres IP dla interfejsów sieciowych zreplikowanych elementów. Jeśli nie określono adresu IP, usługi Site Recovery przypisać dalej dostępny adres IP z podsieci do interfejsu sieciowego w trybie failover.

1. Kliknij w dowolnym interfejsie dostępnej sieci, aby otworzyć blok ustawień interfejsu sieciowego.

2. Wybierz odpowiednią podsieć na liście dostępnych podsieci.

3. Wprowadź żądane IP (zgodnie z wymaganiami).

    ![Ustawienia interfejsu sieciowego](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Kliknij przycisk OK, aby zakończyć edycję i wróć do bloku "Obliczeń i sieci".

5. Powtórz kroki 1 – 4 dla innych interfejsów sieciowych.

6. Kliknij przycisk "Zapisz", aby zapisać wszystkie zmiany.

## <a name="next-steps"></a>Kolejne kroki
  [Dowiedz się więcej](../virtual-network/virtual-network-network-interface-vm.md) o interfejsów sieciowych do obsługi maszyn wirtualnych platformy Azure.
