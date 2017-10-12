---
title: "Migrowanie na platformę Azure za pomocą usługi Site Recovery | Microsoft Docs"
description: "Ten artykuł zawiera omówienie migrowania maszyn wirtualnych i serwerów fizycznych na platformę Azure za pomocą usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/05/2017
ms.author: raynew
ms.openlocfilehash: f4dfe430fba51bd009431ca72279a21be55e3a40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrowanie na platformę Azure za pomocą usługi Site Recovery

Przeczytaj ten artykuł, aby zapoznać się z usługą Azure Site Recovery pod kątem migrowania maszyn wirtualnych i serwerów fizycznych.

Usługa Site Recovery jest usługą platformy Azure, która wspiera strategię BCDR przez organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub dodatkowego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania. Dowiedz się więcej w temacie [Co to jest usługa Site Recovery?](site-recovery-overview.md) Za pomocą usługi Site Recovery można też migrować istniejące obciążenia lokalne na platformę Azure w celu przyspieszenia przejścia do chmury i korzystania z szerokiej oferty funkcji dostępnych na platformie Azure.

W tym filmie wideo przedstawiono krótkie omówienie procesu migracji.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

W tym artykule opisano wdrażanie w witrynie [Azure Portal](https://portal.azure.com). [Klasycznej witryny Azure Portal](https://manage.windowsazure.com/) można używać do obsługi istniejących magazynów usługi Site Recovery, ale nie można tworzyć nowych magazynów za jej pomocą.

Komentarze możesz zamieścić na dole tego artykułu. Zadawaj pytania techniczne na [Forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Co mamy na myśli przez migrację?

Usługę Site Recovery można wdrożyć na potrzeby replikacji lokalnych maszyn wirtualnych i serwerów fizycznych na platformę Azure lub do lokacji dodatkowej. Maszyny są duplikowane, przechodzą do trybu failover z lokacji głównej, gdy wystąpi awaria, i z powrotem do lokacji głównej, gdy zostanie wykonane odzyskiwanie. Oprócz tego usługa Site Recovery umożliwia migrowanie maszyn wirtualnych i serwerów fizycznych na platformę Azure, dzięki czemu użytkownicy mają do nich dostęp jako do maszyn wirtualnych na platformie Azure. Migracja obejmuje replikację i przejście do trybu failover z lokacji głównej na platformę Azure oraz pełną migrację.

## <a name="what-can-site-recovery-migrate"></a>Co może migrować usługa Site Recovery?

Możesz:

- Przeprowadzić migrację obciążeń działających na lokalnych maszynach wirtualnych funkcji Hyper-V, maszynach wirtualnych programu VMware i serwerach fizycznych, aby działały na maszynach wirtualnych platformy Azure. W ramach tego scenariusza możesz również wykonać pełną replikację i powrót po awarii.
- Przeprowadzić migrację [maszyn wirtualnych IaaS platformy Azure](site-recovery-migrate-azure-to-azure.md) między regionami platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.
- Przeprowadzić migrację [wystąpień usługi AWS dla systemu Windows](site-recovery-migrate-aws-to-azure.md) do maszyn wirtualnych IaaS platformy Azure. W tym scenariuszu aktualnie jest obsługiwana tylko migracja, co oznacza, że powrót po awarii nie jest obsługiwany.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migracja lokalnych maszyn wirtualnych i serwerów fizycznych

Aby zmigrować lokalne maszyny wirtualne funkcji Hyper-V, maszyny wirtualne oprogramowania VMware i serwery fizyczne, należy wykonać prawie te same czynności co w przypadku zwykłej replikacji.

1. Konfigurowanie magazynu usługi Recovery Services
2. Skonfiguruj wymagane serwery zarządzania (VMware, VMM, Hyper-V — w zależności od tego, co jest migrowane), dodaj je do magazynu, a następnie określ ustawienia replikacji.
3. Włączanie replikacji dla maszyn, które mają zostać zmigrowane
4. Po początkowej migracji uruchom szybkie testowe przejście do trybu failover, aby się upewnić, że wszystko działa prawidłowo.
5. Po zweryfikowaniu, że środowisko replikacji działa, należy użyć planowanego lub nieplanowanego przejścia do trybu failover w zależności od tego, [co obsługuje](site-recovery-failover.md) scenariusz. Zaleca się przeprowadzenie planowanego przejścia do trybu failover, jeśli jest to możliwe.
6. W przypadku migracji nie jest koniecznie zatwierdzanie trybu failover ani jego usuwanie. Zamiast tego należy wybrać opcję **Zakończ migrację** dla każdej maszyny, którą chcesz zmigrować.
     - W obszarze **Replikowane elementy** kliknij prawym przyciskiem myszy maszynę wirtualną i kliknij pozycję **Zakończ migrację**. Kliknij przycisk **OK**, aby zakończyć. Postęp można śledzić we właściwościach maszyny wirtualnej, monitorując zadanie kończenia migracji w obszarze **Zadania usługi Site Recovery**.
     - Akcja **Zakończ migrację** kończy proces migracji, usuwa replikację dla maszyny i zatrzymuje naliczanie opłat za usługę Site Recovery dla maszyny.

![pełna_migracja](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Migracja między regionami platformy Azure

Maszyny wirtualne platformy Azure można migrować między regionami przy użyciu usługi Site Recovery. W tym scenariuszu jest obsługiwana tylko migracja. Inaczej mówiąc, możesz zreplikować maszyny wirtualne platformy Azure i przełączyć je w tryb failover do innego regionu, ale nie możesz wykonać powrotu po awarii. W ramach tego scenariusza należy skonfigurować magazyn usługi Recovery Services, wdrożyć lokalny serwer konfiguracji na potrzeby zarządzania replikacją, dodać go do magazynu i określić ustawienia replikacji. Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover. Potem należy uruchomić nieplanowane przejście do trybu failover z opcją **Zakończ migrację**.

## <a name="migrate-aws-to-azure"></a>Migracja usługi AWS na platformę Azure

Możesz zmigrować wystąpienia usługi AWS na maszyny wirtualne platformy Azure. W tym scenariuszu jest obsługiwana tylko migracja. Inaczej mówiąc, możesz zreplikować wystąpienia usługi AWS i przełączyć je w tryb failover na platformę Azure, ale nie możesz wykonać powrotu po awarii. Na potrzeby migracji wystąpienia usługi AWS są obsługiwane w taki sam sposób jak serwery fizyczne. Należy skonfigurować magazyn usługi Recovery Services, wdrożyć lokalny serwer konfiguracji na potrzeby zarządzania replikacją, dodać go do magazynu i określić ustawienia replikacji. Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover. Potem należy uruchomić nieplanowane przejście do trybu failover z opcją **Zakończ migrację**.




## <a name="next-steps"></a>Następne kroki

- [Migrowanie maszyn wirtualnych programu VMware na platformę Azure](site-recovery-vmware-to-azure.md)
- [Migrowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM na platformę Azure](site-recovery-vmm-to-azure.md)
- [Migrowanie maszyn wirtualnych funkcji Hyper-V (bez programu VMM) na platformę Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrowanie maszyn wirtualnych platformy Azure między regionami platformy Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrowanie wystąpień usługi AWS na platformę Azure](site-recovery-migrate-aws-to-azure.md)
- [Przygotuj migrowane maszyny do włączenia replikacji](site-recovery-azure-to-azure-after-migration.md) do innego regionu na potrzeby odzyskiwania po awarii.
- Zacznij chronić obciążenia, [replikując maszyny wirtualne platformy Azure.](site-recovery-azure-to-azure.md)
