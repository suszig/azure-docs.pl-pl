---
title: "Dotyczące migracji w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób migracji lokalnych i maszyn wirtualnych platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: 966d532a33626a8fcc3a3b93790d203aadfd81b4
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="about-migration"></a>Dotyczące migracji

Przeczytaj ten artykuł, aby szybko zapoznać sposób [usługi Azure Site Recovery](site-recovery-overview.md) usługi ułatwia migrację maszyn. 

Oto co można przeprowadzić migrację za pomocą usługi Site Recovery:

- **Migrację z lokalnej na platformie Azure**: migrowania maszyn wirtualnych funkcji Hyper-V lokalnych maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure. Po zakończeniu migracji obciążenia uruchomione na maszynach lokalnych będą działać na maszynach wirtualnych platformy Azure. 
- **Przeprowadzić migrację na platformie Azure**: przeprowadź migrację maszyn wirtualnych platformy Azure między regionami świadczenia usługi Azure. 
- **Przeprowadzić migrację usługi AWS**: przeprowadź migrację wystąpień usługi AWS dla systemu Windows do maszyn wirtualnych IaaS platformy Azure. 


## <a name="what-do-we-mean-by-migration"></a>Co mamy na myśli przez migrację?

Oprócz odzyskiwania po awarii lokalnymi i maszyn wirtualnych platformy Azure przy użyciu usługi Site Recovery, można użyć usługi Site Recovery celu przeprowadzenia migracji. Jaka jest różnica?

- Podczas odzyskiwania po awarii możesz replikować maszyny regularnie na platformie Azure. Po awarii, awaryjnie maszyn z lokacji podstawowej do lokacji dodatkowej Azure i uzyskiwać do nich dostęp z tego miejsca. Gdy lokacja główna jest ponownie dostępny, nie z platformy Azure.
- W przypadku migracji można replikować maszyny lokalnej do Azure lub maszynach wirtualnych platformy Azure w regionie pomocniczym. Następnie przełączyć maszyny Wirtualnej z lokacji głównej na serwerze pomocniczym i ukończyć proces migracji. Nie ma powrotu po awarii.  


## <a name="migration-scenarios"></a>Scenariusze migracji

**Scenariusz** | **Szczegóły**
--- | ---
**Migrację z lokalnej na platformie Azure** | Można migrować lokalnych maszyn wirtualnych VMware, maszyn wirtualnych funkcji Hyper-V i serwerów fizycznych do platformy Azure. W tym celu należy wykonać prawie te same kroki jak w przypadku odzyskiwania po awarii pełna. Po prostu nie zakończyć się niepowodzeniem maszyny platformy Azure z powrotem do lokacji lokalnej.
**Migracja między regionami platformy Azure** | Maszyny wirtualne Azure można migrować z jednego regionu Azure do innego. Po zakończeniu migracji można skonfigurować odzyskiwania po awarii dla maszyn wirtualnych Azure w regionie pomocniczym, do którego zostały zmigrowane.
**Migrowanie usługi AWS na platformę Azure** | Możesz zmigrować wystąpienia usługi AWS na maszyny wirtualne platformy Azure. Usługa Site Recovery traktuje wystąpień usług AWS jako serwerów fizycznych na potrzeby migracji. 

## <a name="next-steps"></a>Kolejne kroki

- [Migrowanie maszyn lokalnych na platformę Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrowanie maszyn wirtualnych między regionami świadczenia usługi Azure](tutorial-migrate-azure-to-azure.md)
- [Migrowanie usługi AWS na platformę Azure](tutorial-migrate-aws-to-azure.md)
