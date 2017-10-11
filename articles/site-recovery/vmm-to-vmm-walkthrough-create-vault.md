---
title: "Tworzenie magazynu dla funkcji Hyper-V replikacji do lokacji dodatkowej z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje sposób tworzenia magazynu podczas replikowania maszyn wirtualnych funkcji Hyper-V do lokacji dodatkowej programu System Center VMM z usługą Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>Krok 5: Tworzenie magazynu dla funkcji Hyper-V replikacji do lokacji dodatkowej

Po przygotowaniu lokalnymi [programu System Center Virtual Machine Manager (VMM) serwerów i klastrów hostów Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md) na lokacji dodatkowej przy użyciu replikacji funkcji Hyper-V [usługi Azure Site Recovery](site-recovery-overview.md), można utworzyć magazyn usług odzyskiwania i wybrać scenariusz replikacji.

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Wybierz cel ochrony

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.

1. Kliknij przycisk **usługi Site Recovery** > **krok 1: Przygotowanie infrastruktury** > **cel ochrony**.
2. Wybierz **do odzyskiwania lokacji**i wybierz **tak, z funkcją Hyper-V**.
3. Wybierz **tak** aby wskazać, że używasz programu VMM na potrzeby zarządzania hostami funkcji Hyper-V.
4. Wybierz **tak** Jeśli pomocniczy serwer programu VMM. Jeśli wdrażasz replikacji między chmurami na jednym serwerze programu VMM, kliknij przycisk **nr**. Następnie kliknij przycisk **OK**.

    ![Wybieranie celów](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 6: Konfigurowanie replikacji źródłowych i docelowych](vmm-to-vmm-walkthrough-source-target.md).