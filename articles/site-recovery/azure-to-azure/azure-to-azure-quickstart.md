---
title: Replikacja maszyny Wirtualnej platformy Azure do innego regionu Azure (wersja zapoznawcza)
description: Ta opcja szybkiego startu zawiera kroki wymagane do replikowania maszyny Wirtualnej platformy Azure w jednym regionie Azure w innym regionie.
services: site-recovery
author: rajani-janaki-ram
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 4c52f15ba5be9171c00ccd466045d66973d3d5de
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-an-azure-vm-to-another-azure-region-preview"></a>Replikacja maszyny Wirtualnej platformy Azure do innego regionu Azure (wersja zapoznawcza)

[Usługi Azure Site Recovery](../site-recovery-overview.md) usługi przyczynia się do ciągłości i odzyskiwaniem po awarii (BCDR) odzyskiwania strategią biznesową, zatrzymując aplikacji biznesowych i uruchamiając dostępne podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza i organizuje odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure (maszyny wirtualne), łącznie z replikacji, trybu failover i odzyskiwania.

Ta opcja szybkiego startu opisano sposób replikowania maszyny Wirtualnej platformy Azure w innym regionie Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="enable-replication-for-the-azure-vm"></a>Włącz replikację dla maszyny Wirtualnej Azure

1. W portalu Azure kliknij **maszyn wirtualnych**i wybierz maszynę Wirtualną, którą chcesz replikować.

2. W **ustawienia**, kliknij przycisk **odzyskiwania po awarii (wersja zapoznawcza)**.
3. W **konfiguracji odzyskiwania po awarii** > **region docelowy** wybierz region docelowego, do którego będą replikowane.
4. W przypadku tego przewodnika Szybki Start należy zaakceptować ustawienia domyślne.
5. Kliknij przycisk **włączyć replikację**. Spowoduje to uruchomienie zadania, aby włączyć replikację dla maszyny Wirtualnej.

    ![Włączanie replikacji](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Sprawdź ustawienia

Po zakończeniu zadania replikacji można sprawdzić stan replikacji, Modyfikuj ustawienia replikacji i testowanie wdrożenia.

1. W menu maszyny Wirtualnej, kliknij przycisk **odzyskiwania po awarii (wersja zapoznawcza)**.
2. Można sprawdzić kondycję replikacji, punkty odzyskiwania, które zostały utworzone i źródłowy i docelowy regionów na mapie.

   ![Stan replikacji](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Maszyny Wirtualnej w regionie podstawowym zatrzymuje replikowanie po wyłączeniu dla niego replikację:

- Ustawienia replikacji źródła są automatycznie wyczyszczone.
- Lokacja odzyskiwania rozliczeń dla maszyny Wirtualnej również zatrzymuje.

Zatrzymaj replikację, w następujący sposób:

1. Wybierz maszynę Wirtualną.
2. W **odzyskiwania po awarii (wersja zapoznawcza)**, kliknij przycisk **więcej**.
3. Kliknij przycisk **Wyłącz replikację**.

   ![Wyłącz replikację](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Następne kroki

W tym szybkiego startu jednej maszyny Wirtualnej jest replikowany w regionie pomocniczym.

> [!div class="nextstepaction"]
> [Konfiguracja odzyskiwania po awarii dla maszyn wirtualnych platformy Azure](azure-to-azure-tutorial-enable-replication.md)
