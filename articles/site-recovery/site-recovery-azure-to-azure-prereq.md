---
title: "Wymagania wstępne dotyczące replikacji na platformie Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera podsumowanie wymagań wstępnych dotyczących replikację maszyn wirtualnych i maszyn fizycznych do platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: fb5b8c9ac96ac44d0112919664a177f33ef392da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-to-another-region-by-using-azure-site-recovery"></a>Wymagania wstępne dotyczące replikowanie maszyn wirtualnych platformy Azure do innego regionu za pomocą usługi Azure Site Recovery

> [!div class="op_single_selector"]
> * [Replikacja z platformy Azure do platformy Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikacja z lokalnego do platformy Azure](site-recovery-prereq.md)

Usługa Azure Site Recovery przyczynia się do strategii biznesowej ciągłości i odzyskiwaniem po awarii odzyskiwania (BCDR) poprzez organizowanie:
* Replikacja maszyn wirtualnych platformy Azure do innego regionu systemu Azure.
* Replikacji lokalnych serwerów fizycznych i maszyn wirtualnych na platformie Azure lub dodatkowego centrum danych. 

W przypadku wystąpienia awarii w lokalizacji głównej, możesz w trybie Failover do dodatkowej lokalizacji do zachowania aplikacji i obciążeń dostępne. Może zostać ponownie do lokalizacji głównej, gdy powróci ona do normalnego działania. Aby uzyskać więcej informacji na temat odzyskiwania lokacji, zobacz [co to jest usługa Site Recovery?](site-recovery-overview.md).

Ten artykuł zawiera podsumowanie wymagań wstępnych wymaganych do rozpoczęcia replikacji usługi Site Recovery z lokalnej na platformie Azure.

Opublikuj wszelkie komentarze u dołu artykułu lub zadawaj pytania techniczne na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Wymagania systemu Azure

**Wymaganie** | **Szczegóły**
--- | ---
**Konto platformy Azure** | A [Microsoft Azure](http://azure.microsoft.com/) konta.<br/><br/> Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
**Usługa Site Recovery** | Aby uzyskać więcej informacji o cenach usługi Site Recovery, zobacz [cenach usługi Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). Zaleca się tworzenie magazynu usług odzyskiwania w docelowym regionie Azure, który ma być używany jako lokalizację odzyskiwania po awarii. Na przykład jeśli źródłowe maszyny wirtualne są uruchomione w wschodnie stany USA, a chcesz replikować do środkowe stany USA, zaleca się utworzenie magazynu w środkowe stany USA.|
**Pojemność Azure** | Dla celu region platformy Azure, który ma być używany jako lokalizacja odzyskiwania po awarii musisz mieć subskrypcji z wystarczającą pojemność dla maszyn wirtualnych, kont magazynu i składników sieciowych. Można skontaktuj się z pomocą techniczną w celu dodania większej pojemności.
**Wskazówki dotyczące magazynu** | Upewnij się, że stosujesz [wskazówki magazynu](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) źródła Azure maszyn wirtualnych, aby uniknąć problemów z wydajnością. Jeśli wykonujesz ustawień domyślnych usługi Site Recovery tworzy kont magazynu wymagana, na podstawie konfiguracji źródła. Jeśli należy dostosować i wybrać własne ustawienia, upewnij się, należy przestrzegać [wartości docelowe skalowalności dysków maszyny wirtualnej](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Wskazówki dotyczące sieci** | Musisz dozwolonych łączność wychodząca z maszyny Wirtualnej platformy Azure dla określonych adresów URL lub adres IP zakresu. Aby uzyskać więcej informacji, zapoznaj się [wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure networking](site-recovery-azure-to-azure-networking-guidance.md) artykułu.
**Maszyna wirtualna platformy Azure** | Upewnij się, że wszystkie najnowsze certyfikaty głównego są obecne w systemie Windows lub maszyny Wirtualnej systemu Linux. Jeśli nie podano najnowsze certyfikaty główne, maszyny Wirtualnej nie można zarejestrować do odzyskiwania lokacji z powodu ograniczeń zabezpieczeń.

>[!NOTE]
>Aby uzyskać więcej informacji na temat obsługi dla określonej konfiguracji odczytu [macierz obsługi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej o [wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure networking](site-recovery-azure-to-azure-networking-guidance.md).
- Włączyć ochronę obciążeń przez [replikowanie maszyn wirtualnych platformy Azure](site-recovery-azure-to-azure.md).
