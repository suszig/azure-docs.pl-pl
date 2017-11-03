---
title: "Migrowanie maszyn wirtualnych z usług AWS na platformie Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób migracji maszyn wirtualnych działających na platformie Azure przy użyciu usługi Azure Site Recovery w portalu Amazon usług sieci Web (AWS)."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: b3c0727a279649f4f7dae30d41027129ce5b04ee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrowanie maszyn wirtualnych w Amazon Web Services (AWS) na platformie Azure za pomocą usługi Azure Site Recovery

W tym artykule opisano sposób migracji wystąpień systemu Windows usług AWS do maszyn wirtualnych platformy Azure z [usługi Azure Site Recovery](site-recovery-overview.md) usługi.

Migracja jest skutecznie trybu failover z usług AWS na platformie Azure. Nie można wykonać powrotu po awarii maszyn usług AWS i nie ma trwającej replikacji. W tym artykule opisano kroki dotyczące migracji w portalu Azure i są oparte na instrukcji [replikację do platformy Azure maszyny fizycznej](site-recovery-vmware-to-azure.md).

Zamieść wszelkie komentarze lub pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

Usługa Site Recovery może służyć do migracji wystąpień EC2 z dowolnym z następujących systemów operacyjnych:

- Systemu Windows (tylko w 64 bity)
    - Windows Server 2008 R2 z dodatkiem SP1 + (Citrix PV sterowników lub usług AWS PV tylko sterowniki. **Instancje systemem RedHat PV sterowniki nie są obsługiwane**) systemu Windows Server 2012 systemu Windows Server 2012 R2
- Linux (64-bitowy tylko)
    - Red Hat Enterprise Linux 6.7 (tylko w przypadku wystąpienia HVM zwirtualizowanych)

## <a name="prerequisites"></a>Wymagania wstępne

Oto, co jest potrzebne dla tego wdrożenia:

* **Serwer konfiguracji**: Amazon EC2 uruchomionych maszyn wirtualnych systemu Windows Server 2012 R2 jest wdrożony jako serwer konfiguracji. Domyślnie inne składniki usługi Azure Site Recovery (serwer przetwarzania i główny serwer docelowy) są instalowane podczas wdrażania serwera konfiguracji. W tym artykule opisano kroki dotyczące migracji w portalu Azure i są oparte na instrukcji [Dowiedz się więcej](site-recovery-components.md)

* **Wystąpienia EC2**: EC2 Amazon wystąpień maszyn wirtualnych, które chcesz przeprowadzić migrację.

## <a name="deployment-steps"></a>Kroki wdrażania

1. Utwórz magazyn usługi Recovery Services.
2. Grupy zabezpieczeń dla swoich wystąpień EC2 powinien mieć zasady skonfigurowane i umożliwiają komunikację między wystąpienie EC2, które chcesz przeprowadzić migrację i wystąpienia, na którym planujesz wdrożenie serwera konfiguracji.

3. Na tej samej Amazon wirtualnego chmury prywatnej jako swoich wystąpień EC2 wdrożyć serwer konfiguracji automatycznego odzyskiwania systemu. Zobacz VMware / fizycznych do platformy Azure wymagania wstępne dotyczące wymagania dotyczące wdrażania serwera konfiguracji.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Gdy serwer konfiguracji jest wdrożony w usług AWS i zarejestrowany w magazynie usług odzyskiwania, powinny być widoczne konfiguracji serwera i serwera przetwarzania w obszarze infrastruktura usługi Site Recovery w sposób przedstawiony poniżej:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Po wdrożeniu serwera konfiguracji (może potrwać do 15 minustes ma się pojawiać), zweryfikuj, że może komunikować się z maszynami wirtualnymi, które chcesz migrować.

6. [Konfigurowanie ustawień replikacji](site-recovery-setup-replication-settings-vmware.md).

7. Włącz replikację: Włączanie replikacji maszyn wirtualnych chcesz migrować. Może odnajdywać wystąpień EC2 przy użyciu prywatnych adresów IP, które można uzyskać z poziomu konsoli EC2.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Po chronionych wystąpień EC2 i zakończeniu replikacji do platformy Azure, [testować tryb failover](site-recovery-test-failover-to-azure.md) do sprawdzania poprawności wydajności aplikacji na platformie Azure.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Uruchom tryb Failover z usług AWS do platformy Azure dla każdej maszyny Wirtualnej. Opcjonalnie możesz utworzyć plan odzyskiwania i uruchomić przejściu w tryb Failover, migracja wielu maszyn wirtualnych z usług AWS na platformie Azure. [Dowiedz się więcej](site-recovery-create-recovery-plans.md) o planach odzyskiwania.

10. W przypadku migracji nie jest koniecznie zatwierdzanie trybu failover. Zamiast tego wybrano opcję przeprowadzenia migracji dla każdego komputera, który chcesz migrować. Akcja przeprowadzenia migracji kończy proces migracji, spowoduje usunięcie replikacji dla maszyny i zatrzymuje rozliczeń usługi Site Recovery dla maszyny.

    ![Migrate (Migracja)](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Następne kroki

- [Przygotuj migrowane maszyny do włączenia replikacji](site-recovery-azure-to-azure-after-migration.md) do innego regionu na potrzeby odzyskiwania po awarii.
- Zacznij chronić obciążenia, [replikując maszyny wirtualne platformy Azure.](site-recovery-azure-to-azure.md)
