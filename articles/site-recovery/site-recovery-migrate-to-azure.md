---
title: "Migrowanie lokalnych maszyn wirtualnych i serwerów fizycznych na platformę Azure przy użyciu usługi Site Recovery | Microsoft Docs"
description: "W tym artykule opisano sposób migrowania lokalnych maszyn wirtualnych i serwerów fizycznych na platformę Azure za pomocą usługi Azure Site Recovery"
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
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 423a1727efb0e1fd54eb0f8d5971ace3f8efc6cb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrowanie na platformę Azure za pomocą usługi Site Recovery

W tym artykule przedstawiono sposób migrowania lokalnych maszyn wirtualnych i serwerów fizycznych na maszyny wirtualne platformy Azure za pomocą usługi [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Przed rozpoczęciem

Obejrzyj ten film z krótkim omówieniem działań wymaganych do przeprowadzenia migracji na platformę Azure.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]


## <a name="what-do-we-mean-by-migration"></a>Co mamy na myśli przez migrację?

Usługę Site Recovery można wdrożyć na potrzeby replikacji oraz migracji lokalnych maszyn wirtualnych i serwerów fizycznych.

- W przypadku replikacji należy skonfigurować lokalne maszyny pod kątem regularnej replikacji do platformy Azure. Następnie w przypadku awarii należy przełączyć maszyny w tryb failover z lokacji lokalnej do platformy Azure i uzyskiwać do nich dostęp z platformy Azure. Po ponownym udostępnieniu lokacji lokalnej można do niej powrócić po awarii z poziomu platformy Azure.
- W przypadku używania usługi Site Recovery na potrzeby migracji należy zreplikować maszyny lokalne do platformy Azure. Następnie należy przełączyć je w tryb failover z lokacji lokalnej do platformy Azure i zakończyć proces migracji. Nie ma powrotu po awarii.  

## <a name="what-can-site-recovery-migrate"></a>Co może migrować usługa Site Recovery?

Możesz:

- **Przeprowadzić migrację lokalnie**: przeprowadź migrację lokalnych maszyn wirtualnych funkcji Hyper-V, maszyn wirtualnych programu VMware i serwerów fizycznych na platformę Azure. Po zakończeniu migracji obciążenia uruchomione na maszynach lokalnych będą działać na maszynach wirtualnych platformy Azure. 
- **Przeprowadzić migrację na platformie Azure**: przeprowadź migrację maszyn wirtualnych platformy Azure między regionami świadczenia usługi Azure. 
- **Przeprowadzić migrację usługi AWS**: przeprowadź migrację wystąpień usługi AWS dla systemu Windows do maszyn wirtualnych IaaS platformy Azure. 

## <a name="migrate-from-on-premises-to-azure"></a>Migrowanie z zasobów lokalnych na platformę Azure

Aby zmigrować lokalne maszyny wirtualne oprogramowania VMware, maszyny wirtualne funkcji Hyper-V i serwery fizyczne, należy wykonać prawie te same czynności co w przypadku pełnej replikacji. 


## <a name="migrate-between-azure-regions"></a>Migracja między regionami platformy Azure

Aby zmigrować maszyny wirtualne platformy Azure między regionami, należy wykonać prawie te same czynności co w przypadku pełnej migracji.

1. Należy [włączyć replikację](azure-to-azure-tutorial-enable-replication.md) dla maszyn, które mają zostać zmigrowane.
2. Należy [uruchomić szybkie testowanie trybu failover](azure-to-azure-tutorial-dr-drill.md), aby upewnić się, że wszystkie funkcje działają.
3. Potem należy [uruchomić nieplanowane przejście w tryb failover](azure-to-azure-tutorial-failover-failback.md) z opcją **Zakończ migrację**.
4. Po ukończeniu migracji możesz [skonfigurować replikację w celu odzyskiwania po awarii](site-recovery-azure-to-azure-after-migration.md) z regionu świadczenia usługi Azure, do którego przeprowadzono migrację, do regionu pomocniczego.



## <a name="migrate-aws-to-azure"></a>Migracja usługi AWS na platformę Azure

Możesz zmigrować wystąpienia usługi AWS na maszyny wirtualne platformy Azure.
- W tym scenariuszu jest obsługiwana tylko migracja. Inaczej mówiąc, możesz zreplikować wystąpienia usługi AWS i przełączyć je w tryb failover na platformę Azure, ale nie możesz wykonać powrotu po awarii.
- Na potrzeby migracji wystąpienia usługi AWS są obsługiwane w taki sam sposób jak serwery fizyczne. Należy skonfigurować magazyn usługi Recovery Services, wdrożyć lokalny serwer konfiguracji na potrzeby zarządzania replikacją, dodać go do magazynu i określić ustawienia replikacji.
- Następnie należy włączyć replikację dla maszyn, które mają zostać zmigrowane, i uruchomić szybkie testowe przejście do trybu failover. Potem należy uruchomić nieplanowane przejście do trybu failover z opcją **Zakończ migrację**.






## <a name="next-steps"></a>Następne kroki

- [Migrowanie maszyn lokalnych na platformę Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrowanie maszyn wirtualnych między regionami świadczenia usługi Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrowanie usługi AWS na platformę Azure](tutorial-migrate-aws-to-azure.md)
